# 1. RV32I - Integer Instructions Set:
## 1.1. Encoding Variants:
  ![image](https://github.com/atfox272/risc-v-note/assets/99324602/2233dd47-3fd6-408b-9508-272c4309a2b6)

## 1.2. Integer Computional Instruction:
### 1.2.1. Immediate-Register:
	ADDI 		(1) 	(pg.18)
	SLTI 		(2) 	(pg.18)
	SLTIU 		(3) 	(not given)
	ANDI		(4) 	(pg.18)
	ORI		(5)	(pg.18)
	XORI		(6)	(pg.18)
	SLLI		(7)	(pg.18)
	SRLI		(8)	(pg.19)
	SRAI		(9)	(pg.19)
	LUI		(10) 	(pg.19)
	AUIPC		(11)	(pg.19)	
### 1.2.2. Register-Register:
	ADD		(12)	(pg.19)
	SUB		(13)	(pg.19)
 	SLT 		(14)	(not given)
	SLTU		(15)	(pg.20)
	AND		(16)	(pg.20)
	OR		(17)	(pg.20)
	XOR		(18)	(pg.20)
	SLL		(19)	(pg.20)
	SRL		(20)	(pg.20)
	SRA		(21)	(pg.20)
### 1.2.3. Exception
No integer computational instructions cause **arithmetic exceptions**. (We did not include special instruction-set support for overflow checks on integer arithmetic operations in the base instruction set, as many overflow checks can be cheaply implemented using RISC-V branches. More detail in pg.18,19)

## 1.3. Control Transfer
### 1.3.1. Unconditional Jumps
	JAL		(22)	(pg.20)
	JALR		(23)	(pg.21)
### 1.3.2. Conditional Branch
	BEQ		(24)	(pg.22)
	BNE		(25)	(pg.22)
	BLT		(26)	(pg.22)
	BLTU		(27)	(pg.22)
	BGE		(28)	(pg.22)
	BGEU		(29)	(pg.22)
 
### 1.3.3. Exception
The _unconditional jumps_ instructions will generate an instruction-address-misaligned exception if the target address is not aligned to a four-byte boundary.

The _conditional branch_ instructions will generate an instruction-address-misaligned exception if the target address is not aligned to a four-byte boundary and **the branch condition evaluates to true**. If the branch condition evaluates to false, the instruction-address-misaligned exception will not be raised.

## 1.4. Load and Store Instructions:
### 1.4.1.
	LW		(30)	(pg.24)
	LH		(31)	(pg.24)
	LHU		(32)	(pg.24)
	LB		(33)	(pg.24)
	LBU		(34)	(pg.24)
	SW		(35)	(pg.24)
	SH		(36)	(pg.24)
	SHU		(37)	(pg.24)
	SB		(38)	(pg.24)
	SBU		(39)	(pg.24)
### 1.4.2. Exception:
    Regardless of EEI (Execution Environment Interface), loads and stores whose effective addresses are naturally aligned shall not raise an 
    address-misaligned  exception. Loads and stores where the effective address is not naturally aligned to the referenced datatype (i.e., on 
    a four-byte boundary for 32-bit accesses, and a two-byte boundary for 16-bit accesses) have behavior dependent on the EEI.
    
    1. An EEI may guarantee that misaligned loads and stores are fully supported, and so the software running inside the execution 
    environment will never experience a contained or fatal address-misaligned trap. In this case, the misaligned loads and stores 
    can be handled in hardware, or via an invisible trap into the execution environment implementation, or possibly a combination 
    of hardware and invisible trap depending on address.
    
    2. An EEI may not guarantee misaligned loads and stores are handled invisibly. In this case, loads and stores that are not 
    naturally aligned may either complete execution successfully or raise an exception. The exception raised can be either an 
    address-misaligned exception or an access-fault exception. For a memory access that would otherwise be able to complete except 
    for the misalignment, an access exception can be raised instead of an address-misaligned exception if the misaligned access 
    should not be emulated, e.g., if accesses to the memory region have side effects. When an EEI does not guarantee misaligned loads 
    and stores are handled invisibly, the EEI must define if exceptions caused by address misalignment result in a contained trap 
    (allowing software running inside the execution environment to handle the trap) or a fatal trap (terminating execution).

## 1.5. Memory Ordering Instructions:
_(For multicore system => Shared memory => Consistency)_

	FENCE		(40)	(pg.26)

## 1.6. Environment Call and Breakpoint
### 1.6.1.
	ECALL		(41)	(pg.27)
	EBREAK		(42)	(pg.27)

### 1.6.2. Exception
These two instructions cause a precise requested trap to the supporting execution environment.

The _ECALL_ instruction is used to make a service request to the execution environment. The EEI will define how parameters for the service request are passed, but usually these will be in defined locations in the integer register file. (like syscall instruction)

The _EBREAK_ instruction is used to return control to a debugging environment (add breakpoint and trap program). "**EBREAK semihosting mode??**"

## 1.7. HINT Instructions 
(_pg.28_)

_Like the NOP instruction (rd = x0) that not change any invisible part of program, but may allow for more efficient execution_

	Example: A simple example might be for a processor to have some HINT instructions encoding "this loop will repeat n times". 
 	That way, the processors branch predictor can use this information to know when to not repeat anymore. 
  	Crucially, this is still just a hint (hence the name), so the processor still has to verify its guess later.

_&#8594; Can ignore the HINT instructions_

___

# 2. "Zifencei" Instruciton-Fetch Fence 
(_pg.31_)
	
 &#8594; For multicore or multi-hart (hartware thread)

____
# 3. RV32E - Integer Instruction Set
(_pg.33_)
	
 	This chapter describes a draft proposal for the RV32E base integer instruction set, which is a
	reduced version of RV32I designed for embedded systems. The only change is to reduce the number
	of integer registers to 16.

____

 # 4. RV64I - Integer Instruction Set:
 (_pg.35_)

 This chapter describes the RV64I base integer instruction set, which builds upon the RV32I variant described in [Chapter 1](#1-rv32i---integer-instructions-set)

## 4.1. Registers State 
RV64I widens the integer registers and supported user address space to 64 bits (XLEN = 64)

## 4.2. Integer Computional Instructions
_Additional instruction variants are provided to manipulate 32-bit values in RV64I, indicated by a ‘W’ suffix to the opcode. **These “*W” instructions ignore the upper 32 bits** of their inputs and always produce 32-bit signed values, i.e. bits XLEN-1 through 31 are equal._

### 4.2.1. Integer Register-Immidiate Instructions
	ADDI 		(1) 	(not given - pg.130,131)
	SLTI 		(2) 	(not given - pg.130,131)
	ANDI		(3) 	(not given - pg.130,131)
	ORI		(4)	(not given - pg.130,131)
	XORI		(5)	(not given - pg.130,131)
	ADDIW 		(6) 	(pg.36)
	SLLI		(7)	(pg.36)
	SRLI		(8)	(pg.36)
	SRAI		(9)	(pg.36)
	SLTI 		(10) 	(pg.36)
	SLLIW		(11)	(pg.36)
	SRLIW		(12)	(pg.36)
	SRAIW		(13)	(pg.36)
	LUI		(14) 	(pg.36)
	AUIPC		(15)	(pg.36)	

### 4.2.2. Integer Register-Register Instruction
	ADD		(11)	(not given - pg.130,131)
	SUB		(12)	(not given - pg.130,131)
	ADDW		(11)	(pg.37)
	SUBW		(12)	(pg.37)
	SLTU		(13)	(not given - pg.130,131)
	AND		(14)	(not given - pg.130,131)
	OR		(15)	(not given - pg.130,131)
	XOR		(16)	(not given - pg.130,131)
	SLL		(17)	(pg.37)
	SRL		(18)	(pg.37)
	SRA		(19)	(pg.37)
	SLLW		(17)	(pg.37)
	SRLW		(18)	(pg.37)
	SRAW		(19)	(pg.37)

### 4.2.3. Exception
(_Like RV32I_)

No integer computational instructions cause **arithmetic exceptions**. (We did not include special instruction-set support for overflow checks on integer arithmetic operations in the base instruction set, as many overflow checks can be cheaply implemented using RISC-V branches. More detail in pg.18,19)

## 4.3. Control Transfer
(_Like RV32I_)

### 4.3.1. Unconditional Jumps
	JAL		(20)	(not given - pg.130,131)
	JALR		(21)	(not given - pg.130,131)
### 4.3.2. Conditional Branch
	BEQ		(22)	(not given - pg.130,131)
	BNE		(23)	(not given - pg.130,131)
	BLT		(24)	(not given - pg.130,131)
	BLTU		(25)	(not given - pg.130,131)
	BGE		(26)	(not given - pg.130,131)
	BGEU		(27)	(not given - pg.130,131)
 
### 4.3.3. Exception
The _unconditional jumps_ instructions will generate an instruction-address-misaligned exception if the target address is not aligned to a four-byte boundary.

The _conditional branch_ instructions will generate an instruction-address-misaligned exception if the target address is not aligned to a four-byte boundary and **the branch condition evaluates to true**. If the branch condition evaluates to false, the instruction-address-misaligned exception will not be raised.

## 4.4. Load and Store Instructions:
### 4.4.1.
	LD		(28)	(pg.37,38)
	LW		(28)	(pg.37,38)
	LWU		(28)	(not given - pg.130,131)
	LH		(29)	(pg.37,38)
	LHU		(30)	(pg.37,38)
	LB		(31)	(pg.37,38)
	LBU		(32)	(pg.37,38)
	SD		(33)	(pg.37,38)
	SW		(33)	(pg.37,38)
	SH		(34)	(pg.37,38)
	SHU		(35)	(pg.37,38)
	SB		(36)	(pg.37,38)
	SBU		(37)	(pg.37,38)
### 4.4.2. Exception
(_Like RV32I_)

    Regardless of EEI (Execution Environment Interface), loads and stores whose effective addresses are naturally aligned shall not raise an 
    address-misaligned  exception. Loads and stores where the effective address is not naturally aligned to the referenced datatype (i.e., on 
    a four-byte boundary for 32-bit accesses, and a two-byte boundary for 16-bit accesses) have behavior dependent on the EEI.
    
    1. An EEI may guarantee that misaligned loads and stores are fully supported, and so the software running inside the execution 
    environment will never experience a contained or fatal address-misaligned trap. In this case, the misaligned loads and stores 
    can be handled in hardware, or via an invisible trap into the execution environment implementation, or possibly a combination 
    of hardware and invisible trap depending on address.
    
    2. An EEI may not guarantee misaligned loads and stores are handled invisibly. In this case, loads and stores that are not 
    naturally aligned may either complete execution successfully or raise an exception. The exception raised can be either an 
    address-misaligned exception or an access-fault exception. For a memory access that would otherwise be able to complete except 
    for the misalignment, an access exception can be raised instead of an address-misaligned exception if the misaligned access 
    should not be emulated, e.g., if accesses to the memory region have side effects. When an EEI does not guarantee misaligned loads 
    and stores are handled invisibly, the EEI must define if exceptions caused by address misalignment result in a contained trap 
    (allowing software running inside the execution environment to handle the trap) or a fatal trap (terminating execution).

## 4.5. Memory Ordering Instructions:
(_Like RV32I_)
_(For multicore system => Shared memory => Consistency)_

	FENCE		(38)	(not given - pg.130,131)

## 4.6. Environment Call and Breakpoint
(_Like RV32I_)
### 4.6.1.
	ECALL		(39)	(not given - pg.130,131)
	EBREAK		(40)	(not given - pg.130,131)
 
## 4.7. HINT Instructions 
(_Like RV32I_)

_Like the NOP instruction (rd = x0) that not change any invisible part of the program, but may allow for more efficient execution_

	Example: A simple example might be for a processor to have some HINT instructions encoding "this loop will repeat n times". 
 	That way, the processor's branch predictor can use this information to know when to not repeat anymore. 
  	Crucially, this is still just a hint (hence the name), so the processor still has to verify its guess later.

_&#8594; Can ignore the HINT instructions_


___


# 5. "M" Standard Extension for Integer Multiplication and Division (RV64M)
(_pg.43_)
## 5.1. Multiplication Operation:
### 5.1.1. Instructions:
	MUL		(43)	(pg.43)
	MULH		(43)	(pg.43)
	MULHU		(43)	(pg.43)
	MULHSU		(43)	(pg.43)
	MULW		(43)	(pg.43)

### 5.1.2. Special note:
	"source register (rs) specifiers must be in same order"
&#8594; Multiplication of signed and unsigned number

 	"rdh cannot be the same as rs1 or rs2"
&#8594; The result after each cycle will be directly **overwritten** to _rdh_

## 5.2. Division Operations 
### 5.2.1. Instructions
	DIV		(43)	(pg.44)
	DIVU		(43)	(pg.44)
	REM		(43)	(pg.44)
	REMU		(43)	(pg.44)
	DIVW		(43)	(pg.44)
	DIVUW		(43)	(pg.44)
	REMW		(43)	(pg.44)
	REMUW		(43)	(pg.44)
### 5.2.2. Exception or Not Exception
	We considered raising exceptions on integer divide by zero, with these exceptions causing a trap in
	most execution environments. However, this would be the only arithmetic trap in the standard
	ISA (floating-point exceptions set flags and write default values, but do not cause traps) and
	would require language implementors to interact with the execution environment’s trap handlers
	for this case. Further, where language standards mandate that a divide-by-zero exception must
	cause an immediate control flow change, only a single branch instruction needs to be added to
	each divide operation, and this branch instruction can be inserted after the divide and should
	normally be very predictably not taken, adding little runtime overhead.
	The value of all bits set is returned for both unsigned and signed divide by zero to simplify
	the divider circuitry. The value of all 1s is both the natural value to return for unsigned divide,
	representing the largest unsigned number, and also the natural result for simple unsigned divider
	implementations. Signed division is often implemented using an unsigned division circuit and
	specifying the same overflow result simplifies the hardware.
	
  ![image](https://github.com/atfox272/risc-v-note/assets/99324602/a71a8351-69be-473f-89cb-92854caa4500)


___

# 6. “A” Standard Extension for Atomic Instructions

&#8594; For multiple hart (hardware thread)

