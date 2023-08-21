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

git checkout --track -b v10-branch origin/v10-branch

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



  <details>
	  <summary><strong>Integer Number Representation</strong>  </summary>


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

        


Here below we can see the representation of unsigned numbers in 64 bit architecture.



The format and memory for different data types are given below.


</details>

<details>
    
  </details>
  
<details>
