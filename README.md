# ASIC_RISCV_Workshop

This github repository summarizes the progress made in the ASIC class about RISCV :


## Day 1-Introduction to RISC-V ISA And GNU compiler toolchain 


<details>
<summary> <strong>Installing tools</strong> </summary>
	
I installed the needed tools in ubuntu .
    
Steps to install Risc-tools
Open the terminal and type the following commands given below

```bash
sudo apt install libboost-all-dev

git clone https://github.com/kunalg123/riscv_workshop_collaterals.git

cd riscv_workshop_collaterals

chmod +x run.sh

./run.sh                                       // you may get an error, ignore it  and type the following command

cd ~/riscv_toolchain/iverilog/

git checkout                                   //track -b v10-branch origin/v10-branch

git pull 

chmod 777 autoconf.sh 

./autoconf.sh 

./configure 

make

sudo make install

```
- set the path name in bashrc file. go to the file and add the path at the bottom of the file.using the following commands:

```

gedit .bashrc                         //opens the file, type or copy the below path into the file

export PATH="/home/ammula-shiva-kumar/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH"            //Instead of ammula-shiva-kumar put your username

source .bashrc                   //save the file and close and type this cmd in terminal.

```

</details>

<details>
  <summary><strong>Introduction to RISC-V ISA </strong></summary>

ISA stands for "Instruction Set Architecture." It is a crucial concept in computer architecture that defines the set of instructions that a computer's hardware can execute. An ISA essentially serves as an interface between the hardware and the software, allowing software programs to communicate with and control the underlying hardware components of a computer.

The ISA defines various aspects of a computer's operation, including:

**Instruction Set:** The specific instructions that a computer's processor can understand and execute. These instructions can include arithmetic operations, memory operations, control flow instructions, and more.

**Data Types:** The types of data that the processor can handle, such as integers, floating-point numbers, and characters.

**Registers:** Special storage locations within the processor that are used to hold data temporarily during instruction execution. Different ISAs may have different numbers and types of registers.

**Memory Addressing:** The way in which the ISA specifies how memory locations are addressed and accessed. This includes addressing modes, which determine how operands are retrieved from memory.

**Control Flow:** How the sequence of instructions is controlled, including branching (conditional and unconditional jumps) and subroutine calls.
    
**I/O Operations:** The instructions and mechanisms for interacting with input and output devices.

There are two main types of ISAs:

**Complex Instruction Set Computer (CISC):** In CISC architectures, instructions are relatively complex and can perform multiple tasks in a single instruction. This reduces the number of instructions needed to accomplish a task but can make the hardware more complex.

**Reduced Instruction Set Computer (RISC):** RISC architectures focus on simpler instructions that are executed in a single clock cycle. RISC architectures often have a smaller set of instructions, but more instructions might be required to complete a complex task. This design simplifies the hardware and can lead to better performance and efficiency in certain scenarios.


- **ISA Base and extensions-RISC**:
  
**RISC-V** has a modular design, consisting of alternative base parts, with added optional extensions. The ISA base and its extensions are developed in a collective effort between industry, the research community and educational institutions.

