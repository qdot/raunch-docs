# Launch Protocol

## Commands

The command protocol for the launch consists of two byte packets.

```
0xYY 0xZZ
```

- YY represents the desired position of the toy. Valid inputs are 0-99
  in [BCD format](https://en.wikipedia.org/wiki/Binary-coded_decimal).
- ZZ represents the desired speed the toy should move to the position
  listed at. Valid inputs are 0-99
  in [BCD format](https://en.wikipedia.org/wiki/Binary-coded_decimal).
  
Invalid inputs are ignored.

## Buttons

Status of buttons is read via notifications sent by the Status
Notification characteristic. They arrive as an array of 14 ints,
looking like:

```
[0x03, 0xCC, 0x03, 0x5A, 0x03, 0xA3, 0x03, 0xF4, 0x03, 0xAF, 0x03, 0x67, 0x03, 0xCF]
```

This array represents a set of 7 16-bit big-endian numbers, reflecting
the state of the buttons on the device. The index of the values
mirrors the order of the buttons on the device, with (assuming the
device is facing you and upright):

- the first 3 values representing the buttons on the left side of the
  device, from left to right
- the 4th value representing the blue LED square button in the middle
- the last 3 values representing the buttons on the right side of the
  device, from left to right

The array shown above is the idle state, with no buttons currently
being pressed. Detecting button presses requires testing for a certain
threshold of the button value. Whenever a button is pressed, the value
will value decrease (up to ~80-90 from its idle state), and whenever
it is let go, the value will increase back to normal. 

For example: Button 0, when idle, would register 0x03cc. When pressed, it might go
as low as 0x0360. 

## Warning Against Using Buttons In Interfaces

Due to the hardware used for the buttons, they do not have a noticable
"range" where pressing them works or doesn't. While this choice was
most likely made to lessen mechanical wear and simplify the mold and
construction of the device, it also makes it extremely difficult for
users to figure out where to press and have their action register.

On top of this, the 4th button (the blue LED button) is reserved.
Pressing this button puts the device into "manual" mode, meaning that
bluetooth will stay connected, but commands sent to the device will
not do anything. As far as we can tell, this is built into the
firmware and cannot be turned off by external code (this may be wrong,
in which case we will update this document when we find out).

We recommend avoiding using device buttons in interfaces if possible.
If the buttons must be used, we recommend taking great care to refine
and tests debounce algorithms, and provide ways for users to
familiarize themselves with your interface before they are required to
use them. While debounce is normally done in an amount of time that
would not be perceivable by the user, the ~10hz update rate of the
launch combined with the slow reaction of the sensors could cause
confusion.
