# 08 · FPGA VGA / HDMI pattern generator

**Tier:** ⭐⭐ Intermediate · **~5–6 weeks**

A satisfying project because it lights up a real monitor. It is all about precise timing
generation and a clean pixel pipeline.

## Problem statement
Design a display controller that generates 640×480 @ 60 Hz VGA timing (25.175 MHz pixel clock),
driving HSYNC/VSYNC and an RGB bus, and renders a test pattern (colour bars, a moving box, and
text or a bouncing sprite).

## Interface sketch
```
pix_clk, rst_n
hsync, vsync            // active-low
r[3:0], g[3:0], b[3:0]  // to a resistor-ladder DAC or FPGA RGB pins
```
640×480 timing: H = 640 visible + 16 front + 96 sync + 48 back = 800; V = 480 + 10 + 2 + 33 = 525.

## Module breakdown
- `pix_clk_gen` — PLL/MMCM to 25.175 MHz (or 25 MHz, which most monitors tolerate).
- `timing_gen` — H and V counters producing `hsync`, `vsync`, `video_on`, and `(x,y)`.
- `pattern_gen` — map `(x,y)` → colour: colour bars, gridlines, a box whose origin advances each frame.
- (stretch) `char_rom` + text overlay, or a small sprite with a line buffer.

## Verification plan
- In simulation, assert HSYNC/VSYNC pulse widths and the visible-region boundaries against the
  720/525 totals.
- Dump one frame of `(x,y,rgb)` and render it to a PNG in Python to eyeball the pattern.
- On hardware: confirm the monitor locks at 640×480@60.

## Stretch goals
- Read the pattern from block RAM (a tiny framebuffer) over an AXI-Lite write port.
- HDMI/DVI output via TMDS encoding (serialise the RGB) instead of analog VGA.
- Double-buffering for tear-free animation.

## Deliverables
Spec with the full timing table, block diagram, timing-assertion testbench, a PNG rendered from a
simulated frame, a photo of the monitor if you have the board, synthesis/utilisation report.
