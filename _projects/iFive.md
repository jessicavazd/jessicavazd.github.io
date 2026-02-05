---
# layout: page
# title: iFive
# description: with background image
# img: assets/img/12.jpg
# importance: 1
# category: work
# related_publications: false
layout: page
title: iFive
description: High-performance Out-of-Order RISC-V CPU featuring explicit register renaming, reorder buffer, split load-store queue and early branch recovery.
# img: assets/img/projects/ooo/ooo_pipeline.png
img: assets/img/12.jpg
importance: 1
category: work
related_publications: false
---

 
## Overview

This project involved designing and verifying a **32-bit RV32IM out-of-order processor** to explore modern CPU microarchitecture concepts such as instruction-level parallelism, speculative execution, and dynamic scheduling.

The processor preserves program semantics while executing instructions out-of-order to improve throughput and eliminate false dependencies. Key structures enabling this behavior include the **Register Alias Table (RAT), Reservation Stations, and Reorder Buffer (ROB)**.  

Our goal was to build an energy-efficient processor capable of recovering quickly from branch mispredictions while maintaining strong performance.



## Architecture

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/iFive/iFive_block_diagram.png" title="iFive baseline architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Initial datapath of the out-of-order processor showing fetch, rename/dispatch, reservation stations, functional units, and ROB.
</div>

The processor follows the classic pipeline stages:

- Fetch  
- Decode  
- Execute  
- Memory  
- Writeback :contentReference[oaicite:2]{index=2}  

Out-of-order execution allows the CPU to continue doing useful work instead of stalling on dependencies, significantly improving instructions-per-cycle (IPC). :contentReference[oaicite:3]{index=3}  



## Key Microarchitectural Features

### Explicit Register Renaming
Our design centers on **Explicit Register Renaming (ERR)**, which dynamically maps architectural registers to a larger pool of physical registers.

**Benefits:**
- Eliminates false dependencies  
- Enables more in-flight instructions  
- Improves functional unit utilization  
- Scales cleanly with wider execution pipelines :contentReference[oaicite:4]{index=4}  



### Dispatch, Reservation Stations, and ROB

- **Dispatch:** Allocates physical registers and ROB entries while decoding instructions. :contentReference[oaicite:5]{index=5}  
- **Reservation Stations:** Hold instructions until operands are ready. :contentReference[oaicite:6]{index=6}  
- **Reorder Buffer:** Commits instructions in program order, ensuring precise architectural state. :contentReference[oaicite:7]{index=7}  

Together, these components enable safe speculative execution.



## Memory System

We implemented a **Load-Store Queue (LSQ)** to track memory operations in program order while allowing address computation out-of-order. :contentReference[oaicite:8]{index=8}  

An arbiter prioritizes instruction fetch requests over data accesses, improving front-end throughput. :contentReference[oaicite:9]{index=9}  



## Advanced Features

### Split Load-Store Queue

The unified LSQ was later divided into:

- 8 load reservation stations  
- 8-entry store queue  

This allowed loads to execute out-of-order relative to stores, increasing memory parallelism. :contentReference[oaicite:10]{index=10}  



### Early Branch Recovery

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/ooo/ebr_results.png" title="Early Branch Recovery Performance" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Early Branch Recovery significantly reduced pipeline flush delays caused by mispredictions.
</div>

Instead of waiting until commit to flush mispredicted branches, we implemented bookkeeping structures to recover earlier — reducing wasted cycles and power. :contentReference[oaicite:11]{index=11}  

Branches occur frequently in workloads, so shifting delays toward shorter flush times produced measurable speedups. :contentReference[oaicite:12]{index=12}  



## Performance

**Synthesis Results:**

- **Area:** 178,982 µm²  
- **Fmax:** 535.91 MHz :contentReference[oaicite:13]{index=13}  

Benchmarking across workloads such as CoreMark, FFT, AES, and ray tracing demonstrated consistent IPC improvements after architectural optimizations. :contentReference[oaicite:14]{index=14}  



## Power Optimizations

### Clock-Gated SRAM
Profiling revealed the data cache consumed ~25% of total energy. Clock gating ensured SRAM was only active when required, saving **~3.6 mW**. :contentReference[oaicite:15]{index=15}  

### Retimed Multiplier
By restructuring the multiplier pipeline, we reduced clock delay from **1850 ps to 1790 ps**, improving the critical path. :contentReference[oaicite:16]{index=16}  



## Outcome

Despite implementing relatively few advanced features, the processor ranked **4th out of 30 teams** on the class leaderboard — validating our focus on energy efficiency and branch recovery. :contentReference[oaicite:17]{index=17}  

This project strengthened expertise in:

- Modern CPU microarchitecture  
- Speculative execution  
- Hardware verification  
- Performance and power tradeoffs  
- Collaborative chip design  



## Tech Stack

**Languages:** SystemVerilog, C/C++  
**Tools:** Synopsys Design Compiler, Verilator, SpyGlass  
**ISA:** RV32IM  



## Future Improvements

- Integrate a high-accuracy branch predictor  
- Expand memory disambiguation  
- Increase instruction-level parallelism  
- Further optimize cache hierarchy  

 -->
