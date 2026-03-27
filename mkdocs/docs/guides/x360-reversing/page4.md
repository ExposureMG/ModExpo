# Chapter 4: Kernel Patches

By **DrSchottky** - 10-10-2015
Original Page (Internet Archive): [RaizelConsole](https://web.archive.org/web/20240221004844/http://www.razielconsole.com:80/forum/guide-e-tutorial-xbox-360/956-[x360-reversing]-chapter-4-kernel-patches.html)

## Disclaimer

All the stuff i'll write comes from what i read and/or reversed. It might be inaccurate or totally wrong, please don't blame me for this.

---

## Intro

Ok, things start to get interesting. I originally planned to write a single thread for HV + Kernel pacthes, but i realized that there would be too many things to write, making the thread chaotic.

In this chapter i'll try to understand and explain Kernel pacthes that, in brief, remove XEXs' security measures and (some) DVD drive checks.

**How can we split HV and Kernel?** Take your CE+CG (decompressed, decrypted etc etc):

- HV: begin - `0x3FFFF` (the first 256KB)
- Kernel: `0x40000` - end

Patches mainly spoofs value returned by functions, so i'll not go into details.

This is the patchset for Kernel 16574

## Patch #1

`00 07 B1 50 00 00 00 02 38 60 00 00 4E 80 00 20`

Forces `XexpConvertError` to return 0, whatever r3's value is.

## Patch #2

`00 07 BC E8 00 00 00 01 38 60 00 01`

Replace `XexpVerifyMediaType` call with `li r3, 1`, allowing XEX booting from any media.

## Patch #3

`00 07 BD F8 00 00 00 01 38 60 00 00`

Replace `RtlImageXexHeaderString` call with `li r3, 0`.

## Patch #4

`00 07 BE 60 00 00 00 01 39 60 00 00`

`li r11, 0`

## Patch #5

`00 07 BE B0 00 00 00 01 39 60 00 00`

`li r11, 0`

## Patch #6

`00 07 A7 38 00 00 00 02 38 60 00 00 4E 80 00 20`

`XexpVerifyMinimumVersion` returns immediatly `0`.

## Patch #7

`00 09 44 28 00 00 00 01 3A E0 00 10`

Modifies the value of a register in `SfcxInspectLargeDataBlock`. Maybe something related to flash layout(dunno).

## Patch #8

`00 09 8D 80 00 00 00 01 2B 0B 00 FF`

In `SataCdRomAuthenticationExInitialize` compares r11 with FF instead of 1, altering branch path.

## Patch #9

`00 09 87 64 00 00 00 05 38 60 00 00 60 00 00 00 60 00 00 00 60 00 00 00 60 00 00 00`

Removes `E66` from `SataCdRomActivateHCDFRuntimePatch`.

## Patches #10,#11...,#20

They all do the same thing: after stack frame init they set r3 (0 or 1) and return.
Target functions are:

- `XeKeysVerifyRSASignature`
- `XeKeysSecurityConvertError`
- `XeKeysDvdAuthExConvertError`
- `_XeKeysRevokeIsValid`
- `XeKeysRevokeIsRevoked`
- `_XeKeysRevokeIsRevoked`
- `XeKeysRevokeIsDeviceRevoked`
- `XeKeysRevokeConvertError`
- `XeKeysConsoleSignatureVerification` (for this one the patch is a little different, but nevermind)
- `XeCryptBnQwBeSigVerify`
- `SataDiskAuthenticateDevice`

## Patch #21

It's a payload placed into the almost "dead" body of `SataDiskAuthenticateDevice`. This code will be called by subsequent patches.

## Patch #22

`00 06 13 D0 00 00 00 01 48 0F 8F 01`

After XAM init branches to the first chunk of the payload which loads `launch.xex`.

## Patch #23

`00 07 CF 68 00 00 00 01 48 0D D3 A5`

`XexLoadExecutable` is patched to `bl` to the second chunk of the payload. TBH i don't know what kind of black magic it does.

## Patch #24

`00 10 9C 78 00 00 00 01 48 05 06 BC`

`XeKeysGetKeyProperties` is a wrapper for `HvxKeysGetKeyProperties`.
It branches to the third chunk of our payload (instead of `HvxKeysGetKeyProperties`) and, if function's argument is equal to `0x14` it does the same black magic of [Patch #23](page4.md#patch-23) (keeps reading data at `0x12345678` until it's `0`, dunno why).

---

[>> Next Chapter >>](page5.md)

## Credits
* xeBuild Team
* Free60
* RGLoader