![Screenshot from 2023-08-21 10-34-19](https://github.com/ammulashiva/ASIC_RISCV_Workshop/assets/140998900/291bdb7e-9a3d-491e-a975-6e077ea67bd7)

- in this Workshop the instruction set we use is the Base integer instruction set. Each base integer set is characterized by the  width  of the register (XLEN) and size of the user address space. The most important advantage of RISC-V is that it is an open standard instruction which is easily available for academics and commercial purposes free of cost. The different instructions included in RISC-V are listed below.

1. Pseudo instructions - For e.g- mv,li,ret etc
2. Base integer instruction (RV64I, RV32I)-For e.g-lui,addi etc
3. Multiply extension (RV64M) -For e.g- mulw,divw etc
4. Single and double floating point instruction (RV64F, RV64D) -For e.g- flw,fadd etc
5. Application binary instruction 
6. Memory allocation and stack pointer

The detail of the RISC-V instructions set manual can be found [here] (https://riscv.org).



</details>

<details>
	
  <summary><strong>Lab Work for Software ToolChain</strong></summary>

 **Example_1**
 
Consider a c program to compile sum from 1 to n was written whose  codes given below as [sum_1ton.c]

```

#include <stdio.h>

int main () {
	int i,sum = 0, n = 5;
	for (i = 1; i <=n; ++i) {
		sum += i;
	}
	printf("The sum of the number from 1 to %d is %d\n", n,sum);
	return 0;
	}

```
- The commands used in the terminal are :
  
```

$ vim sum_1ton.c         //create and open the .c file

$ gcc sum_1ton.c

$./a.out                 //run the .exe file

$riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum_1ton.o sum_1ton.c       //compiles the .c file using riscv64i ISA and outputs object file

$ls -ltr sum_1ton.o      //list the details of the file

$riscv64-unknown-elf-objdump -d sum_1ton.o | less    //dumps the object file on to the screen

```
Here -01 gives 15 instructions set while -0fast gives us 12 instructions set(ubuntu) here in macos we get 15 instructions

More generic command with different options:

    `riscv64-unknown-elf-gcc <compiler option -O1 ; Ofast> <ABI specifier -lp64; -lp32; -ilp32> <architecture specifier -RV64 ; RV32> -o <object filename> <C      filename>`
    
Below figure shows the object file created for sum_1ton.c :

![Screenshot from 2023-08-21 11-08-52](https://github.com/ammulashiva/ASIC_RISCV_Workshop/assets/140998900/71315ae1-2fa5-485b-a392-d36fd24b9d82)

The following commands are used to simulate the object file :
```
spike pk sum_1ton.o         //runs the file

spike -d pk sum_1ton.o   //debugger runs instruction by instruction

```
below figure shows the simulation of the program instruction by instruction executing :

![Screenshot from 2023-08-21 11-13-58](https://github.com/ammulashiva/ASIC_RISCV_Workshop/assets/140998900/a34cd959-d0a7-4217-b008-e82d90446e8f)

</details>


<details>  
  <summary><strong>Integer Number Representation</strong></summary>


## 64 Bit Number System for unsigned numbers and signed numbers

In a 64-bit computer architecture,

**Byte:**
        A "byte" in a 64-bit architecture consists of 8 bits, just like in other architectures.
        Each byte can represent a range of values from 0 to 255 (2^8 - 1).
        Bytes are fundamental units of storage and data representation, commonly used for characters, numbers, and other small data units.
        For example, a single ASCII character like 'A' is represented by a byte.

**Word:**
        In a 64-bit architecture, a "word" typically refers to a unit of data that is 64 bits, or 8 bytes.
        This size is often chosen to match the size of the processor's general-purpose registers, enabling efficient processing of data.
        Words are commonly used for integer arithmetic, memory addressing, and data manipulation operations.

**Double Word:** In a 64-bit architecture, a "double word" is a unit of data that is twice the size of a word, hence 128 bits or 16 bytes.Double words are used for larger data structures, floating-point numbers, and certain specialized operations that require more storage space.

        
![Screenshot from 2023-08-21 11-54-55](https://github.com/ammulashiva/ASIC_RISCV_Workshop/assets/140998900/e23174aa-f94a-4055-9164-ac3665acabe9)


Here below we can see the representation of unsigned numbers in 64 bit architecture.

![Screenshot from 2023-08-21 11-55-13](https://github.com/ammulashiva/ASIC_RISCV_Workshop/assets/140998900/20c7e992-1c74-463d-83c1-f2d834bedbf1)

The format and memory for different data types are given below.

![Screenshot from 2023-08-21 11-56-58](https://github.com/ammulashiva/ASIC_RISCV_Workshop/assets/140998900/21a0dd58-5f49-4558-895f-13b49c08b3e0)

**Example_1**

Consider an example to show the maximum value of an unsigned long long integer :

```bash

#include<stdio.h>
#include<math.h>

int main()
{
        unsigned long long int max = (unsigned long long int)(pow(2,64) -1);
        printf("maximum of unsigned long long int is %llu \n ", max) ;
        return 0 ;
}

```
Below is the figure showing the execution of above program :

![Screenshot from 2023-08-21 12-05-19](https://github.com/ammulashiva/ASIC_RISCV_Workshop/assets/140998900/88c33ad0-2894-4ac3-81f8-a71420fb6e3c)

**Example_2**

Consider an example to show the maximum value of an unsigned long long integer :

```bash

#include<stdio.h>
#include<math.h>

int main()
{
        long long int max = ( long long int)(pow(2,63) -1);
         long long int min = ( long long int)(pow(2,63) *-1);

        printf("maximum of unsigned long long int is %lld \n ", max) ;
         printf("minimum of unsigned long long int is %lld \n ", min) ;

        return 0 ;
}

```

Below is the figure showing the execution of above program :

![Screenshot from 2023-08-21 12-13-07](https://github.com/ammulashiva/ASIC_RISCV_Workshop/assets/140998900/9e38eac2-8d84-4fff-b578-2a9fd920045c)


here you can experiment the above program for the signed and unsigned int by changin the power value greater than 64 , and also by changing the format of integer:


</details>

## Day 2 - Introduction to ABI and Basic Verification Flow

<details>
	<summary><strong>Application Binary Interface</strong></summary>

 ## Application Binary Interface

The Application Binary Interface (ABI) for the RISC-V architecture defines a set of rules and conventions for the interaction between software components at the binary level. It encompasses how functions are called, how data is represented and manipulated, how memory is managed, and how system calls are made. The RISC-V ABI ensures compatibility and interoperability between different software modules, making it possible for programs to work seamlessly on different systems that adhere to the same ABI.

![Screenshot from 2023-08-21 12-31-22](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/32abbb59-73c4-4ef4-9340-6a5300c6b8c1)

### Memory Allocation 

There are two different ways to load the data into registers,the data can be loaded directly to registers but as we dont have large number of registers, we can load the data into the memory and then from memory we can load the data into the registers.

![Screenshot from 2023-08-21 12-31-43](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/e4a04c1c-0c8d-4c67-ad7d-8a99422e6496)

RiscV belongs to little-endian memory addressing system.Little-endian memory addressing is a way of organizing and storing data in computer memory where the least significant byte (LSB) of a multi-byte value is stored at the lowest memory address, while the most significant byte (MSB) is stored at a higher memory address.This byte order is opposite to big-endian memory addressing.

![Screenshot from 2023-08-21 12-32-17](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/56b024dd-d94b-41db-a961-4344dc0ec991)

**ld:** This mnemonic stands for "load double-word." It's an instruction used to load a 64-bit (8-byte) value from memory into a register.

**x8:** This is the destination register where the loaded value will be stored. In RISC-V assembly language, registers are denoted by the "x" prefix followed by a number (e.g., x0, x1, x2, ..., x31).

**16:** This is the immediate offset value, which indicates the offset from the address stored in register x23. The offset is added to the address in x23 to calculate the memory address from which the value will be loaded.

**(x23):** This indicates that the address to be used for loading the value is stored in register x23. x23 is the base register, and the offset is added to its value to compute the effective memory address.
    
![Screenshot from 2023-08-21 12-32-54](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/4fb07b77-b0e1-46db-94de-beeaa1f9912a)

The format of instruction can be seen below.

![Screenshot from 2023-08-21 12-33-28](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/9690b9ec-54ce-4fab-8e0f-6e7c6949034c)

The add instruction below performs the addition operation by adding the values stored in registers x24 and x8 together. The result of the addition is then stored back in register x8, overwriting the previous value.

![Screenshot from 2023-08-21 12-34-01](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/8e192c94-5f53-4a1e-af0a-6f58a5446119)

![Screenshot from 2023-08-21 12-34-44](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/1f3da0f3-067a-4ea8-8c03-0f9c5b20faff)

The below image shows the different ABI names,registers and its usages.

![Screenshot from 2023-08-21 12-35-13](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/a7f233a0-5ea2-45b0-92d8-652388ccdb7b)

</details>

<details>
	<summary><strong>Lab Work using ABI function calls</strong></summary>

Let us perform the lab to rewrite c program in asm language.The main c program passes a0 and a1 to ASM block and the ASM returns a0 back to the main c program.

![Screenshot from 2023-08-21 12-46-06](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/fbdf6af9-86ec-477e-bff9-d06a9ae0d423)

Here we develope an alogorithm to count the sum of numbers from point **a(a0)** to point **b(a1)** .The algorithm for the operation of program can be seen below.

![Screenshot from 2023-08-21 12-46-28](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/a3426bdb-68ee-48fe-897a-0363b301a747)

**Example_1**

Consider 1to9_custom.c file shown below : 

```bash

#include<stdio.h>

extern int load(int x, int y);

int main() {
        int result = 0 ;
        int count = 9 ;
        result = load(0x0, count+1 );
        printf("Sum of numbers from 1 to %d is %d \n ",count , result );
}

```
This is load.S file

```bash

.section .text
.global load
.type load, @function

load:
        add  a4, a0, 0x0
        add  a2, a0, a1
        add  a3, a0, 0x0
loop:   add  a4, a3, a4
        addi a3, a3, 1
        blt  a3, a2, loop
        add  a0, a4, 0x0
        ret

```

Use the below commands to run the above example :

```
$riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o 1to9_custom.o 1to9_custom.c load.S
$spike pk 1to9_custom.o
$riscv64-unknown-elf-objdump -d 1to9_custom.o | less

```

Below is the screenshot showing the exicution of aboe program :

![Screenshot from 2023-08-21 12-55-26](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/552aec3a-36c5-4dcb-984c-52a1f3980d47)

Below is the Screen Shot showing the object file created :

![Screenshot from 2023-08-21 12-55-00](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/747595cf-9037-4c53-9de1-935cf4c9d7db)

## Lab to run C program on RISC-V CPU


![Screenshot from 2023-08-21 14-15-11](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/3be5916d-cf5d-4a98-9f1e-e5a05bcd84cf)

Here we have riscv cpu program code through which we send the HEX format file of c program to show output the output of the given code 

```
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
cd riscv_workshop_collaterals
cd labs
chmod 777 rv32im.sh
./rv32im.sh 

```
![Screenshot from 2023-08-21 14-09-41](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/641a84d6-5b1d-42dc-8cf6-cb77e5aa0d34)

Input hex file to sent through verilog code:

firmware.hex:

![Screenshot from 2023-08-21 14-16-39](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/45c06406-a921-4f49-84bf-8ee43867f7e2)

firmware32.hex:

![Screenshot from 2023-08-21 14-16-54](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/1d58b210-0e5c-4901-9c6a-590d6acac451)

</details>

## Day 3- Digital Logic With TL Verilog and Makerchip 

<details>
	<summary><strong>Combinational Logic in TL Verilog</strong></summary>


 
</details>



