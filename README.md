# Input Control

Give users control over input.

If you're working on a game jam, quick, drop this library in!
Otherwise, quick, drop this library in!

### Goals

- **Provide a clean API for receiving controls.**

	- Without using a library,
	I've found it works fairly well to have a `Controller` object
	with properties for the available controls
	and subclasses like `KeyboardController` and `GamepadController`
	that provide an `update` method.
	A `CoupledController` can be used to easily support keyboard or a gamepad,
	(I've also done a networked `RemoteController` once
	although it wasn't the smoothest multiplayer.
	It helped to synchronize the clients between updates,
	but it )

	- We'd support arbitrary controls and controllers though,
	so it would probably always use something like the equivalent of a `CoupledController`
	although with sometimes only one sub-controller.
	And it probably doesn't make sense to have the properties directly on the object.
	You might want to use the verb `update` as a control name for instance.
	We could use a title-case naming scheme and recommend always using bracket notation,
	i.e. `controller["Update"]`
	and then you shouldn't run into any problems making a game about prototyping.
	But that wouldn't be ideal,
	and neither would storing the properties on a single-char property to try to preserve terseness
	i.e. `controller._.update`.
	Being able to say `controller.jump` is nice.
	Not being able to use `controller.update` would be: not nice.
	<!-- This isn't the hugest issue, but API comfort is important. -->


- **Allow high level assignment of controllers**
(keyboard/mouse, gamepads etc.)
to players
<!-- you shouldn't have to remap every control
when you realize the gamepads are 0 indexed
even though it defaults to Gamepad 1 for Player 2
because Player 1 has the Keyboard/Mouse and it just gives a player the gamepad corresponding to the slot--
okay, input mapping was a particularly bad in that game;
but the point is it's nice to have high level control over this
(like splitscreen when inevitably you're on the opposite sides from the way you're sitting,
even though it would obviously be better split top and bottom since it's a frickin' FPS on a widescreen TV
but that isn't an option
*cough* Borderlands, PS3) -->
<!-- Cortex Command does this part nicely if I recall correctly -->

<!--
Also touch controls,
and the flexibility to defer to other control schemes
(touch controls could be handled by a library)
-->

<!--
* Could support mobile-device-as-a-controller, a la [node-virtual-gamepads](https://github.com/miroof/node-virtual-gamepads) or [nunchuck.js](https://github.com/ehzhang/nunchuck)
-->


- **Allow remapping** of individual keyboard, mouse, gamepad, and other inputs.


- **Support multiplayer**, local and networked.


- **Modular UI**:
It could use Redux or similar as a basis for handling the UI in a modular way.
Icon sets could go in modules.
Could possibly leverage the DOM behind the scenes for accessibility.

<!-- Previously writ:
* UI that can be overridden at multiple levels,
i.e. either themes that can make functional changes or
themes combined with other points to modify or replace the UI
(VR, anyone?) -->


- **Controls need to be displayed** in menus, HUDs, and the game world,
as text, icons, or both.

	<!-- - It needs to be *really easy* to do. -->

	- “Press up to jump” should be
	“Press <kbd>W</kbd> to jump” if that's what the jump control is configured as.

	- Icons should indicate their physical appearance if possible,
	and in different themes, with the option to create a custom theme
	or render them however you want.

	- In a local multiplayer game,
	you need to show the controls for both/all players.
	(Sometimes they're the same, i.e. multiple controllers of the same type,
	but you can't guarantee this.)

	- You should have full control over the text displayed,
	and should be able to handle it intelligently.
	“Use left and right to move around”,
	“Use the left analog stick to move around”,
	“Use the arrow keys or WASD to move around, or plug in a controller”

<!-- Previously the above was:
* Provide names and visuals for inputs
	* Allow easy usage inside help text
-->

<!-- - **Let the user receive updates to the library right away**
hosting npm npm has a cdn dsfright?

- **Dynamically load a newer version of the library**
from the settings
and somehow have it plug in to itself and take over,
ideally with the ability to switch back.
(It'd need be able to load new icons as well, or to fall back.) -->
<!-- Input Control is all about giving power back to the users. -->

- **It should be incredibly easy to integrate.**
With this library, there should be *no excuse*
not to allow users to configure their controls.
<!-- except for like an explicit decision to limit control options
so that pretty much everyone who's playing the game is using the same input
which I could begrudgingly award some theoretical merit -->
<!--
The easier it is, the more devs will adopt it;
the more games that have it, the more users can enjoy it,
and the more people will seeee it
and waant it and ask for it
and PR it and get it
and enjoy it
-->

There are theoretical benefits to having a shared system:
- **Familiarity**: if users recognize that the UI works in the same ways as in other games, they can pick it up and use it faster; they should appreciate the consistency
(assuming the UI is done well, but it should be done best; after all...)
- **Concerted development**: improvements to the library go to all the games that use it!
- As well as **simply all the features that wouldn't make sense for any one game to implement**
unless it's a core gameplay conceit, like using a phone as a controller (or more specifically as a lightsaber [in a Star Wars game](https://www.chromeexperiments.com/experiment/lightsaber-escape)).
Or undo/redo. No game is gonna implement undo/redo.
Come on. But they should. And a library can.
And once enough games start using the library...

<!--
### Problems / the state of things / this is a real mess

Users could install remapping software--
sometimes have to install remapping software--
in order to play a game with the controls or type of controller they want or have available.
But this doesn't cover UI in-game, where controls need to be displayed in tutorials and in menus.
(btw, you might want to display controls in the game world hand-drawn on a sign post for instance, so you will need absolute control over rendering)
In games that *do* support multiple types of inputs (keyboard/mouse, controller)
they often utterly fail to display the correct inputs for a player!

Usually they'll only ever show one type of input, controller or keyboard & mouse,
and often hardcoded with the default controls (so if you find a better set of controls for yourself, parts of the game make less sense now yay)>

Even if a game programmer has experience with all the GUI stuff...
and they probably want to focus on the *game*.
Even if [games are user interfaces](http://www.gamasutra.com/view/news/126450/Opinion_UI_Is_The_Game_The_Game_Is_UI.php) and input is part of the game,
it still generally feels tangentially related to your game,
and let's face it, it *is* a lot of work.
Especially if you're doing a game jam (which is fun btw), you don't have time for that stuff!

This library gives you *no excuse*.

Yeah, so -
- power to the player
- useful features you'd never bother to implement yourself (or hadn't even thought of)
- consistency and solidity
- and long term benefits *across* games to adoption.

It's a piece of cake, or at your choice, pie (heck, maybe some pie menus?)
with some ice cream thrown in there for FREE
that's the goal.
-->
