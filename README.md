# RISCV
## Day-1
## Introduction to RISC-V ISA and GNU compiler toolchain

### Tool Installation
Install the dependencies using the following command :
```
sudo apt-get install libboost-all-dev
```

**Steps to install the toolchain**
```
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
cd riscv_workshop_collaterals
chmod 777 run.sh
./run.sh
cd ~/riscv_toolchain/iverilog/
git checkout --track -b v10-branch origin/v10-branch
git pull 
chmod 777 autoconf.sh 
./autoconf.sh 
./configure 
make
sudo make install 
```

Once the toolchain is installed it is necessary to create a PATH variable in .bashrc file. To create the path variable follow the steps given below :

```
gedit .bashrc
```

Type at last line
```
export PATH="/home/nitish/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH" 
```
close the .bashrc file and type in terminal
```
source .bashrc
```
### Introduction to RISC-V ISA
* RISC-V ISA is a base integer ISA and must be present in any implemenatation along with some optional extension. * The RISC-V has been designed to support extensive customization and specialization which can be extended with one or more optional instruction-set extensions, but the base integer instructions cannot be redefine.
* The different instructions included in RISC-V are listed below.

* Pseudo instructions - For e.g- mv,li,ret etc
* Base integer instruction (RV64I, RV32I)-For e.g-lui,addi etc
* Multiply extension (RV64M) -For e.g- mulw,divw etc
* Single and double floating point instruction (RV64F, RV64D) -For e.g- flw,fadd etc
* Application binary instruction
* Memory allocation and stack pointer
* The detail of the RISC-V instructions set manual can be found here.

* Each base integer set is characterized by the width of the register (XLEN) and size of the user address space. * * The most important advantage of RISC-V is that it is an open standard instruction which is easily available for academics and commercial purposes free of cost.




### Illustration of the RISC-V gnu toolchain

#### O1 mode 
Consider the simple C program given below which calculates the sum of the number form 1 to n. 

```
#include<stdio.h>
int main()
{
    int i ,sum=0,n=5;
    for (i=1;i<=n;i++)
        sum+=i;
    printf("The sum of numbers from 1 to %d is %d\n",n,sum);
    return 0;
}
```
![fig-1](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-1.png)



### Data Representation
![fig-2](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-2.png)


In RISC-V and computer architecture in general, several terms relate to data representation and storage. Let's explore them:

1. **Byte:** - A byte is the fundamental unit of data storage and representation in computers. It consists of 8 bits and can represent a single character or value.

2. **Word:** - A word typically refers to the natural data size that a processor operates with. In RISC-V, the term "word" can vary based on the architecture. For example, in RV32 (32-bit architecture), a word is 4 bytes (32 bits), while in RV64 (64-bit architecture), a word is 8 bytes (64 bits).

3. **Double Word:** - A double word is twice the size of a word. In RISC-V, for example, in RV32, a double word is 8 bytes (64 bits), and in RV64, a double word is 16 bytes (128 bits).

4. **Least Significant Bit (LSB):** -  The least significant bit is the lowest-order bit in a binary representation as shown in above figure. 

5. **Most Significant Bit (MSB):** -  The most significant bit is the highest-order bit in a binary representation as shown in above figure. It has the greatest influence on the overall value of a number. The MSB is the bit that represents the largest power of two.


6. **Endianess:** - Endianess refers to how multi-byte data is stored in memory. In a big-endian system, the most significant byte is stored at the lowest memory address, while in a little-endian system, the least significant byte is stored at the lowest memory address. RISC-V supports both big-endian and little-endian modes.

7. **Byte addressing** -  is a memory addressing scheme used in computer systems to identify and access individual bytes of data within the computer's memory. In byte addressing, each individual byte in the memory has a unique address, allowing direct access to and manipulation of single bytes of data. In RISC-V, like in many other computer architectures, memory is byte-addressable.

Understanding of these terms is crucial when working with data representation, memory allocation, and programming in computer systems, including the RISC-V architecture.


### Representation of Signed and Unsigned Numbers
#### Unsigned Numbers
* Unsigned numbers don’t have any sign, these can contain only magnitude of the number.
*  So, representation of unsigned binary numbers are all positive numbers only.
* Since there is no sign bit in this unsigned binary number, so N bit binary number represent its magnitude only.
* Zero (0) is also unsigned number.
* Every number in unsigned number representation has only one unique binary equivalent form, so this is unambiguous representation technique.
* The range of unsigned binary number is from  **0 to ((2^n)-1)**.

