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

## To identify the type of instructions in RISCv as R, I, S, B, U, J type, understand the differences between manually coding 32 bit pattern of instructions and when the instructions are executed using RISCv ISA.  

Different type of instructions in RISCv:  

Instructins are classified in RISCv based on their formats and functionality:  

 ##1. R- Type(Register):  
     Used for arithmetic and logical operations where all operands are registers.  
     Example: ADD, SUB, AND, OR, XOR, SLL, SRL, SLT.  
     Format:  
   


     func7
     Size: 7 bits
     Location: Bits 25 to 31 of the instruction 
     Description : It is used to differntiate between instructions.It provides additional information to specify the         exact operation to be performed by the instruction and differentiate between similar instructins that share the         same 'opcode' and 'funct3'. Primarilly used in R type instructions but can also appear in other types for specific      instructions.

     rs2(Source register 2)
     Size: 5 bits
     Location: Bits 20 to 24 of the instruction 
     Description : It specifies the second source register in the instruction. Used to indicate the register which is        used to store the second operand for operation. Present in instruction types like R, S and B type.

     rs1(source register 1)
     Size: 5 bits
     Location: Bits 15 to 19 of the instruction 
     Description : It specifies the first source register in the instruction which indicates the ,first operand f            for operation. Present in instruction types like R, S and B type.

     funct3
     Size: 3 bits
     Location: Bits 12 to 14 of the instruction 
     Description : It provides the mid level differentiation of operarions wwithin the generala category specified           by the opcode.

   
     rd
     Size: 5 bits
     Location: Bits 7 to 11 of the instruction 
     Description: Specifies the destination register where the result of the operation will be stored.
   
     opcode
     Size: 7 bits
     Location: Bits 0 to 6 of the instruction 
     Description: Specifies the general operation category (e.g., arithmetic, logical, load, store, etc.).



## 2.I type(Immediate):  
  Used for operations involving an immediate value.  
  Example: ADDI, SLTI, ANDI, ORI, XORI, LW.  
  Format:
     


     opcode
     Size: 7 bits
     Location: Bits 0 to 6 of the instruction.
     Description: Specifies the general operation category (e.g., arithmetic with immediate, load, etc.).

     rd
     Size: 7 bits
     Location: Bits 7 to 11 of the instruction
     Description: Specifies the destination register where the result of the operation will be stored.

     funct3
     Size: 3 bits
     Location: Bits 12 to 14 of the instruction 
     Description : It provides the mid level differentiation of operarions wwithin the generala category                     specified by the opcode.

     rs1(source register 1)
     Size: 5 bits
     Location: Bits 15 to 19 of the instruction 
     Description : It specifies the first source register in the instruction which indicates the ,first operand              for operation. Present in instruction types like R, S and B type.

     imm
     Size: 12 bits
     Location: 20 to 31 of the instruction 
     Description: Represents a 12-bit immediate value used in the operation.


## 3.S-Type():
   Used for storing instructions where data from a register is stored into memory. 
   Format:
     

     opcode
     Size: 7 bits
     Location: Bits 0 to 6 of the instruction
     Description: Identifies the type of operation (e.g., store).

     imm
     Size: 5 bits
     Location: Bits 7 to 11 of the instruction
     Description: Represents the lower 5 bits of the immediate value for address calculation.

     func3:
     Size: 3 bits
     Location: Bits 12 to 14 of the instruction
     Description: Specifies the specific type of store operation within the general category determined by the        

     rs1:
     Size: 5 bits
     Location: Bits 15 to 19 of the instruction
     Desription: Specifies the base register which holds the base address for the store operation.

     rs2:
     Size: 5 bits
     Location: Bits 20 to 24 of the instruction
     Desription: Specifies the source register which holds the data to be stored.

     Imm:
     Size: 7 bits
     Location: Bits 25 to 31 of the instruction
     Desription: Represents the upper 7 bits of the immediate value for address calculation.
     

