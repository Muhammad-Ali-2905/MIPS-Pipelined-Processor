# 32-bit MIPS Pipelined Processor

A 32-bit MIPS-style pipelined processor implemented in **Verilog HDL** as a computer architecture project. The design demonstrates instruction pipelining, arithmetic and logical operations, memory access, control flow, and data hazard handling.

## Overview

The processor uses a five-stage pipeline to overlap the execution of multiple instructions:

1. **Instruction Fetch (IF):** Fetches instructions from instruction memory using the program counter.
2. **Instruction Decode (ID):** Decodes instructions, generates control signals, and reads source operands from the register file.
3. **Execute (EX):** Performs arithmetic and logical operations using the ALU and calculates branch conditions.
4. **Memory Access (MEM):** Performs data memory reads and writes for load and store instructions.
5. **Write Back (WB):** Writes the selected result back to the register file.

Pipeline registers carry data and control signals between successive stages.

## Features

- **32-bit datapath:** Processes 32-bit instructions' operands and results.
- **Five-stage pipeline:** Separates instruction execution into IF, ID, EX, MEM, and WB stages.
- **Arithmetic and logical operations:** Includes operations such as addition, subtraction, multiplication, AND, OR, and set-less-than.
- **Memory operations:** Supports load-word and store-word operations.
- **Control flow:** Implements conditional branching and jump instructions.
- **Data hazard handling:** Includes forwarding/bypassing paths and stall-based handling.
- **Configurable hazard handling:** The `enable_forwarding` input selects the forwarding-oriented or stall-oriented hazard-handling mode.
- **Separate instruction and data memories:** Uses dedicated instruction ROM and data RAM modules.
- **Modular RTL design:** Separates the datapath, control logic, ALU, register file, and pipeline registers into individual Verilog modules.

## Architecture

The processor follows this general instruction flow:

`IF → ID → EX → MEM → WB`

### Pipeline Registers

- **IF/ID:** Holds the fetched instruction and its associated program-counter value.
- **ID/EX:** Carries decoded operands, immediate values, register indices, and control signals into the execute stage.
- **EX/MEM:** Transfers execution results and memory-related control signals to the memory stage.
- **MEM/WB:** Carries the ALU result or memory-read data and destination-register information to write-back.

### Hazard Handling

Data dependencies between instructions can cause pipeline hazards.

- **Forwarding mode (`enable_forwarding = 1`):** Uses bypass paths to supply results from later pipeline stages to dependent instructions. Load-use dependencies are also handled with stall logic.
- **Stall mode (`enable_forwarding = 0`):** Uses the implemented stall logic to delay dependent instructions until their operands can be read correctly.

The design also includes branch-related pipeline flushing and jump control.

## Instruction Support

The included instruction memory demonstrates a subset of MIPS-style instructions, including:

| Category | Operations |
|---|---|
| Arithmetic | ADD, SUB, MULT |
| Logical | AND, OR |
| Comparison | SLT-style comparison |
| Immediate | Immediate arithmetic |
| Memory access | LW, SW |
| Branching | BEQ |
| Jump | J |

This is an educational implementation of a MIPS-style instruction subset, not a complete implementation of the entire standard MIPS ISA. Instruction encodings and behavior should be interpreted according to the project's RTL.

## Project Structure

| File | Purpose |
|---|---|
| `TopLevel_PP.v` | Top-level processor integration and datapath coordination |
| `PC_PP.v` | Program counter and next-instruction selection |
| `Decoder_PP.v` | Instruction field extraction |
| `Control_PP.v` | Main instruction control unit |
| `ALU_Control_PP.v` | ALU operation decoding |
| `ALU_PP.v` | Arithmetic and logical operations |
| `RegisterFile_PP.v` | 32-bit register file |
| `ROM_PP.v` | Instruction memory with a predefined instruction sequence |
| `RAM_PP.v` | Data memory for load and store operations |
| `IF_ID_Register.v` | IF/ID pipeline register |
| `ID_EX_Datapath.v` | ID/EX datapath pipeline register |
| `ID_EX_Control.v` | ID/EX control pipeline register |
| `EX_MEM_Datapath.v` | EX/MEM datapath pipeline register |
| `EX_MEM_Control.v` | EX/MEM control pipeline register |
| `MEM_WB_Datapath.v` | MEM/WB datapath pipeline register |
| `MEM_WB_Control.v` | MEM/WB control pipeline register |

## Simulation

The top-level module is `TopLevel_PP`.

### Inputs

- `clk`: Processor clock.
- `rst`: Reset signal.
- `enable_forwarding`: Selects the hazard-handling mode.

### Outputs

- `PC_Out`: Current program-counter value.
- `Instr`: Instruction fetched from instruction memory.
- `Rs_Val`: First ALU operand after operand selection and forwarding.
- `Rt_Val`: Second ALU operand after operand selection and forwarding.

To simulate the design:

1. Add all Verilog source files to a project in a simulator such as ModelSim or Questa.
2. Create a testbench that instantiates `TopLevel_PP` and generates a clock and reset sequence.
3. Set `enable_forwarding` to `1` or `0` to examine the two hazard-handling modes.
4. Observe the program counter, fetched instructions, operand values, and relevant internal pipeline signals in the waveform viewer.

The instruction sequence is initialized in `ROM_PP.v`, while initial data values are provided in `RAM_PP.v` and `RegisterFile_PP.v`.

## Technologies

- Verilog HDL
- Digital Logic Design
- Computer Architecture
- Instruction Pipelining
- Data Hazard Handling
- Vivado

## Learning Outcomes

This project explores how a pipelined processor is organized, how instructions progress through multiple execution stages, how control signals coordinate the datapath, and how data dependencies affect correct instruction execution.

