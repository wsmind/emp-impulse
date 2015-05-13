# Introduction #

The game is divided in 3 main layers: low-level engine, scene abstraction and gameplay. There is also a utility math package, usable everywhere needed.

# Engine #

The engine contains raw feature. To a certain extent, it is supposed to be powerful and flexible. Its core is SFML, a free 2D multimedia library. But because we had other needs, some other additions have been made to it, namely particle systems, sprite animation, collision detection and some UI shapes.

The engine also offers a simple resource manager, which basically avoids loading the same asset multiple times in memory when only one instance is enough.

# Scene #

The scene provide a quick and convenient way for the gameplay to access technical things. The flexibility and power of the engine has a cost in complexity, and the scene is here to balance that. In other words, the scene provides _what the gameplay needs_, instead of _what the engine can do_.

To achieve this goal, the technical representation of the game (i.e sprites, sounds, fx, etc... anything not directly related to gameplay) has been broken down into a few predefined categories that will be used directly by game objects:
  * Background (image, clouds)
  * Level elements
    * Animated sprites
    * Sounds
    * Collision shapes
    * Particle emitters
  * UI
  * Post-effects
  * Music
  * Camera

Note that this is only a first version... if this list must be refactored/changed, it will be ;)

Because the scene is also responsible for creating the window, it provides a simple input state abstraction (e.g left, right, jump, action).

Another important point is that the scene, like the engine, knows very few about time. The game has to regularly call update() function for objects to move. That will allow fine-grained time manipulations in the gameplay.

On the other hand, the scene completely hides the way it is rendered (drawing graphics, making sounds, etc.). The game only has to place objects in the scene and they will be rendered next frame.

# Game #

The central element in the game package is the world, which contains game objects. Game objects are _more or less_ C++ objects, with a few important design decisions:
  * Simple string identification. You won't see game objects pointers here and there. Every game object has a unique name in the world, stored as a standard string. Everytime a reference to an object is needed, this string is used.
  * Event-based communication. Because making methods to implement for everything that can happen in the world would be so tedious, game objects communicate exclusively with each other using semi-structured events. If you want to "talk" to another game object, you send an event. And at the other end, you handle it.
  * Flexible event structure. Events are composed of a name, and a set of named Variant values. That means you can send almost _eveything_ without being annoyed with type problems or limited event structure.
  * Light introspection. Every object must be able to declare its fields in an easy and consistend manner. This will allow, among other things, to automatically load and save the world state, and help edition a lot.


Obviously, game objects will have full access to the scene, in order to represent themselves with the help of technical entities. On the other hand, nothing in the game package is supposed to use the engine directly.

# Zooming out #

![https://docs.google.com/drawings/pub?id=1MUGxf5HgsXtLmqoPvA__PeF-O0e6FqtFRCekEMWuIn0&w=756&h=599&wikirequiresextension=.png](https://docs.google.com/drawings/pub?id=1MUGxf5HgsXtLmqoPvA__PeF-O0e6FqtFRCekEMWuIn0&w=756&h=599&wikirequiresextension=.png)

# About paradigms #

Because each layer of the architecture has different goals, they are various philosophies behind them.

The engine layer follows an immediate paradigm: whenever you want to do something, you have to call a method. For instance, to load a sprite, you call sf::Sprite::LoadFromFile(), and to render it, you need to write window->Draw(sprite).

The scene layer has a declarative style. If you want to place something in the scene, you just need to create/declare it, and then it will be loaded and rendered automatically. In other words, the scene::XXX implementations will handle these things by calling the immediate engine API.

The gameplay layer is very similar to an agent-based simulation. Game objects are agents, exchanging events. No communication happens between objects except using events. To find other agents to send events to, you can either store their name (i.e configured during map creation) or query the world for objects near a certain point or intersecting a given shape.