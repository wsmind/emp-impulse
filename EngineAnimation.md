_Module manager: Bloutiouf_

# Introduction #

This component adds the sprite animation feature to the engine, which is not present in SFML. It does not actually rely on SFML to achieve this task because its central goal is to provide sub-rectangles that will be fed into SFML sprites by higher-level layers.

Removal of this dependency has two major advantages:
  * The main output of this module being numerical only, it is much more friendly with automated unit testing.
  * It will avoid having to reimplement uselessly some features that are already in SFML (e.g SetPosition, SetRotation, etc.)

# Detailed technical needs #

This module have to implement the following features:
  * Support for multiple animations (e.g walk, jump, ...). No transition or blending are required when switching from one animation to another.
  * Time-scaling (playing animations at any speed). In this low-level layer, this will be implemented by simply **not** implementing time tracking. Updates will be done by a simple api call taking the time delta as an argument (i.e update(float dt)).
  * To ensure consistency with SFML time API, most of time values should be stored as float, in seconds.
  * Animation tracks (i.e positions of the sub-rectangle for each frame) will be stored in dedicated animation files, which must be loaded by the module. A simple ASCII-based format will probably be enough for these files. Another option would be to generate these positions based on a simple pattern (e.g square, regularly aligned sub-rectangles in the sprite image). In that case, the animation file should store at least start and end frames for each animation, along with (relative) square size.
  * Sub-rectangles must be in sprite-relative coordinates (`[`0-1`]`x`[`0-1`]`, top-left origin), allowing texture resolution to change without impact on animation tracks.

# Workflow considerations #

It would be great is artists had a simple way of generating sprite files with their associated animation files. That would probably involve transforming a sequence of still images into a tiled sprite image and an animation file. Tools may be written to help with this task, but this will be the next step, when this module is ready.

# Recommended testing methods #

While tests are completely up to the developer writing this module, two complementary approaches could be taken to ensure that this module is working properly:
  * Automated sub-rectangle verifications: loading an animation file, updating it with fixed time values, checking output rectangles.
  * Funnier things: making a small non-automated graphical test to actually render an animated sprite using SFML. This would involve taking the rectangle output of this module and feeding it into the SetSubRect() method of sf::Sprite.

# Current implementation #

The main concepts are:

## Animation data ##

Each object (especially character) is made up of two files:
  * an big image, which stores all the images the object may show.
  * an animation data file, which stores information on where the images are located in the big image and how they follow on from each other.

These two files are independent. The design allows for using a single animation data file on several objects, but for now each character has its own animation data.

Animation data file defines sequences. They are finite timeline. They return rectangles and events: they know nothing about images themselves. It is up to the scene to select the correct image (inside the big image) given the returned rectangle. Events are a convenient way to define special animation times.

## Animation state ##

An animation state is an instance of an animation data. In itself, the animation data / animation sequence objects store no state: they are just information. An animation state stores a current playing sequence, and a time in the timeline. Each sprite should have its own animation state.

## Animation compiler ##

The animation compiler tool merges images and a configuration file into a big image and an animation data file.

The configuration file must have the following format:
```
    basepath = "./"

    rate = 25

    scale = 0.5

    sequences =
    {
       bounce =
       {
          path = basepath .. "bounce images/ball%04d.png",

          index = 1,

          rate = 2,

          offset =
          {
             x = 128,
             y = 224
          },

          events =
          {
             [0] = "ground",
             [1.05] =
             {
                "foo",
                "bar"
             }
          }
       }
    }
```

  * A global array "sequences" must be defined, defining sequences.
  * Each sequence is index by its name in the "sequences" array (in this example there is a single sequence named "bounce"), and is itself an table containing the following fields: "path", "index", "rate", "scale", "offset", "events".
  * The path may be: a path for a single image, a path for multiple images when a pattern is given (internally it uses sprintf, giving the ability to match the exported filename patterns), or an array (simple list) of path.
  * Index is needed only if you give a path with pattern. It is the first index replacing the wildcard characters (usually 0 or 1). If not defined, set to 0. (not yet implemented, the indexes actually start at 1)
  * The rate is the number of images per second. Alternatively, a global variable rate may be defined, this serves as a default value for sequences which don't have a rate field (in this example I show both).
  * The scale resizes images. A value of 0.5 will produce images four times smaller. Like rate, a global variable may be defined instead. Defaut is 1 (no resize). Bilinear interpolation.
  * Offset is... well, is offset points to the pelvis, then when you set (ingame) the sprite position, the pelvis will be at this position. Offset uses dimensions before the images are scaled.
  * Events is the list of the events generated when playing the animation (actually this is not gameplay events, but they may be strongly related). This is an array of strings, indexed by the time of the event occurrence in second. Alternatively, if you want to set some events at a single time, you may give an array (simple list) of strings. Time may not be a multiple of the frame duration (1/rate): they can happen at any time in range `[0, frame number * frame duration]`.
  * "basepath" is not part of the specs: because this file is a Lua script, we may define variable for convenience. ".." is the operator "concatenation".

The tool does not detect identical images.