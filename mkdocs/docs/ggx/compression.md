# GGX Compression

Implements `compress()` and `decompress()` functions in the interface. Hardcoded settings specialized for the xbox 360 CE, CF and CG CAB LZX.

## LibLZX

Relatively new open source library by `` implementing Cab LZX compression. Forked from `wimlib`'s WIM LZX implementation. GPL v3 licensed.

### Settings

(Filler data)
| Setting | Value |
|---------|-------|
| Window Size | 32 KB |
| Match Finder | Hash |
| Match Length | 258 |
| Match Distance | 16384 |
| Literal Threshold | 16 |
| Match Threshold | 3 |
| Block Size | 131072 |
| Compression Level | 3 |

## Libmspack / LZXD

Well-known FOSS C library by `Kyzer` implementing a number of decompression algorithms including CAB LZX. LGPL v2 licensed.

### Settings

(Filler data)
| Setting | Value |
|---------|-------|
| Window Size | 32 KB |
| Match Finder | Hash |
| Match Length | 258 |
| Match Distance | 16384 |
| Literal Threshold | 16 |
| Match Threshold | 3 |
| Block Size | 131072 |
| Compression Level | 3 |