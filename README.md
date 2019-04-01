# Input Control

Give people control over input.

If you're working on a game jam, "quick, drop this library in!"
Otherwise: "quick, drop this library in!"
That is, it'll still be quick either way.

### Goals

- On the dev side, **provide a clean API for consuming inputs.**

	- Without using a library,
	I've found it works fairly well to have a `Controller` object
	with properties for the available controls (like `controller.jump`)
	and subclasses like `KeyboardController` and `GamepadController`
	that provide an `update` method that actually updates the input properties.

		- A `CoupledController` can be used to support both keyboard and gamepad,
		good for single-player games with hard-coded controls.

		- I've also used an `AIController` for NPCs, which works well.
		An `AIController` generally needs access to the entity it's controlling and the world/level/room it resides in.
		(If you have several entities with AI, you might want separate `Controller` classes for each
		or you might want to simply handle the AI in the entities' regular update/step. But having controllers decoupled is fun, it lets you swap things around and make an AI-controlled player or play as an enemy.)

	<!--
	- We'd support arbitrary controls and controllers though,
	so it would probably always use something like the equivalent of a `CoupledController`
	although with sometimes only one sub-controller.
	And it probably doesn't make sense to have the properties directly on the object.

	You might want to use the verb `update` as a control name for instance.
	We could use a title-case naming scheme and recommend always using bracket notation,
	i.e. `controller["Update"]`
	and then you shouldn't run into any problems making a game about prototyping.
	But that wouldn't be ideal.
	Being able to say `controller.jump` is nice.
	-->
	<!-- This isn't the hugest issue, but API comfort is important. -->

	<!-- - **Support multiplayer**, local and networked.
	I don't have much experience doing networked multiplayer games.
	When I did, it helped to have a `Controller`
	that I could serialize and deserialize,
	to sync up inputs between clients rather than just velocities and such, because it implies syncing up (what leads to) acceleration instead of just velocity and position. 
	I used a server-side `Controller` that received input from a client over the network. -->


- **Allow remapping** of individual keyboard, mouse, gamepad, and other inputs.


- **Allow high level assignment of controllers to players**, so you can say okay, Player 1 is keyboard+mouse and Player 2 is gamepad 1 (and then when you realize the side by side splitscreen doesn't match how you're sitting with this setting, you can just swap it, without having to remap every control.)
Cortex Command has a nice version of this if I recall correctly, including setting players as AIs; that could be useful.


- Allow deferring to other means of control
such as touchscreen controls for mobile devices
(which could be handled by a library.)


- Support mobile-device-as-a-controller, a la [node-virtual-gamepads](https://github.com/miroof/node-virtual-gamepads), [nunchuck.js](https://github.com/ehzhang/nunchuck), ...or [snex.io](https://snex.io)!


- MIDI devices as controllers - why not! This would open up to a weird range of devices, which could be fun.


- **Provide a clean API for rendering controls** as text, icons, or both,
for use in menus, HUDs, sign posts in a tutorial level, etc.

	- In a local multiplayer game,
	you need to show the controls for both/all players.
	(Sometimes they're the same, i.e. multiple controllers of the same type,
	but you can't guarantee this.)

	- You should have full control over the text displayed,
	and should be able to handle it intelligently.
	“Press up to jump” should be
	“Press <kbd>W</kbd> to jump” if that's what the jump control is configured as.
	“Use left and right to move around”,
	“Use the left analog stick to move around”,
	“Use the arrow keys or WASD to move around, or plug in a controller”


- **Provide modular UI**:

	- The base of the UI would be an abstract model of the interface.
	Could use [Redux](http://redux.js.org/) or similar.

	- Icon sets / themes could go in modules,
	with the option to create a custom theme,
	or render whatever you want based on a descriptor.

		- Something like `{type: "stick", name: "left-stick", pressed: true, x: 0, y: 0}`
		(and other types like `key`, `button`, `dpad-button`, maybe the whole `dpad`)

		- It would be nice to have dynamic icons for analog sticks.
		(For instance you might want to indicate the player should move the stick in a circular motion or back and forth)

			- We could even have a light API for animating the descriptors

				- Based on higher level descriptors,
				something like `{type: "stick", name: "left-stick", animation: "counterclockwise", t: 0.5}`

				- Or multiple methods,
				more like `.animateCircularMotion()`

		- Icons should indicate their physical appearance if known
		or render them however you want.

		- All button icons should have a normal and pressed state.
	
	- Provide a basic GUI
	with [React](https://facebook.github.io/react/)
	or [d3](https://d3js.org/) or similar.
	We might want to render straight to a canvas,
	but we could still leverage the DOM behind the scenes for accessibility.
	
	- Provide a basic VR GUI
	with [a-frame](https://aframe.io/)

- **It should be incredibly easy to integrate.**
With this library, there should be *no excuse*
not to allow users to configure their controls.
<!-- except for games geared towards a specific kind of input
like text adventures and stuff -->
The easier it is, the more devs will adopt it;
the more games that have it, the more users can enjoy it,
(and then more people will see it,
and want it and ask for it,
and PR for it and get it,
and enjoy it.)

<!-- - **Let the user receive updates to the library ASAP**
	- (Input Control is all about giving power back to the users.)
	- Could recommend loading the script from [unpkg](https://unpkg.com/) with a version range,
	but I tend to download scripts locally, and I don't know about that.
	You'd definitely want to fall back to a local version if it fails to load.
	Also, I wouldn't want to be in a position to accidentally break a bunch of games, if possible.
	- Could have a button in the settings
	that tries to dynamically load a newer version of the library
	and somehow have it plug in and have it take over,
	ideally with the ability to switch back.
		- It'd need be able to update themes as well,
		unless the library is built with each of the default themes built-in,
		and it would need fall backs since new controls could be added etc.
		- This seems complicated. -->

There are theoretical benefits to having a shared system:
- **Familiarity**: if users recognize that the UI works in the same ways as in other games, they can pick it up and use it faster; they should appreciate the consistency
(assuming the UI is done well, but it should basically be done *best*; after all...)
- **Concerted development**: improvements to the library go to all the games that use it!
(Open source FTW)
- As well as **simply all the features that wouldn't make sense for any one game to implement**
unless it's a core gameplay conceit, like using a phone as a controller (and perhaps as a lightsaber [in a Star Wars game](https://www.chromeexperiments.com/experiment/lightsaber-escape)).
Or undo/redo. Come on, no game is gonna implement undo/redo, in their control configuration screen, if they even have one.
But *ideally* tho, ideally, on an ideal situation, they would. And a library can.

<!--
### Problems / the state of things / this is a real mess of a section

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
-->

Yeah, so -
- ease for the dev
- power to the player
- useful features individual games would generally never bother to implement
- consistency and solidity
- and long term benefits *across* games to adoption.


This is not in development. Development has not started! And I'm not planning on starting in the near foreseeable future. But if you're interested in something like this, particularly in creating 
