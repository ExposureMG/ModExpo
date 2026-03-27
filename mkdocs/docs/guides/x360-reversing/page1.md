# Chapter 1: CB_B patches

By **DrSchottky** - 10-10-2015
Original Page (Internet Archive): [RaizelConsole](https://web.archive.org/web/20250430191253/http://www.razielconsole.com:80/forum/guide-e-tutorial-xbox-360/944-[x360-reversing]-chapter-1-cb_b-patches.html)

## Disclaimer

All the stuff i'll write comes from what i read and/or reversed. It might be inaccurate or totally wrong, please don't blame me for this.

---

## Intro

Before you start reading: executables and patches are written in Assembly (O RLY?).
If you don’t know what ASM is and/or hardware basics you can stop your reading here, sorry.

Let’s take our disassembled CB_B 9188 (we’re working on a Trinity, do you remember?) and start looking the related patch file.
Oh, as you probably noticed all instructions have fixed length. This is very helpful!

## Patch #1

`00 00 4D F4 00 00 00 01 60 00 00 00`

That’s pretty easy! It replaces whatever’s at `0x4DF4` with `0x60000000` (opcode for nop).
But what’s at `0x4DF4`? CB LDV (fuseline 02).
The first check compares the position (counting from left) of the rightmost F in fuseline 02
with data stored at `0x3B1` (sequence byte), and if it doesn’t match it jumps to the second check (otherwise it’ll continue boot).
We patch that jump with a nop.

## Patch #2

`00 00 4F 50 00 00 00 03 60 00 00 00 60 00 00 00 60 00 00 00`

Three nops starting from `0x4F50`, cut off error 0xA3 (haven’t found info about it nor tried to reverse what is).

## Patch #3

`00 00 56 60 00 00 00 01 38 60 00 00`

Replaces call to `XeCryptMemDiff` (that checks CD hash) with `li r3,0`.
This because next instruction checks for r3 (which should be `XeCryptMemDiff`’s return value) comparing it with 0: If they’re equal boot continues.

---

[>> Next Chapter >>](page2.md)

## Credits
* xeBuild Team
* Free60
* RGLoader