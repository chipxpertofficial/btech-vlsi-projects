# 01 · UART with configurable baud rate + FIFO buffering

**Tier:** ⭐ Starter · **~3–4 weeks**

A universal asynchronous receiver/transmitter is the classic "first real block." It forces you
to handle oversampling, framing, a parameterised baud divisor, and clock-domain safety on the
receive line.

## Problem statement
Design a UART that transmits and receives 8-N-1 frames (8 data bits, no parity, 1 stop bit) at a
baud rate selectable at run time, with a small FIFO on each side so the host does not lose bytes
between accesses.

## Interface sketch
```
clk, rst_n
baud_div[15:0]      // = f_clk / (16 * baud)
tx_data[7:0], tx_valid  -> tx_ready       // write side
rx_data[7:0], rx_valid  <- rx_ready       // read side
uart_txd (out), uart_rxd (in)
```

## Module breakdown
- `baud_gen` — generates a 16× oversampling tick from `baud_div`.
- `uart_tx` — FSM: IDLE → START → DATA×8 → STOP.
- `uart_rx` — start-bit detect on the oversampling tick, mid-bit sampling, framing-error flag.
- `sync_2ff` — two-flop synchroniser on `uart_rxd` (the asynchronous input).
- `fifo` — small synchronous FIFO (depth 16) on each path.

## Verification plan
- Loopback: tie `txd`→`rxd`, send 256 random bytes, assert received stream equals sent.
- Baud sweep: 9600, 115200, 1 Mbaud; check bit periods on the waveform.
- Corner: back-to-back frames with no idle gap; FIFO full/empty flags.

## Stretch goals
- Add parity (odd/even) and a parity-error flag.
- Auto-baud detection from a known sync byte (0x55).
- Wishbone or AXI4-Lite register interface for `baud_div` and status.

## Deliverables
Spec page, block diagram, self-checking loopback testbench, waveform screenshots for one full
frame and one framing error, and a Yosys synthesis report (LUT/FF count, Fmax).
