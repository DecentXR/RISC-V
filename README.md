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

The single-cycle processor is contained within the `single-cycle/` directory and includes the following key files:

* **`t2b_riscv_cpu.v`**: Top-level wrapper for simulation and integration.
* **`riscv_cpu.v`**: The core CPU module that connects the datapath and controller.
* **`controller.v`**: Contains the control logic and instruction decoders.
* **`datapath.v`**: Contains the main datapath components like the ALU, Register File, and multiplexers.
* **`instr_mem.v`**: Instruction Memory module.
* **`data_mem.v`**: Data Memory module.
* **`program_dump.hex`**: The hex file containing the program to be executed.

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
* **Modular Stage-Based Design:** Each pipeline stage is encapsulated in its own Verilog module.

### Project Structure (Pipelined)

The pipelined processor is contained within the `pipelined/` directory and includes the following key files:

* **`pipeline_tb.v`**: The top-level testbench for simulation.
* **`pipeline_cpu.v`**: The top-level module connecting all pipeline stages and hazard units.
* **`fetch_cycle.v`**: Logic for the Instruction Fetch (IF) stage.
* **`decode_cycle.v`**: Logic for the Instruction Decode (ID) stage.
* **`execution_cycle.v`**: Logic for the Execute (EX) stage.
* **`memory_cycle.v`**: Logic for the Memory (MEM) stage.
* **`write_back.v`**: Logic for the Write-Back (WB) stage.
* **`hazard_unit.v`**: Handles data hazard detection and forwarding.
* **`branch_detection.v`**: Manages control hazards and pipeline flushing.
* **`memfile.hex`**: The hex file containing the program to be executed.

---

## 📚 Instruction Set Coverage

Both processors implement a significant portion of the **RV32I** base integer instruction set, including:

* **Load/Store:** `LW`, `SW`, `LB`, `LH`, `SB`, `SH`, `LBU`, `LHU`
* **Arithmetic:** `ADD`, `SUB`, `ADDI`
* **Logical:** `AND`, `OR`, `XOR`, `SLT`, `SLL`, `SRL`, `SRA` and their immediate variants.
* **Control Flow:** `BEQ`, `BNE`, `BLT`, `BGE`, `JAL`, `JALR`
* **Upper Immediate:** `LUI`, `AUIPC`

---

## 🚀 Getting Started: Simulation

### Prerequisites

You will need a Verilog simulator. The following are common choices:
* [Verilator](https://verilator.org/guide/latest/install.html) (Open-source, recommended)
* [Icarus Verilog](http://iverilog.icarus.com/)
* [ModelSim](https://www.intel.com/content/www/us/en/software-kit/665978/modelsim-intel-fpgas-standard-edition-software-version-18-1.html)

### Simulating the Single-Cycle CPU

1.  Navigate to the `single-cycle/` directory.
2.  Ensure you have a testbench that instantiates `t2b_riscv_cpu`.
3.  Compile and run using your simulator.
    *Example with Verilator:*
    ```bash
    verilator --cc --exe --build -j 0 your_testbench.v --top-module your_tb_module
    ./obj_dir/Vyour_tb_module
    ```

### Simulating the Pipelined CPU

1.  Navigate to the `pipelined/` directory.
2.  Use the provided `pipeline_tb.v` as your top-level testbench.
3.  Compile and run using your simulator.
    *Example with Verilator:*
    ```bash
    verilator --cc --exe --build -j 0 pipeline_tb.v --top-module tb
    ./obj_dir/Vtb
    ```
4.  View the output `dump.vcd` file with a waveform viewer like GTKWave.

---

## 🧪 Testing a Program

To run a custom program on either processor:
1.  Write your program in RISC-V assembly (`.s`).
2.  Use a RISC-V toolchain to assemble it and produce a hex file.
3.  Replace the contents of the relevant hex file (`program_dump.hex` or `memfile.hex`) with your new program.
4.  Rerun the simulation.

---

## 🤝 Contributing

Contributions are welcome! If you have suggestions for improvements or find any bugs, please feel free to open an issue or submit a pull request.

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request
