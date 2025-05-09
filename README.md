
# Hardware/Software Co-Design Project Report  
**6-Bit Processor Design with VHDL and Assembler**  

---

## 📌 Overview  
A 6-bit processor designed in **VHDL** with a custom **Python assembler**, supporting:  
- **Base Instructions**: `LOAD`, `ADD`, `SUB`, `JNZ`  
- **Extended Instruction**: `MULT` (hardware multiplication)  
- **Tools**:  
  - *Simulation*: Functional verification via testbenches.  
  - *Assembler*: Python script for assembly-to-binary conversion.  

**Deadline**: 15th Tir 1403 (5th July 2024).  

---

## 🛠️ Project Phases  

### Phase 1: Core Processor (40%)  
**Objective**: Validate basic operations (addition/subtraction).  

#### Key Components  
- **Registers**: `R0`-`R3`, `PC` (Program Counter), `IR` (Instruction Register).  
- **ALU**: 2-bit `CMD` (`00`=ADD, `01`=SUB).  
- **Control Unit**: Finite State Machine (FSM) with states `S0` (Reset) to `S6` (Jump).  

#### Test Case  
1. Load values `7` and `4` into registers.  
2. Add them.  
3. **Expected**: `R0 = 11` (binary `001011`).  

---

### Phase 2: Software Multiplication (20%)  
**Objective**: Multiply via repeated addition (no hardware support).  

#### Assembly Workflow  
1. Initialize registers:  
   - `R0 = 6`, `R1 = 8` (operands), `R2 = 1` (decrement), `R3 = 0` (result).  
2. Loop:  
   - Add `R0` to `R3` (`R3 += R0`).  
   - Decrement `R1` until zero (`JNZ` loop).  
3. **Result**: `R3 = 48` (binary `110000`).  

---

### Phase 3: Hardware Multiplication (40%)  
**Objective**: Extend ALU to support `MULT` instruction.  

#### Modifications  
- **ALU Upgrade**: Added `CMD = "10"` for multiplication.  
- **Opcode Expansion**: 3-bit opcodes (`100` = `MULT`).  
- **FSM State**: New state `S8` to handle `MULT`.  

#### Test Case  
1. Load `6` and `8` into registers.  
2. Execute `MULT R0, R1`.  
3. **Result**: `R0 = 48` (verified in simulation).  

---

## 🎁 Bonus: Python Assembler  
**Features**:  
- Translates assembly to 6-bit binary.  
- Supports labels (e.g., `L0:` for loops).  
- Auto-calculates jump addresses.  

#### Example Output  
```plaintext
000000  ; LOAD R0, 6
000110  
000100  ; LOAD R1, 8
001000  
100000  ; MULT R0, R1
111111  ; HALT
