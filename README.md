# asic-design-class
<details>
<summary>Lab 1</summary>
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

</details>


<details>
# LAB2
<summary>Lab 2</summary>

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
</details>


<details>
  <summary>Lab 3</summary>
# **LAB 3**

## To identify the type of instructions in RISC-V as R, I, S, B, U, J type, understand the differences between hardcoding 32-bit pattern of instructions and when the instructions are executed using RISC-V ISA.

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


</details>

<details>
  <summary>Lab 4</summary>
Task : To verify the output through gcc compiler and riscv compiler
Steps:
  
1. Write a code which can be compiled using gcc and riscv gcc
   
  ```#include <limits.h>
#include <stdbool.h>
#include <stdio.h>

#define V 9	

int minDistance(int dist[], bool sptSet[])	
{
    // Initialize min value
    int min = INT_MAX, min_index;

    for (int v = 0; v < V; v++)
        if (sptSet[v] == false && dist[v] <= min)
            min = dist[v], min_index = v;

    return min_index;
}


void printSolution(int dist[])
{
    printf("Vertex \t\t Distance from Source\n");
    for (int i = 0; i < V; i++)
        printf("%d \t\t\t\t %d\n", i, dist[i]);
}

void dijkstra(int graph[V][V], int src)
{
    int dist[V]; 

    bool sptSet[V]; 

    
    for (int i = 0; i < V; i++)
        dist[i] = INT_MAX, sptSet[i] = false;

   
    dist[src] = 0;

    
    for (int count = 0; count < V - 1; count++) {
        
        int u = minDistance(dist, sptSet);

        
        sptSet[u] = true;

        
        for (int v = 0; v < V; v++)

           
            if (!sptSet[v] && graph[u][v]
                && dist[u] != INT_MAX
                && dist[u] + graph[u][v] < dist[v])
                dist[v] = dist[u] + graph[u][v];
    }

   
    printSolution(dist);
}


int main()
{
   
    int graph[V][V] = { { 0, 4, 0, 0, 0, 0, 0, 8, 0 },
                        { 4, 0, 8, 0, 0, 0, 0, 11, 0 },
                        { 0, 8, 0, 7, 0, 4, 0, 0, 2 },
                        { 0, 0, 7, 0, 9, 14, 0, 0, 0 },
                        { 0, 0, 0, 9, 0, 10, 0, 0, 0 },
                        { 0, 0, 4, 14, 10, 0, 2, 0, 0 },
                        { 0, 0, 0, 0, 0, 2, 0, 1, 6 },
                        { 8, 11, 0, 0, 0, 0, 1, 0, 7 },
                        { 0, 0, 2, 0, 0, 0, 6, 7, 0 } };

    
    dijkstra(graph, 0);

    return 0;
}
```
2. Compile the code using gcc

