# GGX Project

## Overview

GGX is a collection of libraries / utilities and interfaces for working with Xbox 360 NANDs and Updates.

## Tools

* [`Patcher`](patcher.md): A tool for patching Xbox 360 game files.

* [`Builder`](builder.md): A tool for building Xbox 360 game files.

* [`Core`](core.md): Command, Session and Interface manager.

* [`Crypto`](excrypt.md): Rust bindings for ExCrypt.

## Interfaces

xeBuild style CLI always available. All GGX functions exposed over FFI. All CLI / FFI inputs exposed to the interpreter / LuaVM.

* [`LuaGG`](luagg.md): Embedded tiny mlua LuaVM. All libraries and utilities exported as mlua modules.

* [`PyGG`](pygg.md): Embedded tiny rust-python interpreter. All libraries and utilities exported as python modules.


## Dependencies

* [`LibLZX`](compression.md): A library for compressing and decompressing Xbox 360 game files.
* [`Libmspack`](compression.md): A library for compressing and decompressing Xbox 360 game files.
* [`ExCrypt`](excrypt.md): Bit-perfect Xbox 360 crypto library.
* [`rust-python`](pygg.md): A rust implementation of a python interpreter.
* [`mlua`](luagg.md): A rust implementation of the LuaVM.
