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

![fig-1](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-2-1.png)

```add x8,x29,x8
```
here add is function,x8 is destination register (rd),x29 & x8 is source register. This is R type Instructions :
![fig-2](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-2-2.png)
```
sd x8,8(x23)
```
here store is store doubleword,x8 is data registers,8 tell offset(immediate) ,x23 is source register. This is S type Instructions :
![fig-3](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-2-3.png)

Here in each Instructions set we can see register are of 5 bits so total number of register = 2^5 = 32 registers
![fig-4](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-2-4.png)

![fig-5](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-2-5.png)

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
spike pk custom1_to9.o
riscv64-unknown-elf-objdump -d custom1_to9.o | less
```

**Outputs of the Lab**
![fig-6](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-2-6.png)

![fig-2-7](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-2-7.png)


### Lab to run c program on RISC-V CPU
```
cd ~/riscv_workshop_collaterals/labs/
chmod 777 rv32im.sh
./rv32im.sh  # Contains necessary commands to convert C to hex
```
**Output, Script(rv32im.sh) and firmare.hex**
![fig-2-8](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-2-8.png)

* Input hex file to sent through verilog code:
* firmware.hex:

![fig-2-9](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-2-9.png)

firmware32.hex:

![fig-2-10](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-2-10.png)

## Day - 3 : Digital Logic with TL-Verilog and Makerchip
### Introduction 
* You can code, compile, simulate, and debug Verilog designs, all from your browser.
* Makerchip is a free online environment for developing high-quality integrated circuits.
* Your code, block diagrams, and waveforms are tightly integrated.
* Projects on Makerchip can be completely designed using TL-Verilog.
* Transaction Level - Verilog standard is an extension of Verilog which has various advantages like simpler syntax, shorter codes and easy pipelining
  
* All the examples shown below are done on Makerchip IDE using TL-verilog
### Lab for Combinational logic
Makerchip IDE
Makerchip is a free online environment for developing high-quality integrated circuits. You can code, compile, simulate, and debug Verilog designs, all from your browser. Your code, block diagrams, and waveforms are tightly integrated.
## Logic Gates
### Multiplexer Using Ternary Operator
Consider the verilog code for multiplexer gicen below.
```
assign f = s ? x1 : x0;
```
This code uses ternary operator that will realize a simple 2:1 multiplexer hardware in which the output f follows x1 if s is 1 otherwise it will follow x0. The harware and logic gate representation l is shown below : 

![fig-31](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-31.png)

The higher bit multiplexers can also be realized using the coditional operator. Consider the 4:1 multiplexer code given below :
```
assign f = sel[0] ? a : (sel[1] ? b : (sel[2] ? c : d));
```
This code creates a priority for the inputs with input a getting the highest and input d getting the least. Instead of realizing as a single 4:1 multiplexer it will create a series of 2:1 multiplexers. In this case the sel is a one hot vector i.e, only one of the bit in the sel will be high at a time. The hardware realization is shown below :

![fig-32](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-32.png)

### AND Gate Example on Makerchip IDE
The TL-Verilog code is shown below :
```
 $out = $in1 && $in2;
```
![fig-33](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-33.png)

### OR gate
The TL-Verilog code is shown below :
```
   $out = $in1 || $in2;
```
![fig-34](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-34.png)

### Vector Addition
The TL-Verilog code is shown below :
```
   $out[4:0] = $in1[3:0] + $in2[3:0];
```
![fig-35](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-35.png)

### 2:1 Multiplexer
The TL-Verilog code is shown below :
```
   $out = $sel ? $in1 : $in0;
```
![fig-36](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-36.png)

### 2:1 Vector Multiplexer
The TL-Verilog code is shown below :
```
   $out[7:0] = $sel[2:0] ? $in1[7:0] : $in0[7:0];
```
![fig-37](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-37.png)
### Calculator
The TL-Verilog code is shown below :
```
 $reset = *reset;
   
   
   $op[1:0] = $random[1:0];
$val1[31:0] = $rand[3:0];
$val2[31:0] = $rand[3:0];

$div[31:0] = $val1/$val2;
$add[31:0] = $val1+$val2;
$mul[31:0] = $val1*$val2;
$sub[31:0] = $val1-$val2;

$out[31:0] = $op[1] ? ($op[0] ? $div : $add):($op[0] ? $mul : $sub) ;
```
![fig-38](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-38.png)
## Sequential Circuits
* sequential circuit is a type of digital circuit that employs memory elements to store information and produce outputs based not only on the current input values but also on the circuit's previous state.
 * Unlike combinational circuits, which generate outputs solely based on the present input values, sequential circuits incorporate feedback loops and memory elements like flip-flops or registers to maintain and utilize their internal state.




## Basic Sequential Circuits in Makerchip
### Fibonacci Series
The TL-Verilog code for fibonacci series is shown below :
```
 $reset = *reset;
   $num[31:0] = $reset ? 1 : (>>1$num + >>2$num);
