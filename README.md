# Raspberry Pi Pico SDK Display Examples - Early Access

## RP2350 Instructions

Everything below this section is from the stock pico-examples, so ignore URLs etc., but generally instructions are the same.

The Pico SDK default continues to be to build for RP2040 (PICO_PLATFORM=rp2040), so to build for RP2350, you need to pass -DPICO_PLATFORM=rp2350 to CMake (or -DPICO_PLATFORM=rp2350-riscv for RISC-V).

Most, but not all examples, currently work on RP2350 however you should be able to do a full build with any of the above platforms (PICO_PLATFORM=host however currently fails on some examples)

For RISC-V compilation, you should take a compiler from here: https://www.embecosm.com/resources/tool-chain-downloads/#riscv-stable

## Getting started

See [Getting Started with the Raspberry Pi Pico](https://rptl.io/pico-get-started) and the README in the [pico-sdk](https://github.com/raspberrypi/pico-sdk) for information on getting up and running.