#### Signed Numbers
* Generally 2's complement representation is used for the signed numbers.
* 2’s complement of a number is obtained by inverting each bit of given number plus 1 to least significant bit (LSB).
* So, positive numbers are represented in binary form and negative numbers are represented in 2’s complement form. * There is extra bit for sign representation.
* If value of sign bit is 0, then number is positive and you can directly represent it in simple binary form, but if value of sign bit 1, then number is negative and 2’s complement of given binary number should be taken.
* In this representation, zero (0) has only one (unique) representation which is always positive. The range of 2’s complement form is from  **(-2^(n-1))  to ((2^(n-1))-1)**.

### Illustration of Signed and Unsigned Numbers in RISC-V
#### Unsigned Numbers

Consider the C code given below which demostrates the maximum unsigned number the RV64I can store. 

```
#include<stdio.h>
#include<math.h>

int main()
{
    unsigned long long int max = (unsigned long long int)(pow(2,64)-1); //Line 1
    // unsigned long long int max = (unsigned long long int)(pow(2,127)-1);// Line 2
    // unsigned long long int max = (unsigned long long int)(pow(2,64)*-1);// Line 3
    // unsigned long long int max = (unsigned long long int)(pow(-2,64)-1);// Line 4
    // unsigned long long int max = (unsigned long long int)(pow(-2,63)-1);// Line 5
    // unsigned long long int max = (unsigned long long int)(pow(2,10)-1);// Line 6
    printf("Highest number represented by unsigned long long  int is %llu \n",max);
    return 0;
}
```

- Line 1 will execute and give the result of (2^64)-1.</br>
- Line 2 will execute and give the result of (2^64)-1 instead of (2^127)-1 since the maximum unsigned value that can be stored in the 64 bit register is (2^64)-1.</br>
- Line 3 will execute and give the result of 0 instead of -(2^64) since the minimum unsigned value that can be stored in 64 bit register is 0.</br>
- Line 4 will execute and give the result of 0 instead of (2^64)-1.</br>
- Line 5 will execute and give the result of 0 instead of -(2^64) since the minimum unsigned value that can be stored in 64 bit register is 0.</br>
- Line 6 will execute and give the result of 1024 since the value of max is less that (2^64)-1.

To compile and execute the C code in RISC-V gnu toolchain follow the steps given below:

```

riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o unsigned.o unsigned.c 
spike  pk unsigned.o 
```

![fig-3](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-3.png)



#### Signed Numbers

Consider the C code given below which demostrates the maximum and minimum signed number the RV64I can store. 


```
#include<stdio.h>
#include<math.h>

int main()
{
    long long int max = (long long int)(pow(2,63)-1);
    long long int min = (long long int)(pow(-2,63));
    printf("Highest number represented by long long  int is %lld \n",max);
    printf("Smallest number represented by long long  int is %lld \n",min);
    return 0;
}
```
To compile and execute the C code in RISC-V gnu toolchain follow the steps given below:

```

riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o signedHighest.o unsignedHighest.c 
spike  pk signedHighest.o
```
to result use following command
```
spike pk signedHighest.o
```

## Day - 2 : Introduction to ABI and Basic Verification Flow
### Introduction to Application Binary Interface
* The application program can directly access the registers of the RISC V architecture using something known as system calls.
* The ABI also known as system call interface enables the application to access the hardware resources via registers.  
* In RISC V architecture, the width of the register is defined as XLEN.
*  For RV64 and RV32, the widths are 64 bits and 32 bits, respectively.  
* RISC V belongs to the little endian memory addressing system, which means that the least significant byte of a word is stored in the smallest memory address.

* An Application Binary Interface is a set of rules enforced by the operating system on a specific architecture.
*  So, Linker converts relocatable machine code to absolute machine code via ABI interface specific to the architecture of machine.
*  Just like how application program interface (API) is used by application programs to access the standard libraries, an application binary interface or system  call interface is utilised to access hardware resources .
*  The ISA is inherently divided into two parts: *User & System ISA* and *User ISA*  the latter is available to the user directly by system calls. 
  