```
The block digram of Fibonacci series is shown below :
![fig-39](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-39.png)

The block diagram of the fibonacci series generator using makerchip IDE shown below :
![fig-40](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-40.png)
### Free running counter
The TL-Verilog code for free running counter is shown below :
```
   $reset = *reset;
   $cnt[31:0] = $reset ? 0 : (>>1$cnt + 1);
```
The block diagram of the free running counter is shown below :

![fig-331](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-331.png)
### Sequential Calculator
The TL-verilog code for sequential calculator is shown below :
```
 $reset = *reset;
   
   $cnt2[2:0] = $reset ? 0 : (>>1$cnt2 + 1);
   $cnt3[1:0] = $reset ? 0 : (>>1$cnt3 + 1);
   
   $op[1:0] = $cnt3;
   
   $val1[31:0] = >>1$out;
   $val2[31:0] = $cnt2;
   $sum[31:0] = $val1+$val2;
   $diff[31:0] = $val1-$val2;
   $prod[31:0] = $val1*$val2;
   $div[31:0] = $val1/$val2;
   
   $out[31:0] = $reset ? 32'h0 : ($op[1] ? ($op[0] ? $div : $prod):($op[0] ? $diff : $sum));
```
The block diagram of sequential calculator using makerchip IDE is shown below :

![fig-332](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-332.png)

![fig-333](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-333.png)

## Pipelining
* Pipelining or timing abstract is an important feature in TL verilog as it can be implemented very easily with fewer codes as compared to system verilog which reduces bugs to a great extent.
* An example of the pipeling for pythogoras theorem using both TL verilog and system verilog in this repo .
* In TL verilog pipeling can be implemented by defining the pipeline as calc and the different pipeline stages should be properly align and are indicated by @1, @2 and so on.
### Pipelined Pythagorean
The TL-Verilog code is given below:
```
m4_TLV_version 1d: tl-x.org
\SV
   `include "sqrt32.v";
   m4_makerchip_module
\TLV
   
   |calc
      
      // Pythagora's Theorem
      @1
         $aa_sq[7:0] = $aa[3:0] ** 2;
         $bb_sq[7:0] = $bb[3:0] ** 2;
      // [+] @2
         $cc_sq[8:0] = $aa_sq + $bb_sq;
      // [+] @3
         $cc[4:0] = sqrt($cc_sq);


      // Print
         \SV_plus
            always_ff @(posedge clk) begin
               \$display("sqrt((\%2d ^ 2) + (\%2d ^ 2)) = \%2d", $aa, $bb, $cc);
            end

   // Stop simulation.
   *passed = *cyc_cnt > 40;
\SV
endmodule
```
@ -- Represents the pipelined stage number.