## 4.B-Type()
   The B-type format in the RISC-V instruction set architecture (ISA) is used for conditional branch                       instructions. These instructions perform a comparison between two registers and branch to a specified                   instruction if the condition is met.
   Format:
     

      opcode:
      Size: 7 bits
      Location: Bits 0 to 6 of the instruction
      Desription: Specifies the general operation category (e.g., branch).

      imm[11]:
      Size: 1 bit
      Location: Bit 7 of the instruction
      Description: Represents the 11th bit of the immediate value used for address calculation.

      imm[4:1]
      Size: 4 bits
      Location: Bits 8 to 11 of the instruction
      Description: represents bits 4 to 1 of the immediate value used for address calculation.

      func3:
      Size: 3 bits
      Location: Bits 12 to 14 of the instruction
      Description: Specifies the specific type of branch operation (e.g., BEQ, BNE).

      rs1:
      Size: 5 bits
      Location: Bits 15 to 19 of the instruction
      Description: Specifies the first source register for the comparison.

      rs2:
      Size: 5 bits
      Location: Bits 20 to 24 of the instruction
      Description: Specifies the second source register for the comparison.

      imm[10:5]:
      Size: 6 bits
      Location: Bits 25 to 30 of the instruction
      Description: Represents bits 10 to 5 of the immediate value used for address calculation.

      imm[12]:
      Size: 1 bit
      Location: Bit 31
      Description: Represents the 12th bit of the immediate value used for address calculation.
      
## 5.U-Type
  The U-type format is used for instructions that need a 20-bit immediate value. These instructions include               LUI (Load Upper Immediate) and AUIPC (Add Upper Immediate to PC).
     
      opcode:
      Size: 7 bits
      Location: Bit 0 to 6 of the instruction
      Description: Specifies the general operation category.

      rd:
      Size: 5 bits
      Location: Bit 7 to 11 of the instruction
      Description: Specifies the destination register.

      imm[19:12]:
      Size: 8 bits
      Location: Bits 12 to 19 of the instruction
      Description: Represents bits 19 to 12 of the immediate value used for the jump target address.

      imm[11]:
      Size: 1 bit
      Location: Bit 20
      Description: Represents bit 11 of the immediate value used for the jump target address.

      imm[10:1]:
      Size: 10 bits
      Location: Bits 21 to 30 of the instruction
      Description: Represents bits 10 to 1 of the immediate value used for the jump target address.

      imm[20]:
      Size: 1 bit
      Location: Bit 31
      Description: Represents bit 20 of the immediate value used for the jump target address.


