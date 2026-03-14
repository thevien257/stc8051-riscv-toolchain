# STC8051 RISC-V Toolchain

Bundled `riscv64-unknown-elf-gcc` 13.2.0 toolchain for [STC8051 Arduino Core](https://github.com/thevien257/STC_Arduino_Core).

Used by the rv51 RISC-V emulation pipeline to compile C/C++ code for STC8H8K64U.

## Installation

This toolchain is automatically downloaded by Arduino IDE/CLI when you install the STC8051 board package via the boards manager URL.

## Contents

- `riscv64-unknown-elf-gcc/g++/as/ar/ld/objcopy/size`
- GCC internal tools (cc1, cc1plus, lto1, collect2, lto-wrapper)
- LTO plugin for archive handling
- GCC headers and libgcc for rv32em/ilp32e
- picolibc headers and libs for rv32em/ilp32e

## Platform Support

- Linux x86_64
