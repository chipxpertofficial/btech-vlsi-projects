# B.Tech VLSI Projects

A curated set of **final-year B.Tech project ideas in VLSI / digital design**, each with a
scoped problem statement, a suggested RTL module list, difficulty tier, tool-chain, and the
deliverables an examiner actually asks for. These are starting points you are meant to *build*,
not finished code to submit — the fun (and the marks) are in the implementation.

Every project here is doable with **free, open tools**: write Verilog/SystemVerilog, simulate
with [Icarus Verilog](https://github.com/steveicarus/iverilog) or
[Verilator](https://github.com/verilator/verilator), view waveforms in
[GTKWave](https://github.com/gtkwave/gtkwave), and (for the synthesis/FPGA tiers) push through
[Yosys](https://github.com/YosysHQ/yosys) + [nextpnr](https://github.com/YosysHQ/nextpnr) or a
vendor flow.

## How to pick

| Tier | Time | What it proves |
|------|------|----------------|
| ⭐ Starter | 3–4 weeks | Clean RTL + a self-checking testbench |
| ⭐⭐ Intermediate | 5–8 weeks | A protocol/block integrated and verified |
| ⭐⭐⭐ Advanced | full semester | A small SoC subsystem, synthesis + timing closure, maybe FPGA |

## The projects

| # | Project | Tier | Core topic |
|---|---------|------|------------|
| 01 | [UART with configurable baud + FIFO](projects/01-uart-fifo.md) | ⭐ | Serial I/O, CDC, FIFOs |
| 02 | [SPI master + slave pair](projects/02-spi-master-slave.md) | ⭐ | Serial protocol, FSM |
| 03 | [Pipelined integer MAC / FIR filter](projects/03-fir-filter.md) | ⭐⭐ | DSP, pipelining |
| 04 | [I2C master controller](projects/04-i2c-master.md) | ⭐⭐ | Open-drain bus, clock stretching |
| 05 | [AXI4-Lite register slave](projects/05-axi4lite-slave.md) | ⭐⭐ | Bus protocol, handshakes |
| 06 | [5-stage RISC-V (RV32I) core](projects/06-riscv-rv32i.md) | ⭐⭐⭐ | Datapath, hazards, forwarding |
| 07 | [AES-128 encryption core](projects/07-aes128.md) | ⭐⭐⭐ | Crypto datapath, key schedule |
| 08 | [FPGA VGA/HDMI pattern generator](projects/08-vga-controller.md) | ⭐⭐ | Timing generation, line buffers |

Each page lists: the problem, an interface sketch, a module breakdown, a verification plan,
stretch goals, and the report/demo deliverables.

## Turning a project into a report

Examiners look for the same things every year:
1. **Spec** — a one-page interface + behaviour definition before you write RTL.
2. **Block diagram** — datapath and control separated.
3. **Verification evidence** — a self-checking testbench, a coverage note, waveforms for the corner cases.
4. **Synthesis results** — cell/LUT count, max frequency, critical path (even a Yosys report counts).
5. **Honest limitations** — what you did *not* implement and why.

## Learning the fundamentals first

If a block below assumes something you haven't covered — FSM encoding, setup/hold, CDC,
metastability — the companion
[digital-electronics-reference](https://github.com/chipxpertofficial/digital-electronics-reference)
has concise notes, and
[vlsi-verilog-examples](https://github.com/chipxpertofficial/vlsi-verilog-examples) has
simulation-verified Verilog for most of the primitives these projects build on.

For structured, mentor-led project guidance, the
[ChipXpert VLSI training programs](https://chipxpert.in/) run final-year students through this
kind of flow end to end.

## License

MIT — see [LICENSE](LICENSE). Use these ideas freely in your coursework and portfolio.
Maintained by the ChipXpert team.
