
### Organization of a Computer

The computer is built of various things

- Memory is a big long series of bytes

Bus is a group of wires assigned on purpose.
Address Bus: purpose is to tell the memory what the adress is.

Data bus: bus on which processor and memory communicate.

PC: program counter
Instruction register: Inside this will be decoded and operation will be carried out.

ALU: arithmetic and logical unit, actually does the operations.

RISC-V follows 3 operand notation as studied in [[Main - CMPUT 229]].

add a, b, c is a <- b+c

addi: add immediate
an immediate means in the assembly text we are writing an actual number.

In assembly we write registers. 

sub - subtraction

risc-v does operations one at a time

mapping - how does processor find values
assumptions such as f is in s0 and g is in s1

> RISC-V has 32, 32-bit register file

Register is used for frequently accessed data.
Register access is fast
Memory access is slow

Registers are numberted from 0 to 31.
A "word" is formed by 32 bits. Size of data easiest for computer to work with.

#### Assembler names

t0,t1 .. t6 for termporary variables
s0,s1, .. s11 for saved variables

#### Addressing an integer array

Uses 0-based indexing

Array is homogenous data list
First address of array is called the base of array.

say we use word sized integer -> 4 bytes to an integer

RISC V is a little endian architecture
- Least significant bytes at the lowest address.

How to move from memory to processor: 

"load word" loads from specified memory location into a a registor in the processor
"store word" from a register into a specified memory location

- only one memory operand (restriction in risc v), can't do memory to memory

#### Addressing Memory

```
A[0] = h + A[2]

Assumptions:
h <> s2
A <> s1
```

This is in C
This in Assembly would look like

```
lw t0, 8(s1) 
add t0, s2, t0
sw t0, 0(s1)
```

Here we use **8(s1)** to specify that we need word 3 from Array 0,4,8.

There is no subtraction immediate

Design Principle 3: make the common case fast
small constants are common

Memory operands

Main memory used for composite data like an array or string or a struct.
String is an array of characters.

Memory is byte addressed

Can't do aligned load

Q. Why is register access faster than memory access?
A.

Control Bus: 
Carries read signal 

Memory Controller

> Instructions also have addresses

### Lecture 4: Binary Representation

- machine code
- opcodes
- few formats

#### R-Type 

takes 3 register operands to perform some operation

```
add t0, s7, s5 
	x5, x23, x21
```

|funct7|rs2|rs1|funct3|rd|opcode|
|---|---|---|---|---|---|
|0x00|21|23|0x00|5|0x33|

| Field  | Description                 |
|--------|-----------------------------|
| **opcode** | Basic operation of the instruction |
| rd     | Destination register        |
| funct3 | Additional opcode           |
| rs1    | First register source operand |
| rs2    | Second register source operand |
| funct7 | Additional opcode           |

rd, rs1, rs2 are 5 bits as there are 32 registers. 2^5

assembler converts assembly to binary

#### I-Type

2 registers
instructions with immediates.

```
lw t0, 23, s3
```

|immediate|rs1|func3|rd|opcode|
|---|---|---|---|---|
|23|19|0x2|5|0x03|


#### S-Type

2 registers

```
sw t0, 1200(t1)
	x5,     x6
```

|immediate[11:5]|rs2|rs1|func3|imm[4:0]|opcode|
|---|---|---|---|---|---|
|37|5|6|0x2|16|0x23|


#### U-type

used for bigger immediates than 12 bits

```
lui t0, 0x0F008

load upper immediate
```

### Lecture 5: Instructions for Making Decisions

#### Branches 

```C
if (i==j)
	f= g + :
else
	f = g - hl 
```

```
bne s3, s4 Subtract
add s0, s1, s2
jal zero, Exit

Subtract: sub s0, s1, s2
Exit: 
```

|immediate[12\|10:5]|rs2|rs1|func3|imm[4:1\|11]|opcode|
|---|---|---|---|---|---|
|0\|0x00|20|19|0x1|0x6\|0|0x63|

#### Jump

Unconditional jump to an address

Distance with a branch is limited 
Jump can go farther

#### Jump and Link

Unconditional jump to first instruction of another function

Must record, in a register, the return address

RISC V does not have a jump and instruction. It uses jal for a jump 
Records the return address in x0 as x0 is always 0

Register zero is the constant 0. Hardwired to zero and cannot be overwritten.
Used for : 
- moving between registers
- setting a variable to 0
- two ways to perform an unconditional jump

#### Jump Instructions: UJ-Type


|immediate[20\|10:1\|11\|19:12]|rd|opcode|
|---|---|---|


## Logic Instructions in RISC V

#### Shift left logical immediate

```
slli t2, s0, 4
```

#### Shift left logical