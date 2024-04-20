## 32-bit Beta CPU

This repository contains the source code that implements 32-bit Beta CPU in Lucid programming language. 
Simply open `beta-32-document.alp` with [Alchitry Lab](https://alchitry.com/alchitry-labs), **compile**, and **flash** it to Alchitry Au + Alchitry Io Element Board.
The Beta CPU contains a set of 200 lines of instructions that are used to run Techno Twirl.

### Details about Looping Louie

Techno Twirl is inspired by a childhood game named Looping Louie. 
Looping Louie is a tabletop board game where players compete to protect their precious tokens from the swooping attacks of Louie, the daring pilot. Louie, flying his plane in loops, attempts to steal players' tokens using his spinning plane. The last player with tokens left standing wins the game!
Players take turns activating a lever to flip Louie's plane, attempting to protect their tokens while trying to knock out opponents' tokens. With each round, the tension builds as Louie's plane swoops lower and faster, testing players' reflexes and strategic prowess.
![Looping Louie Board Game](img/looping-louie.jpg)

### Details on Techno Twirl

Inspired by the fast-paced, family-friendly and exciting board game, we developed Techno Twirl which runs on Lucid Programming language. 
In place of Pilot Louie, we use lights to display the current location of Louie. 
At the start of each game, players are allocated 5 lives. As Louie loops around the board on his bright blue display, players press their respective buttons to prevent Louie from taking away their lives. In Techno Twirl, players are discouraged from spamming on the button when Louie is not at their location, we do so by implementing the rule that players will lose a live if they do press their button even though Louie is not at their location.

![Looping Louie Board Game](img/Final.gif)

Ensure that **ALL** `io_dip` is switched OFF when flashing the binary.

Once flashed, you may set the following to run the Beta:

1. io_dip[2][7]: toggles between manual (0) and auto mode (1)
2. io_dip[2][6]: toggles between slow (0) or faster clock (1)
3. `button[4]`: this is the "right" button on Io Element Board. Press this to advance by 1 instruction when you're in **manual** mode

### Debug Signals

The debug signals spans from `io_led[1:0]` and also the 7 segment.

`io_dip[0]` can be changed to "view" various states presented at `io_led[1]` and `io_led[0]` (16 bits of values at once). Simply set it to represent the values below, e.g: `0x3` means that `io_dip[0]` is set to `00000011` (turn the rightmost two switches on). Here are the exhaustive list:

1. `0x0`: MSB 16 bits of current instruction (id[31:16])
2. `0x1`: LSB 16 bits of current instruction (id[15:0])
3. `0x2`: LSB 16 bits of instruction address (ia[15:0])
4. `0x3`: LSB 16 bits of EA (this is also ALU output) (ma[15:0])
5. `0x4`: MSB 16 bits of EA (this is also ALU output) (ma[31:16])
6. `0x5`: LSB 16 bits of Mem[EA] (mrd[15:0])
7. `0x6`: MSB 16 bits of Mem[EA] (mrd[31:16])
8. `0x7`: LSB 16 bits of RD2 (mwd[15:0])
9. `0x8`: MSB 16 bits of RD2 (mwd[31:16])
10. `0x9`: LSB 16 bits of pcsel_out
11. `0xA`: LSB 16 bits of asel_out
12. `0xB`: LSB 16 bits of bsel_out
13. `0xC`: LSB 16 bits of wdsel_out
14. `0xD`: MSB 16 bits of instruction address. Useful to see PC31 (kernel/user mode) (ia[31:16])
15. `0xE`: LSB 16 bits of beta input buffer. This is a dff that's hardwired to reflect Mem[0x10]
16. `0xF`: LSB 16 bits of beta output buffer. This is a dff that's hardwired to reflect Mem[0xC]

### Interrupt button

`button[2:0]` can be pressed at anytime and it will trigger an interrupt signal. You can observe that the button press is eventually "captured" by the Beta and stored in `Mem[0x10]`. You can set `io_dip[0] = 0xD` to view this result at the 7 segment.

### Reset button

If you press Alchitry Au reset button, it will reset the Beta to its original state (`PC` set to `0x80000000`)

### Additional Signals

There are 4 additional signals indicated in `io_led[2][7:4]`. This is just a status signal to indicate which clock your Beta CPU is running on.

1. `io_led[2][7]`: auto mode is ON or OFF
2. `io_led[2][6]`: `fastclock` is ON or OFF
3. `io_led[2][5]`: signifies `slowclock` signal
4. `io_led[2][4]`: signifies `fastclock` signal
