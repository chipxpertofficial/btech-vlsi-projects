# 06 · 5-stage pipelined RISC-V (RV32I) core

**Tier:** ⭐⭐⭐ Advanced · **full semester**

The flagship final-year project: a working CPU. RV32I is small enough to finish and real enough
to run compiled C. You will touch datapath design, hazard handling, and a full verification flow.

## Problem statement
Design a 5-stage (IF, ID, EX, MEM, WB) pipelined core implementing the RV32I base integer ISA,
with full data-forwarding, load-use stall, and branch handling, able to run a compiled bare-metal
program to completion.

## Module breakdown
- `regfile` — 32×32, two read / one write, x0 hardwired to 0.
- `decode` — generate control signals for every RV32I opcode (R, I, S, B, U, J).
- `alu` + `alu_ctrl` — add/sub, logic, shifts, SLT/SLTU, branch comparisons.
- `forward_unit` — EX/MEM and MEM/WB bypass.
- `hazard_unit` — load-use stall (1 bubble), branch flush.
- `imem`/`dmem` — simple synchronous memories, program loaded via `$readmemh`.

## Verification plan
- Start with directed tests per instruction class (one assembly snippet each).
- Then run the **riscv-tests** `rv32ui` suite (or a trimmed subset); each test writes a
  pass/fail code to a known address — assert it.
- Compare the architectural register trace against a reference (e.g. Spike) for a sorting program.
- Specifically exercise: forwarding, load-use, taken/not-taken branches, `jal`/`jalr`.

## Stretch goals
- Add `M` extension (mul/div) — a multi-cycle unit with stalls.
- Add CSRs + machine-mode trap handling for `ecall`/timer interrupt.
- Put it on an FPGA driving a UART "hello world" from compiled C.

## Deliverables
Spec + pipeline diagram with hazards annotated, per-class directed tests, a riscv-tests run log,
a trace-compare for one real program, synthesis + timing report (critical path, Fmax), and an
honest list of unimplemented instructions.
