# 05 · AXI4-Lite register slave

**Tier:** ⭐⭐ Intermediate · **~5–7 weeks**

Almost every SoC IP you will ever write hangs off an AXI bus. A correct AXI4-Lite slave — five
independent channels with VALID/READY handshakes — is a portfolio piece on its own.

## Problem statement
Design an AXI4-Lite slave exposing a small register file (say 8 × 32-bit registers), correctly
implementing all five channels (AW, W, B, AR, R) with independent handshakes, byte strobes on
writes, and a legal response code.

## Interface sketch
Standard AXI4-Lite: `AWADDR/AWVALID/AWREADY`, `WDATA/WSTRB/WVALID/WREADY`,
`BRESP/BVALID/BREADY`, `ARADDR/ARVALID/ARREADY`, `RDATA/RRESP/RVALID/RREADY`.

## Module breakdown
- Write path: accept AW and W independently (either may arrive first), commit when both seen,
  then raise B. Apply `WSTRB` per byte.
- Read path: accept AR, register the addressed word, raise R with `OKAY`/`SLVERR`.
- Decode: map `AWADDR[4:2]` / `ARADDR[4:2]` to the register index; out-of-range → `SLVERR`.

## Verification plan
- A small AXI-Lite driver task set (write(addr,data,strb), read(addr)).
- Interleavings: AW-before-W, W-before-AW, back-to-back writes, read-after-write ordering.
- `WSTRB` partial-byte writes verified against a shadow model.
- Out-of-range address returns `SLVERR`; VALID never drops before READY.

## Stretch goals
- A read-only status register and a write-1-to-clear interrupt register.
- Attach a real block behind it (e.g. the UART from project 01) and drive it over AXI.
- A simple formal check: VALID held stable until handshake.

## Deliverables
Spec + register map, channel handshake diagrams, directed + randomised testbench with a shadow
model, waveforms showing an interleaved AW/W, synthesis report.
