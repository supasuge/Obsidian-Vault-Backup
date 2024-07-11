## Table of Contents

      - [Opcodes](#Opcodes)
  - [Types of Operands](#Types\of\Operands)
- [General instructions](#general\instructions)
    - [**The LEA Instruction**](#**The\LEA\Instruction**)
    - [**The NOP Instruction**](#**The NOP Instruction**)
    - [**Shift Instructions**](#**Shift\Instructions**)

byte - 8 bits (hex)
Opcodes and operancds denote the hex for actual operations, and operands are the registers or memory locations which the operationsa performed.


#### Opcodes
numbers that correspond to instructions performed by the CPU. 

Disassemblers - tranlates opcodes to assembly

For example, the instruction for moving 0x5F to the eax register is:

`mov eax, 0x5f`
When looking at it in a disassembler, we will see:

`040000:    b8 5f 00 00 00    mov eax, 0x5f`

Here, the `040000:` corresponds to the address where the instruction is located. `b8` refers to the opcode of the instruction `mov eax`, and `5F 00 00 00` indicates the other operand `0x5f`. Please note that due to [endianness](https://en.wikipedia.org/wiki/Endianness), the operand 0x5f is written as `5f 00 00 00`, which is actually `00 00 00 5f` but in little-endian notation. Similarly, there is an opcode for each instruction in the assembly language. There are [references](http://ref.x86asm.net/index.html) for converting opcodes into assembly instructions. Still, unless we are writing a disassembler, we will not need them, as a disassembler does that work pretty well. However, it is good to understand what is happening under the hood for a better picture overall.

## Types of Operands

In general, there are three types of operands in the assembly language.

- _Immediate Operands_ can also be considered constants. These are fixed values like we had the `0x5f` in the above example.
- _Registers_ can also be operands. The above example shows `eax` as a register where the immediate operand is stored.
- _Memory operands_ are denoted by square brackets, and they reference memory locations. For example, if we see `[eax]` as an operand, it will mean that the value in eax is the memory location on which the operation has to be performed.


# General instructions

**The MOV Instruction**

The mov instruction moves a value from one location to another. Its syntax is as follows:  
`mov destination, source`

The mov instruction can move a fixed value to a register, a register to another register, and a value in a memory location to a register. The following examples will help explain.

The following instruction copies a fixed value to a register. In this particular instruction, 0x5f is being moved to eax:  
`mov eax, 0x5f`

In this particular case, the value stored in eax is being moved to ebx:  
`mov ebx, eax`

The following instruction copies the value stored in a memory location to a register:  
`mov eax, [0x5fc53e]`

As seen above, we use square brackets when referencing memory. Similarly, suppose we see a register in square brackets. In that case, that will mean that the value in that register will be treated as a memory location, and the value in that memory location will be moved to the destination. This means that the example `mov eax, [0x5fc3e]` and the below example will have the same result.  
`mov ebx, 0x5fc53e   mov eax, [ebx]`

We can use the mov instruction to perform arithmetic calculations when referencing memory addresses. For example, the below instruction calculates ebp+4 (adding an offset of 4 bytes to the memory location) and moves the value in the resulting memory address into eax:  
`mov eax, [ebp+4]`

### **The LEA Instruction**

The lea instruction stands for "load effective address." The format of this instruction is as follows:  
`lea destination, source`

While the mov instruction moves the data at the source memory address to the destination, the lea instruction moves the address of the source into the destination. In the example below, the ebp value will be increased by four and moved to eax. However, if we had used a mov instruction here instead of lea, it would have moved the value in the memory location ebp+4.  
`lea eax, [ebp+4]`

Here, we can notice that we have performed an arithmetic operation on a register and saved the result in another register using a single instruction. The `lea` instruction is often used by compilers not for referencing memory locations but so that an arithmetic operation is performed on a register and saved to another using a single instruction. This is true, especially if the arithmetic operations are more complex, like adding and multiplying in a single instruction. As we will see in the next task, using arithmetic operations for this operation will need several instructions.

### **The NOP Instruction**

The nop instruction stands for no operation. This instruction exchanges the value in eax with itself, resulting in no meaningful operation. Hence, the execution moves to the next instruction without changing anything. The nop instructions are used for consuming CPU cycles while waiting for an operation or other such purposes. It has the following syntax:  
`nop`

The nop instruction is used by malware authors when redirecting execution to their shellcode. The exact location where the execution will redirect is often unknown, so the malware author uses a bunch of nop instructions to ensure that the shellcode execution does not start from the middle. This padding of nop instructions is called a nop sled.

### **Shift Instructions**

The CPU uses shift instructions to shift each register bit to the adjacent bit. There are two shift instructions for shifting either to the right or left. The shift instructions have the following syntax:  
`shr destination, count   shl destination, count`

In x86 assembly language, the CPU has several flags that indicate the outcome of certain operations or conditions. These flags are bits in a special register known as the **flags register** or **EFLAGS** register. Each flag represents a specific condition or result of the most recent arithmetic or logical operation. Here’s a table with the most common flags in x86 assembly and their explanations:

|Flag|Abbreviation|Explanation|
|---|---|---|
|Carry|CF|Set when a carry-out or borrow is required from the most significant bit in an arithmetic operation. Also used for bit-wise shifting operations.|
|Parity|PF|Set if the least significant byte of the result contains an even number of 1 bits.|
|Auxiliary|AF|Set if a carry-out or borrow is required from bit 3 to bit 4 in an arithmetic operation (BCD arithmetic).|
|Zero|ZF|Set if the result of the operation is zero.|
|Sign|SF|Set if the result of the operation is negative (i.e., the most significant bit is 1).|
|Overflow|OF|Set if there's a signed arithmetic overflow (e.g., adding two positive numbers and getting a negative result or vice versa).|
|Direction|DF|Determines the direction for string processing instructions. If DF=0, the string is processed forward; if DF=1, the string is processed backward.|
|Interrupt Enable|IF|If set (1), it enables maskable hardware interrupts. If cleared (0), interrupts are disabled.|




