# Xbox 360 Reversing

An overview about xeBuild’s hacked images

By **DrSchottky** - 10-10-2015
Original Page (Internet Archive): [RaizelConsole](https://web.archive.org/web/20230201182301/http://www.razielconsole.com:80/forum/guide-e-tutorial-xbox-360/943-[x360-reversing]-intro.html)

Dumped and reformatted by **ExposureMG** - 03-27-2026

## Disclaimer

All the stuff i'll write comes from what i read and/or reversed. It might be inaccurate or totally wrong, please don't blame me for this.

## Intro

As you might know, Xbox’s chain of trust is composed by a series of loaders and ends with the Kernel (I won’t dwell on this, boot process is explained quiet well on [Free60](https://www.free60.org/wiki/Boot_process)),
but how is made a modified bootchain?
Assuming that you know what an ECC is and how it works, let’s focus on xeBuild’s patches.


> To avoid confusion: all my examples will be based on RGH2 Trinity, with MFG CB_A(9188) !

There are three items that need to be patched on a hacked image: CB_B, CD and Kernel/HV; and this is what xeBuild does.

CB_B/CD patches are applied to the original files before they’re used to build an image, Kernel patches are stored as plaintext in the built image and applied at runtime by CD on an already decompressed and merged CE + CG (for this reason CF doesn’t need to be patched).

xeBuild’s patches format is documented in `about_patches.S`, and in summary is:

`[CB_B patches] FF FF FF FF [CD patches] FF FF FF FF [Kernel patches] FF FF FF FF`

Each patch looks like this:

- Address (32 bit)
- Number of patches (32 bit)
- Data (32 bit x number of patches)

If you want to look inside a loader (CB_A/B, CD, CF)

- load it into IDA;
- set ppc as processor;
- look at `0x8` (4 bytes) for the entry point and jump to it;
- press C to disassemble.

Loading into IDA a kernel image requires some further step since it’s stored in your nand as a compressed base (CE) + update (CG).

I think that RGLoader Image Editor is the quickest way to get an updated kernel (ready to be disassembled) from your nand dump.

Most of Crypto/Hash functions used in loaders are equal to those used in the kernel: if you have a kernel with symbols it’s easy to make a labeler script that searches for well known patterns in your code.

In later chapters we’ll see in detail the various patches applied by xeBuild.

[>> Next Chapter >>](page1.md)

## Credits
* xeBuild Team
* Free60
* RGLoader