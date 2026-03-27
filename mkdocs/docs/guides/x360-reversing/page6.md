# Chapter 6: Find and patch Kernel Errors

By **DrSchottky** - 10-10-2015
Original Page (Internet Archive): [RaizelConsole](https://web.archive.org/web/20230201182301/http://www.razielconsole.com:80/forum/guide-e-tutorial-xbox-360/943-[x360-reversing]-intro.html)

## Disclaimer

All the stuff i'll write comes from what i read and/or reversed. It might be inaccurate or totally wrong, please don't blame me for this.

---

Black screen with white text saying to contact customer service, followed by a Exx.
Those are kernel errors, and some of them (mainly DVD, HDD and ETH errors) can be bypassed
with kernel patches (obviously you bypass the error, the device you excluded from kernel won't work).

**How can we find those checks in kernel?**

`VdDisplayFatalError` is a function that takes the error as parameter (`r3`, as always) and shows it on Video/ROL.

Once you have your kernel (and symbols) you can find a pattern that uniquely identifies the function and search for it in your CE+CG image (IDC scripting is the way!).

Once you found the function in your kernel you simply look for callers. The last byte of `r3` before the `bl` is the hex of the error that will be shown.

An example of a error that COULD be easily bypassed? `E64`.
An example of a error that COULD NOT be easily bypassed? `E74`.

Here's a little gift

```C
#include <idc.idc>

static main()
{
	auto addr = 0;
	addr = FindBinary(addr,SEARCH_DOWN,"DB E1 FF D0 94 21 FE D0");
	MakeNameEx(addr - 8, "VdDisplayFatalError", 0);
}
```

---

[>> Next Chapter >>](page7.md)

## Credits
* xeBuild Team
* Free60
* RGLoader