![pipe-1](https://github.com/nitishkumar515/RISCV/blob/main/day-1/pytha.png)

Error Detection Demo
The TL-Verilog code is given below :
```
 m5_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV
   $reset = *reset;
   |comp
      @1
        $error1 = $bad_input || $illegal_optput;
   
      @2
        $error2 = $error1 || $overflow;
      @3
        $error3 = $error2 || $div_by_zero;
      
   
   //...
   
   // Assert these to end simulation (before Makerchip cycle limit).
   //*passed = *cyc_cnt > 40;
   //*failed = 1'b0;
\SV
   endmodule
```
![error-1](https://github.com/nitishkumar515/RISCV/blob/main/day-1/error.png)

### Counter and Calculator in Pipeline
The block diagram of the counter with calculator in pipeline is shown below :

The TL-Verilog code is given below :
```
 $reset = *reset;
   $op[1:0] = $random[1:0];
   $val2[31:0] = $rand2[3:0];
   
   |calc
      @1
         $val1[31:0] = >>1$out;
         $sum[31:0] = $val1+$val2;
         $diff[31:0] = $val1-$val2;
         $prod[31:0] = $val1*$val2;
         $div[31:0] = $val1/$val2;
         $out[31:0] = $reset ? 32'h0 : ($op[1] ? ($op[0] ? $div : $prod):($op[0] ? $diff : $sum));
         
         $cnt[31:0] = $reset ? 0 : (>>1$cnt + 1);
```
### 2 Cycle Calculator
The block diagram of the 2 cycle calculator is shown below:
![pipe-3](https://github.com/nitishkumar515/RISCV/blob/main/day-1/pipe3.png)

The TL-verilog code is shown below :
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
![pipe-4](https://github.com/nitishkumar515/RISCV/blob/main/day-1/pipe4.png)


### Validity
* Validity is TL-verilog means signal indicates validity of transaction and described as "when" scope else it will work as don't care. Denoted as ?$valid.
* Validity provides :
* Easier debug
* Cleaner design
* Better error checking
* Automated Clock gating
### Illustration of Validity
#### Distance Accumulator
The block diagram of the distance accumulator is shown below :

The TL-Verilog code is given below:
```
 calc
      @1
         $reset = *reset;
      ?$valid
         @1
            $aa_sq[31:0] = $aa[3:0] * $aa;
            $bb_sq[31:0] = $bb[3:0] * $bb;
         @2
            $cc_sq[31:0] = $aa_sq + $bb_sq;
         @3
            $out[31:0] = sqrt($cc_sq);
      @4
         $tot_dist[31:0] = $reset ? '0 : ($valid ? (>>1$tot_dist + $out) : $RETAIN);
```
Once the valid signal is asserted the previous value of result will be added with the current value and it result will get updated otherwise the previous value is retained.

![vailidity](https://github.com/nitishkumar515/RISCV/blob/main/day-1/vailidity.png)
## Day 4-Basic RISC-V CPU micro-architecture
### Basic RISC-V CPU microarchitecture
The block diagram of a basic RISC-V microarchitecture is as shown in figure below. Now, using the Makerchip platform the implementation of the RISC-V microarchitecture or core is done. For starting the implementation a starter code present in reference is used. The starter code consist of -

* A simple RISC-V assembler.
* An instruction memory containing the sum 1..9 test program.
* Commented code for register file and memory.
* Visualization.

![fig-411](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-411.png)

## Fetch and Decode
Designing of processor is based on three core steps 
* fetch
* decode
* execute 

Here we gonna design RiscV Cpu Core for which block diagram is given below :
![fig412](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-412.png)

## Template For Running Vizualization:
```
\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
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
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;



      // YOUR CODE HERE
      // ...

      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      //m4+imem(@1)    // Args: (read stage)
      //m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
      //m4+myth_fpga(@0)  // Uncomment to run on fpga

   //m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic. @4 would work for all labs.
\SV
   endmodule
```
## PC Logic
The Program Counter, often referred to as the "PC," is a fundamental component of a processor that keeps track of the address of the next instruction to be executed. In the RISC-V architecture, the PC is typically called "pc" or "pc_reg." Overall, the PC logic is crucial for the control flow of a program. It determines which instruction will be executed next and how the program progresses. RISC-V, as a RISC (Reduced Instruction Set Computer) architecture, emphasizes simplicity and regularity in its design, which extends to its PC handling mechanisms.
![fig413](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-413.png)
```
|cpu
      @0
         $reset = *reset;
         
         $pc[31:0] = >>1$reset ? 32'b0 : >>1$pc + 32'd4;
         
         
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   ```

output :

![fig414](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-414.png)
### fetch
During the fetch stage, processors fetches the instruction from the memory to the address pointed by the program counter. The program counters holds the address of the next stage, in our case it is after 4 cycle and the instruction memory holds the set of instruction to be executed. The snapshot of the fetch stage is shown below.

Fetch Block diagram part 1:
![fig415](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-415.png)
```
|cpu
      @0
         $reset = *reset;
         
         $pc[31:0] = >>1$reset ? 32'b0 : >>1$pc + 32'd4;
         
         
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      //m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic

```

output :

![fig416](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-416.png)

viz: 

![fig-417](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-417.png)

Correct fetch Block Diagram :

![fig-418](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-418.png)
```
|cpu
      @0
         $reset = *reset;
         
         $pc[31:0] = >>1$reset ? 32'b0 : >>1$pc + 32'd4;
         
      @1 
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $instr[31:0] = $imem_rd_data[31:0];
         
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;   
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      //m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)
```
output:
![fig-419](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-419.png)
viz:
![fig-420](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-420.png)

### Decode
For decoding a particular instruction, it is necessary that the isntruction type and format is known to the processor. The decoding is a crucial part and has to be done properly according to the given format to avoid error. There are 6 instructions type in RISC-V :

1. Register (R) type
2. Immediate (I) type
3. Store (S) type
4. Branch (B) type
5. Upper immediate (U) type
6. Jump (J) type

![fig-421](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-421.png)

Following the decoding of the above, the instruction immediate decode for all the above, except the register type is added. The 6 others instruction format/fields including the opcode, 2 source register, destination register, funct3 and funct7 decode is included. Next the instruction field decode of the different instruction type is inserted to ensure that only valid registers are used. Finally the base instruction set decode for the various fields is incorporated.
![fig-422](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-422.png)

Instruction Type Decode Block diagram:

![fig-423](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-423.png)

```
@1
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
```
output:
![fig-424](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-424.png)

viz:

![fig-425](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-425.png)

## LAB ON INSTRUCTION IMMEDIATE DECODE
![fig-426](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-426.png)
```
		      $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
```
output:
![fig-427](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-427.png)

viz:

![fig-428](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-428.png)
## LAB ON INSTRUCTION DECODE
![fig-429](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-429.png)
```
	 $rs2[4:0] = $instr[24:20];
         $rs1[4:0] = $instr[19:15];
         $rd[4:0]  = $instr[11:7];
         $opcode[6:0] = $instr[6:0];
         $func7[6:0] = $instr[31:25];
         $func3[2:0] = $instr[14:12];

```
output:

![fig-430](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-430.png)

viz:

![fig-431](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-431.png)
## Lab To Decode Instruction Field Based
![fig-432](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-432.png)
```
	$rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
```
output:
![fig-433](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-433.png)

viz:

![fig-434](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-434.png)
## LAB ON Individual Instruction Decode
```
	 $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;

```
![fig-435](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-435.png)
output:

![fig-436](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-436.png)

viz:

![fig-437](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-437.png)
## Risc-V Control Logic
### Execute and Register file read/write
The next task is to 'read from' and 'write into' the registers. In this operation, 2 read and write operation can be carried out simulatenously. The two src1_value/src2_value takes input from the two read register rf_read_data1/ rf_read_data2 and pass it on to the ALU unit. At present, ADDI and ADD is execute whose result is obtained in register rf_write_data. The figure below shows the input and output registers.

![fig-438](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-438.png)
```
	 $rf_wr_en = 1'b0;
         $rf_wr_index[4:0] = 5'b0;
         $rf_wr_data[31:0] = 32'b0;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;

```
The snapshot of the REGISTER FILE READ operation is included below:
![fig-439](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-439.png)
## Lab on Changes on Register File Read
![fig-440](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-440.png)
```
	 $rf_wr_en = 1'b0;
         $rf_wr_index[4:0] = 5'b0;
         $rf_wr_data[31:0] = 32'b0;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
         $src1_value[31:0] = $rf_rd_data1; //new
         $src2_value[31:0] = $rf_rd_data2;
```
The makerchip implementation output:
![fig-441](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-441.png)
viz:

![fig-442](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-442.png)
### Lab on ALU
![fig-443](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-443.png)
```
$result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx ;
```
Implementation:
![fig-444](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-444.png)

viz:

![fig-445](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-445.png)
### Lab On Register File Write
![fig-446](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-446.png)

```
	 $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
```
Implementation:
![fig-447](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-447.png)

## ARRAYS
In RISC-V, arrays are typically implemented using a combination of registers and memory. Arrays can be stored in memory, where each element is stored at a specific memory address. The processor can use load and store instructions to access these elements. For example, you might load an element from an array in memory into a register, perform operations on it, and then store the result back into memory.

Alternatively, arrays can also be partially stored in registers. If an array is small enough to fit into the available registers, the elements can be loaded into registers for faster processing. This approach can improve performance for certain operations, as register access is generally faster than memory access.

In both cases, whether an array is stored in memory or registers, the RISC-V architecture provides instructions to perform operations on array elements, such as loading, storing, and arithmetic operations.

In summary, the register file in RISC-V architecture consists of a set of registers that are used for temporary storage and fast access to data, while arrays are collections of elements that can be stored either in memory or registers and are manipulated using load, store, and computation instructions.
![fig-448](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-448.png)

### Lab For Implementing Branch Instructions
The next stage in the building of the RISC-V microarchitecture, is the addition of branches. Apart from immediate addition or addition, there may certain other conditions to be satisfied which requires to direct the PC to the branch target address. Now, we have simply added few branch instruction and updated the PC. Some Branching Instructions are :
* BEQ;==
* BNE:!=
* BLT: (x1 < ×2) ^ (x1[311!=×2[311)
* BGE: (×1 >= ×2) ^ (x1[31]!=×2[31])
* BLTU: <
* BGEU: >=

  code:
  ```
  $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
         `BOGUS_USE($taken_branch)

  ```
  ![fig-449](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-449.png)
### Lab For Complementing Branch Instructions
Block Diagram:
![fig-450](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-450.png)
  code:
  ```
  	   //BRANCH INSTRUCTIONS 1
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
         `BOGUS_USE($taken_branch)
         
         //BRANCH INSTRUCTIONS 2
         $br_target_pc[31:0] = $pc +$imm;
  ```
  output:
  ![fig-451](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-451.png)
  ### Lab For Testbench
  code:
  ```
  *passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9) ;

```
![fig-452](https://github.com/nitishkumar515/RISCV/blob/main/day-1/fig-452.png)
## Day 5-Complete Pipelined RiscV CPU Micro-architecture
### Pipelining the CPU
Now pipelining of the CPU core is done, which allows easy retiming and reduces functional bug to a great extent . Pipelining allows faster computaion. For pipelining as mentioned earlier we simply need to add @1, @2 and so on. The snapshot of the pipelining is as shown below. In TL verilog, another advantage is defining of pipeline in systematic order is not necessary. More inforamtion on timming abstract can be found in the IEEE paper "Timing-Abstract Circuit Design in Transaction-Level Verilog"  by Steeve Hoover in makerchip platform itself or else [here](https://ieeexplore.ieee.org/document/8119264).
### Lab on 3 cycle valid signal
Block Diagram:
![fig-501]()

code:
```
	
$valid = $reset ? 1'b0 : ($start) ? 1'b1 : (>>3$valid) ;
         $start_int = $reset ? 1'b0 : 1'b1;
         $start = $reset ? 1'b0 : ($start_int && !>>1$start_int);
```
output:
![fig-502]()
### Lab for generating valid signals for each instruction
code:
```
//Generate valid signals for each instruction fields
         $rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct7_valid           = $is_r_instr;

```
Below is snapshot of pipelined CPU with a test case of assembly program which does summation from 1 to 9 then stores to r10 of register file. In snapshot r10 = 45. Test case:

```
*passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9);

```
![fig-503]()
### Load, store and data memory
The load and store option is also included for which a 1 read/write data memory is added. Similar to branch instruction the load also has 3 cycle delay. For checking the functionality of load and store instructions a test bench is added and the data is on address 4 of Data Memory and loaded that value from Data Memory to r17.
### Code For Lab For Register File Bypass To Address Rd-After-Wr Hazard
```
//Register file bypass logic - data forwarding from ALU to resolve RAW dependence
         $src1_value[31:0] = ((>>1$rf_wr_en) && (>>1$rd == $rs1 )) ? (>>1$result): $rf_rd_data1; 
	 $src2_value[31:0] = ((>>1$rf_wr_en) && (>>1$rd == $rs2 )) ? (>>1$result) : $rf_rd_data2;
```
Code For Branches To Correct The Branch Target Path
```
 //Current instruction is valid if one of the previous 2 instructions were not (taken_branch or load or jump)
         $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid 	|| >>1$jump_valid);
         
         //Current instruction is valid & is a taken branch
         $valid_taken_br = $valid && $taken_br;
         
         //Current instruction is valid & is a load
         $valid_load = $valid && $is_load;
         
         //Current instruction is valid & is jump
         $jump_valid = $valid && $is_jump;
         $jal_valid  = $valid && $is_jal;
         $jalr_valid = $valid && $is_jalr;

	 $pc[31:0] = (>>1$reset) ? 32'b0 : (>>3$valid_taken_br) ? (>>3$br_tgt_pc) :  (>>3$int_pc)  ;
         //$valid = $reset ? 1'b0 : ($start) ? 1'b1 : (>>3$valid) ; no need for this
```
Added test case to check fucntionality of load/store. Stored the summation of 1 to 9 on address 4 of Data Memory and loaded that value from Data Memory to r17.

```
*passed = |cpu/xreg[17]>>5$value == (1+2+3+4+5+6+7+8+9);
```
Below is snapshot from Makerchip IDE after including load/store instructions:

![fig-504]()











## Word of Thanks
I sciencerly thank **Mr. Kunal Gosh**(Founder/**VSD**) for helping me out to complete this flow smoothly.

## Acknowledgement
- Kunal Ghosh, VSD Corp. Pvt. Ltd.
- Chatgpt
- Kanish R,Colleague,IIIT B
- Sumanto Kar,Sr. Project Technical Assistant , IIT Bombay
- Pruthvi Parate,Colleague,IIIT B
- Emil Jayanth Lal,Colleague,IIIT B
- Bhargav Dv,Colleague,IIIT B
- Sai Sampath,Colleague,IIIT B
- Geetima Kachari,Assistant professor
- Shivani Shah,IIIT B Senior
- Bala Dhinesh,Engineer,Testorrent
- Steve Hoover,Redwood Eda
  
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
