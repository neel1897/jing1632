# jing1632
Jing1632 is a functional 32-bit computer which can perform basic Arithmetic and Logic operations, Shifting operations and Branching operations. It also comes with a low power 16-bit mode that does away with the Shifting and branching operations... It's just a hobby project :)

The objective of this project is to design an efficient 32-bit Processor. We are able to achieve a lower Power Constraint compared to the RV32-I Processor by up to 50% by incorporating a low power mode by designing a 16-bit Processor into the 32-bit processor and effectively switching off half the transistors in the system. 

By enabling the user to switch between a low power 16-bit processor and a more powerful 32-bit processor, we provide versatility in various applications. While power constraints are low and the processor is being used in remote applications with low data processing, the computer can be used as a 16-bit processor. When there is a higher threshold for power and the demand for data-processing is high, by enabling the 32-bit pin, we can access the full power of the processor, and it can be used in applications where there is constant source of power such as a desktop.

Data Memory:
The data memory is essentially a RAM module consisting of 1K Registers each of 32-bit width length. This module contains four inputs and a bi-directional 32-bit bus for data transfer between the Register Bank and the RAM Module. There exists a 10-bit address bus to address the registers in memory for read and write purpose. A clock input is given connected to the central clock of the System for synchronized data read or write in each clock cycle. A Load and Store bit exists to allow the Loading of data from memory (read) or storing of data back to memory (write) functions. These bits are controlled by the control unit of the CPU. Controlled buffers and 16-bit Splitters are used to separate the lower 16 bits from the upper 16 bits during low power mode. They also help choose whether the bi-directional bus is used for reading or writing at any given time. The signals to these controlled buffers are controlled by the control unit of the system.

Program Counter:
The Program Counter module is a 32-bit counter. Although the width of the counter supports 32 bits, the computer itself has the capability to support only 16 bits to address the instruction from the instruction memory. The counter comes with a reset pin, which when set to high, the counter sets value to zero. The counter uses an increment pin and will increment to the address of the next instruction in subsequent clock cycles, only when the increment pin is set to high. Since the computer is capable of branching instructions, the counter also supports a load pin and a load value which can change the instruction to which the program counter points. These pins are controlled by the control unit of the computer. 

Register Bank:
The Register Bank of the computer consists of 8 general purpose registers. The implementation we follow includes the load and store architecture, i.e., any data that has to be processed by the computer’s ALU or Barrel Shifter must first be stored in the 8 dedicated general-purpose registers provided in the register bank of the computer. Data can be stored in the register bank directly from Data Memory or as a subsequent output of the ALU. Data can then be read back into the Data Memory to overwrite the General-Purpose Registers. The module consists of 3 Address inputs, each are 3-bit wide to allow the user to choose the address of Register-A (Operand 1: ALU), Register-B (Operand 2: ALU) and Register-D (Destination Register). Values can be written into these registers when the control pin Write is set to high. This is done through the control unit of the computer. In addition, a 32-bit data bus is provided as input to the Register bank to store a 32-bit value into one of the eight registers. At any given point of time, we can access the values of 2 Registers of the register bank by providing their respective addresses and making use of the two output data busses. 

Barrel Shifter:
The Barrel Shifter module helps perform shifting operations on registers. This module can be accessed by the CPU only while running it in it’s 32-bit mode. It has the ability to perform:\n
•	LSL: Logical Shift Left\n
•	LSR: Logical Shift Right\n
•	ASR: Arithmetic Shift Right\n
•	ROL: Rotate Left\n
•	ROR: Rotate Right\n
The type of shift is controlled by a 2-bit function input to the Barrel Shifter. The input data bus for the Shifter is 32-bit wide and comes as the second register output of the Register Bank. This design is in congruence with existing RISC-V and ARM architectures, where only the second operand to the ALU has the ability to go through a shift operation. These shifting operations make it faster for the computer to perform more repetitive operations such as multiplication and division. The output Data bus of the barrel shifter is then connected to the operand 2 input of the ALU.\n

Arithmetic and Logic Unit – ALU:
The ALU module is the heart of the computer and is responsible for executing all Arithmetic and Logical Instructions. It has similar instructions in both 16-bit and the larger 32-bit modes. The only difference is the data bus size reduces in the 16-bit state, thereby saving power by turning off the circuit for the upper 16-bit state. The ALU can perform the following operations based on a 4-bit opcode given to it as an input:

•	ADD: Addition

•	SUB: Subtraction\n

•	RSB: Reverse Subtraction

•	AND: Bitwise-And

•	ORR: Bitwise-Or

•	BIC: Bit-Clear

•	EOR: Exclusive Or

•	MOV: Move

•	MVN: Move Negate

There are 2 input data bus lines to the ALU, which can be configured to either a 16-bit state or a 32-bit state. The state control is done by the Enable32 pin. Based on the mode of the processor, the output data bus is also configured to the same length as input data bus. The ALU also has configurations to display 3 flag bits. This enables the user to perform conditional branching instructions in the 32-bit state. The flags are:

•	N: Negative Output

•	Z: Zero Output

•	C: Carry generated

The flag bits help determine the type of output produced by the ALU.

Instruction Memory:
The Instruction memory is a 32-bit wide memory with a 16-bit address space. It is effectively a ROM module containing of instructions encoded using the encoding table provided (Refer Appendix). The instruction memory can also be read as 16-bit instructions, or 32-bit instructions based on the Enable32 pin. The address pointing to the instruction is provided by the program counter, and the resultant instruction is then read into a parallel instruction register for decoding and executing the instruction. The Jing1632 computer follows the Harvard Architecture, where the data and instruction are stored in separate memory. This allows for parallel access of data and instructions and increases the overall speed of the computer without increasing other parameters such as clock frequency. The module contains only 2 I/O pins, a 16-bit wide address pin and a 32-bit wide instruction pin. This module can vary from computer to computer based on the various technologies used for ROM in modern computers and will not be discussed in detail while understanding lower levels of abstraction.

Control Unit:
The final piece that ties the entire computer together is the Control Unit. It is effectively just an instruction register. Based on the mode of the computer, the instruction register will hold either 16-bit short instructions or the full 32-bit length instructions. These instructions are then broken up using a 32-bit splitter to decode each bit. Some of the bits are used as address bits for the Register Banks, control signals for Read/Write in data memory, program counter and other modules of the computer. This can be achieved using combiners, multiplexers, controlled buffers, tunnels and other gate level elements. To understand how each instruction is decoded in it’s 16-bit state or it’s 32-bit state, refer the Instruction Decode Tables present in the Appendix Section.

How to encode the instructions for Jing1632?
Here is the encoding data sheet for Enable32 = Low:
![image](https://github.com/neel1897/jing1632/assets/88762634/c165c77e-a3de-48e3-ad38-6796c47f7b19)

