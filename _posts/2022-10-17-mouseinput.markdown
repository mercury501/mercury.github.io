---
title:  "Exploring Silent Hill 2's input handling!"
date:   2022-10-17 
---

<div class="embed-container">
  <iframe
      src="https://www.youtube.com/embed/tO0ptcqxxKk"
      width="700"
      height="480"
      frameborder="0"
      allowfullscreen="true">
  </iframe>
</div>

### [](#header-1)Laying down the groundwork
When I joined the Silent Hill 2: Enhanced Edition project, much of the work to hook DirectInput8 was already done, all that was left was digging into the old documentation to find in which form the various methods returned the input data. <br>My goal was to find out if I could implement a mouse turning feature, similar to Silent Hill 3 PC.

### [](#header-2)Following the clues
Reverse engineering can sometimes feel like a mystery to solve, following the clues to solve the case. This time, I was looking for a way to inject James' rotation, since I already hooked the `GetDeviceState` and `GetDeviceData`, from the `IDirectInputDevice8A` class. 
<br>From here, I could easily intercept input going from DInput to the game, and read or change whatever I wanted.
<br>
 noticed that, in the project, there was a ControllerTweaks file by [Silent](https://cookieplmonster.github.io/), and I used it to find the HandleDInput function. This function's caller--we’ll call it HandleMovement--contained a function that, after a bit of reversing and variable naming, had a couple of promising sections, such as:

```c
// around 0x52E685
ControllerLXAxis = (char) __ftol2(unaff_EDI);
ControllerLYAzis = (char) __ftol2(unaff_EDI);
ControllerRXAxis = (char) __ftol2(unaff_EDI);
ControllerRYAxis = (char) __ftol2(unaff_EDI);
```

where __ftol2() was a function that ghidra incorrectly identified as an asm function that converted float to long, but was actually a function that retrieved axis data from DInput. This value is a signed byte, that represented how far was the joystick tilted on that axis.

With this knowledge, I was able to implement mouse turning, reading relative mouse input from FUNCTION, and transforming it to Axis data. But this solution presented several problems, when used in conjunction with the keyboard for character movement.
<br>Luckily, Silent restored dpad movement in his patch:

```cpp
const DWORD povAngle = joystickState.rgdwPOV[0];
const bool centered = LOWORD(povAngle) == 0xFFFF;
if ( !centered )
{
  // Override analog values with DPad values
  const double angleRadians = static_cast<double>(povAngle) * M_PI / (180.0 * 100.0);

  joystickState.lX = static_cast<LONG>(std::sin(angleRadians) * 32767.0);
  joystickState.lY = static_cast<LONG>(std::cos(angleRadians) * -32767.0);
}
```
Providing a simple way to inject axis data into a controller struct that was then passed to the game.
But my old work on those strange functions wasn't in vain, as I used the Right Analog functions to inject right stick movement, to use with the Search View camera function.

```cpp
int8_t GetControllerRYAxis_Hook(DWORD* arg)
{
	if (GetSearchViewFlag() == 0x6 && !VirtualRightStick.IsCentered())
	{
		return VirtualRightStick.YAxis;
	}
	else
	{
		return orgGetControllerRYAxis.fun(arg);
	}
}
```

### [](#header-3)The trivial things
One thing that, I figured, should've been trivial was injecting inputs to use mouse buttons in game, we wanted to use the right mouse button to back out of menus, and to ready your weapon while in game. Unfortunately the [Event index](https://github.com/JokieW/SilentHillDatabase/blob/219062a2a3cb596281a9fcf8bf0e01582739894e/SH2/SH2%20PC%20memory%20addresses#L37) wasn't reliable enough, so we had to get creative to fix some edge case behaviours, and in the end the function switching code ended up something like this:

```cpp
bool InputTweaks::SetRMBAimFunction()
{
	return (GetEventIndex() == EVENT_IN_GAME &&
		(ElevatorFix() || (HotelFix())) || JamesVaultingBuildingsFix() || 
		RosewaterParkFix() || HospitalMonologueFix() || FleshRoomFix() ||
		GetFullscreenImageEvent() != 0x02);
}
```
a huge shoutout to [Polymega](https://github.com/Polymega) for working with me to fix these issues.
We've also had to implement a "holding RMB" check, since for some reason the game skips cutscenes if the player is holding rmb before triggering one. This feature alone was the most time consuming of my whole work on the EE!

### [](#header-4)Finishing touches
On the tail end of this feature development, I got the idea of implementing a Toggle sprint functionality, since it would be handy when controlling James with mouse and keyboard. That was easy enough, just check for the Run key toggle, add a variable for it, and inject the run input if needed. The stylish touch was suggested by our Polymega, which was to change the `Analog` movement option to `Toggle`, and this is accomplished by just changing which string index is retrieved while opening the menu:

```
004621DD push 68 // Text Layer 1 (Movement Type - Analog string)
00461F69 push 68 // Text Layer 2 (Movement Type - Analog string)
00464466 push 68 // Arrow Spacing (Movement Type - Analog string)
```

```cpp
// Change the Analog string (0x68) with Toggle (0x2F)
UpdateMemoryAddress((void*)AnalogStringOne, "\x2F", 1);
UpdateMemoryAddress((void*)AnalogStringTwo, "\x2F", 1);
UpdateMemoryAddress((void*)AnalogStringThree, "\x2F", 1);
```

<br>During testing, one of our playtesters ([Balthazor44](https://github.com/Balthazor44)) found out you couldn't steer the boat with the mouse while holding forward on keyboard. As it's tradition with SH2, things are rarely consistent, since during the whole game it handles input from various sources just fine.
<br>So I've had to find a flag that indicated when James was in the boat, and mix keyboard and mouse movement into a single value to pass to our analog stick;

```cpp
// Boat stage movement fix
if (GetBoatFlag() == 0x01)
{
	joystickState.lY = static_cast<LONG>(InputTweaksRef.GetForwardAnalog() * 32767.0);

	if (InputTweaksRef.GetTurningAnalog() != 0)
		joystickState.lX = static_cast<LONG>(InputTweaksRef.GetTurningAnalog() * 32767.0);
}
```
### [](#header-5)Last but not least
Thank you for reading my first blog post! Huge thanks to Polymega, Silent and all the people who playtested this feature! Another step towards having the best experience we can have with this wonderful game.

[![](https://visitcount.itsvg.in/api?id=mercury501&label=Article%20Views&color=12&icon=3&pretty=true)](https://visitcount.itsvg.in)
