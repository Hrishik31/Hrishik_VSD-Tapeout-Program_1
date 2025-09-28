# Day 4: RTL Design Workshop
## Gate-Level Simulation, Assignment Types in Verilog, and Synthesis Mismatches

<div align="center">

[![RISC-V](https://img.shields.io/badge/RISC--V-Digital%20Design-blue?style=for-the-badge&logo=riscv)](https://riscv.org/)
[![Workshop](https://img.shields.io/badge/RTL-Workshop-orange?style=for-the-badge)](https://vsdiat.vlsisystemdesign.com/)
![Day](https://img.shields.io/badge/Day-4-green?style=for-the-badge)

</div>

## Workshop Overview

This session explores critical concepts in digital design verification and RTL coding practices. We examine gate-level simulation methodologies, analyze different assignment techniques in Verilog, and investigate potential discrepancies between synthesis and simulation results. Through practical laboratory exercises, we demonstrate proper coding techniques and common pitfalls to avoid in RTL design.

---

## Learning Objectives

By the end of this workshop, participants will understand:
- Gate-level simulation fundamentals and applications
- Proper usage of blocking and non-blocking assignments
- Common causes of synthesis-simulation discrepancies
- Best practices for writing synthesizable RTL code

---

## Table of Contents

1. [Gate-Level Simulation Fundamentals](#gate-level-simulation-fundamentals)
2. [Understanding Synthesis-Simulation Discrepancies](#understanding-synthesis-simulation-discrepancies)
3. [Assignment Types in Verilog](#assignment-types-in-verilog)
4. [Laboratory Exercises](#laboratory-exercises)
5. [Key Takeaways](#key-takeaways)

---

## Gate-Level Simulation Fundamentals

Gate-Level Simulation represents a crucial verification methodology in digital design where the post-synthesis netlist undergoes simulation to ensure design integrity.

### Purpose and Benefits

**Verification Objectives:**
- Validate logical functionality after synthesis transformation
- Confirm timing performance with realistic gate delays
- Verify testability features and scan chain functionality
- Obtain accurate power consumption estimates

**Implementation Timeline:**
- Executed following RTL-to-netlist synthesis
- Performed prior to physical implementation stages
- Used for early detection of design issues

### Simulation Categories

**Functional Verification:**
- Logic-only simulation approach
- Utilizes zero or unit delay models
- Focuses on correctness validation

**Timing Analysis:**
- Incorporates real-world delay annotations
- Uses Standard Delay Format (SDF) files
- Identifies timing constraint violations

---

## Understanding Synthesis-Simulation Discrepancies

Synthesis-simulation mismatches occur when RTL simulation behavior differs from gate-level netlist simulation or actual hardware performance.

### Root Causes

**Non-Synthesizable Constructs:**
- Usage of delay statements in RTL code
- Implementation of initial blocks
- Inclusion of simulation-specific constructs

**Ambiguous RTL Coding:**
- Incomplete sensitivity lists in always blocks
- Missing else clauses in conditional statements
- Improper reset handling mechanisms

**Tool Interpretation Differences:**
- Varying interpretation of ambiguous code
- Different optimization strategies between tools
- Version-specific behavioral differences

### Prevention Strategies

- Write explicit and unambiguous RTL code
- Follow established coding guidelines
- Implement comprehensive sensitivity lists
- Use proper reset mechanisms

---

## Assignment Types in Verilog

Verilog provides two distinct assignment mechanisms for procedural blocks, each serving specific design purposes.

### Blocking Assignments (`=`)

**Characteristics:**
- Utilizes `=` operator for assignment
- Executes sequentially and immediately
- Updates variables instantly during simulation
- Appropriate for combinational logic modeling

**Application Areas:**
- Combinational logic circuits
- Temporary variable assignments
- Always blocks with `@(*)` sensitivity

**Example Implementation:**
```verilog
always @(*) begin
  result = input_a & input_b;
end
```

### Non-Blocking Assignments (`<=`)

**Characteristics:**
- Uses `<=` operator for assignment
- Schedules updates for end of time step
- Executes concurrently with other non-blocking assignments
- Suitable for sequential logic modeling

**Application Areas:**
- Sequential circuit design
- Register and flip-flop modeling
- Clocked always blocks

**Example Implementation:**
```verilog
always @(posedge clock) begin
  output_reg <= input_data;
end
```

### Comparison Summary

| **Blocking Assignments**               | **Non-Blocking Assignments**           |
|----------------------------------------|----------------------------------------|
| Sequential execution model             | Concurrent execution model             |
| Immediate variable updates             | Scheduled end-of-timestep updates      |
| Combinational logic synthesis          | Sequential logic synthesis             |
| Temporary variable handling            | Register and flip-flop modeling        |
| `always @(*)` block usage             | `always @(posedge clk)` block usage    |

---

## Laboratory Exercises

### Lab 1: Ternary Operator Multiplexer

**Objective:** Implement and simulate a 2:1 multiplexer using ternary operator

**RTL Implementation:**
```verilog
module ternary_operator_mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```

**Functional Description:**
- Output `y` equals `i1` when `sel` is high
- Output `y` equals `i0` when `sel` is low

**RTL Simulation Results:**
<img width="1277" height="780" alt="gtkwave_ternary_operator_mux" src="https://github.com/user-attachments/assets/a24cac17-78fb-4127-94fb-1605adada85e" />


---

### Lab 2: Synthesis with Yosys

**Objective:** Convert RTL design to gate-level netlist using Yosys synthesis tool

**Synthesis Flow:**
1. Read RTL design files
2. Synthesize using target technology library
3. Generate gate-level netlist
4. Analyze synthesis reports

**Synthesis Results:**
<img width="1278" height="632" alt="ternary_operator_mux" src="https://github.com/user-attachments/assets/f014420b-b66b-4523-a328-2ccfd2944a06" />


---

### Lab 3: Gate-Level Simulation of Multiplexer

**Objective:** Verify synthesized netlist functionality through gate-level simulation

**Simulation Command:**
```bash
iverilog <library_path>/primitives.v <library_path>/sky130_fd_sc_hd.v ternary_operator_mux.v testbench.v
```

**GLS Verification Results:**
<img width="1280" height="785" alt="gtkwave_ternary_operator_mux_GLS" src="https://github.com/user-attachments/assets/4939c878-bc56-4094-a58e-ef2b94491170" />


---

### Lab 4: Problematic Multiplexer Design

**Objective:** Demonstrate common RTL coding errors and their consequences

**Flawed Implementation:**
```verilog
module bad_mux (input i0, input i1, input sel, output reg y);
  always @ (sel) begin
    if (sel)
      y <= i1;
    else 
      y <= i0;
  end
endmodule
```

**Identified Issues:**
- Incomplete sensitivity list (missing `i0` and `i1`)
- Inappropriate use of non-blocking assignments
- Potential synthesis-simulation mismatch

**Corrected Version:**
```verilog
always @ (*) begin
  if (sel)
    y = i1;
  else
    y = i0;
end
```

**RTL Simulation of Flawed Design:**
<img width="1211" height="757" alt="gtk_wave_bad_mux" src="https://github.com/user-attachments/assets/3a896c02-871a-4350-8b26-725fc9537883" />


---

### Lab 5: Gate-Level Simulation of Flawed Design

**Objective:** Compare GLS results with RTL simulation for problematic code

**Expected Outcome:** Simulation discrepancies due to coding issues

**GLS Results for Flawed Design:**
<img width="1147" height="722" alt="gtk_wave_bad_mux_syn_mismatch" src="https://github.com/user-attachments/assets/bdf05b5e-769c-4759-a742-5777428980d9" />


---

### Lab 6: Blocking Assignment Issues

**Objective:** Illustrate problems arising from incorrect assignment ordering

**Problematic Code:**
```verilog
module blocking_caveat (input a, input b, input c, output reg d);
  reg x;
  always @ (*) begin
    d = x & c;  // Uses old value of x
    x = a | b;  // Updates x after d is computed
  end
endmodule
```

**Issue Analysis:**
- Variable `d` uses previous value of `x`
- Assignment order affects simulation behavior
- Creates unintended logic dependencies

**Corrected Implementation:**
```verilog
always @ (*) begin
  x = a | b;  // Compute x first
  d = x & c;  // Use updated value of x
end
```

**RTL Simulation Results:**
<img width="1291" height="628" alt="gtkwave_blocking_caveat" src="https://github.com/user-attachments/assets/fed4f7c3-7c70-4573-97ca-bbb9ed8a774b" />


---

### Lab 7: Synthesis of Blocking Assignment Module

**Objective:** Analyze synthesis results for blocking assignment correction

**Synthesis Analysis:**
- Compare synthesized logic for original vs. corrected code
- Verify proper combinational logic inference
- Examine optimization results

**Synthesis Results:**
<img width="771" height="531" alt="blocking_caveat" src="https://github.com/user-attachments/assets/98ff4f41-2b7b-4337-bcc9-dd4bd7a7b30e" />


---

### Lab 8: GLS for Blocking Caveat

**Objective:** Perform gate-level simulation on the blocking assignment module to verify post-synthesis behavior

**Simulation Setup:**
- Use synthesized netlist from Lab 7
- Apply same testbench used for RTL simulation
- Compare GLS waveforms with RTL simulation results

**Expected Results:**
- Gate-level simulation should match RTL behavior for corrected code
- Timing delays may be visible in GLS waveforms
- Functional equivalence verification between RTL and netlist

**GLS Simulation Command:**
```bash
iverilog <library_path>/primitives.v <library_path>/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
```

**GLS Waveform Results:**
<img width="1262" height="760" alt="gtkwave_blocking_caveat_GLS" src="https://github.com/user-attachments/assets/2f8c1af2-6497-495f-b728-ad0287fa7d1a" />


**Analysis Notes:**
- Compare signal transitions between RTL and GLS
- Verify correct logic implementation in gate-level netlist
- Document any observed timing differences

---

## Key Takeaways

### Best Practices for RTL Design

**Coding Guidelines:**
- Use blocking assignments for combinational logic
- Apply non-blocking assignments for sequential circuits
- Include comprehensive sensitivity lists
- Write clear and unambiguous code structures

**Verification Methodology:**
- Always perform both RTL and gate-level simulations
- Compare simulation results between design phases
- Address synthesis tool warnings promptly
- Validate timing requirements through annotated simulations

**Quality Assurance:**
- Follow established coding standards
- Implement proper reset mechanisms
- Use meaningful signal and variable names
- Document design intent clearly

### Common Pitfalls to Avoid

- Mixing blocking and non-blocking assignments inappropriately
- Creating incomplete sensitivity lists
- Using non-synthesizable constructs in RTL
- Ignoring synthesis tool warnings and errors
- Failing to verify gate-level simulation results

---

## Conclusion

This workshop demonstrated essential concepts in RTL design verification and proper Verilog coding practices. Understanding the differences between blocking and non-blocking assignments, recognizing synthesis-simulation mismatches, and implementing proper gate-level simulation methodologies are crucial skills for successful digital design projects.

Through hands-on laboratory exercises, we explored real-world scenarios and learned to identify and correct common coding errors that can lead to functional discrepancies between simulation and hardware implementation.

---

> **Important Note:** Always validate your designs through multiple simulation phases and carefully review synthesis reports to ensure reliable hardware implementation.
---
*"The path of the righteous designer is beset by timing violations and poor optimization."*

