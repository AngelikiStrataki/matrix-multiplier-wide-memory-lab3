# matrix-multiplier-wide-memory-lab3
# Matrix Multiplier – Wide Memory Optimization (Lab 3)

## Overview
This project explores advanced memory access optimizations for matrix multiplication on FPGA accelerators using **wide memory interfaces** and **DDR bank partitioning**. Implemented with Xilinx **Vitis**, this lab demonstrates techniques to accelerate data movement and computation via 512-bit vector types (`ap_uint<512>`).

> 📘 **Note:** The lab report and project presentation are written in Greek.

## Objectives
- Accelerate matrix multiplication using 512-bit data vectors.
- Optimize memory access by splitting input/output data across different DDR memory banks.
- Evaluate the performance benefit over traditional designs from previous labs.

## Contents
- `host.cpp.txt`: Host application that sets up the buffers, computes software reference, and compares with FPGA kernel results.
- `wide_vadd.cpp.txt`: FPGA kernel implementation using `ap_uint<512>` with inner-loop unrolling and custom multiply logic.
- `αναφορά.docx`: Lab report (in Greek) detailing methodology, design changes, and performance analysis.

## How It Works
- Matrix size: 16 × 16 (flattened to 1D buffers of size 256).
- Each buffer element holds a 512-bit wide data word (16 × 32-bit integers).
- The kernel computes one row of the output matrix per outer loop iteration by computing dot-products using 32-bit sub-words.
- Memory banks are partitioned to allow concurrent access to `in1`, `in2`, and `out` buffers.
- Achieved kernel execution time: **0.009s**, showing a ~9× improvement over Lab 2’s baseline of 0.082s.

## Key Optimizations
- Use of `ap_uint<512>` for wide memory access (16× 32-bit values per word).
- Removal of `dataflow` and `stream` pragmas to manually control loop structure.
- Full loop unrolling for the inner 32-bit multiplications.
- Explicit memory partitioning via different AXI memory bundles (`gmem`, `gmem1`, `gmem2`).

## How to Run
1. Open **Vitis IDE** and create a new application targeting an FPGA platform (e.g., Alveo U200).
2. Import the `wide_vadd.cpp.txt` kernel and `host.cpp.txt` as the host code.
3. Set up platform and linker files as needed.
4. Build and run the project:
   - Software Emulation (optional)
   - Hardware Emulation
   - Hardware Execution (with real FPGA)
5. Check console output to verify correctness and compare performance.

## Authors
- Dimitrios Orestis Vagenas (10595)
- Angeliki Strataki (10523)
