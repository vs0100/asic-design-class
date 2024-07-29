# asic-design-class
LAB1

Task1: Create a C program and verify it using gcc compiler.
STEPS:

1.Create a file in editor in Linux environment.

![Screenshot from 2024-07-17 07-29-28](https://github.com/user-attachments/assets/7a9b5721-a2bd-4a11-9e49-fc43695f4ab9)

2.Compile code in termianl window using gcc compiler.

![Screenshot from 2024-07-17 07-27-37](https://github.com/user-attachments/assets/5ca86dc0-89c1-49e0-9e10-27c8faebe04b)

3.Executables will be generated after compilation,to run and check the output use the following command. 

![Screenshot from 2024-07-17 07-30-16](https://github.com/user-attachments/assets/6a38e971-0fe9-43e3-a660-22dc143eace3)

INITIAL CODE AND FINAL OUTPUT

![Screenshot from 2024-07-17 07-49-06](https://github.com/user-attachments/assets/90c15770-1cf6-4309-8d82-b1fe1c4ceddb)




Task2: Run the C program on a Riscv gcc compiler.

STEPS:

1.Compile the C code on Riscv compier using the following command.

![Screenshot from 2024-07-17 08-02-18](https://github.com/user-attachments/assets/ff557bf6-3b51-47cb-8c23-c0481ea32ea7)

2.Generate a new executable file.

![Screenshot from 2024-07-17 08-03-33](https://github.com/user-attachments/assets/7b8710da-989f-425d-b248-190ead3a1b50)

3.To look at the assembly code for the given C program use the following command.

![Screenshot from 2024-07-17 08-04-50](https://github.com/user-attachments/assets/5d98c9db-574c-4fe8-a384-48b2466ffdef)

4.To view some part of file at once use the following command and search for main to know the number of bytes used for its instruction set.

![Screenshot from 2024-07-17 08-06-05](https://github.com/user-attachments/assets/8fec103c-9ba3-4721-bf13-4e192687e7a2)

5.Now, instead of O1 in command we use Ofast so that we can reduce the number of bytes used by the main program.

![Screenshot from 2024-07-17 08-07-17](https://github.com/user-attachments/assets/df071282-7ac7-40b7-b362-6582d1172cf1)

6.Generate the assembly code for the above file and notice that the number of bytes used gets reduced to 12 instead of 15.
![Screenshot from 2024-07-17 08-08-52](https://github.com/user-attachments/assets/45e90a05-96fb-4614-b58b-e9a28c376b71)


INITIAL AND FINAL OUTPUT
![Screenshot from 2024-07-17 08-11-55](https://github.com/user-attachments/assets/60c5e1ee-1179-4bec-8dd8-0b661dbd5b32)




# LAB2


In Lab1 we compiled and executed our C code on gcc compiler, also in the second task we compiled our code on riscv compiler.
In this lab we will compile, execute and debug C code using riscv Simulator in Spike Simulation environment

## STEPS:


1.We executed our code using gcc compiler and spike simulator to check if both have the same result.
  To compile code in Spike Simultor we use the following command:
  
  ```bash
  riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c 
  ```
  
  To run code in Spike Simulator we use the following command:
  
 ```bash 
 spike pk sum1ton.o
  ```
  
  Now we compared our gcc output and riscv output wich came out same.
  
  ![Screenshot from 2024-07-22 18-23-05](https://github.com/user-attachments/assets/a08930f4-1a75-44ac-bba4-e6cdda465dbd)

2.We tried to debug our code using spike by using the following command:

```bash   
spike -d pk sum1ton.o
 ```  
   
   Complete assembly code is shown into the terminal
   
  ![Screenshot from 2024-07-22 18-25-16](https://github.com/user-attachments/assets/b7d1e1a6-9168-40d7-bc47-4885d6780c14)


3.We want debug only the main part of the code manually so all the part which was before that we executed using the command
```bash
 until pc 0 100b0
```

  ![Screenshot from 2024-07-22 18-26-25](https://github.com/user-attachments/assets/99f7e1c1-9008-4726-98fa-544d167cbc03)
  

4.After this we wanted to run each step manually and noting down the values which are changing in each registers.
  First, we checked the values of register a2 by using the command
  ```bash
    reg 0 a2
  ```
  
  Initially it had 0x00000000000000b0
  After this we executed one line of code by pressing Enter so that the next line gets executed.
  The next was lui a2 0x1 which load the upper immediate that is the the bits from 12 to 31 with value 1 i.e, 0x00000000000100b0
  Again we checked the value of the register which now contained the changed value.
  
  ![Screenshot from 2024-07-22 18-27-34](https://github.com/user-attachments/assets/172905d9-82e6-4cf6-9db7-b71a060c78e8)

5.After this in the next instruction we loaded register a0 with value 0x21 and checked it.

 ![Screenshot from 2024-07-22 18-28-29](https://github.com/user-attachments/assets/31f2fb08-e19e-4642-87c2-1e88f6ada302)


6.The next instruction decremented the value present in the stack pointer by 16(in decimal) which is 10 in binary.
  Initially it had 0x0000003ffffffb50

After the execution of instruction it had 0x0000003ffffffb40

![Screenshot from 2024-07-22 18-33-49](https://github.com/user-attachments/assets/8261d937-119d-425e-a719-263faf653f87)

We can do the same for other instructions as well.




# **LAB 3**

## To identify the type of instructions in RISC-V as R, I, S, B, U, J type, understand the differences between manually coding 32-bit pattern of instructions and when the instructions are executed using RISC-V ISA.

### Different Types of Instructions in RISC-V

Instructions are classified in RISC-V based on their formats and functionality:

#### 1. R-Type (Register)
Used for arithmetic and logical operations where all operands are registers.  
Example: ADD, SUB, AND, OR, XOR, SLL, SRL, SLT.  
Format:

- **func7**  
  - Size: 7 bits  
  - Location: Bits 25 to 31 of the instruction  
  - Description: Differentiates between instructions within the same opcode and funct3.  

- **rs2 (Source Register 2)**  
  - Size: 5 bits  
  - Location: Bits 20 to 24 of the instruction  
  - Description: Specifies the second source register.  

- **rs1 (Source Register 1)**  
  - Size: 5 bits  
  - Location: Bits 15 to 19 of the instruction  
  - Description: Specifies the first source register.  

- **funct3**  
  - Size: 3 bits  
  - Location: Bits 12 to 14 of the instruction  
  - Description: Provides mid-level differentiation within the general category specified by the opcode.  

- **rd (Destination Register)**  
  - Size: 5 bits  
  - Location: Bits 7 to 11 of the instruction  
  - Description: Specifies the destination register.  

- **opcode**  
  - Size: 7 bits  
  - Location: Bits 0 to 6 of the instruction  
  - Description: Specifies the general operation category (e.g., arithmetic, logical, load, store, etc.).  

#### 2. I-Type (Immediate)  
Used for operations involving an immediate value.  
Example: ADDI, SLTI, ANDI, ORI, XORI, LW.  
Format:

- **opcode**  
  - Size: 7 bits  
  - Location: Bits 0 to 6 of the instruction.  
  - Description: Specifies the general operation category (e.g., arithmetic with immediate, load, etc.).  

- **rd**  
  - Size: 5 bits  
  - Location: Bits 7 to 11 of the instruction  
  - Description: Specifies the destination register.  

- **funct3**  
  - Size: 3 bits  
  - Location: Bits 12 to 14 of the instruction  
  - Description: Provides mid-level differentiation within the opcode category.  

- **rs1 (Source Register 1)**  
  - Size: 5 bits  
  - Location: Bits 15 to 19 of the instruction  
  - Description: Specifies the first source register.  

- **imm (Immediate Value)**  
  - Size: 12 bits  
  - Location: Bits 20 to 31 of the instruction  
  - Description: Represents a 12-bit immediate value used in the operation.  

#### 3. S-Type  
Used for storing instructions where data from a register is stored into memory.  
Format:

- **opcode**  
  - Size: 7 bits  
  - Location: Bits 0 to 6 of the instruction  
  - Description: Identifies the type of operation (e.g., store).  

- **imm (Immediate Value, lower 5 bits)**  
  - Size: 5 bits  
  - Location: Bits 7 to 11 of the instruction  
  - Description: Represents the lower 5 bits of the immediate value for address calculation.  

- **funct3**  
  - Size: 3 bits  
  - Location: Bits 12 to 14 of the instruction  
  - Description: Specifies the specific type of store operation.  

- **rs1 (Source Register 1)**  
  - Size: 5 bits  
  - Location: Bits 15 to 19 of the instruction  
  - Description: Specifies the base register for the store operation.  

- **rs2 (Source Register 2)**  
  - Size: 5 bits  
  - Location: Bits 20 to 24 of the instruction  
  - Description: Specifies the source register holding the data to be stored.  

- **imm (Immediate Value, upper 7 bits)**  
  - Size: 7 bits  
  - Location: Bits 25 to 31 of the instruction  
  - Description: Represents the upper 7 bits of the immediate value for address calculation.  

#### 4. B-Type  
Used for conditional branch instructions.  
Format:

- **opcode**  
  - Size: 7 bits  
  - Location: Bits 0 to 6 of the instruction  
  - Description: Specifies the general operation category (e.g., branch).  

- **imm[11] (Immediate Bit 11)**  
  - Size: 1 bit  
  - Location: Bit 7 of the instruction  
  - Description: Represents the 11th bit of the immediate value.  

- **imm[4:1] (Immediate Bits 4 to 1)**  
  - Size: 4 bits  
  - Location: Bits 8 to 11 of the instruction  
  - Description: Represents bits 4 to 1 of the immediate value.  

- **funct3**  
  - Size: 3 bits  
  - Location: Bits 12 to 14 of the instruction  
  - Description: Specifies the specific type of branch operation.  

- **rs1 (Source Register 1)**  
  - Size: 5 bits  
  - Location: Bits 15 to 19 of the instruction  
  - Description: Specifies the first source register for comparison.  

- **rs2 (Source Register 2)**  
  - Size: 5 bits  
  - Location: Bits 20 to 24 of the instruction  
  - Description: Specifies the second source register for comparison.  

- **imm[10:5] (Immediate Bits 10 to 5)**  
  - Size: 6 bits  
  - Location: Bits 25 to 30 of the instruction  
  - Description: Represents bits 10 to 5 of the immediate value.  

- **imm[12] (Immediate Bit 12)**  
  - Size: 1 bit  
  - Location: Bit 31  
  - Description: Represents the 12th bit of the immediate value.  

#### 5. U-Type  
The U-type format is used for instructions that need a 20-bit immediate value. These instructions include LUI (Load Upper Immediate) and AUIPC (Add Upper Immediate to PC).

- **opcode**  
  - Size: 7 bits  
  - Location: Bits 0 to 6 of the instruction  
  - Description: Specifies the general operation category.

- **rd**  
  - Size: 5 bits  
  - Location: Bits 7 to 11 of the instruction  
  - Description: Specifies the destination register.

- **imm[19:12]**  
  - Size: 8 bits  
  - Location: Bits 12 to 19 of the instruction  
  - Description: Represents bits 19 to 12 of the immediate value.

- **imm[11]**  
  - Size: 1 bit  
  - Location: Bit 20  
  - Description: Represents bit 11 of the immediate value.

- **imm[10:1]**  
  - Size: 10 bits  
  - Location: Bits 21 to 30 of the instruction  
  - Description: Represents bits 10 to 1 of the immediate value.

- **imm[20]**  
  - Size: 1 bit  
  - Location: Bit 31  
  - Description: Represents bit 20 of the immediate value.

#### 6. J-Type  
The J-type format is primarily used for jump instructions, such as JAL (Jump and Link).

- **opcode**  
  - Size: 7 bits  
  - Location: Bits 0 to 6 of the instruction  
  - Description: Specifies the operation. For JAL, the opcode is 1101111.

- **rd**  
  - Size: 5 bits  
  - Location: Bits 7 to 11 of the instruction  
  - Description: Specifies the destination register where the return address (PC + 4) will be stored.

- **imm[19:12]**  
  - Size: 8 bits  
  - Location: Bits 12 to 19 of the instruction  
  - Description: Represents bits 19 to 12 of the immediate value.

- **imm[11]**  
  - Size: 1 bit  
  - Location: Bit 20  
  - Description: Represents bit 11 of the immediate value.

- **imm[10:1]**  
  - Size: 10 bits  
  - Location: Bits 21 to 30 of the instruction  
  - Description: Represents bits 10 to 1 of the immediate value.

- **imm[20]**  
  - Size: 1 bit  
  - Location: Bit 31  
  - Description: Represents bit 20 of the immediate value.


## RISC-V Instructions with Hexadecimal Encodings


  ADD r8, r9, r10  
  #R-Type Instruction Format  
  func7 - 0000000  
  rs2 - 01010  
  rs1 - 01001  
  func3 - 000  
  rd - 01000  
  opcode - 0110011  
  0000000 01010 01001 000 01000 0110011
  Hex: 0x00a48433

  SUB r10, r8, r9  
  #R-Type Instruction Format  
  func7 - 0100000  
  rs2 - 01001  
  rs1 - 01000  
  func3 - 000  
  rd - 01010  
  opcode - 0110011  
  0100000 01001 01000 000 01010 0110011
  Hex: 0x40a48533

  AND r9, r8, r10  
  #R-Type Instruction Format  
  func7 - 0000000  
  rs2 - 01010  
  rs1 - 01000  
  func3 - 111  
  rd - 01001  
  opcode - 0110011  
  0000000 01010 01000 111 01001 0110011
  Hex: 0x00a407b3

  OR r8, r9, r5  
  #R-Type Instruction Format  
  func7 - 0000000  
  rs2 - 00101  
  rs1 - 01001  
  func3 - 110  
  rd - 01000  
  opcode - 0110011  
  0000000 00101 01001 110 01000 0110011
  Hex: 0x0054c733

  XOR r8, r8, r4  
  #R-Type Instruction Format  
  funct7: 0000000  
  rs2: 00100  
  rs1: 01000  
  funct3: 100  
  rd: 01000  
  opcode: 0110011  
  0000000 00100 01000 100 01000 0110011
  Hex: 0x00448633

  SLT r0, r1, r4  
  #R-Type Instruction Format  
  funct7: 0000000  
  rs2: 00100  
  rs1: 00001  
  funct3: 010  
  rd: 00000  
  opcode: 0110011  
  0000000 00100 00001 010 00000 0110011
  Hex: 0x0040a033

  ADDI r2, r2, 5  
  #I-Type Instruction Format  
  immediate: 000000000101  
  rs1: 00010  
  funct3: 000  
  rd: 00010  
  opcode: 0010011  
  000000000101 00010 000 00010 0010011
  Hex: 0x00510113

  SW r2, r0, 4  
  #S-Type Instruction Format  
  immediate: 0000000 00010  
  rs2: 00010  
  rs1: 00000  
  funct3: 010  
  opcode: 0100011  
  0000000 00010 00000 010 00100 0100011
  Hex: 0x00412023

  SRL r6, r1, r1  
  #R-Type Instruction Format  
  funct7: 0000000  
  rs2: 00001  
  rs1: 00001  
  funct3: 101  
  rd: 00110  
  opcode: 0110011  
  0000000 00001 00001 101 00110 0110011
  Hex: 0x001092b3

  BNE r0, r0, 20  
  #B-Type Instruction Format  
  immediate: 000000 01000 0  
  rs2: 00000  
  rs1: 00000  
  funct3: 001  
  opcode: 1100011  
  0000000 00000 00000 001 01000 1100011
  Hex: 0x01400063

  BEQ r0, r0, 15  
  #B-Type Instruction Format  
  immediate: 000000 01111 0  
  rs1: 00000  
  rs2: 00000  
  funct3: 000  
  opcode: 1100011  
  0000000 00000 00000 000 01111 1100011
  Hex: 0x00f00063

  LW r3, r1, 2  
  #I-type instruction format  
  immediate: 000000000010  
  rs1: 00001  
  funct3: 010  
  rd: 00011  
  opcode: 0000011  
  000000000010 00001 010 00011 0000011
  Hex: 0x00210183

  SLL r5, r1, r1  
  #R-Type instruction format  
  funct7: 0000000  
  rs2: 00001  
  rs1: 00001  
  funct3: 001  
  rd: 00101  
  opcode: 0110011  
  0000000 00001 00001 001 00101 0110011
  Hex: 0x001091b3



  ### RISC-V Instructions and Hexadecimal Values Table
  
  | Instruction      | Hexadecimal Value |
  |------------------|-------------------|
  | ADD r8, r9, r10  | 0x00a48433        |
  | SUB r10, r8, r9  | 0x40a48533        |
  | AND r9, r8, r10  | 0x00a407b3        |
  | OR r8, r9, r5    | 0x0054c733        |
  | XOR r8, r8, r4   | 0x00448633        |
  | SLT r0, r1, r4   | 0x0040a033        |
  | ADDI r2, r2, 5   | 0x00510113        |
  | SW r2, r0, 4     | 0x00412023        |
  | SRL r6, r1, r1   | 0x001092b3        |
  | BNE r0, r0, 20   | 0x01400063        |
  | BEQ r0, r0, 15   | 0x00f00063        |
  | LW r3, r1, 2     | 0x00210183        |
  | SLL r5, r1, r1   | 0x001091b3        |



 ### Verilog Hardcoded values and RISCv ISA 


  

  | Assembly Code        | Hardcoded Hex Value | 32-bit RISC-V Instruction (Hex) |
  |----------------------|---------------------|---------------------------------|
  | add r6, r1, r2       | 32'h02208300        | 0x00208333                      |
  | sub r7, r1, r2       | 32'h02209380        | 0x402083b3                      |
  | and r8, r1, r3       | 32'h0230a400        | 0x0030a333                      |
  | or r9, r2, r5        | 32'h02513480        | 0x005123b3                      |
  | xor r10, r1, r4      | 32'h0240c500        | 0x0040a333                      |
  | slt r11, r2, r4      | 32'h02415580        | 0x004123b3                      |
  | addi r12, r4, 5      | 32'h00520600        | 0x00520613                      |
  | sw r3, r1, 2         | 32'h00209181        | 0x00212023                      |
  | lw r13, r1, 2        | 32'h00208681        | 0x00208683                      |
  | beq r0, r0, 15       | 32'h00f00002        | 0x00f00063                      |
  | add r14, r2, r2      | 32'h00210700        | 0x002103b3                      |
  | bne r0, r1, 20       | 32'h01409002        | 0x01409063                      |
  | sll r15, r1, r2(2)   | 32'h00208783        | 0x002087b3                      |
  | srl r16, r14, r2(2)  | 32'h00271803        | 0x002718b3                      |

Waveform snapshots for command for Verilog netlist and testbench

![Screenshot from 2024-07-29 12-55-34](https://github.com/user-attachments/assets/a4422c3d-d251-4c78-bbc6-8bb002c71959)

We obtained the following waveforms by executing the following commands on terminal:

```
iverilog -o iiitb_rv32i_tb iiitb_rv32i.v iiitb_rv32i_tb.v
```
This command compiles the Verilog source files and generates an executable named iiitb_rv32i_tb.

```
vvp iiitb_rv32i_tb
```
This command executes the simulation, and during this process, it generates a Value Change Dump (VCD) file (named iiitb_rv32i.vcd specified in the testbench).

```
gtkwave iiitb_rv32i.vcd
```

This command launches GTKWave and opens the specified VCD file, to visually inspect the waveforms and analyze the behavior of Verilog design and testbench.

```
Add R6, R2, R1
```

![Your paragraph text (1)](https://github.com/user-attachments/assets/b7027ff8-64eb-4a5b-89ac-dea30e2b5749)  


```
SUB R7, R1, R2
```

![32 bit instruction for SUB R7, R1, R2](https://github.com/user-attachments/assets/7876f010-14eb-4a0b-9779-c7581ca3c2d0)  



```
AND R8, R1, R3
```

![output of 3 and 1 will be 1 (0011) (0001) = (0001)](https://github.com/user-attachments/assets/70005853-ba64-44aa-89ff-99acfb30d479)  

  
```
OR R9, R2, R5
```
![output of 2  5 will be 7 (0010)  (0101) = (0111)](https://github.com/user-attachments/assets/3cc92644-9524-42a8-9962-402a19cc668e)  


```
XOR R10, R1, R4
```
 ![Untitled design](https://github.com/user-attachments/assets/54035e1f-6630-493a-948e-e353ee2f2208)  
 
```
SLT R1, R2, R4
```  
     
![Values stored in two different registers](https://github.com/user-attachments/assets/5eb768f6-09ec-42fc-93db-9a1c355dea2d)  

```
ADDI R12, R4, R5
```
![Your paragraph text](https://github.com/user-attachments/assets/c3c70c0e-f9d6-4ff1-97bc-ebb4cbf797c3)  

```
BEQ R0, R0, 15
```
![value stored in register R0](https://github.com/user-attachments/assets/a8d327c7-dfe5-4610-8dbb-67f2e569a5c8)      







