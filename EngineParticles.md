_Module manager: Davy_

# Introduction #

Because game designers and artists have gone completely crazy on this project, we need particle systems. The purpose of this engine module is to implement simple systems supporting:
  * Particle emission and death. Particles may be emitted either regularly with a given rate, or with immediate bursts of a lot of particles at a time.
  * Sprite-based particles. Flying things should be SFML sprites, so that we can draw other things that just color points.
  * Particle life style. Particle color (including alpha), size, position and rotation should evolve during its lifetime.
  * Simple dynamics: particle moves should follow a simple Euler-integrated formula, and support basic force generators (wind, gravity, ...). To avoid having these generators in the every particle system, the API should take a simple force accumulator, such as update(dt, totalForces).
  * No collision is needed. Neither with scenery elements nor between particles themselves.

# Implementation considerations #

Internally, the number of particles should be fixed. This will allow pool-based particle allocation, removing completely the need for advanced memory management techniques. That means that all the particles are stored in a big dynamic array whose size can be changed, but not at every frame.

Care should be taken not to consume to much resource during the update and rendering of particles. For example, maybe one sf::Sprite is enough for rendering all the particles.

The number of particles can get very high. The performance target for this module could be around 1000 particles on screen with respectable performance. But because this also depends on how SFML is implemented internally, this value _could_ be lowered.

# Demo #

Because all things produced by this module are graphical, tests can be done manually with a real particle system drawing. Some intermediate classes may be tested automatically, though.

# API philosophy #

Class design is up to the developer making this module (reviewed, though :p), but the central API functions should probably be the following ones:
  * update(float dt, math::Vec2 forces) // time in seconds
  * draw(sf::Window)