## 6.J-type
   The J-type format is primarily used for jump instructions, such as JAL (Jump and Link).
   Format:
    
      opcode:  
      Size: 7 bits
      Location: Bits 0 to 6 of the instruction
      Description: Specifies the operation. For JAL, the opcode is 1101111.

      rd:
      Size: 5 bits
      Location: Bits 7 to 11 of the instruction
      Description: Specifies the destination register where the return address (PC + 4) will be stored.

      imm[19:12](8 bit):
      Size: 8 bits
      Location: Bits 12 to 19 of the instruction
      Description: Represents bits 19 to 12 of the immediate value used for the jump target address.

      imm[11](1 bit):
      Size: 1 bit
      Location: Bits 20 of the instruction
      Description: Represents bit 11 of the immediate value used for the jump target address.

      imm[10:1](10 bits):
      Size: 10 bits
      Location: Bits 21 to 30 of the instruction
      Description: Represents bits 10 to 1 of the immediate value used for the jump target address.

      imm[20][1 bit]:
      Size: 1 bit
      Location: Bits 31
      Description: Represents bit 20 of the immediate value used for the jump target address.


       Below is the list of instructions which we have to classify
       ADD r8, r9, r10
       SUB r10, r8, r9
       AND r9, r8, r10
       OR r8, r9, r5
       XOR r8, r8, r4
       SLT r00, r1, r4
       ADDI r02, r2, 5
       SW r2, r0, 4
       SRL r06, r01, r1
       BNE r0, r0, 20
       BEQ r0, r0, 15
       LW r03, r01, 2
       SLL r05, r01, r1  

  ADD r8, r9, r10  
  #R-Type Instruction Format     
  func7 - 0000000  
  rs2 - 01010  
  rs1 - 01001  
  func3 - 000  
  rd - 01000  
  opcode - 0110011  
  ```
  0000000 01010 01001 000 01000 0110011
  ```
  SUB r10, r8, r9  
  #R-Type Instruction Format   
  func7 - 0100000  
  rs2 - 01001  
  rs1 - 01000  
  func3 - 000  
  rd - 01010  
  opcode - 0110011  

  ```
  0100000 01001 01000 000 01010 0110011
```
  AND r9, r8, r10    
  #R-Type Instruction Format    
  func7 - 0000000  
  rs2 - 01010  
  rs1 - 01000  
  func3 - 111  
  rd - 01001  
  opcode - 0110011  

  ```
0000000 01010 01000 111 01001 0110011
```

  OR r8, r9, r5  
  #R-Type Instruction Format    
  func7 - 0000000  
  rs2 - 00101  
  rs1 - 01001  
  func3 - 110  
  rd - 01000  
  opcode - 0110011  
  ```
0000000 00101 01001 110 01000 0110011
```

  XOR r8, r8, r4  
  #R-Type Instruction Format    
  funct7: 0000000  
  rs2: 00100  
  rs1: 01000  
  funct3: 100  
  rd: 01000  
  opcode: 0110011  
```
  0000000 00100 01000 100 01000 0110011
```

  SLT r0, r1, r4  
  #R-Type Instruction Format   
  funct7: 0000000  
  rs2: 00100  
  rs1: 00001  
  funct3: 010  
  rd: 00000  
  opcode: 0110011  
  ```
    0000000  00100 00001 010 00000 0110011
```


  ADDI r02, r2, 5  
  #I-Type Instruction Format  
  immediate: 000000000101  
  rs1: 00010  
  funct3: 000  
  rd: 00010  
  opcode: 0010011  
  ```
    000000000101 00010 000 00010 0010011
```


  SW r2, r0, 4  
  #S-Type Instruction Format  
  immediate: 0000000 00010  
  rs2: 00010  
  rs1: 00000  
  funct3 : 010  
  opcode : 0100011  
  ```
    0000000    00010 00000 010    00100    0100011
   ```

  SRL r06, r01, r1  
  #R-Type Instruction Format  
  funct7: 0000000  
  rs2: 00001  
  rs1: 00001  
  funct3: 101  
  rd: r6 00110  
  opcode: 0110011  
  ```
    0000000 00001 00001 101 00110 0110011
```

  BNE r0, r0, 20  
  #B-Type Instruction Format  
  immediate: 000000 01000 0  
  rs2: 00000  
  rs1 : 00000  
  funct3 : 001  
  opcode : 1100011  
  ```
    0000000 00000 00000 001 01000 1100011
```


  BEQ r0, r0, 15  
  #B-Type Instruction Format  
  immediate: 000000 01111 0  
  rs1: 00000  
  rs2: 00000  
  funct3: 000  
  opcode: 1100011  
  ```
  0000000 00000 00000 000 01111 1100011
```


  LW r03, r01, 2  
  #I-type instruction format:  
  immediate: 000000000010  
  rs1: 00001  
  funct3: 010  
  rd: 00011  
  opcode: 0000011  
  ```
    000000000010 00001 010 00011 0000011
```

  SLL r05, r01, r1  
  #R-Type instruction format:  
  funct7: 0000000  
  rs2: 00001  
  rs1: 00001  
  funct3: 001  
  rd: 00101  
  opcode: 0110011  
  ```
0000000 00001 00001 001 00101 0110011
```
  

   


















   
   
    

     
     
     

     

     

   
   

   







