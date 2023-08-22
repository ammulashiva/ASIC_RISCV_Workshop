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
	<summary><strong>Introduction</strong></summary>

Transaction-Level Verilog (TL-Verilog) is an extension of traditional Verilog, a hardware description language (HDL) used for designing digital electronic systems. TL-Verilog was developed by a company called Redwood EDA and is designed to improve the productivity and ease of designing digital systems, especially for larger and complex designs.

TL-Verilog introduces several key concepts and features that aim to simplify the design process and make it more accessible to a wider range of engineers, including those who might not have extensive experience in digital design. Some of the main features of TL-Verilog include:

**Transaction-Level Modeling:** In traditional Verilog, designers work with low-level signals and logic gates. TL-Verilog, on the other hand, operates at a higher level of abstraction by allowing designers to describe designs using transaction-level semantics. This means that designers can focus on describing the functional behavior of their designs rather than worrying about low-level implementation details.

**Simplified Parallelism:** TL-Verilog introduces constructs that make it easier to describe parallelism in designs. For instance, the "fsm" block allows designers to specify finite state machines in a more intuitive way, making it simpler to design complex control logic.

**Pipeline Abstraction:** TL-Verilog enables easy description of pipelined designs, which are common in modern digital systems. This abstraction makes it straightforward to express designs with stages that process data sequentially.

**Untimed Abstraction:** Designs in TL-Verilog can be specified in an untimed manner, focusing on the relative timing of operations rather than precise clock cycles. This abstraction is particularly useful for describing algorithms and high-level behavior.

**Automatic Pipelining:** TL-Verilog compilers can automatically insert pipeline stages based on the provided design description. This simplifies the process of designing and optimizing pipelined systems.

**Hierarchical Design:** TL-Verilog encourages hierarchical design by allowing the description of modules with clearly defined interfaces. This helps manage the complexity of larger designs.
 
</details>


<details>
	<summary><strong>Combinational Logic in TL Verilog</strong></summary>

Makerchip IDE

Makerchip is a free online environment for developing high-quality integrated circuits. You can code, compile, simulate, and debug Verilog designs, all from your browser. Your code, block diagrams, and waveforms are tightly integrated.

### Loading pythagorean Example on Makerchip IDE

![Screenshot from 2023-08-21 14-52-02](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/3369a259-97df-4105-81ad-9c041822fbf3)

### AND Gate Example on Makerchip IDE

we start with understanding the Makerchip IDE platform by trying some basic digital logic gate with And Gate being the standard one. In TL verilog we simply code the logic itself viz $out = $in1 & $in2  without requiring to declare the variables separately and $in assignment is also not required. The output of the above is as shown in figure below. We note that simultaneous highlighting of the variable is possible at the output.

![Screenshot from 2023-08-21 17-11-47](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/9362e65a-410e-48ee-81f4-18f6439f0472)

### Lab On Understanding Usage Of Vector

![Screenshot from 2023-08-21 17-12-01](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/939e7af2-9959-401e-ad73-64fc378b77b0)

### Multiplexer on Makerchip IDE

![Screenshot from 2023-08-21 17-12-13](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/ce50a98f-1688-4d7d-99b2-aa2afd4b8919)

### Calculator on Makerchip IDE

Now a lab on combinational calculator is implemented that can perform +, -, *, / on two input values. The snapshot of the code, waveform and diagram is as shown below.

![Screenshot from 2023-08-21 17-12-24](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/b362edfc-e98f-4f4b-86de-08930abb4041)
 
</details>

<details>
	<summary><strong> Sequential Logic in TL Verilog</strong></summary>

Sequential logic refers to a type of digital logic circuit or system in which the output depends not only on the current inputs but also on the previous states of the circuit. Unlike combinational logic, which only considers the current inputs to generate outputs, sequential logic incorporates memory elements to store information and generate outputs based on both current inputs and past history.

## Lab On Free Running Counter

![Screenshot from 2023-08-21 16-33-29](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/1f839c83-130b-4a1e-bf92-74506251b680)

Output:

![Screenshot from 2023-08-21 16-32-42](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/461bff85-15fb-490d-840f-8fffe8fec0bc)

## Sequential Calculator To Remembers Previous Results For Next Calculations 

The sequential calculator is implemented where the output of the previous stage serves as the input of this stage. The snapshot of the sequential calculator is included below and the code is provided 

![Screenshot from 2023-08-21 16-34-03](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/bb01883c-16e3-4b6d-8bf5-af107168b6e2)

