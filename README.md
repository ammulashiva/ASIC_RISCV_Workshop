# ASIC_RISCV_Workshop

This github repository summarizes the progress made in the ASIC class about RISCV :


## Day 1-Introduction to RISC-V ISA And GNU compiler toolchain 


<details>
<summary> <strong>Installing tools</strong> </summary>
I installed the needed tools in ubuntu .
    
Steps to install Risc-tools

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
  <summary>Introduction to RISC-V ISA </summary>

- **ISA Base and extensions**:
  
RISC-V has a modular design, consisting of alternative base parts, with added optional extensions. The ISA base and its extensions are developed in a collective effort between industry, the research community and educational institutions.



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
 
Consider a c program to compile sum from 1 to n was written whose  codes given below as [sum_1to9.c]

```

#include <stdio.h>

int main () {
	int i,sum = 0, n = 9;
	for (i = 1; i <=n; ++i) {
		sum += i;
	}
	printf("The sum of the number from 1 to %d is %d\n", n,sum);
	return 0;
	}

```
- The commands used in the terminal are :
  
```

$ vim sum_1to9.c         //create and open the .c file
$ gcc sum_1to9.c
$./a.out                 //run the .exe file
$riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum_1to9.o sum)1to9.c       //compiles the .c file using riscv64i ISA and outputs object file
ls -ltr sum_1to9.o      //list the details of the file
riscv64-unknown-elf-objdump -d sum_1ton.o | less    //dumps the object file on to the screen

```
Here -01 gives 15 instructions set while -0fast gives us 12 instructions set(ubuntu) here in macos we get 15 instructions

More generic command with different options:

    `riscv64-unknown-elf-gcc <compiler option -O1 ; Ofast> <ABI specifier -lp64; -lp32; -ilp32> <architecture specifier -RV64 ; RV32> -o <object filename> <C      filename>`
    




  <details>
	  <summary>
	  Number system in RiscV
  </summary>
	  
The RISC-V architecture defines several different data types and number systems to represent and manipulate data. Here, I'll explain the basic number systems used in RISC-V:

- Binary Number System: RISC-V, like most digital systems, primarily operates on binary data. In the binary number system, numbers are represented using only two symbols: 0 and 1. Each digit in a binary number represents a power of 2. For example, the binary number "1101" represents (1 * 2^3) + (1 * 2^2) + (0 * 2^1) + (1 * 2^0) = 13 in decimal.
- Integer Representation: RISC-V supports different integer data types with varying sizes. The most common are 32-bit and 64-bit integers, denoted as "RV32" and "RV64" respectively. Integers are typically represented in two's complement form, which allows both positive and negative values to be stored and manipulated using the same hardware.
- Floating-Point Representation: RISC-V also supports floating-point operations for real numbers. Floating-point numbers are represented using a sign bit, an exponent, and a fraction (also known as mantissa). RISC-V defines different formats for floating-point numbers, including the IEEE 754 standard formats (single precision, double precision, etc.). These formats allow a wide range of values to be represented with varying levels of precision.
- Hexadecimal Notation: While binary is the fundamental representation in RISC-V, hexadecimal (base-16) notation is often used to represent binary numbers in a more human-readable form. Each hexadecimal digit represents four bits. For example, the binary number "11011010" can be represented as "DA" in hexadecimal.
- Memory Addressing: RISC-V CPUs use memory addresses to access data stored in memory. Memory addresses are typically represented in hexadecimal form. The exact memory addressing scheme depends on the specific RISC-V implementation and the memory model being used.
Overall, the RISC-V architecture provides a flexible framework for representing and manipulating different types of numbers, allowing software developers and hardware designers to efficiently perform arithmetic and logical operations on various data types within the context of RISC-V-based systems.

In computer architecture, the terms "bit," "byte," "word," and "double word" refer to different units of data storage and manipulation. These terms are used to describe the size of data that a computer's memory and processing units can handle. The specific sizes of these units can vary based on the architecture and implementation, but I'll provide you with some common interpretations:

- Bit: A bit is the smallest unit of data in computing. It can represent one of two values: 0 or 1. Bits are the building blocks of all digital information and are used to represent various types of data and instructions in a computer's memory and processing units.
- Byte: A byte is a group of 8 bits. It is the basic addressable unit of memory storage in most computer architectures. Bytes are commonly used to represent characters, numbers, and other small data elements. For example, the ASCII code for the letter 'A' is 65, which can be represented as a byte with the binary value 01000001.
- Word: The term "word" refers to the natural data size that a computer's central processing unit (CPU) can process in a single operation. The size of a word can vary between different computer architectures. In the context of x86 and x86-64 architectures, a word is typically 16 bits, while in other architectures like RISC-V, a word can be 32 bits or 64 bits. The size of a word determines the maximum amount of data that the CPU can manipulate at once, which can impact the efficiency of data processing.
- Double Word (Dword): The term "double word" (often abbreviated as "dword") is used to describe a data unit that is twice the size of a standard word. In x86 and x86-64 architectures, a double word is 32 bits, while in some other architectures, it can refer to a 64-bit value. The term "dword" is often used in the x86 family of processors to describe a 32-bit data value.
It's important to note that the exact sizes of these units can vary based on the computer architecture and implementation.
**Total Number of pattern by RV64 will be 2^64**
**RISC- doubleword can represent '0' to '(2^64 - 1)' unsigned numbers or positive numbers**

 
## Signed Numbers
To find negative number: we find 2's complement
- First the normal binary of the +ve number 
- Then we invert the number
- Then we add +1 to it
(-1,152,991,877,645,991,936)dec = (0001000000000000010000000000000100000000000000000000000000000000)bin
				  (1110111111111111101111111111111011111111111111111111111111111111)bin
											+1
  				    (1110111111111111101111111111111100000000000000000000000000000000)bin


  </details>
  
<details>
