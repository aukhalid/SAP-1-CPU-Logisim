# Simple-As-Possible (SAP-1) CPU in Logisim Evolution

## Project Overview

This repository contains the **Logisim Evolution** implementation of a Simple-As-Possible (SAP-1) CPU. The SAP-1 is a foundational computer architecture used to teach the basic principles of CPU design, including the data path, control unit, and instruction set architecture.

Our implementation features a fully functional **hardwired control unit**, which automates the fetch-decode-execute cycle, allowing the CPU to run machine code programs autonomously. The project culminates in successfully executing a simple addition program, loading two pre-defined 8-bit values, adding them, and storing the sum in memory.

---

## SAP-1 Architecture Components

The SAP-1 CPU is composed of several fundamental building blocks:

* **Program Counter (PC):** A 4-bit counter that stores the memory address of the next instruction to be executed. It increments automatically after each instruction fetch.

    ![Program Counter](pc_component.png)

* **Random Access Memory (RAM):** An 8-bit wide memory unit used to store both machine code instructions and data. Our implementation uses a 16-byte RAM.

    ![RAM](ram_component.png)

* **Memory Address Register (MAR):** A 4-bit register that holds the address of the memory location currently being accessed (for reading or writing).

* **Instruction Register (IR):** An 8-bit register that temporarily holds the instruction fetched from RAM. It's split into a 4-bit opcode and a 4-bit operand (memory address).

* **Registers A & B (Accumulator & B-Register):** 8-bit general-purpose registers. Register A (Accumulator) is typically used for arithmetic operations and storing results. Register B holds the second operand for ALU operations.

    ![Registers A & B](register_a_b_components.png)

* **Arithmetic Logic Unit (ALU):** An 8-bit unit capable of performing basic arithmetic (addition, subtraction) and logical operations on data from Registers A and B.

    ![ALU](alu_component.png)

* **Output Register:** (Implicit in SAP-1, often just Register A or a direct output).

* **Control Unit:** The "brain" of the CPU. It generates the necessary control signals (pin activations) at the correct time to sequence the micro-operations for fetching, decoding, and executing instructions. This project focuses heavily on its hardwired implementation.

    ![Main Control Unit](control_unit_main.png)

---

## The Hardwired Control Unit

The control unit is implemented using combinational logic (AND, OR, NOT gates) and a state counter (often called a ring counter in SAP-1 context). It orchestrates the entire CPU operation.

### Sub-Components of the Control Unit:

* **State Counter (RC):** A 3-bit counter that cycles through T-states (T1, T2, T3, T4, T5, T6). Each T-state represents a distinct phase within an instruction cycle.

    ![State Counter](state_counter_rc.png)

* **Opcode Decoder:** A 4-to-16 decoder connected to the most significant 4 bits (opcode) of the Instruction Register. It generates a unique HIGH signal for each recognized instruction (e.g., `isLDA`, `isADD`, `isHLT`).

* **Control Matrix (Logic Gates):** This is the network of AND and OR gates that takes the T-state signals from the State Counter and the instruction signals from the Opcode Decoder as inputs. Its outputs are the various control pins that govern data flow and operations across the CPU.

    ![Control Matrix Logic](control_unit_logic.png)

### Boolean Logic for Control Pins

The following Boolean equations define when each control pin is activated (goes HIGH). These are implemented directly using AND and OR gates in the Control Matrix. `cpu_mode` is `NOT(debug)`, ensuring automated operation only when `debug` is OFF.
