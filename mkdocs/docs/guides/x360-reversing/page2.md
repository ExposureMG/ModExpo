# Chapter 2: CD patches

By **DrSchottky** - 10-10-2015
Original Page (Internet Archive): [RaizelConsole](https://web.archive.org/web/20230201190325/http://www.razielconsole.com:80/forum/guide-e-tutorial-xbox-360/945-[x360-reversing]-chapter-2-cd-patches.html)

## Disclaimer

All the stuff i'll write comes from what i read and/or reversed. It might be inaccurate or totally wrong, please don't blame me for this.

---

## Patch #1

`00 00 02 8C 00 00 00 01 48 00 4C 95`

Jumps to a custom subroutine located at `0x4F20` (Patch #4).

## Patch #2

`00 00 05 B4 00 00 00 01 48 00 4C 38 00`

Jumps to a custom subroutine located at `0x51EC`.

## Patch #3

`00 00 08 30 00 00 00 01 60 00 00 00`

nop a check during CF execution.

## Patch #4

`00 00 4F 20 00 00 00 DC ...`

Custom subroutine, basically it's GliGli's CD with extra stuff.
It asks SMC how the console was turned on and starts kernel or xell.

![Patch #4](https://web.archive.org/web/20230201190325im_/http://i60.tinypic.com/2vmy9av.jpg)

At `0x509C`, `0x50BC`, `0x51C0` and `0x51DC` there are other subcalls used by `0x4F20`. They init PCI, regs, SMC and other stuff.

Subroutine at `0x51EC` (called by [Patch #2](page2.md#patch-2)) loads from Flash Kernel/HV patches, apllies them and jumps to HV.

---

[>> Next Chapter >>](page3.md)

## Credits
* xeBuild Team
* Free60
* RGLoader