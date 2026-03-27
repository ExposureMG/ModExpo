# Chapter 5: Miscellaneous kernel patches

By **DrSchottky** - 10-10-2015
Original Page (Internet Archive): [RaizelConsole](https://web.archive.org/web/20230201182301/http://www.razielconsole.com:80/forum/guide-e-tutorial-xbox-360/943-[x360-reversing]-intro.html)

## Disclaimer

All the stuff i'll write comes from what i read and/or reversed. It might be inaccurate or totally wrong, please don't blame me for this.

---

## Intro

Now let's take a look at the various kernel patches provided with xeBuild.
These are disabled by default, but can be enabled under particular circumstances.

## nofcrt.bin

Removes ODD C/R requirement

`00 06 11 A0 00 00 00 01 60 00 00 00`

Nop the branch to `XeKeysFcrtLoad`, during post `0x6C` (`INIT_SECURITY`)

`00 02 D7 E0 00 00 00 03 2B 0A 00 0F 60 00 00 00 3B E0 00 00`

HV patch, not reversed yet.

## nohdd.bin

Disable HDD

`00 15 A6 20 00 00 00 01 38 60 00 00`

Replace branch to `SataChannelDetectDevice` (subcall of `SataDiskInitialize`) with `li r3,0` (faking return value)

## nohdmiwait.bin

Disable HDCP handshake

`00 15 85 90 00 00 00 01 48 00 00 4C`

In `HalpReadCurrentAVPack` exclude `bl HalpRebootSystem` from branch case (turns `bne` into `b`).

## nointmu.bin

Disables internal MU

`00 08 D6 A8 00 00 00 01 3D 40 60 00`

Spoofs mobo revision(?) in `SfcxMuMountStorage`

`00 0E 35 20 00 00 00 01 3D 40 60 00`

Spoofs mobo revision(?) in `MassDriverEntry`

`00 08 E9 D0 00 00 00 01 4E 80 00 20`

`MmcxMuMountStorage` returns immediatly to caller.

## nolan.bin

Disable LAN PHY

`00 0D 2D DC 00 00 00 01 60 00 00 00`

Nop `bl _NicInit_CNicEmac__QAAXXZ` from `NicDriverEntry` (called during driver init)

---

[>> Next Chapter >>](page6.md)

## Credits
* xeBuild Team
* Free60
* RGLoader