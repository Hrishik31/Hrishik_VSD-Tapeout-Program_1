# Day 1: Fundamentals of Verilog RTL Design & Logic Synthesis

[![RISC-V](https://img.shields.io/badge/RISC--V-Digital%20Design-blue?style=for-the-badge&logo=riscv)](https://riscv.org/)
[![Workshop](https://img.shields.io/badge/RTL-Workshop-orange?style=for-the-badge)](https://vsdiat.vlsisystemdesign.com/)
![Day](https://img.shields.io/badge/Day-1-green?style=for-the-badge)


Welcome to **Day 1** of your RTL Design Journey!  
Today marks the beginning of your exploration into digital circuit design through Verilog HDL, open-source simulation using **Icarus Verilog (iverilog)**, and fundamental logic synthesis concepts with **Yosys**. This comprehensive guide provides hands-on laboratories, core principles, and detailed explanations to establish a solid foundation in RTL methodology.

---

## 📚 Table of Contents

1. [Understanding Simulators, Designs, and Test Environments](#1-understanding-simulators-designs-and-test-environments)
2. [Exploring iverilog Fundamentals](#2-exploring-iverilog-fundamentals)
3. [Hands-on Lab: 2-to-1 Multiplexer Simulation](#3-hands-on-lab-2-to-1-multiplexer-simulation)
4. [Deep Dive into Verilog Implementation](#4-deep-dive-into-verilog-implementation)
5. [Yosys Synthesis Tool & Standard Cell Libraries](#5-yosys-synthesis-tool--standard-cell-libraries)
6. [Practical Synthesis Laboratory](#6-practical-synthesis-laboratory)
7. [Key Takeaways](#7-key-takeaways)

---

## 1. Understanding Simulators, Designs, and Test Environments

### 🔧 Simulator Overview

A **simulator** serves as a software platform that validates your digital circuit behavior by injecting test stimuli and observing responses. This verification step is crucial before physical hardware realization.

### 🎯 Design Definition

Your **design** represents the Verilog HDL code that describes the desired digital logic behavior and functionality.

### 🧪 Test Environment (Testbench)

A **testbench** creates a controlled simulation environment that generates input patterns for your design and validates the corresponding output responses.

<div align="center">
  <img src="https://github.com/user-attachments/assets/93927b96-df80-4da5-b801-284fc2cc6757" alt="Design & Testbench Architecture" width="70%">
</div>

---

## 2. Exploring iverilog Fundamentals

**Icarus Verilog** represents a robust open-source simulation engine for Verilog HDL. The standard simulation workflow follows this pattern:

<div align="center">
  <img src="https://github.com/user-attachments/assets/3ca190fb-cfa4-4abb-b9e1-0151b3c4bdba" alt="iverilog Workflow Diagram" width="70%">
</div>

- Design files and testbenches serve as inputs to the iverilog compiler
- The simulation engine generates a `.vcd` waveform database for analysis in GTKWave

---

## 3. Hands-on Lab: 2-to-1 Multiplexer Simulation

Let's implement and simulate a basic **2-to-1 multiplexer** using the iverilog toolchain!

### 📥 Step 1: Repository Setup

```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

### ⚙️ Step 2: Tool Installation

```bash
sudo apt install iverilog
sudo apt install gtkwave
```

### 🚀 Step 3: Execute Simulation

Compile both design and testbench files:

```bash
iverilog good_mux.v tb_good_mux.v
```

Execute the compiled simulation:

```bash
./a.out
```

Launch waveform viewer:

```bash
gtkwave tb_good_mux.vcd
```

<div align="center">
  <img width="1040" height="712" alt="GTK wave good_mux v" src="https://github.com/user-attachments/assets/81e041a9-123c-4247-af28-6f0598460992" />

</div>

---

## 4. Deep Dive into Verilog Implementation

**Multiplexer implementation code (`good_mux.v`):**

```verilog
module good_mux (input i0, input i1, input sel, output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

### 🔍 **Functional Analysis**

- **Input Ports:** `i0`, `i1` (data channels), `sel` (selection control)
- **Output Port:** `y` (registered output signal)
- **Operational Logic:** When `sel` equals 1, output `y` receives `i1`; when `sel` equals 0, output `y` receives `i0`

---

## 5. Yosys Synthesis Tool & Standard Cell Libraries

### 🛠️ Yosys Overview

**Yosys** stands as a comprehensive open-source synthesis framework for digital hardware design. It transforms your behavioral Verilog descriptions into technology-mapped gate-level implementations.

#### Core Yosys Capabilities

- **HDL Synthesis:** Transforms behavioral descriptions into structural logic
- **Logic Optimization:** Enhances timing performance and area efficiency  
- **Technology Mapping:** Maps generic logic to specific standard cell libraries
- **Design Verification:** Ensures functional equivalence between abstraction levels
- **Flow Customization:** Supports tailored synthesis methodologies

### 🔬 Standard Cell Library Variants

Liberty `.lib` files contain multiple implementations of each logic function with distinct characteristics:

- **Speed Optimization:** High-performance variants for timing-critical paths vs. power-efficient options
- **Power Profiles:** Low-leakage cells for power-sensitive applications
- **Physical Dimensions:** Compact cells for area-constrained designs
- **Drive Capabilities:** Various output strengths to handle different fan-out requirements
- **Process Corners:** Cells optimized for different manufacturing conditions
- **Selection Strategy:** Synthesis tools automatically choose optimal variants based on design constraints

---

## 6. Practical Synthesis Laboratory

Let's synthesize our `good_mux` design using the Yosys synthesis engine!

### 📋 Complete Yosys Synthesis Flow

1. **Initialize Yosys Environment**
    ```bash
    yosys
    ```

2. **Load Standard Cell Library**
    ```bash
    read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
    ```

3. **Import Verilog Source**
    ```bash
    read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v
    ```

4. **Execute Synthesis Process**
    ```bash
    synth -top good_mux
    ```

5. **Perform Technology Mapping**
    ```bash
    abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
    ```

6. **Generate Visual Schematic**
    ```bash
    show
    ```

<div align="center">
  <img width="702" height="747" alt="Netlist" src="https://github.com/user-attachments/assets/4d5b4d4f-0e0f-4240-91ea-786f7d7f40ae" />

</div>

---

## 7. Key Takeaways

✅ **Simulation Mastery:** Gained understanding of simulators, design methodologies, and verification environments  
✅ **Practical Experience:** Successfully executed Verilog simulation using iverilog and analyzed results with waveform viewers  
✅ **Code Comprehension:** Analyzed and understood 2-to-1 multiplexer behavioral implementation  
✅ **Synthesis Introduction:** Explored Yosys synthesis capabilities and learned about standard cell library diversity  
✅ **Hands-on Synthesis:** Completed end-to-end synthesis flow from RTL to gate-level implementation

---



*"Hope is a good thing, maybe the best of things in RTL design. And good code never dies."*
