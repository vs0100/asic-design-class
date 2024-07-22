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


Lab2
In Lab1 we compiled and executed our C code on gcc compiler, also in the second task we compiled our code on riscv compiler.
In this lab we will compile, execute and debug C code using riscv Simulator in Spike Simulation environment

Steps:
1.We executed our code using gcc compiler and spike simulator to check if both have the same result.
  To compile code in Spike Simultor we use the following command:
  
  riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c 
  
  To run code in Spike Simulator we use the following command:
  
  spike pk sum1ton.o
  
  Now we compared our gcc output and riscv output wich came out same.
  
  

2. We tried to debug our code using spike by using the following command:
   spike -d pk sum1ton.o
   Complete assembly code is shown into the terminal


3.We want debug only the main part of the code manually so all the part which was before that we executed using the command
    until pc 0 100b0





4.After this we wanted to run each step manually and noting down the values which are changing in each registers.
  First, we checked the values of register a2 by using the command
    reg 0 a2
  Initially it had 0x00000000000000b0
  After this we executed one line of code by pressing Enter so that the next line gets executed.
  The next was lui a2 0x1 which load the upper immediate that is the the bits from 12 to 31 with value 1 i.e, 0x00000000000100b0
  Again we checked the value of the register which now contained the changed value.



5. After this in the next instruction we loaded register a0 with value 0x21 and checked it.

6. The next instruction decremented the value present in the stack pointer by 16.
  Initially it had
  After the execution of instruction it had 
    













