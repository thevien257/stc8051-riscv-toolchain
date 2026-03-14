# STC8051 RISC-V Toolchain

Bundled `riscv64-unknown-elf-gcc` toolchain for the [STC8051 Arduino Core](https://github.com/thevien257/STC_Arduino_Core).  
Used by the **rv51** RISC-V emulation pipeline to compile C/C++ code that runs on the STC8H8K64U microcontroller.

## How It Works

The STC8H8K64U is an 8051 MCU, but the STC8051 Arduino Core uses a **RISC-V emulator (rv51)** running on the 8051 to execute Arduino sketches compiled as RISC-V code. This toolchain provides the RISC-V cross-compiler that makes it all possible.

```
Arduino Sketch (.ino)
        │
        ▼
  riscv64-unknown-elf-g++  ← this toolchain
        │
        ▼
  RISC-V ELF (rv32em/ilp32e)
        │
        ▼
  rv51 emulator binary + app binary
        │
        ▼
  Flashed to STC8H8K64U via USB
```

## Installation

This toolchain is **automatically downloaded** by Arduino IDE/CLI when you install the STC8051 board package.

Add the boards manager URL in Arduino IDE:
```
https://raw.githubusercontent.com/thevien257/STC_Arduino_Core/main/package_stc8051_index.json
```

## Platform Support

| Platform | GCC Version | Archive | Size |
|----------|-------------|---------|------|
| **Linux x86_64** | 13.2.0 (system) | [tar.gz](https://github.com/thevien257/stc8051-riscv-toolchain/releases/download/v1.0.0/riscv-toolchain-1.0.0-linux-x86_64.tar.gz) | 43 MB |
| **Windows x86_64** | 13.4.0 (xPack) | [zip](https://github.com/thevien257/stc8051-riscv-toolchain/releases/download/v1.0.0/riscv-toolchain-1.0.0-windows-x86_64.zip) | 53 MB |

## Contents

Each archive extracts to `riscv-toolchain-1.0.0-{platform}/` with:

```
bin/
  riscv64-unknown-elf-gcc         # C compiler
  riscv64-unknown-elf-g++         # C++ compiler
  riscv64-unknown-elf-as          # Assembler
  riscv64-unknown-elf-ar          # Archiver
  riscv64-unknown-elf-ld          # Linker
  riscv64-unknown-elf-objcopy     # Binary converter
  riscv64-unknown-elf-size        # Size reporter
lib/gcc/riscv64-unknown-elf/VER/
  cc1, cc1plus                    # C/C++ compiler backends
  collect2, lto1, lto-wrapper     # LTO and linking tools
  liblto_plugin.{so,dll}          # LTO plugin for ar/ld
  include/                        # GCC built-in headers
  rv32em/ilp32e/                  # Multilib: libgcc.a, crt*.o
picolibc/
  include/                        # C standard library headers
  lib/                            # picolibc for rv32em/ilp32e
```

## Build Flags

The toolchain is invoked by [`sdcc.py`](https://github.com/thevien257/STC_Arduino_Core/blob/main/tools/wrapper/sdcc.py) with these flags:

```
-march=rv32em_zicsr -mabi=ilp32e
-nostdlib -nostartfiles
-Os -flto
-ffunction-sections -fdata-sections -fno-common
```

## Building from Source

### Linux

The Linux archive is built from system packages (`riscv64-unknown-elf-gcc`, `picolibc-riscv64-unknown-elf`) using the `pack` mode in `sdcc.py`:

```bash
cd /path/to/STC_Arduino_Core
python3 tools/wrapper/sdcc.py pack /tmp
```

### Windows

The Windows archive is repacked from the [xPack RISC-V GCC](https://github.com/xpack-dev-tools/riscv-none-elf-gcc-xpack) release, with binaries renamed from `riscv-none-elf-*` to `riscv64-unknown-elf-*` and picolibc copied from the Linux build (target libraries are platform-independent).

## License

The toolchain binaries are from GCC and are licensed under GPLv3+.  
picolibc is licensed under BSD-3-Clause.  
The xPack redistributed binaries follow the same licensing as upstream GCC.
