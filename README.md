# Tiva Microcontroller Terminal Pong

A two-player terminal Pong game written in C for a TI TM4C123 microcontroller. Potentiometers control the paddles, UART renders the court, and multiplexed LEDs display the score.

Developed in November 2020 as university coursework.

## Hardware demonstration

[Watch or download the hardware demonstration (MP4)](media/tiva-pong-demo.mp4)

## Peripheral and interrupt design

| Component | Source-level implementation |
|---|---|
| Paddle input | PB4/PB5 analog inputs sampled through ADC0 sequence 1 |
| Terminal connection | UART0 through PA0/PA1 |
| Input sampling | Timer 0 triggers ADC conversion and updates paddle positions |
| Ball movement | Timer 1 updates movement, collisions and scoring |
| Screen redraw | SysTick renders the terminal and handles restart |
| LED score | Timer 2 alternates PA6/PA7 rows and GPIO E0–E4 outputs |

The game calls `SetSysClock_80MHz`. UART divisors and control bits express an intended approximately 115200-baud, eight-data-bit, odd-parity, one-stop-bit connection at that clock. Actual board, terminal settings and timings have not been reverified.

## Toolchain

`Lab2_Thu.uvprojx` identifies TM4C123GH6PM and ARMCC 5.06 update 6, build 750. Device startup/system files are included. The Keil device packs must supply the device/CMSIS headers.

The Keil project links `Objects/SysClock.lib` and `Objects/L2_Thursday.lib`. Their source and redistribution permissions require clarification.

## Verification and limitations

Rebuilding the project on a fresh setup has not yet been verified. A complete setup guide still needs the board wiring and required library/device-pack versions.

Blocking UART operations occur inside SysTick, and a timer handler polls ADC completion. Interrupt latency and shared-state handling need review before design reuse. Timing comments describe intended settings, without a measured frame rate or hard-real-time guarantee.

The scoring routine multiplexes LED rows. A seven-segment display implementation is not established by this source.

## Attribution

Game coursework by Campbell Reid. Device files and precompiled libraries are supplied dependencies and retain applicable notices.
