# 03 · Pipelined integer MAC / FIR filter

**Tier:** ⭐⭐ Intermediate · **~5–6 weeks**

The first project that makes you think about throughput, fixed-point scaling, and pipeline
latency — and gives you a MATLAB/Python golden model to check against.

## Problem statement
Design a parameterised N-tap FIR filter with signed fixed-point coefficients, a transposed or
pipelined multiply-accumulate datapath that accepts one sample per clock, and correct
rounding/saturation on the output.

## Interface sketch
```
clk, rst_n
coeff[N-1:0][W-1:0]   // loaded via a small config port
x_in[W-1:0], x_valid  -> y_out[W-1:0], y_valid
```
Parameters: `N` (taps), `W` (word width), `COEFF_W`, `ACC_W = W + COEFF_W + ceil(log2 N)`.

## Module breakdown
- `mac` — one signed multiplier + accumulator stage, registered.
- `fir_pipe` — chain of `N` MAC stages (transposed form keeps the critical path short).
- `round_sat` — arithmetic rounding then saturation back to `W` bits.

## Verification plan
- Generate coefficients (e.g. a low-pass via `scipy.signal.firwin`), quantise them, feed an
  impulse → the output should be the quantised coefficients (impulse response).
- Feed a chirp; compare RTL output against the Python fixed-point model sample-by-sample within
  ±1 LSB.
- Check latency = pipeline depth, and that `y_valid` tracks it.

## Stretch goals
- Symmetric-coefficient folding (half the multipliers).
- Reloadable coefficients mid-stream.
- Compare resource/Fmax of direct vs transposed form in your report.

## Deliverables
Spec with the fixed-point scaling worked out, Python golden model in the repo, bit-accurate
cross-check testbench, impulse-response waveform, synthesis report comparing both structures.
