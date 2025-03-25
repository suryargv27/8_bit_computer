# 8-Bit Computer using Logisim

## Overview
This project implements a simple 8-bit computer using Logisim, capable of executing basic arithmetic and logical operations. The computer is built using fundamental components such as an Arithmetic Logic Unit (ALU), a register file, a program counter, a control unit, and main memory. Instructions are 16-bit wide and follow a structured format for register and immediate-based operations.

## Instruction Format
Each instruction consists of 16 bits, structured as follows:
- **First Bit (1 bit):** Determines whether Source-2 uses immediate addressing (1) or direct register addressing (0).
- **Opcode (4 bits):** Specifies the ALU operation.
- **Source-1 (3 bits):** Selects the first source register.
- **Source-2 (3 bits) or Immediate Value (8 bits):** Either selects a register (when immediate addressing is not used) or holds an 8-bit immediate value.

```
Immediate  | Opcode  | Source-1  | Source-2 / Immediate
   1-bit    |  4-bit  |   3-bit    | 3-bit / 8-bit
```

## Instruction Set
The computer supports the following instructions:

| Opcode  | Instruction | Description  |
|---------|-------------|--------------|
| `0001`  | ADD        | Add two values |
| `0010`  | SUB        | Subtract two values |
| `0011`  | JMP        | Jump to a specified address |
| `0100`  | AND        | Bitwise AND operation |
| `0101`  | OR         | Bitwise OR operation |
| `0110`  | MUL        | Multiply two values |
| `0111`  | XOR        | Bitwise XOR operation |
| `1000`  | MOV        | Move data between registers |
| `1001`  | LOD        | Load a value from memory |
| `1010`  | STR        | Store a value to memory |
| `1011`  | NOT        | Bitwise NOT operation |
| `1100`  | DIV        | Divide two values |
| `1111`  | HLT        | Halt execution |

## Register File
The computer includes six general-purpose registers:
- `R1`, `R2`, `R3`, `R4`, `R5`, `R6`

Each register has a corresponding enable signal (`E1` to `E6`). Two multiplexers (`Mux A` and `Mux B`) are used to select the appropriate register values for computation based on `AddA` and `AddB` signals.

### Data Flow:
- `In_A` receives either the ALU output or an instruction from the Memory Data Register (MDR), depending on load/write signals.
- A demultiplexer (DMX) selects the target register for data storage.
- Another demultiplexer enables the corresponding register based on the Read/Write signal.

## Program Counter (PC)
The Program Counter stores the address of the next instruction to execute. It increments with every clock cycle. When a `JMP` instruction is executed, the program counter updates based on the provided jump offset, allowing control flow changes.

### PC Control Signals:
- `C7`: PC enable signal (goes high every clock cycle).
- An 8-bit full adder allows the PC to update either sequentially or via jump offset.

## Control Unit Signals
The control unit manages various signals required for instruction execution:

| Signal | Purpose |
|--------|---------|
| `C1`   | Selects upper 8 bits of instruction |
| `C2`   | Selects lower 8 bits of instruction |
| `C4`   | Load enable for memory read |
| `C5`   | Store enable for memory write |
| `C7`   | Enables program counter |

## Main Memory
The memory unit supports read and write operations. The Read/Write signal determines the operation mode.

### Memory Operations:
- **Mem Selection:** Determines whether fetched data is an address or an immediate value.
- **Write Operation:** Writes `DIN` value into the selected address.
- **Memory Data Register (MDR):** Holds fetched instruction or data before being transferred to registers.

## Main Clock and Read/Write Flags
The system uses a clock to synchronize all operations. The Program Counter Address (`PCA`) determines the next instruction location:
- `PCA`: Used when direct addressing is enabled.
- `PCA1`: Used when immediate addressing is enabled.
- A multiplexer selects between `PCA`, `PCA1`, or the Immediate value based on control signals `C1` and `C2`.

## Conclusion
This Logisim-based 8-bit computer provides a functional CPU capable of executing basic arithmetic, logical, and memory operations. The structured instruction set and control signals allow for efficient execution of programs with both direct register and immediate addressing modes. This project serves as an educational foundation for understanding CPU design and instruction processing.

---
### How to Use:
1. Load the Logisim circuit file.
2. Configure the input signals as per the desired instruction.
3. Step through clock cycles to observe data flow and execution.
4. Experiment with various instructions and memory operations.

---
### Future Improvements:
- Implement additional instructions such as conditional jumps.
- Expand memory addressing capabilities.
- Introduce pipelining for improved performance.

### Reference:
Refer to the `coa_prog.pdf` for a detailed explanation.