![Screenshot from 2024-08-14 10-47-36](https://github.com/user-attachments/assets/87c8daa8-6af9-4adf-b248-daab4f6cd8c9)

3.Compile the code using riscv gcc

![Screenshot from 2024-08-14 10-47-45](https://github.com/user-attachments/assets/5db17abd-bdac-4cda-b59d-f84953b22a4f)

4. Comparing the outputs of both

![Screenshot from 2024-08-14 10-50-03](https://github.com/user-attachments/assets/7b63b14b-9097-487c-bc60-a79a795127f6)

Conlusion:
Output of GCC and RISCV are same
</details>

<details>
<summary>Lab 5</summary>

# Task - 1 : Digital logic using MakerChip IDE

Makerchip IDE
![Screenshot (431)](https://github.com/user-attachments/assets/8848c3f6-43ca-46af-bf73-5e2ed4b627d5)

### Sequential Calculator
![Screenshot 2024-08-22 001427](https://github.com/user-attachments/assets/66e5486f-deb1-487b-a226-1633c6d0174f)
 ```
\m5_TLV_version 1d: tl-x.org
\m5
   
   // =================================================
   // Welcome!  New to Makerchip? Try the "Learn" menu.
   // =================================================
   
   //use(m5-1.0)   /// uncomment to use M5 macro library.
\SV
   // Macro providing required top-level module definition, random
   // stimulus support, and Verilator config.
   m5_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV
   $reset = *reset; 
   $clk_vai = *clk;

           
         
   $val1[31:0] = $rand1[3:0];
   $val2[31:0] = $rand2[3:0];
   $sum[31:0] = >>1$f + $val2;
   $diff[31:0] = >>1$f - $val2;
   $prod[31:0] = >>1$f * $val2;
   $quot[31:0] = >>1$f / $val2;

   $f[31:0] = $reset ? 0:(($sel[1:0] == 0) ? $sum : ($sel[1:0]==1) ? $diff : ($sel[1:0]==2) ? $prod : $quot);

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule

​

```
![Screenshot (441)](https://github.com/user-attachments/assets/10567c16-457e-4afc-8459-b26aa31da3b6)


### Cycle Calculator

![Screenshot 2024-08-22 003128](https://github.com/user-attachments/assets/6b51b546-1e19-477d-b548-c1fa7e223bc3)

```
\m5_TLV_version 1d: tl-x.org
\m5
   
   // =================================================
   // Welcome!  New to Makerchip? Try the "Learn" menu.
   // =================================================
   
   //use(m5-1.0)   /// uncomment to use M5 macro library.
\SV
   // Macro providing required top-level module definition, random
   // stimulus support, and Verilator config.
   m5_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV
   
   
   |calc
      @1
         $reset = *reset;
         $clk_vai = *clk;
         $val1[31:0] = $rand1[3:0];
         $val2[31:0] = $rand2[3:0];
         $sum[31:0] = >>2$f + $val2;
         $diff[31:0] = >>2$f - $val2;
         $prod[31:0] = >>2$f * $val2;
         $quot[31:0] = >>2$f / $val2;
         
        
         
      @2
         $valid = $reset ? 1 : (!>>1$valid + 1);
         $valid_or_reset = ~$valid || $reset;
         $f[31:0] = $valid_or_reset ? 0 : (($sel[1:0] == 0) ? $sum : ($sel[1:0]==1) ? $diff : ($sel[1:0]==2) ? $prod : $quot);
         
   
  
   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule

```
![Screenshot (442)](https://github.com/user-attachments/assets/69451ddf-5706-47fc-8ca9-ebb245db5918)


### Total Distance Calculator
```
\m4_TLV_version 1d: tl-x.org
\SV

	`include "sqrt32.v";
	 m4_makerchip_module
   
\TLV
   
   |calc
      @1
         $reset = *reset;
      ?$valid
         @1
            $aa_sq[31:0] = $aa[3:0] * $aa;
            $bb_sq[31:0] = $bb[3:0] * $bb;
         @2
            $cc_sq[31:0] = $aa_sq + $bb_sq;
         @3
            $cc[31:0] = sqrt($cc_sq);
      @4
         $tot_dist[63:0] = $reset ? '0 : $valid ? >>1$tot_dist + $cc : >>1$tot_dist;//$RETAIN
         
   //...
   
   // Assert these to end simulation (before Makerchip cycle limit).
!  *passed = *cyc_cnt > 16'd30;

\SV
   endmodule

```
![Screenshot (439)](https://github.com/user-attachments/assets/d26a613e-44f1-4274-93bb-7b4b937458e2)

## Task - 2 : RISCv Core with Pipeline
```
\m4_TLV_version 1d: tl-x.org
\SV
   // Template code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   m4_asm(SW, r0, r10, 10000)           // Store r10 result in dmem
   m4_asm(LW, r17, r0, 10000)           // Load contents of dmem to r17
   m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         
         //PC fetch - branch, jumps and loads introduce 2 cycle bubbles in this pipeline
         $pc[31:0] = >>1$reset ? '0 : (>>3$valid_taken_br ? >>3$br_tgt_pc :
                                       >>3$valid_load     ? >>3$inc_pc[31:0] :
                                       >>3$jal_valid      ? >>3$br_tgt_pc :
                                       >>3$jalr_valid     ? >>3$jalr_tgt_pc :
                                                     (>>1$inc_pc[31:0]));
         // Access instruction memory using PC
         $imem_rd_en = ~ $reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
         
      @1
         //Getting instruction from IMem
         $instr[31:0] = $imem_rd_data[31:0];
         
         //Increment PC
         $inc_pc[31:0] = $pc[31:0] + 32'h4;
         
         //Decoding I,R,S,U,B,J type of instructions based on opcode [6:0]
         //Only [6:2] is used here because this implementation is for RV64I which does not use [1:0]
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] == 5'b11001;
         
         $is_r_instr = $instr[6:2] == 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] == 5'b10100;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_b_instr = $instr[6:2] == 5'b11000;
         
         $is_j_instr = $instr[6:2] == 5'b11011;
         
         //Immediate value decode
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}} , $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}} , $instr[30:25] , $instr[11:8] , $instr[7]} :
                      $is_b_instr ? { {20{$instr[31]}} , $instr[7] , $instr[30:25] , $instr[11:8] , 1'b0} :
                      $is_u_instr ? { $instr[31] , $instr[30:12] , { 12{1'b0}} } :
                      $is_j_instr ? { {12{$instr[31]}} , $instr[19:12] , $instr[20] , $instr[30:21] , 1'b0} :
                      >>1$imm[31:0];
         
         //Generate valid signals for each instruction fields
         $rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct7_valid           = $is_r_instr;
         
         //Decode other fields of instruction - source and destination registers, funct, opcode
         ?$rs1_or_funct3_valid
            $rs1[4:0]    = $instr[19:15];
            $funct3[2:0] = $instr[14:12];
         
         ?$rs2_valid
            $rs2[4:0]    = $instr[24:20];
         
         ?$rd_valid
            $rd[4:0]     = $instr[11:7];
         
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         
         $opcode[6:0] = $instr[6:0];
         
         //Decode instruction in subset of base instruction set based on RISC-V 32I
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         //Branch instructions
         $is_beq   = $dec_bits ==? 11'bx_000_1100011;
         $is_bne   = $dec_bits ==? 11'bx_001_1100011;
         $is_blt   = $dec_bits ==? 11'bx_100_1100011;
         $is_bge   = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu  = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu  = $dec_bits ==? 11'bx_111_1100011;
         
         //Jump instructions
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_jal   = $dec_bits ==? 11'bx_xxx_1101111;
         $is_jalr  = $dec_bits ==? 11'bx_000_1100111;
         
         //Arithmetic instructions
         $is_addi  = $dec_bits ==? 11'bx_000_0010011;
         $is_add   = $dec_bits ==  11'b0_000_0110011;
         $is_lui   = $dec_bits ==? 11'bx_xxx_0110111;
         $is_slti  = $dec_bits ==? 11'bx_010_0010011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0010011;
         $is_xori  = $dec_bits ==? 11'bx_100_0010011;
         $is_ori   = $dec_bits ==? 11'bx_110_0010011;
         $is_andi  = $dec_bits ==? 11'bx_111_0010011;
         $is_slli  = $dec_bits ==? 11'b0_001_0010011;
         $is_srli  = $dec_bits ==? 11'b0_101_0010011;
         $is_srai  = $dec_bits ==? 11'b1_101_0010011;
         $is_sub   = $dec_bits ==? 11'b1_000_0110011;
         $is_sll   = $dec_bits ==? 11'b0_001_0110011;
         $is_slt   = $dec_bits ==? 11'b0_010_0110011;
         $is_sltu  = $dec_bits ==? 11'b0_011_0110011;
         $is_xor   = $dec_bits ==? 11'b0_100_0110011;
         $is_srl   = $dec_bits ==? 11'b0_101_0110011;
         $is_sra   = $dec_bits ==? 11'b1_101_0110011;
         $is_or    = $dec_bits ==? 11'b0_110_0110011;
         $is_and   = $dec_bits ==? 11'b0_111_0110011;
         
         //Store instructions
         $is_sb    = $dec_bits ==? 11'bx_000_0100011;
         $is_sh    = $dec_bits ==? 11'bx_001_0100011;
         $is_sw    = $dec_bits ==? 11'bx_010_0100011;
         
         //Load instructions - support only 4 byte load
         $is_load  = $dec_bits ==? 11'bx_xxx_0000011;
         
         $is_jump = $is_jal || $is_jalr;
         
      @2
         //Get Source register values from reg file
         $rf_rd_en1 = $rs1_or_funct3_valid;
         $rf_rd_en2 = $rs2_valid;
         
         $rf_rd_index1[4:0] = $rs1[4:0];
         $rf_rd_index2[4:0] = $rs2[4:0];
         
         //Register file bypass logic - data forwarding from ALU to resolve RAW dependence
         $src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
         $src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
         
         //Branch target PC computation for branches and JAL
         $br_tgt_pc[31:0] = $imm[31:0] + $pc[31:0];
         
         //RAW dependence check for ALU data forwarding
         //If previous instruction was writing to reg file, and current instruction is reading from same register
         $rs1_bypass = >>1$rf_wr_en && (>>1$rd == $rs1);
         $rs2_bypass = >>1$rf_wr_en && (>>1$rd == $rs2);
         
      @3
         //ALU
         $result[31:0] = $is_addi  ? $src1_value +  $imm :
                         $is_add   ? $src1_value +  $src2_value :
                         $is_andi  ? $src1_value &  $imm :
                         $is_ori   ? $src1_value |  $imm :
                         $is_xori  ? $src1_value ^  $imm :
                         $is_slli  ? $src1_value << $imm[5:0]:
                         $is_srli  ? $src1_value >> $imm[5:0]:
                         $is_and   ? $src1_value &  $src2_value:
                         $is_or    ? $src1_value |  $src2_value:
                         $is_xor   ? $src1_value ^  $src2_value:
                         $is_sub   ? $src1_value -  $src2_value:
                         $is_sll   ? $src1_value << $src2_value:
                         $is_srl   ? $src1_value >> $src2_value:
                         $is_sltu  ? $sltu_rslt[31:0]:
                         $is_sltiu ? $sltiu_rslt[31:0]:
                         $is_lui   ? {$imm[31:12], 12'b0}:
                         $is_auipc ? $pc + $imm:
                         $is_jal   ? $pc + 4:
                         $is_jalr  ? $pc + 4:
                         $is_srai  ? ({ {32{$src1_value[31]}} , $src1_value} >> $imm[4:0]) :
                         $is_slt   ? (($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]}):
                         $is_slti  ? (($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]}) :
                         $is_sra   ? ({ {32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0]) :
                         $is_load  ? $src1_value +  $imm :
                         $is_s_instr ? $src1_value + $imm :
                                    32'bx;
         
         $sltu_rslt[31:0]  = $src1_value <  $src2_value;
         $sltiu_rslt[31:0] = $src1_value <  $imm;
         
         //Jump instruction target PC computation
         $jalr_tgt_pc[31:0] = $imm[31:0] + $src1_value[31:0]; 
         
         //Branch resolution
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) :
                     1'b0;
         
         //Current instruction is valid if one of the previous 2 instructions were not (taken_branch or load or jump)
         $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid || >>1$jump_valid);
         
         //Current instruction is valid & is a taken branch
         $valid_taken_br = $valid && $taken_br;
         
         //Current instruction is valid & is a load
         $valid_load = $valid && $is_load;
         
         //Current instruction is valid & is jump
         $jump_valid = $valid && $is_jump;
         $jal_valid  = $valid && $is_jal;
         $jalr_valid = $valid && $is_jalr;
         
         //Destination register update - ALU result or load result depending on instruction
         $rf_wr_en = (($rd != '0) && $rd_valid && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = $valid ? $rd[4:0] : >>2$rd[4:0];
         $rf_wr_data[31:0] = $valid ? $result[31:0] : >>2$ld_data[31:0];
         
      @4
         //Data memory access for load, store
         $dmem_addr[3:0]     =  $result[5:2];
         $dmem_wr_en         =  $valid && $is_s_instr;
         $dmem_wr_data[31:0] =  $src2_value[31:0];
         $dmem_rd_en         =  $valid_load;
         
      
         //Write back data read from load instruction to register
         $ld_data[31:0]      =  $dmem_rd_data[31:0];
         
      
      

      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   //Checks if sum of numbers from 1 to 9 is obtained in reg[17] and runs 10 cycles extra after this is met
   *passed = |cpu/xreg[17]>>10$value == (1+2+3+4+5+6+7+8+9);
   //Run for 200 cycles without any checks
   //*passed = *cyc_cnt > 200;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule

```

Attached is the image of the fully pipelined CPU

![Screenshot 2024-08-22 024458](https://github.com/user-attachments/assets/4dccdd38-f011-41a8-9fa3-c3cb9e95abc7)

![Screenshot (448)](https://github.com/user-attachments/assets/1357ac00-2aca-41ae-9b08-627b99c534dd)





![Screenshot (445)](https://github.com/user-attachments/assets/1c03eebc-106f-4b94-9904-c915b8edb9a5)


</details>
<details>
	<summary> Lab 6</summary>

 
#  Task - Conversion of TLV code into Verilog code using Sandpiper and then simulate it using iverilog and GTKwave

1. Install the packages using the given command
   ```
   sudo apt install make python python3 python3-pip git iverilog gtkwave
   sudo apt-get install python3-venv
   python3 -m venv .venv
   source ~/.venv/bin/activate
   pip3 install pyyaml click sandpiper-saas
   ```
2. Clone the repositry and move into the required repositry
   ```
   git clone https://github.com/manili/VSDBabySoC.git
   cd VSDBabySoC
   ```
3.Converte .tllv file into .v file
   ```
  sandpiper-saas -i home/vsduser/VSDBabySoC/src/module/vaishnavi_rvmyth.tlv -o vaishnavi_rvmyth.v --bestsv -- 
  noline -p verilog --outdir home/vsduser/VSDBabySoC/src/module/
  make pre_synth_sim
  ```
4. Compile amd simulate using the following command
  ```
   iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM home/vsduser/VSDBabySoC/src/module/testbench.v -I 
   src/include -I home/vsduser/VSDBabySoCsrc/module
   cd output
   ./pre_synth_sim.out
  ```
5. Open the waveform using the following command
   ```
   gtkwave pre_synth_sim.out
   ```   



![Screenshot from 2024-08-27 08-26-44](https://github.com/user-attachments/assets/97e80f54-b01d-461d-aecc-ac5898dd9035)



Comparison with Makerchip results

From GTK waveform
![Screenshot from 2024-08-27 02-45-01](https://github.com/user-attachments/assets/4a7cd729-475b-486e-b59c-e5bc78a07fb2)

From Makerchip IDE
![Screenshot (445)](https://github.com/user-attachments/assets/1c03eebc-106f-4b94-9904-c915b8edb9a5)

</details>


