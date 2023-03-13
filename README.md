## 32-bit Beta CPU

This repository contains the source code that implements 32-bit Beta CPU in Lucid programming language. Simply open `BetaComponents32.alp` with [Alchitry Lab](https://alchitry.com/alchitry-labs), **compile**, and **flash** it to Alchitry Au + Alchitry Io Element Board.

### Instruction Data

Currently, only 27 instructions are hardcoded inside `instruction_rom.luc`. These instructions are designed to _loop_ to make it easier to demonstrate the workings of the Beta CPU. You may change the content of this instruction rom to load any Beta CPU instruction.

Please change `const MEMORY_SIZE` in `au_top.luc` to match your custom instruction.

### Running the Beta

Ensure that **ALL** `io_dip` is switched OFF when flashing the binary.

Once flashed, you may set the following to run the Beta:

1. io_dip[2][7]: toggles between manual (0) and auto mode (1)
2. io_dip[2][6]: toggles between slow (0) or faster clock (1)
3. `button[4]`: this is the "right" button on Io Element Board. Press this to advance by 1 instruction when you're in **manual** mode

### Debug Signals

The debug signals spans from `io_led[1:0]` and also the 7 segment.

`io_dip[0]` can be changed to "view" various states presented at `io_led[1]` and `io_led[0]` (16 bits of values at once). Simply set it to represent the values below, e.g: `0x3` means that `io_dip[0]` is set to `00000011` (turn the rightmost two switches on). Here are the exhaustive list:

1. `0x0`: MSB 16 bits of current instruction
2. `0x1`: LSB 16 bits of current instruction
3. `0x2`: LSB 16 bits of instruction address
4. `0x3`: LSB 16 bits of EA (this is also ALU output)
5. `0x4`: MSB 16 bits of EA (this is also ALU output)
6. `0x5`: LSB 16 bits of Mem[EA]
7. `0x6`: MSB 16 bits of Mem[EA]
8. `0x7`: LSB 16 bits of RD2 (mwd[15:0])
9. `0x8`: MSB 16 bits of RD2 (mwd[31:16])
10. `0x9`: LSB 16 bits of pcsel_out
11. `0xA`: LSB 16 bits of asel_out
12. `0xB`: LSB 16 bits of bsel_out
13. `0xC`: LSB 16 bits of wdsel_out
14. `0xD`: LSB 16 bits of beta output buffer. This is a dff that's hardwired to reflect Mem[0xC]
15. `0xE`: LSB 16 bits of beta input buffer. This is a dff that's hardwired to reflect Mem[0x10]
16. `0xF`: MSB 16 bits of instruction address. Useful to see PC31 (kernel/user mode)

### Interrupt button

`button[2:0]` can be pressed at anytime and it will trigger an interrupt signal. You can observe that the button press is eventually "captured" by the Beta and stored in `Mem[0x10]`. You can set `io_dip[0] = 0xD` to view this result at the 7 segment.

### Reset button

If you press Alchitry Au reset button, it will reset the Beta to its original state (`PC` set to `0x80000000`)

### Additional Signals

There are 4 additional signals indicated in `io_led[2][7:4]`.

1. `io_led[2][7]`: auto mode is ON or OFF
2. `io_led[2][6]`: `fastclock` is ON or OFF
3. `io_led[2][5]`: signifies `slowclock` signal
4. `io_led[2][4]`: signifies `fastclock` signal
