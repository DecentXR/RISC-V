# RISC-V RV32I Processor Implementations

![RISC-V Logo](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d4/RISC-V-logo.svg/1200px-RISC-V-logo.svg.png)

This repository contains two distinct Verilog implementations of a processor that executes the RISC-V 32-bit base integer instruction set (RV32I). The goal of this project is to explore and demonstrate two fundamental computer architecture designs: a simple **single-cycle** processor and a more complex **5-stage pipelined** processor.

---

## 📋 Table of Contents

- [Implementations Overview](#-implementations-overview)
- [Single-Cycle Implementation](#-single-cycle-implementation)
  - [Architecture](#architecture-single-cycle)
  - [Features](#features-single-cycle)
  - [Project Structure](#project-structure-single-cycle)
- [5-Stage Pipelined Implementation](#-5-stage-pipelined-implementation)
  - [Architecture](#architecture-pipelined)
  - [Features](#features-pipelined)
  - [Project Structure](#project-structure-pipelined)
- [Instruction Set Coverage](#-instruction-set-coverage)
- [Getting Started: Simulation](#-getting-started-simulation)
  - [Prerequisites](#-prerequisites)
  - [Simulating the Single-Cycle CPU](#simulating-the-single-cycle-cpu)
  - [Simulating the Pipelined CPU](#simulating-the-pipelined-cpu)
- [Testing a Program](#-testing-a-program)
- [Contributing](#-contributing)
- [License](#-license)

---

## 📖 Implementations Overview

This project provides two separate processor models:

1.  **Single-Cycle Processor:** A fundamental design where every instruction is executed in exactly one clock cycle. It is simple to understand and debug, making it ideal for learning the basics of CPU datapath and control.
2.  **5-Stage Pipelined Processor:** A more advanced design that increases instruction throughput by overlapping the execution of multiple instructions. It implements a classic 5-stage pipeline (IF, ID, EX, MEM, WB) and includes logic for handling data and control hazards.

---

## 1️⃣ Single-Cycle Implementation

### Architecture (Single-Cycle)

This processor uses a straightforward design where the entire execution of an instruction—from fetch to write-back—is completed within one clock cycle. It consists of two main components: a datapath that processes the data and a controller that generates control signals based on the instruction.

### Features (Single-Cycle)

* **Simplicity:** Easy-to-understand design.
* **Datapath & Controller Separation:** Clear distinction between data-processing hardware (`datapath.v`) and control logic (`controller.v`).
* **Comprehensive Instruction Support:** Implements a wide range of RV32I instructions.

### Project Structure (Single-Cycle)

single-cycle/├── t2b_riscv_cpu.v     # Top-level wrapper for simulation├── riscv_cpu.v         # Core CPU module (connects datapath & controller)├── controller.v        # Control logic and decoders├── datapath.v          # Datapath components (ALU, Reg File, Muxes)├── instr_mem.v         # Instruction Memory├── data_mem.v          # Data Memory└── program_dump.hex    # Hex program file
---

## 5️⃣ 5-Stage Pipelined Implementation

### Architecture (Pipelined)

This processor improves performance by breaking down instruction execution into five stages. This allows multiple instructions to be in different stages of execution simultaneously.

`IF (Fetch) -> ID (Decode) -> EX (Execute) -> MEM (Memory) -> WB (Write-Back)`

### Features (Pipelined)

* **Increased Throughput:** Overlapped instruction execution significantly improves performance over the single-cycle design.
* **Hazard Management:**
    * **Data Hazards:** A `hazard_unit.v` implements data forwarding (bypassing) to prevent most stalls.
    * **Control Hazards:** A `branch_detection.v` unit manages branches by flushing the pipeline to ensure correct program execution.
* **Modular Stage-Based Design:** Each pipeline stage is encapsulated in its own Verilog module (e.g., `fetch_cycle.v`, `decode_cycle.v`).

### Project Structure (Pipelined)

pipelined/├── pipeline_tb.v           # Top-level testbench├── pipeline_cpu.v          # Top-level module connecting all stages├── fetch_cycle.v           # Instruction Fetch (IF) stage├── decode_cycle.v          # Instruction Decode (ID) stage├── execution_cycle.v       # Execute (EX) stage├── memory_cycle.v          # Memory (MEM) stage├── write_back.v            # Write-Back (WB) stage├── hazard_unit.v           # Data hazard detection & forwarding├── branch_detection.v      # Control hazard handling & flushing└── memfile.hex             # Hex program file
---

## 📚 Instruction Set Coverage

Both processors implement a significant portion of the **RV32I** base integer instruction set, including:
* **Load/Store:** `LW`, `SW`, `LB`, `LH`, `SB`, `SH`, `LBU`, `LHU`
* **Arithmetic:** `ADD`, `SUB`, `ADDI`
* **Logical:** `AND`, `OR`,...