```
$val1[31:0] = >>1$out[31:0];
         $val2[31:0] = $rand2[3:0];
         $op[1:0] = $rand3[1:0];
   
         $sum[31:0] = $val1[31:0] + $val2[31:0];
         $diff[31:0] = $val1[31:0] - $val2[31:0];
         $prod[31:0] = $val1[31:0] * $val2[31:0];
         $quot[31:0] = $val1[31:0] / $val2[31:0];
   
         $out[31:0] = $reset ? 32'b0 : (($op[1:0]==2'b00) ? $sum :
                                       ($op[1:0]==2'b01) ? $diff :
                                          ($op[1:0]==2'b10) ? $prod : $quot);
   
   `BOGUS_USE($out);
   `BOGUS_USE($reset);

```

Output:

![Screenshot from 2023-08-21 16-58-34](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/f18bd3ef-228d-45c6-8e5e-580137359356)


</details>
<details> <summary ><strong>Pipelining</strong></summary>
	
**Pipelining** or **timing abstract** is an important feature in TL verilog as it can be implemented very easily with fewer codes as compared to system verilog which reduces bugs to a great extent. An example of the pipeling for pythogoras theorem using both TL verilog and system verilog in this repo . In TL verilog pipeling can be implemented by defining the pipeline as |calc and the different pipeline stages should be properly align and are indicated by @1, @2 and so on.

### Lab To Compute Total Distance 

![Screenshot from 2023-08-21 17-09-18](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/706712af-9058-4c31-96fa-746fded6427b)

In the above implenation, we can observe the errors in the pipeline:


![Screenshot from 2023-08-21 17-09-44](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/211ec635-930a-4856-a3ed-f458ea04403c)


### Lab On Counter and Calculator in Pipeline

Block diagram : 

![Screenshot from 2023-08-21 17-04-03](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/9a813a8f-337c-4956-90c6-2501fb6a9339)

```
   $reset = *reset;
   
   |calc
      @1
         $val1[31:0] = >>1$out;
	 $val2[31:0] = $rand2[3:0];

         $sum[31:0] = $val1+$val2;
         $diff[31:0] = $val1-$val2;
         $prod[31:0] = $val1*$val2;
         $quot[31:0] = $val1/$val2;

         $out[31:0] = $reset ? 0 : ($op[1] ? ($op[0] ? $div : $prod):($op[0] ? $diff : $sum));
         
         $cnt[31:0] = $reset ? 0 : >>1$cnt + 1; 

```

Output:

![Screenshot from 2023-08-21 17-03-39](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/e711bcc2-8433-4cbc-b7a7-ddb828ace3e1)

### Lab On 2 Cycle Calculator

Below the snapshot of the pipeline sequential calcuator is included. Here the first pipeline stage consists of the input followed by arithimetic operation in the second pipeline stage and finally the ouput is included 2 cycles ahead in the third pipeline stage.

```
|calc
      @1
         $reset = *reset;
         
         
         $val1[31:0] = >>2$out[31:0];
         $val2[31:0] = $rand2[3:0];
         $op[1:0] = $rand3[1:0];
         $sum[31:0] = $val1[31:0] + $val2[31:0];
         $diff[31:0] = $val1[31:0] - $val2[31:0];
         $prod[31:0] = $val1[31:0] * $val2[31:0];
         $quot[31:0] = $val1[31:0] / $val2[31:0];
         
         $num = $reset ? 0 : >>1$num+1;
      @2   
         $out[31:0] = ($reset|!$num) ? 32'b0 : (($op[1:0]==2'b00) ? $sum :
                                       ($op[1:0]==2'b01) ? $diff :
                                          ($op[1:0]==2'b10) ? $prod : $quot);
         
         
         
   
   `BOGUS_USE($out);
   `BOGUS_USE($reset);

```
Block diagram : 

![Screenshot from 2023-08-21 17-02-45](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/1a7591db-d8fc-42e5-b520-053544439054)


Output:

![Screenshot from 2023-08-21 16-58-34](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/f18bd3ef-228d-45c6-8e5e-580137359356)

</details>

<details>
	<summary><strong>Validity</strong></summary>

Validity is another feature in TL verilog which is asserted if a particular transactions in a pipeline is valid or true. A new scope, called “when” scope is introduced for this and it is denoted as `?$valid`. This new scope has many advantages - easier design, cleaner debug, better error checking and automated clock gating.
Validity provides :
- Easier debug
- Cleaner design
- Better error checking
- Automated Clock gating
  
### Clock gating

-Why clock gating?

• Clock signals are distributed to EVERY flip-flop.
• Clocks toggle twice per cycle.
• This consumes power.

- Clock gating avoids toggling clock signals.
- TL-Verilog can produce fine-grained gating (or enables).

### Lab On Distance Accumulator with Pythagoran's theorem:

Block diagram :

