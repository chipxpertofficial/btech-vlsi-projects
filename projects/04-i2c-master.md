# 04 · I2C master controller

**Tier:** ⭐⭐ Intermediate · **~5–6 weeks**

I2C teaches open-drain buses, bidirectional lines, ACK/NACK handling, and clock stretching — all
things a synchronous-only designer tends to get wrong the first time.

## Problem statement
Design an I2C single-master controller that performs 7-bit addressed read and write transactions
at 100 kHz and 400 kHz, drives SDA/SCL as true open-drain (drive-low / release-to-Z), and
respects slave clock stretching.

## Interface sketch
```
clk, rst_n
cmd_start, cmd_rw, dev_addr[6:0], wr_data[7:0] -> busy, rd_data[7:0], ack_error, done
scl, sda   // inout, open-drain: out=0 drives low, tri-state releases
```

## Module breakdown
- `clk_div` — generates SCL phases; a quarter-bit counter gives you setup points for SDA.
- `i2c_fsm` — START → ADDR+RW → (ACK) → DATA → (ACK) → STOP, with repeated-START support.
- open-drain cells: `sda_oe`/`scl_oe` select between driving `1'b0` and `1'bz`.
- clock-stretch detect: after releasing SCL high, wait until it actually reads high.

## Verification plan
- Write a behavioural slave model (addressable register file) that ACKs its address.
- Single-byte write, single-byte read, multi-byte, and a wrong-address → NACK case.
- A stretching slave that holds SCL low; confirm the master waits.
- Check START/STOP = SDA edge while SCL high.

## Stretch goals
- Repeated START (write-then-read in one transaction — the EEPROM pattern).
- Bus-busy / arbitration-lost detection.
- Register map so a CPU could drive it.

## Deliverables
Spec, annotated START/STOP/ACK timing diagram, slave model + self-checking testbench, waveforms
for a write and a clock-stretched read, synthesis report.