Now, how does the ABI access the hardware resources? 
- It uses different registers(32 in number) which are each of width `XLEN = 32 bit` for RV32 (~`XLEN = 64 for RV64`) . On a higher level of abstraction these registers are accessed by their respective ABI names.
  
  For base integer instructions there are broadly 3 types of of such registers:
  - I-type : For instructions having immediate values as operands.
  - R-type : For instructions having only registers as operands.
  - S-type : For instructions used for storing operations.

So, it is system call interface used by the application program to access the registers specific to architecture. Overhere the architecture is RISC-V, so to access 32 registers of RISC-V below is the table which shows the calling convention (ABI name) given to registers for the application programmer to use.


### Load,Add And Store Instructions
```
ld x8,16(x23)
```
here ld is for load doubleword,x8 shows destination register (rd),16 is offset,x23 is source register . This is I type Instructions :

![fig-1](https://github.com/nitishkumar515/RISCV/assets/140998638/bc595942-b143-4d73-9ef5-84d56bc68a8d)

```add x8,x29,x8
```
here add is function,x8 is destination register (rd),x29 & x8 is source register. This is R type Instructions :
![fig-2](https://github.com/nitishkumar515/RISCV/assets/140998638/e6bb4e4a-2eac-4619-a810-452f0a705cf3)
```
sd x8,8(x23)
```
here store is store doubleword,x8 is data registers,8 tell offset(immediate) ,x23 is source register. This is S type Instructions :
![fig-3](https://github.com/nitishkumar515/RISCV/assets/140998638/173fc3fc-30c2-44dc-b348-e96b7e4869b3)

Here in each Instructions set we can see register are of 5 bits so total number of register = 2^5 = 32 registers
![fig-4](https://github.com/nitishkumar515/RISCV/assets/140998638/702ecc78-cc1d-44dd-94d3-dbc0b1c15f59)

![fig-5](https://github.com/nitishkumar515/RISCV/assets/140998638/3ab4d12c-a9f6-49f5-8fe0-de058596605e)


### Example of ABI
Consider the C code given below which calculates the sum from 1 to 9 :
```
#include<stdio.h>

extern int load(int x, int y);

int main()
{
    int result = 0;
    int count = 9;
    result = load(0x0,count+1);
    printf("Sum of numbers from 1 to %d is %d\n",count,result);
    
}
```

Consider the assembly code (ASM) given below :
```
.section .text 
.global load
.type load, @function

load:
    add a4, a0, zero
    add a2, a0, a1
    add a3, a0, zero
loop : add a4, a3, a4
       addi a3, a3, 1
       blt a3, a2, loop
       add a0, a4, zero
       ret
```


How to Perform
```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o custom1_to9.o custom1_to_9.c load.S
riscv64-unknown-elf-objdump -d custom1_to9.o | less
spike pk custom1_to9.o
```

**Outputs of the Lab**
![Screenshot from 2023-08-21 09-05-21](https://github.com/nitishkumar515/RISCV/assets/140998638/e1e88f7a-846d-4a65-9829-978ab6ce135d)

![Screenshot from 2023-08-21 09-11-48](https://github.com/nitishkumar515/RISCV/assets/140998638/065967eb-00f4-4aa7-b9fc-42893f50a8de)


### Lab to run c program on RISC-V CPU
```
cd ~/riscv_workshop_collaterals/labs/
chmod 777 rv32im.sh
./rv32im.sh  # Contains necessary commands to convert C to hex
```
**Output, Script(rv32im.sh) and firmare.hex**
![Screenshot from 2023-08-21 10-01-15](https://github.com/nitishkumar515/RISCV/assets/140998638/c259d0e9-25c7-48e2-b804-d73845d13996)

Input hex file to sent through verilog code:

firmware.hex:
![Screenshot from 2023-08-21 10-07-21](https://github.com/nitishkumar515/RISCV/assets/140998638/26a33bb5-f935-46e4-a46d-a4f3aeebce4a)

firmware32.hex:
![Screenshot from 2023-08-21 10-09-05](https://github.com/nitishkumar515/RISCV/assets/140998638/c9f03098-910c-4482-8c9f-f074fccce32c)



