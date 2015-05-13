# Introduction #

The in-game UI is intended to be as clear and light as possible.
Because possible actions may depend on the context (e.g. moving a character can only be done if it's selected), we define two types of in-game user interfaces:

  * the **static interface** is always shown on screen, because the actions it enables are always possible. It is neither immobile nor visually unchanging. It is just never disappearing ;

  * the **contextual interface** is only shown when the actions it enables are available or possible. Just like the static interface, it can be animated. But unlike the static interface, it disappears when it's no longer needed.


# Static interface: characters status #

The static interface only features up to 5 icons representing the up to 5 available characters.

![http://i26.servimg.com/u/f26/14/76/89/16/static10.png](http://i26.servimg.com/u/f26/14/76/89/16/static10.png)

There is one "frame" per character that is available in the game. Thus, at the beginning of the game, there is only one frame (the first white one).

A frame appears when a character first becomes available or controlled. Theoretically, the appearing frame is not grayed, because in-game, the player always gain control of already-spawned elements.
A non grayed frame goes grayed when an element is called back by the hero. A grayed frame goes ungrayed when an element is spawned by the hero.

Each frame has a specific "skin" or theme related to the element it stands for. When selected, a skinned glow sprite appears around the frame, as described here :

![http://i26.servimg.com/u/f26/14/76/89/16/charac14.png](http://i26.servimg.com/u/f26/14/76/89/16/charac14.png)

# Contextual interface #
## Overlays: sprites and text ##

From time to time, some UI or story hints will be displayed to the player, as either a sprite (e.g. a "up arrow" picture), or a text message.

As for User Interaction, the sprites will be needed to show the keys to use to perform some actions.
The sprites can be anchored to some position in the level (and thus can scroll with the level), or they can be considered as a character in a text sequence (thus allowing to display a sentence "SPACEBAR to start", where SPACEBAR would be a sprite representing a Spacebar key).

## The hero: character spawning UI ##

This UI appears when the player takes control of the hero / fifth element, and activates its "power".

Above the fifth element, a semi-circle UI appears.
Its shape and appearance are static (i.e. do not change).

This UI is a menu: there is 4 buttons (1 per element) + 1 button in the center of the circle (let's call it "reset button").

An element button (among the 4) is activated if and only the element it stands for is available in the game.
An unactivated button is grayed.
The picture below represents all buttons as activated (because they are colored Smile ).

Clicking on an activated element-button yields the following effects:

  * If the element is not spawned ingame, then the element is spawned near the hero.
  * If the element is spawned ingame, and is "near" the hero, then the element is removed from the game.
  * If the element is spawned ingame, but is not "near" the hero, then a text message appears onscreen (e.g. "too far away from element to take it back.").

The 5th "reset button" aims at resetting the element spawn status, i.e. calling back each element ingame. Proximity rules apply just like if the player clicked on each ingame element's button.


When the 5th element / hero is selected, the menu shows up when the player activates the 5th element power (e.g. by pressing spacebar).
The player has to maintain spacebar pushed to keep the menu onscreen. If the spacebar button is release, the menu goes away, and no action will be performed whatsoever. **This means that "releasing spacebar" = "cancel".**

While holding spacebar pressed, the player can move a "selection cursor" from a button to another. This can be achieved using the arrows (or WASD-like) equivalent. An explicit "pushing" of a button occurs when the player presses Enter (or any button meaning "OK/Use"). If we eventually decide to use a mouse, the mouse too can be used to click on the desired button.

Clicking on an enabled button closes the menu (so the player can stop pressing spacebar) and executes the intended action.
Clicking on a disabled button does nothing (the menu is still onscreen provided that spacebar is still pressed).

When the menu shows up (spacebar has begun to be pressed), the default selected button is the "reset button" in the middle.

![http://i20.servimg.com/u/f20/15/70/98/46/ui-fif10.jpg](http://i20.servimg.com/u/f20/15/70/98/46/ui-fif10.jpg)

Note that on this image, the 4-coulour flowers are supposed to symbolize the elements. They easily can be replaced by decent element logos.
The middle button's picture was suppose to represent "all elements". Yeah, I'm no good at drawing :s.

We can imagine overlaying this UI with some text, the first times that the player has to use this menu.
Something like that, but less verbose.

![http://i20.servimg.com/u/f20/15/70/98/46/ui-fif11.jpg](http://i20.servimg.com/u/f20/15/70/98/46/ui-fif11.jpg)