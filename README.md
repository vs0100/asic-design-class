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
         $clk_vai = *clk;
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
  sandpiper-saas -i home/vsduser/VSDBabySoC/src/module/vai_rvmyth.tlv -o vai_rvmyth.v --bestsv -- 
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

From GTK wave
![Screenshot from 2024-08-27 02-45-01](https://github.com/user-attachments/assets/4a7cd729-475b-486e-b59c-e5bc78a07fb2)

From Makerchip IDE
![Screenshot (445)](https://github.com/user-attachments/assets/1c03eebc-106f-4b94-9904-c915b8edb9a5)

</details>

<details>

 <summary>Lab 7</summary>
 
 # Task : Integrating PLL, riscv core and DAC on babySOC and verifying its output.
In this task we have used a PLL, Riscv core and DAC.
Input to PLL is a low frequency signal coming from the crystal oscillators of SOC. PLL increases the frquency of input signal and gives it as input to the riscv core. The riscv core gives a 10 bit digital output which is gurther given to a DAC converter. The output of DAC is an analog signal OUT.We further analyze the ouptut in gtkWave.  
VCO_IN is the input to PLL    
CLK is the output of PLL  
clk_vai is the input of riscv   
OUT is the 10 bit output of riscv   
OUT is the output of DAC  
 
Here are the commands to do that  
Time and date are also dislayed:
![Screenshot from 2024-09-02 02-54-52](https://github.com/user-attachments/assets/f2333bb6-b330-418e-93cf-9f1dcc8a804f)





Here is the output of gtkWave

![Screenshot from 2024-09-02 03-10-48](https://github.com/user-attachments/assets/1f825c8a-180c-49e9-8ee6-9cefd2d1b0dd)


</details>


<details>
<summary>Lab 8</summary>

 <details>
 <summary>DAY 0 </summary>

 # Software Installation

  <details>
  <summary>Yosys</summary>

```
     git clone https://github.com/YosysHQ/yosys.git 
     cd yosys-master  
     sudo apt install make  
     sudo apt-get install build-essential clang bison flex \ 
     libreadline-dev gawk tcl-dev libffi-dev git \ 
     graphviz xdot pkg-config python3 libboost-system-dev \ 
     libboost-python-dev libboost-filesystem-dev zlib1g-dev 
     make  
     sudo make install

  ``` 
  </details>
  
  <details>
  <summary>Iverilog</summary>
	  
  ```
	sudo apt-get install iverilog
  ```

  </details>


  <details>
  <summary>GTKWave</summary>
	  
```
sudo apt-get install gtkwave
```
  </details>


  <details>
  <summary>NgSpice</summary>
	  
```
tar -zxvf ngspice-40.tar.gz
cd ngspice-40
mkdir release
cd release
../configure  --with-x --with-readline=yes --disable-debug
make
sudo make install								
```
</details>


<details>
<summary>Magic</summary>
	
```
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev	
```
 </details>


  <details>
  <summary>OpenSTA</summary>

```
sudo apt-get install cmake clang gcctcl swig bison flex
git clone https://github.com/The-OpenROAD-Project/OpenSTA.git
cd OpenSTA
mkdir build
cd build
cmake ..
make	
```
  </details>



  <details>
  <summary>OpenLANE</summary>
	  
```
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git

sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io

sudo docker run hello-world

sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 

# After reboot
docker run hello-world

```


</details>
</details>

<details>
<summary>DAY1</summary>
	

<details>
	<summary>Introduction to Verilog RTL design and Synthesis</summary>
	
**RTL** Design - 
Register transfer level design, or RTL design, is a technique that allows us to move data across registers. In RTL design, we use HDL (Hardware Description Language) programs like Verilog or VerilogHDL to write code for both sequential and combinational circuits. These programs may simulate both hardware and logical functions. An RTL design can consist of a single Verilog code or several. A crucial point to remember is that RTL designs must be written with efficient and synthesizable (physical gate-realizable) code.   
**Testbench** -
Verilog allows us to create test benches that simulate the behavior of a hardware design by providing input signals and checking the resulting output signals. This is crucial for larger and more complex designs, as it helps ensure that the simulation results align with the actual hardware behavior after synthesis. A test bench typically consists of two parts: one that generates input signals for the design under test, and another that verifies the correctness of the output signals. This approach can be summarized as follows :  
![test](https://github.com/user-attachments/assets/4965e89b-18d3-4e32-b0cf-a9d7c6304bc6)

**Simulation** -
RTL designs can be verified against their specifications by simulating them with various input samples. This process helps identify and correct errors in the design early on in the development cycle.   
**Simulator** -
Simulator is the software tool used to execute RTL designs and verify their behavior. It monitors changes in input signals and calculates the corresponding output signals. If there are no changes to the input signals, the simulator will not observe any changes in the output signals.   
**Iverilog** - 
Iverilog is a free and open-source Verilog simulator that can be used to verify the functionality of digital circuits. It supports various Verilog language features, including modules, ports, wires, registers, gates, and behavioral descriptions.  
**gtkWave** -
GtkWave is a free and open-source waveform viewer that is commonly used in digital design and verification. It provides a graphical interface for visualizing and analyzing digital waveforms, making it a valuable tool for debugging and understanding the behavior of hardware designs.   

**Lab Examples using iverilog and gtkWave**
1. Cloning the github repository using the following command to install the necessary code files into system which will be used for synthesis and generation
   
```
$git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```
2. Simulation of verilog code and visualizing the results   

We performed simulation of multiplexer. After the simulation in iverilog the generated vcd file is viewed using gtkwave to view the output waveforms.The output was generated using the inputs given in testbench.

Mutiplexer code:

```

module good_mux (input i0 , input i1 , input sel , output reg y);
always @ (*)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```

Testbench Code:

```
`timescale 1ns / 1ps
module tb_good_mux;
// Inputs
reg i0,i1,sel;
// Outputs
wire y;
  		// Instantiate the Unit Under Test (UUT), name based instantiation
	good_mux uut (.sel(sel),.i0(i0),.i1(i1),.y(y));
	//good_mux uut (sel,i0,i1,y);  //order based instantiation
initial begin
	$dumpfile("tb_good_mux.vcd");
	$dumpvars(0,tb_good_mux);
	// Initialize Inputs
	sel = 0;
	i0 = 0;
	i1 = 0;
	#300 $finish;
end
always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
endmodule

```


 
</details>
<details>
	<summary>Introduction to Yosys synthesizer</summary>
	
**Synthesis:**
Synthesis is the process of transforming a high-level design into a low-level netlist of logic gates, tailored to a specific technology library. It involves breaking down the design into basic logic components, mapping them to actual gates, and optimizing the resulting netlist to meet design constraints. This process ensures that the abstract design can be realized as a physical hardware implementation.	
	
**Synthesizer:**
It is a tool we use to convert out RTL design code to netlist. Yosys is the tool I've used in this workshop. 

Flow of the above process is as follows:

![synthesis](https://github.com/user-attachments/assets/213fa416-0011-4cde-aad4-dc4c013b810d)



**YoSys:**
Yosys is a synthesis tool that converts high-level hardware designs into optimized gate-level representations that can be targeted for various FPGA and ASIC technologies. We can use Yosys by providing our RTL design and a standard cell library file. The tool will then synthesize the design and generate a netlist file.
Below are the commands to perform above synthesis.
RTL Design - read_verilog
.lib - read_liberty
netlist file- write_verilog

**Operational FLow of Yosys Sythesizer**
![op_flow](https://github.com/user-attachments/assets/d4e5a6e6-76db-475d-a1e3-6334a813c36d)


**Verification of Synthesized Design:**
To make sure design has no errors, we verify the given netlist.
The netlist verification flow can be seen as
![veri](https://github.com/user-attachments/assets/b11683b8-b6b2-47cf-9d13-20299fcbb9a0)


Gtkwave output for the RTL design using iVerilog should match the output waveform for the yosys netlist. As netlist and design code have the same set of inputs nad outputs we can use the same testbench and compare the waveforms.

**Lab on Yosys Introduction**

Move to the lib directory and invoke yosys to generate the synthesis of our design using the following commands:

```
yosys> read_liberty -lib <give the path to lib file>
yosys> read_verilog <give the path to verilog file>
yosys> synth -top <top_module_name>
yosys> abc -liberty <give the path to lib file>
yosys> show

```
Output of the synthesized design is as follows:

![Screenshot from 2024-10-20 17-09-42](https://github.com/user-attachments/assets/09f4f5da-ade3-4210-80b2-d6e96c960cbf)

![Screenshot from 2024-10-20 17-11-05](https://github.com/user-attachments/assets/dcac2f2c-d3c9-48f7-9795-52878f2a8083)

Run the following code to generate the netlist file :

```
yosys> write_verilog good_mux_netlist.v
yosys> write_verilog -noattr good_mux_netlist.v
```
The below image shows the netlist file:

![Screenshot from 2024-10-20 16-37-47](https://github.com/user-attachments/assets/065b6b9b-6ff8-4b93-95d3-cee801ca1040)


</details>

</details>



<details>
<summary >DAY 2</summary> 
<details>
<summary>Timing Libs, hierarchial, flat synthesis, efficient flop coding styles</summary>
<details>
<summary>Introduction to timing .libs</summary>
	
```
 sky130_fd_sc_hd__tt_025C_1v80.lib
 ```
Signinficance for the above terms
	
***130** 130nm library  
**tt** sugnifies the process which is typical  
**025c** signifies the temperature  
**1v80** signifies the voltage  

Using this lib file, we get access to different types of cells which has vast number of variants in terms of number of inputs and ouputs along with the functionality.

Leakage power, delay, area, power ports of all the cells is specified within the file for all the input combinations of the given gate.

 ![Screenshot from 2024-10-20 18-14-22](https://github.com/user-attachments/assets/ff5e300e-e4b3-4aee-8c96-2827339b0325)

![Screenshot from 2024-10-20 18-16-02](https://github.com/user-attachments/assets/f2d8b3d7-a085-4e7f-8b6e-720326712ceb)

This image displays the power consumtion and delay comparison.
For power : and2_0 < and2_2 < and_4
For delay : and2_0 > and2_2 > and2_4
![Screenshot from 2024-10-20 18-23-47](https://github.com/user-attachments/assets/3590e07c-98a1-4245-b320-c160934a8572)


</details>
<details>
<summary>Hierarchial Synthesis vs Flat Synthesis</summary>
	
**multiple_module**

Explaination for Hierarchial and Flat Synthesis using an example module given below
This is the verilog code for module
```
module sub_module2 (input a, input b, output y);
	assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
	assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
wire net1;
sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule
```
Launch yosys using the following command
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog multiple_modules.v
yosys> synth -top multiple_modules
yosys> show multiple_modules 

```
The following report is generated
![Screenshot from 2024-10-20 18-44-29](https://github.com/user-attachments/assets/a4e583e4-6ba0-492f-9559-f8f599ffc5c3)


For the above verilog code thos is the schematic which should be generated



But, the Yosys synthesiser generates the following schematic instesd of the above one and within the submodules
![Screenshot from 2024-10-20 19-23-32](https://github.com/user-attachments/assets/bf53240d-45a5-4909-ada6-5cbfe73caac1)

Generating netlist using the following commands:

```
yosys> write_verilog -noattr multiple_modules_hier.v  
yosys> !gvim multiple_modules_hier.v
```

Generated netlist is
![Screenshot from 2024-10-20 19-26-56](https://github.com/user-attachments/assets/02b91218-198e-4f00-8476-1131e4bce8da)

The generated netlist is a hierarchial netlist


Now we will use the following commands to flatten the netlist:

```
yosys> flatten
yosys> write_verilog -noattr multiple_modules_flat.v
yosys> !gvim multiple_modules_flat.v
```
Below is the schematic
![Screenshot from 2024-10-20 19-40-28](https://github.com/user-attachments/assets/f15b51a9-a764-43aa-9bd5-2fc4595d2329)


The following netlist will be obtained
![Screenshot from 2024-10-20 19-30-00](https://github.com/user-attachments/assets/78dccab9-7d6d-4fa0-9403-d02f0d535543)

We can also do the entire process/synthesis even for a single submodule using the following command:

```
yosys> synth -top sub_module1
```
</details>
<details>
	<summary>Various Flop Coding styles and Optimization</summary>
In this session we learnt about how to code various types of flops and various styles of coding a flop

Why Use a Flop?

In combinational circuits, generating the corresponding output for given inputs involves some propagation delay. During this delay, intermediate outputs can appear due to various paths within the circuit, which are known as glitches. The more complex the combinational logic, the higher the likelihood of these glitches, leading to unstable output signals.

To eliminate glitches, we use an element called a flop. A flop stores the output of the combinational circuit, allowing it to become the input to the next stage only when a clock edge (either rising or falling) occurs. This approach ensures that the input to the subsequent combinational circuit remains stable, thereby reducing glitches.

It’s important to initialize the flop; otherwise, the next stage of combinational logic could receive undefined (garbage) values. This initialization is managed through control signals like reset and set.

Flop Coding Styles

- Asynchronous reset
- Synchronous reset
- Asynchronous set
- Synchronous set
- Both Asynchronous reset and Synchronous reset
- Both Asynchronous set and Synchronous set


Lab-Flop synthesis simulations
**d-flipflop with asynchronous reset**
Here the output q goes low whenever reset is high and will not wait for the clock's posedge, i.e irrespective of clock, the output is changed to low.

```
 module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
	always @ (posedge clk , posedge async_reset)
	begin
		if(async_reset)
			q <= 1'b0;
		else	
			q <= d;
	end
endmodule
```
We generate the output using the following commands:

```
$ iverilog dff_async_set.v tb_dff_async_reset.v
$ ./a.out
$ gtkwave tb_dff_async_reset.vcd
```
The output is shown below:
![Screenshot from 2024-10-20 20-12-57](https://github.com/user-attachments/assets/aac36480-8de2-47e3-8f61-f287fec3436b)

Synthesizing using Yosys:
Use the commands given below:
```
yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog dff_asyncres.v 
yosys> synth -top dff_asyncres
yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
![Screenshot from 2024-10-20 20-29-35](https://github.com/user-attachments/assets/4ff5fc60-c688-48c5-a0fd-cf330e95e54f)

**dflipflop with Synchronous reset**
Use the following commands
```
yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog dff_async_set.v 
yosys> synth -top dff_async_set
yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
We obtain the asynchronous set flop structure:
![Screenshot from 2024-10-20 20-33-41](https://github.com/user-attachments/assets/abbc7027-effc-4b84-9076-5239305bd4ca)

**Synchronous Reset Flop**
The output changes only when the clock changes i.e., depends on the clock. The below figure clearly explains the synchronous reset mode.
The verilog code for synchronous reset is :
```
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```
Use the following commands:
```
yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog dff_syncres.v 
yosys> synth -top dff_syncres
yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
Synthesized Circuit
![Screenshot from 2024-10-20 20-44-42](https://github.com/user-attachments/assets/81a390ff-b394-40dd-97a0-348e08947b50)

Gtkwave waveform
![Screenshot from 2024-10-20 20-52-27](https://github.com/user-attachments/assets/5255c88f-bae0-464e-b436-386786179473)

**d-flipflop with Synchronous/Asynchronous reset**
Verilog code:
```
module dff_asyncres_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
    if(async_reset)
            q <= 1'b0;
    else if (sync_reset)
            q <= 1'b0;
    else
            q <= d;
end
endmodule
```
Gtkwave waveform
![Screenshot from 2024-10-20 21-05-48](https://github.com/user-attachments/assets/16687ebc-ff4d-4ffc-b2c8-be029b9f3604)

Synthesis
![Screenshot from 2024-10-20 21-25-46](https://github.com/user-attachments/assets/4a53b7c5-2998-4b56-b6e0-0a593b246f33)
</details>

<details>
	<summary>Interesting Optimizations</summary>
This lab session deals with some automatic and interesting optimisations of the circuits based on logic. In the below example, multiplying a number with 2 doesn't need any additional hardeware and only needs connecting the bits from a to y and grounding the LSB bit of y is enough and the same is realized by Yosys.
	
```
module mul2 (input [2:0] a, output [3:0] y);
	assign y = a * 2;
endmodule
```
Using the commands given below:
```
yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog mult_2.v 
yosys> synth -top mul2
yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
Synthesized design
![Screenshot from 2024-10-20 21-32-54](https://github.com/user-attachments/assets/b3afa963-695b-4396-8e1b-d0b35507efb5)

Generating netlist using the following commands:
```
yosys> write_verilog -noattr mul2_net.v
yosys> !gvim mul2_net.v
```
The generated netlist is as follows:
![Screenshot from 2024-10-20 21-35-13](https://github.com/user-attachments/assets/fbdde11a-4ce9-4226-8bb8-c79321dee2d8)

Let us now synthesize mult_8 using the following commands:
```
yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog mult_8.v 
yosys> synth -top mult8
yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```

Synthesized Circuit:
![Screenshot from 2024-10-20 21-39-04](https://github.com/user-attachments/assets/c0e77cb7-3bf4-44d2-b9a7-47155cbdcdf0)

Netlist is generated using the following commands:
```
yosys> write_verilog -noattr mult8_net.v
yosys> !gvim mult8_net.v
```
The generated netlist is as follows:
![Screenshot from 2024-10-20 21-41-22](https://github.com/user-attachments/assets/135e8bb2-749d-414f-9418-477cb1c01f91)


</details>



</details>
</details>


<details>
<summary >DAY 3</summary>
Combinational logic optimization with examples
Optimising the combinational logic circuit is squeezing the logic to get the most optimized digital design so that the circuit finally is area and power efficient. This is achieved by the synthesis tool using various techniques and gives us the most optimized circuit.

**Techniques for Optimization**
- Constant propagation which is Direct optimization technique
- Boolean logic optimization using K-map or Quine McKluskey

In the above example, if we considor the trasnsistor level circuit of output Y, it has 6 MOS trasistors and when it comes to invertor, only 2 transistors will be sufficient. This is achieved by making A as contstant and propagating the same to output.

Example for Boolean logic optimization:

Let's consider an example concurrent statement 

**assign y=a?(b?c:(c?a:0)):(!c)**

The above expression is using a ternary operator which realizes a series of multiplexers, however, when we write the boolean expression at outputs of each mux and simplify them further using boolean reduction techniques, the outout y turns out be just **~(a^c)**

Command to optimize the circuit by yosys is 

**yosys> opt_clean -purge**

**Example-1**

```
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```
**Optimized Circuit**

![Screenshot from 2024-10-21 03-03-48](https://github.com/user-attachments/assets/93944613-a047-4d5e-91ea-ccf26e8b198f)



**Example-2**
```
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```
**Optimized Circuit**
![Screenshot from 2024-10-21 03-07-18](https://github.com/user-attachments/assets/15cb90fe-43f2-483c-8114-5d7371b31128)



**Example-3**
```
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```

**Optimized Circuitt**
![Screenshot from 2024-10-21 03-07-18](https://github.com/user-attachments/assets/3e4769c6-41ab-404d-b594-04a400fcdd60)



**Example-4**


```
module opt_check4 (input a , input b , input c , output y);
	assign y = a?(b?(a & c ):c):(!c);
endmodule
```

**Optimized Circuit**
![Screenshot from 2024-10-21 03-16-17](https://github.com/user-attachments/assets/8175d344-0205-42e5-b7d7-75939cd02686)


**Example-5**
```
module sub_module(input a , input b , output y);
	assign y = a & b;
endmodule

module multiple_module_opt2(input a , input b , input c , input d , output y);
	wire n1,n2,n3;
	sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
	sub_module U2 (.a(b), .b(c) , .y(n2));
	sub_module U3 (.a(n2), .b(d) , .y(n3));
	sub_module U4 (.a(n3), .b(n1) , .y(y));
endmodule

```
Here there is multiple modules present so we will try to check whether those module are being used or not by using following commands:

```
yosys:read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:read_verilog multiple_module_opt2.v
yosys:synth -top multiple_module_opt2
yosys:abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:flatten
yosys:opt_clean -purge
yosys:show

```

**Before Flatten**

![Screenshot from 2024-10-21 03-22-52](https://github.com/user-attachments/assets/028266f9-dcda-4f37-9264-138f69ecaf68)






**After Flatten**
![Screenshot from 2024-10-21 03-24-54](https://github.com/user-attachments/assets/08509587-5ffa-41a2-826a-5e61428d68c2)






**Example-6**
```
module sub_module1(input a , input b , output y);
assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
assign y = a^b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;
sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1); 
endmodule

```

**Before Flatten**

![Screenshot from 2024-10-21 03-30-18](https://github.com/user-attachments/assets/58454364-5677-4d17-92ab-a97406d4ed4e)




**After Flatten**

![Screenshot from 2024-10-21 03-31-11](https://github.com/user-attachments/assets/ca1b9c2b-922b-482f-8831-3de810915377)






<details>
	<summary>Sequential Logic Optimization with examples</summary>
Below are the various techniques used for sequential logic optimisations:
	-Basic
		Sequential contant propagation
	-Advanced
		-State optimisation
		-Retiming
		-Sequential Logic Cloning (Floor Plan Aware Synthesis)	

**Basic**

Sequential Constant Propagation
Here only the first logic can be optimized as the output of flop is always zero. However for the second flop, the output changes continuously, therefor it cannot be used for contant propagation.

**Advanced**
State Optimisation: This is optimisation of unused state. Using this technique we can come up with most optimised state machine.

Cloning: This is done when performing PHYSICAL AWARE SYNTHESIS. Lets consider a flop A which is connected to flop B and flop C through a combination logic. If B and C are placed far from A in the flooerplan, there is a routing path delay. To avoid this, we connect A to two intermediate flops and then from these flops the output is sent to B and C thereby decreasing the delay. This process is called cloning since we are generating two new flops with same functionality as A.

Retiming: Retiming is a powerful sequential optimization technique used to move registers across the combinational logic or to optimize the number of registers to improve performance via power-delay trade-off, without changing the input-output behavior of the circuit.

**Example-1**
Here flop will be inferred as the output is not constant.
```
module dff_const1(input clk, input reset, output reg q);
	always @(posedge clk, posedge reset)
	begin
		if(reset)
			q <= 1'b0;
		else
			q <= 1'b1;
	end
endmodule

```

**Simulation**

![Screenshot from 2024-10-21 03-46-57](https://github.com/user-attachments/assets/489922f7-9d20-48df-9f03-83db6e49f34c)


**Synthesis**

![Screenshot from 2024-10-21 03-51-50](https://github.com/user-attachments/assets/d7734090-6481-4021-a3dc-41fd64e63aef)

![Screenshot from 2024-10-21 03-53-14](https://github.com/user-attachments/assets/34599beb-2252-4a0b-bc4b-3fc672bbbe0d)



**Example-2**
Here flop will not be inferred as the output is always high.
```
module dff_const2(input clk, input reset, output reg q);
	always @(posedge clk, posedge reset)
	begin
		if(reset)
			q <= 1'b1;
		else
			q <= 1'b1;
	end
endmodule

```
**Simulation**
![Screenshot from 2024-10-21 03-44-13](https://github.com/user-attachments/assets/c9da5de0-f3f9-403d-ba17-6dc6933b9c95)


**Synthesis**
![Screenshot from 2024-10-21 03-56-40](https://github.com/user-attachments/assets/849c995c-24b2-41c8-b16b-088a9003b33d)

![Screenshot from 2024-10-21 03-58-35](https://github.com/user-attachments/assets/d8c6ba5d-5016-4d00-9631-01bb9189721f)




**Example-3**

```
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule

```
**Simulation**
![Screenshot from 2024-10-21 04-02-07](https://github.com/user-attachments/assets/48a32326-0a68-418c-a70a-cbd0ea9ec4b9)


**Synthesis**
![Screenshot from 2024-10-21 04-04-28](https://github.com/user-attachments/assets/b3395476-34ea-4353-8146-20a41b09c3f5)



**Example-4**

```
module dff_const4(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b1;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule

```
**Synthesis**
![Screenshot from 2024-10-21 04-08-35](https://github.com/user-attachments/assets/515619a8-1ea1-4bea-9889-380cbecbe638)


**Simulatiom**
![Screenshot from 2024-10-21 04-10-36](https://github.com/user-attachments/assets/89ff9812-d353-490b-862b-08636fb2f4b9)

![Screenshot from 2024-10-21 04-12-01](https://github.com/user-attachments/assets/867080a8-1840-4762-a3f4-8cd21ef3a64b)


**Example-5**

```
module dff_const5(input clk, input reset, output reg q);
reg q1;
always @(posedge clk, posedge reset)
	begin
	if(reset)
	begin
		q <= 1'b0;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```
**Simulation**

![Screenshot from 2024-10-21 04-18-59](https://github.com/user-attachments/assets/9c303214-4c15-4820-a320-af923eadfe3d)


**Synthesis**
![Screenshot from 2024-10-21 04-20-30](https://github.com/user-attachments/assets/6c1789f3-c241-454b-90dc-dd37fb409f23)

![Screenshot from 2024-10-21 04-19-55](https://github.com/user-attachments/assets/1206d235-b64d-4358-99d0-9e638187506f)




<details>
	<summary>
		Sequential optimisation of unused outputs
	</summary>

**Example-1**
```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];
always @(posedge clk ,posedge reset)
begin
if(reset)
	count <= 3'b000;
else
	count <= count + 1;
end
endmodule

```
**Synthesis**
![Screenshot from 2024-10-21 04-24-44](https://github.com/user-attachments/assets/fb0cae6c-3ad1-4645-a66c-c8b88eb2b275)



![Screenshot from 2024-10-21 04-24-11](https://github.com/user-attachments/assets/a90c8bc4-d243-45df-aa62-dd4b89b3e584)




Updated Counter Logic
```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = {count[2:0]==3'b100};
always @(posedge clk ,posedge reset)
begin
if(reset)
	count <= 3'b000;
else
	count <= count + 1;
end
endmodule
```

**Synthesis**

![Screenshot from 2024-10-22 03-26-59](https://github.com/user-attachments/assets/ef88eb77-734f-4021-bf4e-0326a05a4cb7)

![Screenshot from 2024-10-22 03-28-41](https://github.com/user-attachments/assets/7288aa8d-30ac-4b2a-8a1a-d88196941f1b)




</details>

</details>
</details>



<details>
<summary >DAY 4</summary> 
	
## GLS,blocking vs non-blocking and Synthesis-Simulation mismatch ##

<details>
	<summary>GLS, Synthesis-Simulation mismatch and Blocking, Non-blocking statements</summary>
	
GLS Concepts And Flow Using Iverilog  
In GLS, we run test bench with netlist as the Design under test instead of the RTL code. Basically, Netlist is logically equal to RTL code as the netlist is obtained by converting RTL code into standard cell gates.
We will use GLS to verify the logical correctness of design after synthesis and also to ensure the timing of the design is met. (run with delay annotations.)

![image](https://github.com/user-attachments/assets/f5231cdd-fbda-4aa2-9400-617582861a72)

### Synthesis Simulation Mismatch ###
There are three main reasons for Synthesis Simulation Mismatch:  

* Missing sensitivity list in always block  
* Blocking vs Non-Blocking Assignments  
* Non standard Verilog coding  

### Missing sensitivity list in always block: ###
If the consider - Example-2, we can see the only sel is mentioned in the sensitivity list. During the simulation, the waveforms will resemble a latched output but the simulation of netlist will not infer this as the synthesizer will only look at the statements with in the procedural block and not the sensitivity list.

As the synthesizer doen't look for sensitivity list and it looks only for the statements in procedural block, it infers correct circuit and if we simulate the netlist code, there will be a synthesis simulation mismatch.

To avoid the synthesis and simulation mismatch. It is very important to check the behaviour of the circuit first and then match it with the expected output seen in simulation and make sure there are no synthesis and simulation mismatches. This is why we use GLS.

### Blocking vs Non-Blocking Assignments: ###

Blocking statements execute the statemetns in the order they are written inside the always block. Non-Blocking statements execute all the RHS and once always block is entered, the values are assigned to LHS. This will give mismatch as sometimes, improper use of blocking statements can create latches. Get to see at Example4

</details>
<details>
	<summary>Lab- GLS Synth Sim Mismatch</summary>	
	
**Example-1**
	
```
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
endmodule
```
**Simulation**
![Screenshot from 2024-10-21 04-35-51](https://github.com/user-attachments/assets/92efb6c5-1a07-49d5-9721-cce53ab8ceb1)


**Synthesis**
![Screenshot from 2024-10-21 04-38-36](https://github.com/user-attachments/assets/01a51789-3671-4cc9-adae-d56bf8cce778)


**Netlist Simulation**
Commands for netlist simulation
```
iverilog ../my_lib/verilog_module/primitives.v ../my_lib/verilog_module/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
![Screenshot from 2024-10-22 03-54-48](https://github.com/user-attachments/assets/aa66d115-6852-4e52-8887-bc02163d232f)


**Example-2**

```
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
if(sel)
	y <= i1;
else 
	y <= i0;
end
endmodule
```

**Simulation**
![Screenshot from 2024-10-21 11-19-03](https://github.com/user-attachments/assets/8f265549-d7e9-474b-8cbf-af95f5e5e13c)



**Synthesis**


**Netlist Simulation**

```
iverilog ../my_lib/verilog_module/primitives.v ../my_lib/verilog_module/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```


**Mismatch**

![image](https://github.com/user-attachments/assets/5fcecd22-c6a3-427b-aa89-dd1fb0f068dc)


**Example-3**

```
module good_mux (input i0 , input i1 , input sel , output reg y);
always @ (*)
begin
if(sel)
	y <= i1;
else 
	y <= i0;
end
endmodule
```
**Simulation**
![image](https://github.com/user-attachments/assets/1d9eec09-51e0-499c-820c-bdce770d1fda)


**Synthesis**
![Screenshot from 2024-10-21 11-38-08](https://github.com/user-attachments/assets/8b4e5dc9-ba53-496f-811d-3632e81890a2)


**Netlist Simulation**



</details>
<details>
	<summmary>
		Lab- Synthesis simulation mismatch blocking statement
	</summmary>
 Here the output is depending on the past value of x which is dependednt on a and b and it appears like a flop.

**Example-4**
```
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
	begin
	d = x & c;
	x = a | b;
end
endmodule
```
**Simulation**
![Screenshot from 2024-10-21 11-40-12](https://github.com/user-attachments/assets/37ac190b-8405-4f2c-8e9c-57d76fb2a586)


**Synthesis**
![Screenshot from 2024-10-21 11-42-06](https://github.com/user-attachments/assets/f864b87e-be92-4c33-8d81-5c331edcd0cd)


**Netlist Simulation**

**Mismatch**

Here this how the circuit should behave but this correct waveform is only obtained while doing netlist simulation. Here first pic show the netlist simulation which shows the proper working of the dut while the last pic shows the improper working of dut as we have used blocking statement here which causes synthesis simulation mismatch which is sorted out by GLS while providing netlist simulation.
	
</details>

</details>


</details>
