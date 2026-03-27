## Chapter 7: Setting up Syscall table

By **DrSchottky** - 10-10-2015
Original Page (Internet Archive): [RaizelConsole](https://web.archive.org/web/20230201182301/http://www.razielconsole.com:80/forum/guide-e-tutorial-xbox-360/943-[x360-reversing]-intro.html)

## Disclaimer

All the stuff i'll write comes from what i read and/or reversed. It might be inaccurate or totally wrong, please don't blame me for this.

---

Syscalls are the mechanism used by unprivileged code (like Kernel) to request functionalities to privileged code (HyperVisor).

HV is the most privileged code running on your system: it manages memory access, encryption and low-level security.

Syscalls are invoked by unprivileged code writing in `r0` the number of the required Syscall (from `0x00` to `0xDEPENDS_ON_KERNEL`) and executing `sc` instruction.

`sc` throws an exception that is catched and moves execution to `0xc00`. At that address there's the syscall dispatcher that, after checking `r0` validity, looks in sycall table for the effective address of the required syscall and jump to it.

Syscall table start address is a dword located at `0x48` (HV header) so, to get the address of a syscall implementation you have too look at `sctable_start_address+(syscall number * 4)`.

Syscall table is a simple list of sequential 32 bit addresses, ordered by syscall number (ascending).

This is the sctable for HV 12625. As you can see syscall #0 is at `0x1F20`, syscall #1 at `0x8B4` etc etc..

**MISSING: syscall.png**

In newer Kernels syscall addressing changes a bit: for some syscalls the address in the table doesn't lead to syscall implementation, but to a code snippet then dinamically create the "real" address.

---

[>> Outro >>](outro.md)

## Credits
* xeBuild Team
* Free60
* RGLoader