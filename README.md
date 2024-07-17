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

