<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

# How it works

TinyQV is a small Risc-V SoC, implementing the RV32EC instruction set plus the Zcb and Zicond extensions, with a couple of caveats:


* Addresses are 28-bits
* Program addresses are 24-bits
* gp is hardcoded to 0x1000400, tp is hardcoded to 0x8000000.

Instructions are read using QSPI from Flash, and a QSPI PSRAM is used for memory.  The QSPI clock and data lines are shared between the flash and the RAM, so only one can be accessed simultaneously.

Code can only be executed from flash.  Data can be read from flash and RAM, and written to RAM.

Many of the peripherals making up the SoC are contributed by the Tiny Tapeout community!

## Address map

| Address range | Device |
| ------------- | ------ |
| 0x0000000 - 0x0FFFFFF | Flash |
| 0x1000000 - 0x17FFFFF | RAM A |
| 0x1800000 - 0x1FFFFFF | RAM B |
| 0x8000000 - 0x8000033 | DEBUG  |
| 0x8000040 - 0x800007F | GPIO |
| 0x8000080 - 0x80000BF | UART  |
| 0x80000C0 - 0x80001FF | User peripherals 3-7 |
| 0x8000400 - 0x800043F | Simple user peripherals 0-3 |
| 0xFFFFF00 - 0xFFFFF07 | TIME |

### DEBUG

| Register | Address | Description |
| -------- | ------- | ----------- |
| ID       | 0x8000008 (R) | Instance of TinyQV: 0x41 (ASCII A) |
| SEL      | 0x800000C (R/W) | Bits 6-7 enable peripheral output on the corresponding bit on out6-7, otherwise out6-7 is used for debug. |
| DEBUG_UART_DATA | 0x8000018 (W) | Transmits the byte on the debug UART |
| STATUS   | 0x800001C (R) | Bit 0 indicates whether the debug UART TX is busy, bytes should not be written to the data register while this bit is set. |

See also [debug docs](debug.md)

### TIME

| Register | Address | Description |
| -------- | ------- | ----------- |
| MTIME_DIVIDER | 0x800002C | MTIME counts at clock / (MTIME_DIVIDER + 1).  Bits 0 and 1 are fixed at 1, so multiples of 4MHz are supported. |
| MTIME    | 0xFFFFF00 (RW) | Get/set the 1MHz time count |
| MTIMECMP | 0xFFFFF04 (RW) | Get/set the time to trigger the timer interrupt |

This is a simple timer which follows the spirit of the Risc-V timer but using a 32-bit counter instead of 64 to save area.
In this version the MTIME register is updated at 1/64th of the clock frequency (nominally 1MHz), and MTIMECMP is used to trigger an interrupt.
If MTIME is after MTIMECMP (by less than 2^30 microseconds to deal with wrap), the timer interrupt is asserted.

### GPIO

| Register | Address | Description |
| -------- | ------- | ----------- |
| OUT | 0x8000040 (RW) | Control for out0-7 if the GPIO peripheral is selected |
| IN  | 0x8000044 (R) | Reads the current state of in0-7 |
| AUDIO_FUNC_SEL | 0x8000050 (RW) | Audio function select for uo7 |
| FUNC_SEL | 0x8000060 - 0x800007F (RW) | Function select for out0-7 |

| Function Select | Peripheral |
| --------------- | ---------- |
| 0               | Disabled   |
| 1               | GPIO       |
| 2               | UART       |
| 3 - 15          | User peripheral 3-15 |
| 16 - 31         | User byte peripheral 0-15 |
| 32 - 39         | User peripheral 16-23 |

| Audio function select | Peripheral |
| --------------------- | ---------- |
| 0-3                   | PSRAM B enabled |
| 4                     | 5 PWL Synth out 7 |
| 5                     | 4 Pulse Transmitter out 7 |
| 6                     | ? |
| 7                     | 18 Matt PWM out 7 |

### UART

| Register | Address | Description |
| -------- | ------- | ----------- |
| TX_DATA | 0x8000080 (W) | Transmits the byte on the UART |
| RX_DATA | 0x8000080 (R) | Reads any received byte |
| TX_BUSY | 0x8000084 (R) | Bit 0 indicates whether the UART TX is busy, bytes should not be written to the data register while this bit is set. Bit 1 indicates whether a received byte is available to be read. |
| DIVIDER | 0x8000088 (R/W) | 13 bit clock divider to set the UART baud rate |
| RX_SELECT | 0x800008C (R/W) | 1 bit select UART RX pin: `ui_in[7]` when low (default), `ui_in[3]` when high |

# How to test

Load an image into flash and then select the design.

Reset the design as follows:

* Set rst_n high and then low to ensure the design sees a falling edge of rst_n.  The bidirectional IOs are all set to inputs while rst_n is low.
* Program the flash and leave flash in continuous read mode, and the PSRAMs in QPI mode
* Drive all the QSPI CS high and set SD1:SD0 to the read latency of the QSPI flash and PSRAM in cycles.  SD2 selects whether half a cycle is subtracted from the read latency by driving the SPI clock on the negative edge.
* Clock at least 8 times and stop with clock high
* Release all the QSPI lines
* Set rst_n high
* Set clock low
* Start clocking normally

At the target 24MHz clock a read latency of 1.5 is probably best (SD2:SD0 = 0b110).  The maximum supported latency is 3.

The above should all be handled by some MicroPython scripts for the RP2 on the TT demo PCB.

Build programs using the [customised toolchain](https://github.com/MichaelBell/riscv-gnu-toolchain) and the [tinyQV-sdk](https://github.com/MichaelBell/tinyQV-sdk), some examples are [here](https://github.com/MichaelBell/tinyQV-projects).

# External hardware

The design is intended to be used with this [QSPI PMOD](https://github.com/mole99/qspi-pmod) on the bidirectional PMOD.  This has a 16MB flash and 2 8MB RAMs.

The UART is on the correct pins to be used with the hardware UART on the RP2040 on the demo board.

It may be useful to have buttons to use on the GPIO inputs.
