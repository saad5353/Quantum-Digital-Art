# Quantum Randomness → Colors

**Schrödinger's Gambit — Experiment 01**

## Overview

This experiment turns real quantum measurement outcomes into colors. A quantum circuit is built, measured, and the resulting classical bits are deterministically transformed into RGB colors — from single swatches, to palettes, to a full 1920×1080 digital art mosaic made of 2,073,600 independent quantum-generated colors.

No quantum advantage or cryptographic security is claimed. This is an educational demonstration of turning quantum measurement data into a visual domain.

## What We Did

1. Built a simple quantum circuit: each qubit starts at `|0⟩`, gets a Hadamard gate, and is measured.
2. Ran the circuit on Qiskit Aer's simulator and collected per-shot measurement outcomes (classical bits).
3. Converted groups of bits into RGB values using a fixed, disclosed mapping.
4. Generated a single quantum color, a palette of quantum colors, and (as a large-scale extension) a full 1920×1080 image where every pixel is its own independent quantum-derived color.
5. Compared quantum-derived colors against classical pseudorandom colors, purely for context — not competition.

## How We Did It — The Pipeline

Quantum Circuit → Measurement → Bitstream → Integer Mapping → RGB → HEX → Color / Image


- **Circuit:** `n` qubits, each with `H` then `Measure`. No entanglement — each qubit is an independent equal superposition:
  H|0⟩ = (1/√2)(|0⟩ + |1⟩) → P(0) = P(1) = 0.5
- **Execution:** Qiskit Aer's `AerSimulator`, with `memory=True` to keep every individual shot's outcome (not just aggregated counts).
- **Bitstream:** each shot returns one bitstring, e.g. `"101101...01"`.

## Bit-to-Color Mapping (the algorithm)

Each color needs exactly **24 bits** (one shot of a 24-qubit circuit):

* BITS_PER_CHANNEL = 8
* NUM_CHANNELS = 3 (R, G, B)
* BITS_PER_COLOR = 24 (8 × 3)

* bits[ 0: 8] → R
* bits[ 8:16] → G
* bits[16:24] → B

* int(chunk, 2) → e.g. "01010101" → 85

* RGB = (R, G, B)
* HEX = "#{R:02X}{G:02X}{B:02X}"


For the **1920×1080 mosaic**, this same process is repeated independently for every pixel:

Total pixels = 1920 × 1080 = 2,073,600
1 shot (24 qubits) = 1 pixel's RGB color
2,073,600 shots → 2,073,600 independent quantum colors
→ arranged row by row into a 1080-row × 1920-column image


Bit order follows Qiskit's default (leftmost character = most significant bit). The mapping is arbitrary but fixed and fully disclosed — nothing about it is hidden.

## Notes

- Uses an ideal, noiseless **simulator** — not physical quantum hardware.
- Hadamard + Measure circuits are Clifford circuits, which Aer simulates very efficiently — this is why generating 2M+ independent shots is feasible in Colab.
- Colors after measurement are the result of ordinary, deterministic classical computation — the quantum part is strictly the source of the bits.

Part of [Schrödinger's Gambit](https://schrodingersgambit.com) — a series of experiments exploring how quantum measurement can be translated into other domains.
