# 02 · SPI master + slave pair

**Tier:** ⭐ Starter · **~3–4 weeks**

SPI is a clean way to practise a shift-register datapath and a mode-aware FSM, and building
*both* ends lets you verify them against each other with no external model.

## Problem statement
Design an SPI master supporting all four modes (CPOL/CPHA) with a programmable clock divider and
a byte-at-a-time data interface, plus a matching SPI slave, and verify them connected together.

## Interface sketch
```
Master:  clk, rst_n, start, mode[1:0], clk_div[7:0],
         mosi_byte[7:0] -> busy, miso_byte[7:0], done
         sclk, mosi, cs_n (out), miso (in)
Slave:   sclk, cs_n, mosi (in), miso (out), tx_byte[7:0], rx_byte[7:0], rx_valid
```

## Module breakdown
- `spi_master` — clock-divider + 8-bit shift register; sample/shift edges chosen by `mode`.
- `spi_slave` — shift register clocked by incoming `sclk`, framed by `cs_n`.
- `spi_tb` — wires master→slave and slave→master, drives transactions.

## Verification plan
- All 4 modes × random bytes; assert master's received byte equals slave's `tx_byte` and vice versa.
- Check `cs_n` asserts one `sclk` before the first edge and de-asserts after the last.
- Clock-divider sweep (÷2, ÷4, ÷16).

## Stretch goals
- Multi-byte burst with `cs_n` held low across bytes.
- Multiple slave selects (3–4 `cs_n` lines + address decode).
- Model a real device (e.g. an SPI flash `READ_ID` 0x9F) in the slave.

## Deliverables
Spec, CPOL/CPHA timing diagram you drew yourself, cross-connected self-checking testbench,
waveforms for mode 0 and mode 3, synthesis report.