![Screenshot from 2023-08-21 18-46-37](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/ca127db2-958e-429a-8399-1a694c594ca5)
```
$reset = *reset;

   |calc
      @1
         $reset = *reset;
            
      ?$vaild      
         @1
            $aa_seq[31:0] = $aa[3:0] * $aa;
            $bb_seq[31:0] = $bb[3:0] * $bb;;
      
         @2
            $cc_seq[31:0] = $aa_seq + $bb_seq;;
      
         @3
            $cc[31:0] = sqrt($cc_seq);
            
      @4
         $total_distance[63:0] = 
            $reset ? '0 :
            $valid ? >>1$total_distance + $cc :
                     >>1$total_distance;
         
```

Output:

![Screenshot from 2023-08-21 18-47-09](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/c216743f-fc02-4453-ae03-dfe42b3fedb7)

### Lab on cycle Calculator with validity 

Block Diagram :
![Screenshot from 2023-08-21 18-47-36](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/3b539fe8-9c67-4063-b88d-ebedbff18564)

![Screenshot from 2023-08-21 18-47-59](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/95ebfe2f-b17e-4d9b-9159-a165c94a6f4b)

```
|calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out [31:0];
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1 ;
         $valid_or_reset = $valid || $reset;
         
      ?$vaild_or_reset
         @1   
            $sum [31:0] = $val1 + $val2;
            $diff[31:0] = $val1 - $val2;
            $prod[31:0] = $val1 * $val2;
            $quot[31:0] = $val1 / $val2;
            
         @2   
            $out [31:0] = $reset ? 32'b0 :
                          ($op[1:0] == 2'b00) ? $sum :
                          ($op[1:0] == 2'b01) ? $diff :
                          ($op[1:0] == 2'b10) ? $prod :
                                                $quot ;
            
```

Output:


### Lab on Calculator with single value memory:

Block diagram :

![Screenshot from 2023-08-21 18-48-22](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/dccf2182-098d-446d-ac55-6bbb707cebaf)
```
 |calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out;
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1 ;
         $valid_or_reset = $valid || $reset;
         
      ?$vaild_or_reset
         @1   
            $sum [31:0] = $val1 + $val2;
            $diff[31:0] = $val1 - $val2;
            $prod[31:0] = $val1 * $val2;
            $quot[31:0] = $val1 / $val2;
            
         @2   
            $mem[31:0] = $reset ? 32'b0 :
                         ($op[2:0] == 3'b101) ? $val1 : >>2$mem ;
            
            $out [31:0] = $reset ? 32'b0 :
                          ($op[2:0] == 3'b000) ? $sum :
                          ($op[2:0] == 3'b001) ? $diff :
                          ($op[2:0] == 3'b010) ? $prod :
                          ($op[2:0] == 3'b011) ? $quot :
                          ($op[2:0] == 3'b100) ? >>2$mem : >>2$out ;
            
            
```

Output:

![Screenshot from 2023-08-21 18-48-46](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/0036a27e-951c-4e42-865d-1ca3f79052d4)

</details>

<details>
	<summary><strong>Wrap Up</strong></summary>
Here we gonna jst explore some examples given in makerchip ide 

### Conway Game Of Life

Here we can study Hierarchy Concept

![Screenshot from 2023-08-21 18-49-01](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/82235760-a82b-4afd-9246-45e16243a840)

### Pythagoran's theorem:

Block Diagram:

![Screenshot from 2023-08-21 18-49-12](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/38ec3ab0-6dbd-48a4-993c-6b04e68d8509)

```
   |calc
      
      // DUT
      /coord[1:0]
         @1
            $sq[9:0] = $value[3:0] ** 2;
      @2
         $cc_sq[10:0] = /coord[0]$sq + /coord[1]$sq;
      @3
         $cc[4:0] = sqrt($cc_sq);


      // Print
      @3
         \SV_plus
            always_ff @(posedge clk) begin
               \$display("sqrt((\%2d ^ 2) + (\%2d ^ 2)) = %2d", /coord[0]$value, /coord[1]$value, $cc);
            end


```
Output:

![Screenshot from 2023-08-21 18-49-27](https://github.com/ammulashiva/physical_design_using_asic/assets/140998900/0d02172b-c84f-4702-b81c-6400c57cdd0f)

</details>

## Day 4- Basic RISC-V CPU Micro_Architecture

<details>
	<summary><strong>Introduction to Simple RISC_V micro-architecture</strong></summary>

</details>

<details>
	<summary><strong>Fetch and Decode</strong></summary>

</details>

<details>
	<summary><strong>RISC-V Control Logic</strong></summary>

</details>



## Reference 
- https://www.vsdiat.com
- https://en.wikipedia.org/wiki/Toolchain
- https://en.wikipedia.org/wiki/GNU_toolchain
- https://github.com/riscv/riscv-gnu-toolchain
- https://github.com/KanishR1
- https://github.com/riscv-software-src/homebrew-riscv/tree/main
- https://redwoodeda.com
- https://ieeexplore.ieee.org/document/8119264
- https://github.com/shivanishah269
- https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop
- https://github.com/stevehoover
