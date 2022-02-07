---
tags: CSE
---

# computer organization ch2~ch3
---
> [toc]
---

## MIPS
__MIPS__ (Microprocessor without Interlocked Pipelined Stages) is a reduced instruction set computer([RISC](https://en.wikipedia.org/wiki/Reduced_instruction_set_computer)) instruction set architecture([ISA](https://en.wikipedia.org/wiki/Instruction_set_architecture)).
### registers

| Number | Name | Comments |
| ------ | ---- | -------- |
| 0     | $zero| always zero |
| 1     | $at  | reserved for assembler |
| 2,3  | $v0 - $v1| first and second return values, respectively|
|4,...,7|$a0 - $a3| first four arguments to functions |
|8,...,15|$t0 - $t7| temporary registers |
|16,...,23|$s0 - $s7| saved registers |
|24,25|$t8, $t9| temporary registers |
|26,27|$k0, $k1| reserved for kernel (operating system) |
|28|$gp|global pointer|
|29|$sp|stack pointer|
|30|$fp|frame pointer|
|31|$ra|return address|

### instruction formats
* __R-type:__
op(6)+rs(5)+rt(5)+rd(5)+shamt(5)+funct(6)
* __I-type:__
op(6)+rs(5)+rt(5)+immediate(16)
* __J-type:__
op(6)+address(26)  

_*the unit above is bit_

$op:$ basic operation of the instruction, traditionally called the __opcode__.  
$rs:$ the first register source operand.  
$rt:$ the second register source operand.  
$rd:$ the register destination operand. it gets the result of the operation.  
$shamt:$ shift amount.  
$funct:$ function. this field, often called the _function code_, selects the specific variant of the operation in the op field.

### arithmetic instruction
* ```add $rd, $rs, $rt```  
$rd = $rs + $rt
* ```addu $rd, $rs, $rt```  
$rd = $rs + $rt
* ```addi $rt, $rs, immed```  
$rt = $rs + immed
* ```sub $rd, $rs, $rt```  
$rd = $rs - $rt
* ```subu $rd, $rs, $rt```  
$rd = $rs - $rt
* ```subi $rt, $rs, immed```  
$rt = $rs - immed


__EXAMPLE:__  
turn the c code segment ```f=(g+h)-(i+j);``` into MIPS assembly code.  
the variables ```f,g,h,i,j``` are assigned to the register $s0, $s1, $s2, $s3, $s4, respectively.  

ans:  
```
add $t0, $s1, $s2  
add $t1, $s3, $s4
sub $s0, $t0, $t1
```

### basic memory instruction
* ```lw $rt, offset(\$rs)```  
_load word_, $rt = $rs[offset], the unit of offset is byte
* ```sw $rt, offset(\$rs)```  
_store word_, $rs[offset] = $rt, the unit of offset is byte

__EXAMPLE:__  
trun the c code segment ```g=h+A[8];``` into MIPS assembly code.  
the variables ```g,h``` are assigned to the register $s1, $s2 respectively.  
and the _base address_ of the array is in $s3.  

ans:
```
lw $t0, 32($s3)  # a word is 4 bytes
add $s1, $s2, $t0
```

__EXAMPLE:__  
trun the c code segment ```A[12] = h + A[8];``` into MIPS assembly code.  
the variable ```h``` is assigned to the register $s2 and the base address of the array ```A``` is in $s3.  

ans:  
```
lw $t0, 32($s3)
add $t0, $s2, $t0
sw $t0, 48($s3)
```  

### how instruction represents in the computer
__EXAMPLE:__  
turn ```add $t0, $s1, $s2``` into machine code.  

ans:  
remember the instruction format:  
R-type: op(6)+rs(5)+rt(5)+rd(5)+shamt(5)+funct(6)  
the opcode of ```add```  instruction is 0, the numbe of ```$t0``` is 8, ```$s1``` is 17, ```$s2``` is 18. and they're all corresponding to op, rd, rs, rt.  
so the machine code is  


| op | rs | rt | rd | shamt | funct
|---|---|---|---|---|---|
| 0b000000 | 0b10001 | 0b10010 | 0b01000 | 0b00000 | 0b100000

which is __0x2324020__.  

__EXAMPLE:__  
turn the c code segment ```A[300] = h + A[300];``` into machine code.  
the variable ```h``` is assigned to $s2, and the base of the array is in $t1.

ans:  
the assembly code is  
```
lw $t0, 1200($t1)  
add $t0, $s2, $t0
sw $t0, 1200($t1)
```
the machine code of ```lw``` is:  
| op | rs | rt | immedatiate |
| --- | --- | --- | --- |
| 35=0b100011 | 9=0b01001 | 8=0b01000 | 1200=0b0000010010110000 |

which is __0x8D2804B0__.

,and the machine code of ```add``` is:  
| op | rs | rt | rd | shamt | funct |
| --- | --- | --- | --- | --- | --- |
| 0b000000 | 0b10010 | 0b01000 | 0b01000 | 0b00000 | 0b100000 |

which is __0x2484020__.

,the machine code of ```sw``` is:  
| op | rs | rt | immedatiate |
| --- | --- | --- | --- |
| 43=0b101011 | 9=0b01001 | 8=0b01000 | 1200=0b0000010010110000 |

which is __0xAD2804B0__.  

### bitwise instruction
* ```sll $rd, $rt, shamt```  
$rd = $rt << shamt
* ```srl $rd, $rt, shamt```
$rd = $rt >> shamt
* ```and $rd, $rs, $rt```
* ```andi $rt, $rs, immed```
* ```or $rd, $rs, $rt```
* ```ori $rt, $rs, immed```
* ```nor $rd, $rs, $rt```
* ```nori $rt, $rs, immed```
* ```xor $rd, $rs, $rt```
* ```xori $rt, $rs, immed```

### conditional branch instruction
* ```beq $rs, $rt, Label```  
if ($rs == $rt) goto Label
* ```bne $rs, $rt, Label```  
if ($rs != $rt) goto Label
* ```slt $rd, $rs, $rt```  
\$rd = ($rs < $rt)
* ```sltu $rd, $rs, $rt```  
\$rd = ($rs < $rt), unsigned int comparsion
* ```slti $rt, $rs, immed```  
\$rt = (\$rs < immed)
* ```sltiu $rt, $rs, immed```  
\$rt = (\$rs < immed), unsigned int comparsion

__EXAMPLE:__  
turn the c code segment 
```c
if (i == j) f = g + h;
else f = g - h;
```
into MIPS code.  
the variables ```f``` through ```j``` correspond to the five registers $s0 through $s4.  

ans:  
```
bne $s3, $s4, ELSE
add $s0, $s1, $s2
j EXIT                    # goto EXIT
ELSE: sub $s0, $s1, $s2
EXIT:
```

### loop
turn the c code segment
```c
while (save[i] == k)
    i += 1;
```
into MIPS code.  
```i,k``` is corresponding to $s3, $s5, respectively. and the base address of ```save``` is in $s6.

ans:  
```
WHILE: sll $t0, $s3, 2
add $t0, $s6, $t0
lw $t1, 0($t0)
bne $t1, $s5, EXIT
addi $s3,$s3,1
j WHILE
EXIT:
```
optimization version:
```
sll $t0, $s3, 2
add $t0, $s6, $t0
WHILE: lw $t1, 0($t0)
bne $t1, $s5, EXIT
addi $t0, 4
j WHILE
EXIT:
```

### function call
the outlook of function call stages:
1. Put parameters in a place where the procedure can access them.
2. Transfer control to the procedure.
3. Acquire the storage resources needed for the procedure.
4. Perform the desired task.
5. Put the result value in a place where the calling program can access it.
6. Return control to the point of origin, since a procedure can be called from several points in a program.

MIPS provide several registers for function call:  
* $a0 - $a3 : four argument registers in which to pass parameters.  
* $v0 - $v1 : two value registers in which to return values.
* $ra : one return address register to return to the point of origin.  

MIPS assembly language includes an instruction just for function call:  
```jal address```(jump and link), it jumps to the address and simultaneously saves the calling address in register \$ra.  
to meet the need of jump to the address in register (yeah, ```j address``` takes only immediate value), MIPS provides such an instruction: ```jr $rs``` which can jump to the address stored in register $rs.

note that ```jal address``` change nothing but $ra, so the caller must put the argument into $a0 - $a3 before ```jal```.  
and the callee must place the result in $v0 and $v1 before returning control to the caller using ```jr $ra```

Implicit in the stored-program idea is the need to have a register to hold the address of the current instruction being executed.  
this register is called __program counter__, abbreviated _PC_ in the MIPS architecture.  
the instruction ```jal``` actually saves PC+4 in $ra to link to the following instruction to set up the procedure return.

suppose a compiler needs more registers for a procedure than the four argument and two return value registers.  
we need another data structure to help. the ideal data structure is __stack__.  
the pointer of stack is named $sp in MIPS.  
by historical precedent, stack "grow" from higher address to lower address.  

__EXAMPLE:__  
turn the c function  
```c
int leaf_example(int g, int h, int i, int j)
{
    int f;
    
    f = (g + h) - (i + j);
    return f;
}
```
into MIPS code.  
```f``` is corresponding to $s0.

ans:  
```
LEAF_EXAMPLE:
addi $sp, $sp, -4        # move down the stack pointer
sw $s0, 0($sp)      # store the content of $s0
add $t0, $v0, $v1
add $t1, $v2, $v3
sub $s0, $t0, $t1
add $v0, $s0, $zero
lw $s0, 0($sp)      # return the content of $s0
addi $sp, $sp, 4         # move back the stack pointer
jr $ra
```

we don't need to push the temporary content to the stack, since it's "temporary".

### recurrsion function
turn the c code function
```c
int fact(int n)
{
    if (n < 1) return 1;
    else return n * fact(n - 1);
}
```
into MIPS code.

ans:  
```
FACT:
slti $t0, $a0, 1
beq $t0, $zero, RECU
addi $v0, $zero, 1
jr $ra
RECU:
addi $sp, $sp, -8
sw $ra, 0($sp)
sw $a0, 4($sp)
subi $a0, $a0, 1
jal FACT
lw $a0, 4($sp)
lw $ra, 0($sp)
addi $sp, $sp, 8
mul $v0, $a0, $v0
jr $ra
```

### memory layout
![](https://i.imgur.com/Kj41WWc.png)

* the .bss, data, text section are called _static memory layout_.
* if we want to allocate a space on heap, we need [system call 9](www.doc.ic.ac.uk/lab/secondyear/spim/node8.html), ~~and use $gp to access the data.~~(TBD)

__EXAMPLE:__  
```
int var1 = 0;  // data section
int var2;      // .bss section
int main()
{
    int arr1;  // stack
    int* var3 = (int *)malloc(sizeof(int)); // heap
    
    return 0;
}
```

### the pointer of memory layout

* frame pointer($fp): ![](https://i.imgur.com/Km4p4Wo.png)  
in normal situation, \$fp isn't necessary needed.  
but when in debug, \$fp is needed for trace the function.  

* stack pointer(\$sp)  

* global pointer(\$gp):  
global pointer points to the upper start of static data section.  
it's used to access dynamic allocated data more convenient.

* program count(\$pc)

### dealing with ASCII character
a character of ASCII code is 8-bit length. so we can't just use ```lw,sw```. MIPS provides ```lb $rt, offset($rs), lbu $rt, offset($rs), sb $rt, offset($rs), sbu $rt, offset($rs)``` which only takes the rightmost 8-bits.

__EXAMPLE:__  
trun the c code function
```c
void strcpy(char x[], char y[])
{
    int i;
    
    i = 0;
    while((x[i] = y[i]) != '\0') ++i;
}
```
into MIPS code.

ans:  
```
STRCPY:
addi $sp, $sp, -4
sw $s0, 0($sp)
add $s0, $zero, $zero
WHILE:
add $t0, $s0, $a0
add $t1, $s0, $a1
lbu $t2, 0($t1)
sbu $t2, 0($t0)
beq $t2, $zero, EXIT_WHILE
addi $s0, $s0, 1
j WHILE
EXIT_WHILE:
lw $s0, 0($sp)
addi $sp, $sp, 4
jr $ra
```

### dealing with unicode character
by default unicode, a character takes 16-bits which is half-word.  
MIPS provides ```lh $rt, offset($rs), lhu $rt, offset($rs), sh $rt, offset($rs), shu $rt, offset($rs)``` for the situation.  

### 32-bits immediate operands
though MIPS register can store 32-bit value, the instruction actually limit the max value that can input at one time.  
to solve this, MIPS provide ```lui $rt, $immed``` which store the upper 16-bits.  
what's about the lower 16-bits?  
well, we can use ```ori```(OR bitwise) to done that.  
for example:  
how to store ```0b00000000001111010000100100000000``` into \$s0?  
```
lui $s0, 61         # 0b0000000000111101 = 61
ori $s0, $s0, 2304  # 0b0000100100000000 = 2304
```

*The effect of the ```lui``` instruction. The instruction ```lui``` transfers the 16-bit immediate constant field value into the left most 16 bits of the register, filling the lower 16 bits with 0.

### addressing in branches
remember the format of j-type instruction, the address take 26 bits. while the immediate of branch instruction take only 16 bits.  
if address had to fit in this 16-bit field, that means no program can be larger than $2^{16}$ which is way too small for the program today.  
to solve this, we can locate branch address base on PC.  
this form of branch addressing is called __PC-relative addressing__.  
*the PC-relative addressing refer to the number of _words_.  

__EXAMPLE:__  
```
Loop:sll $t1, $s3, 2
    add $t1, $t1, $s6
    lw $t0, 0($t1)
    bne $t0, $s5, Exit
    addi $s3, $s3, 1
    j Loop
Exit:
```
assume the address of ```Loop``` is 80000.  
the machine code will be:  
![](https://i.imgur.com/I13fPcm.png)

### MIPS addressing mode summary
1. _immediate addressing_, where the operand is a constant within the instruction itself.  
_e.g._ ```j address```
2. _register addressing_, where the operand is a register.  
_e.g._ ```jr $ra```
3. _base_ or _displacement addressing_, where the operand is at the memory location whose address is the sum of a register and a constant in the instruction.  
_e.g._ ```lw $rt, offset($rs)```
4. _PC-relative addressing_, where the branch address is the sum of the PC and a constant in the instruction.  
_e.g._ ```beq $rs, $rt, immed```
5. _pseudodirect addressing_, where the jump address is the 26 bits of the instruction concatenated with the upper bits of the PC.  
_e.g._
```
beq $s0, $s1, L1
```
if L1 is too far away, the assembler will replace the code below with it:  
```
bne $s0, $s1, L2
j L1
L2:
```

### parallelism and synchronization
to support parallelism, MIPS provide ```ll $rt, offset($rs), sc $rt, offset($rs)```, called _load link_ and _store condition_.  
Th ese instructions are used in sequence:  
if the contents of the memory location specified by the load linked are changed before the store conditional to the same address occurs, then the store conditional won't work.  

__EXAMPLE:__  
```
again:addi $t0, $zero, 1    # set locked value
    ll $t1, 0($s1)          
    sc $t0, 0($s1)          
    beq $t0, $zero, again   # if store failed, $t0 would be set to be zero
```

### how a executable file come out
![](https://i.imgur.com/GV8Cf12.png)

### object file
object file is a combination of machine language instructions, data, and information needed to place instruction properly in memory.  

to produce the binary version of each instruction in the assembly language program, the assembler must determine the addresses corresponding to all labels.  
assemblers keep track of labels used in branches and data transfer instructions in a __symbol table__, which is a table that matches names of labels to the addresses of the memory words that instructions occupy.  

The object file for UNIX systems typically contains six sections:  
* the _header_ describes the size and position of the other sections of the object file.
* the _text segment_ contains the machine language code.  
* the _static data segment_ contains data allocated for the life of the program.  
* the _relocation information_ identifies instrution and data words that depend on absolute address when the program is loaded into memory.  
* the _symbol table_ the remaining labels that are not defined, such as external references.  
* the _debugging information_ contains a concise description of how the modules were compiled so that a debugger can associate machine instructions with C source files and make data structure readable.  

### linker
linker is a systems program that combines independently assembled machine language programs and resolves all undefined labels into an executable file.  

the linker uses the relocation information and symbol table in each object module to resolve all undefined labels.  
such references occur in branch instructions, jump instructions, and data addresses, so the job of this program is much like that of an editor: it finds the old addresses and replaces them with the new addresses.   
the reason a linker is useful is that it is much faster to patch code than it is to recompile and reassemble.  

the linker produces an executable file that can be run.  
typically, executable file has the same format as an object file, except that it contains no unresolved reference.  

__EXAMPLE:__  
there are two object file below. the example will show how a linker work.  
there're instructions that refer to the addresses of procedures A and B and the instructions that refer to the addresses of data words X and Y.  
*we show the instructions in assembly language just to make the example understandable; in reality, the instructions would be numbers.  

----
__object file header__:  
| name      | text size   | data size   |
| --------- | ----------- | ----------- |
| procedure A | 0x100     | 0x20        |

__text segment:__  
| address | instruction |
| ------- | ----------- |
| 0       | ```lw $a0, 0($gp)``` |
| 4       | ```jal 0``` |
| ...     | ...         |

__data segment:__  
| address | data |
| --- | --- |
| 0 | X |

__relocation information:__  
| address  | instruction | dependency |
| -------- | -------- | -------- |
| 0        | ```lw```    | X     |
| 4        | ```jal```   | B     |

__symbol table:__  
| label    | address  |
| -------- | -------- |
| X        | -        |
| B        | -        |

---
__object file header__:  
| name      | text size   | data size   |
| --------- | ----------- | ----------- |
| procedure B | 0x200     | 0x30        |

__text segment:__  
| address | instruction |
| ------- | ----------- |
| 0       | ```sw $a1, 0($gp)``` |
| 4       | ```jal 0``` |
| ...     | ...         |

__data segment:__  
| address | data |
| --- | --- |
| 0   | Y   |

__relocation information:__  
| address  | instruction | dependency |
| -------- | -------- | -------- |
| 0        | ```sw```    | Y     |
| 4        | ```jal```   | A     |

__symbol table:__  
| label    | address  |
| -------- | -------- |
| Y        | -        |
| A        | -        |

----

> memory layout  
> ![](https://i.imgur.com/F7uDezS.png)  
> 
after link:

__executable file header:__  
| text size   | data size   |
| ----------- | ----------- |
| 0x300       | 0x50        |

according to the picture above, text segment starts at 0x400000. and the text size of procedure A is 0x100, so the start of the text of procedure B is 0x400100.  
similar as the data segment.  

\$gp points to 0x10008000, which is 268468224 in decimal.  
so the first data is in 0x10000000=268435456, whose offset according to global pointer is -32768.  
the second one is in 0x10000020=268435488, whose offset according to global pointer is -32736.  

the start address of procedure A is 0x400000 and the other is 0x400100.  
since ```jal``` is use 26-bit address to create 28-bit address(fill zeros at lower two positions, which is equal to shift left by 2), the destination of ```jal``` must be divided by 4, which is 0x100040=1048640 and 0x100000=1048576 respectively.

__text segment:__  
| address | instruction |
| ------- | ----------- |
| 0x400000| ```lw $a0, -32768($gp)``` |
| 0x400004| ```jal 1048640``` |
| ...     | ...         |
| 0x400100| ```sw $a1, -32736($gp)``` |
| 0x400104| ```jal 1048576``` |

__data segment:__  
| address |     |
| ------- | --- |
| 0x10000000 | X |
| ...        | ...|
| 0x10000020 | Y |
| ...        | ...|

### loader
The loader follows these steps in UNIX systems:  
1. reads the executable file header to determine size of the text and data segments.
2. creates an address space large enough for the text and data.
3. copies the instructions and data from the executable file into memory.
4. copies the parameters (if any) to the main program onto the stack.
5. initializes the machine registers and sets the stack pointer to the first free location.
6. jumps to a start-up routine that copies the parameters into the argument registers and calls the main routine of the program.  
when the main routine returns, the start-up routine terminates the program with an exit system call.  

__a c sort example__  
turn the c sort code into MIPS assembly code.
```c=
void swap(int v[], int k)
{
    int temp;
    temp = v[k];
    v[k] = v[k+1];
    v[k+1] = temp;
}
void sort(int v[], int n) // v[1, n]
{
    int i, j;
    for (i = 0; i < n; ++i)
        for (j = i - 1; j >= 0 && v[j] > v[j + 1]; --j)
            swap(v, j);
}
```

ans:  
first deal with ```swap(int*, int)```
```
SWAP:
    sll $a1, $a1, 2    # index * 4, it's important
    add $t0, $a0, $a1
    
    lw $t1, 0($t0)
    lw $t2, 4($t0)
    
    sw $t1, 4($t0)
    sw $t2, 0($t0)
    
    jr $ra
```

then deal with ```sort(int*, int)```
```
SORT:
    addi $sp, $sp, -16
    sw $s0, 0($sp)
    sw $s1, 4($sp)
    sw $s2, 8($sp)
    sw $ra, 12($sp)    # since we need function call in sort
    
    add $s0, $zero, $zero         # i = 0
    add $s2, $zero, $a1           # $s2 = n
    LOOP1:
        # LOOP1 condition: i < n
        slt $t0, $s0, $s2         # $t0 = $s0 < $s2 = i < n
        beq $t0, $zero, EXIT_LOOP1 # if (i < n) == 0 goto EXIT_LOOP1
        
        addi $s1, $s0, -1     # j = i - 1
        LOOP2:
            # LOOP2 condition: j >= 0
            slt $t0, $s1, $zero   # $t0 = $s1 < 0 = j < 0
            bne $t0, $zero, EXIT_LOOP2  # if (j < 0) != 0, goto EXIT_LOOP2
            
            # LOOP2 condition: v[j] > v[j+1]
            sll $t4, $s1, 2
            add $t0, $a0, $t4
            lw $t1, 0($t0)        # v[j]
            lw $t2, 4($t0)        # v[j+1]
            slt $t3, $t2, $t1     # $t3 = $t2 < $t1 = v[j+1] < v[j]
            beq $t3, $zero, EXIT_LOOP2  # if (v[j+1] < v[j]) == 0, goto EXIT_LOOP2
            
            add $a1, $s1, $zero   # $a1 = j
            jal SWAP              # function call, $a0 = v, $a1 = j
            
            addi $s1, $s1, -1
            j LOOP2
            
        EXIT_LOOP2:
        addi $s0, $s0, 1
        j LOOP1
    
    EXIT_LOOP1:
    lw $ra, 12($sp)
    lw $s2, 8($sp)
    lw $s1, 4($sp)
    lw $s0, 0($sp)
    addi $sp, $sp, 16
    
    jr $ra
```

## arithmetic for computers
### addition and subtraction

addition: ```a+b = (a^b) | ((a&b) << 1)```  
substraction: ```a-b = a+(-b) = a + ((~b)+1)```  

----
MIPS detects overflow with an __exception__, also called an __interrupt__ on many computers.  
The address of the instruction that overflowed is saved in a register, _exception program counter_ (EPC), and the computer jumps to a predefined address to invoke the appropriate routine for that exception.  
The interrupted address is saved so that in some situations the program can continue after corrective code is executed.  
The instruction _move from system control_ (```mfc0```) is used to copy EPC into a general-purpose register so that MIPS soft ware has the option of returning to the off ending instruction via a jump register instruction.  

This directive leads to an interesting question: since you must fi rst transfer EPC to a register to use with jump register, how can jump register return to the interrupted code and restore the original values of all registers?  
To rescue the hardware from this dilemma, MIPS programmers agreed to reserve registers $k0 and $k1 for the operating system; these registers are not restored on exceptions.  
Exception routines place the return address in one of these registers and then use jump register to restore the instruction address.

----
MIPS can trap on overflow, but unlike many other computers, there is no conditional branch to test overflow.  
A sequence of MIPS instructions can discover overflow.  
For signed addition, the sequence is the following:  
```
addu $t0, $t1, $t2          # $t0 = sum, but don’t trap
xor $t3, $t1, $t2           # Check if signs differ
slt $t3, $t3, $zero         # $t3 = 1 if signs differ
bne $t3, $zero, No_overflow # $t1, $t2 signs isn't equal,
                            # so no overflow
                            
xor $t3, $t0, $t1           # signs is equal; sign of sum match too?
                            # $t3 negative if sum sign different
                            
slt $t3, $t3, $zero         # $t3 = 1 if sum sign different
bne $t3, $zero, Overflow    # All 3 signs ≠; goto overflow
```

For unsigned addition, the test is:  
```
addu $t0, $t1, $t2          # $t0 = sum
nor $t3, $t1, $zero         # $t3 = NOT $t1
                            # (~$t1 = -$t1 - 1)
                            
sltu $t3, $t3, $t2          # (–$t1 – 1) < $t2
                            # ⇒ –1 < $t1 + $t2
                            # ⇒ (uint)2^32 - 1 < $t1 + $t2
                            
bne $t3,$zero,Overflow      # if(2^32 - 1 < $t1 + $t2) goto overflow
```

----

### multiplication
> naive way to do multiplication:  
![](https://i.imgur.com/RlMoymZ.png)  

> multiplication hardware:  
![](https://i.imgur.com/vr1Fat7.png)  
> multiplication algorithm:  
![](https://i.imgur.com/LiKMlkP.png)  

__EXAMPLE:__  
Using 4-bit numbers to save space, multiply $2_{\text{ten}}\times3_{\text{ten}}$.  
![](https://i.imgur.com/R8MOMtg.png)

> refined version of multiplication hardware:  
![](https://i.imgur.com/vsCVBSy.png)  

> signed multiplication  
![](https://i.imgur.com/OwJYEXf.png)

> faster multiplication by divided addition  
> ![](https://i.imgur.com/O5hAQom.png)

----
MIPS provides a separate pair of 32-bit registers to contain the 64-bit product, called _Hi_ and _Lo_.  
To produce a properly signed or unsigned product, MIPS has two instructions: multiply (```mult```) and multiply unsigned (```multu```).  
To fetch the integer 32-bit product, the programmer uses _move from lo_ (```mflo```).  
The MIPS assembler generates a pseudoinstruction for multiply that specifies three general-purpose registers, generating ```mflo``` and ```mfhi``` instructions to place the product into registers.

Both MIPS multiply instructions ignore overflow, so it is up to the software to check to see if the product is too big to fit in 32 bits.  
There is no overflow if Hi is 0 for ```multu``` or the replicated sign of Lo for ```mult```.  
The instruction move from hi (```mfhi```) can be used to transfer Hi to a general-purpose register to test for overflow.

----

### divison

> naive way to do divison:  
> ![](https://i.imgur.com/HE4agki.png)

> divison hardware:  
> ![](https://i.imgur.com/tUDf2bv.png)

> divison algorithm:  
> ![](https://i.imgur.com/wzMOEdt.png)
> *dividend and divisor are both 32-bit.

__EXAMPLE:__  
using a 4-bit version of the algorithm to save pages, let’s try $7_{\text{ten}}\div2_{\text{ten}}$.
![](https://i.imgur.com/uRLgKSb.png)

> signed divison:  
> assume the divisor is $A$ and the dividend is $B$.  
> first calculate $|A|$ divide $|B|$, and check the sign of $A$ and $B$.  
> if the sign of both are equal, no need to change the reslut. if not, the quotient will be set negative and the the remainder will be set as same as dividend.  
> 

> improved version of the division hardward:
![](https://i.imgur.com/DCIHKLF.png)


----
To handle both signed integers and unsigned integers, MIPS has two instructions: divide (```div```) and divide unsigned (```divu```).  
The MIPS assembler allows divide instructions to specify three registers, generating the ```mflo``` or ```mfhi``` instructions to place the desired result into a general-purpose register.  
since MIPS implement division by the improved version of division hardward, as above, Hi will contain the remainder, and Lo will contain the quotient.

----

### float
> __IEEE Standard for Floating-Point Arithmetic (IEEE 754):__  
![](https://i.imgur.com/6S20gNn.png)

__the normal value:__  
$\text{value} = (-1)^s\times2^{\text{exp}-\text{bias}}\times(1+\sum^{L}_{i=1}b_{L-i}2^{-i})$, where $b_i$ is the $i$-th bit counted from the rightmost bit, and $L$ is the lenth of _mantissa_.  

__the subnormal value:__ (occurs when _exp_ equals zero and _mantissa_ doesn't equal to zero)  
$\text{value} = (-1)^s\times2^{\text{1}-\text{bias}}\times(\sum^{L}_{i=1}b_{L-i}2^{-i})$

__zero:__ (occurs when both _exp_ and _mantissa_ are zero)  
$\text{value} = (-1)^s\times0$ (the sign is matter)  

__infinity:__ (occurs when the bits of _exp_ are all one and _mantissa_ equals to zero)  
$\text{value} = (-1)^s\times\infty$ (the sign is matter)  

__NaN:__ (occurs when the bits of _exp_ are all one and _mantissa_ doesn't equal to zero)  
$\text{value} = NaN$ (the sign isn't matter)  

**the bias of signal precision is $127$ and the one of double is $1023$, defined as $2^{L_{\text{exp}}-1}-1$*  

__EXAMPLE:__  
represent the number $-0.75_{\text{ten}}$ in signal and double precision.  

$-0.75$ can be represent as $-3/4=-3\times2^{-2}$  
in base 2, that equal to: $-11\times2^{-2}=-1.1\times 2^{126-127}=-1.1\times2^{1022-1023}$  
so in signal:  
![](https://i.imgur.com/amVofHj.png)

and in double:  
![](https://i.imgur.com/eY5nF7h.png)

__EXAMPLE:__  
what's the decimal number is represented by this signal precision float?  
![](https://i.imgur.com/Jms4xow.png)

it's normal value:  
$\text{value} = (-1)^s\times(2^{\text{exp}-\text{bias}})\times(1+\sum^{L}_{i=1}b_{L-i}2^{-i})\\=-1\times(2^{129-127})\times(1+2^{-2})=-5$  

__float addition:__  
![](https://i.imgur.com/H87ItW5.png)  

__EXAMPLE:__  
add $0.5_{10}$ and $-0.4375_{10}$.  

assume we keep 4 bits of precision:  
$0.5_{10} = 0.1_{2}=(1\times2^{-1})_2$  
$-0.4375_{10} = (-7/16)_{10}=(-111\times2^{-4})_2=(-1.11\times2^{-2})_2$  

align the number whose exp is smaller:  
$-1.11\times2^{-2}=-0.111\times2^{-1}$  

add together:  
$1\times2^{-1}+-0.111\times2^{-1}=0.001\times2^{-1}$  
normalize:  
$0.001\times2^{-1}=1\times2^{-4} = 0.0625_{10}$  

__float multiplication:__  
![](https://i.imgur.com/v3ihmX9.png)

__EXAMPLE:__  
multiplying the number $0.5_{10}$ and $-0.4375_{10}$.  

$0.5_{10}=0.1_{2}=(1\times2^{-1})_{2}$  
$-0.4375_{10}=-7/16_{10}=-(111\times2^{-4})_{2}=-(1.11\times2^{-2})_2$  

$-1+-2=-3=124-127$, so $\text{exp}=124$  
the fraction is $1.000\times1.110=(1000\times1110)\times0.000001=1110000\times0.000001=1.110000$, round, then the result is $1.110$.  

the product is $(1.110\times2^{124-127})_2=1.75/8_{10}=0.21875$  
with the sign, the answer is $-0.21875_{10}$.  

----
MIPS has a floating point coprocessor (numbered 1) that operates on single precision (32-bit) and double precision (64-bit) floating point numbers.  
This coprocessor has its own registers, which are numbered ```$f0-$f31```. Because these registers are only 32-bits wide, two of them are required to hold doubles.  
To simplify matters, floating point operations only use even-numbered registers--including instructions that operate on single floats.   

MIPS supports the IEEE 754 single precision and double precision formats with these instructions:  

arithmetic:
* ```add.s $fd, $fs, $ft```  
signal precision addition, \$fd = $fs + $ft  
* ```add.d $fd, $fs, $ft```  
double precision addition, \$fd = $fs + $ft  
* ```sub.s $fd, $fs, $ft```  
signal precision subtraction, \$fd = $fs - $ft  
* ```sub.d $fd, $fs, $ft```  
double precision subtraction, \$fd = $fs - $ft  
* ```mul.s $fd, $fs, $ft```  
signal precision multiplication, \$fd = $fs * $ft  
* ```mul.d $fd, $fs, $ft```  
double precision multiplication, \$fd = $fs * $ft  
* ```div.s $fd, $fs, $ft```  
signal precision division, \$fd = $fs / $ft  
* ```div.s $fd, $fs, $ft```  
double precision division, \$fd = $fs / $ft  
* ```abs.s $fd, $fs```  
signal precision absolute value, \$fd = |\$fs|  
* ```abs.d $fd, $fs```  
double precision absolute value, \$fd = |\$fs|  
* ```neg.s $fd, $fs```  
signal precision negative value, \$fs = -\$fs  
* ```neg.d $fd, $fs```  
double precision negative value, \$fs = -\$fs  

condition branch:  
* ```c.x.s $fs, $ft```  
signal precision comparison, float-flag = $fs x $ft  
```x``` must be one of ```eq,ne,lt,le,gt,ge``` (e.g. ```c.eq.s```)
* ```c.x.d $fs, $ft```  
double precision comparison, float-flag = $fs x $ft  
* ```bc1t Label```  
if float-flag is true goto Label  
* ```bc1f Label```  
if float-flag is false goto Label  

data transfer:  
* ```lwc1 $ft, offset($rs)```  
load word from \$rs[offset]  
* ```ldc1 $ft, offset($rs)```  
load double word from \$rs[offset]  
* ```swc1 $ft, offset($rs)```  
store word to \$rs[offset]  
* ```sdc1 $ft, offset($rs)```  
store double word to \$rs[offset]  

----

__EXAMPLE:__  
convert the c function  
```c=
float f2c(float fahr)
{
    return (5.0 / 9.0) * (fahr - 32.0);
}
```  
into MIPS code.  
assume that ```fahr``` is passed in ```$f12``` and the result should go in ```$f0```.

ans:  
```
.data
    fp1: .float 5.0
    fp2: .float 9.0
    fp3: .float 32.0
.text
    F2C:
        lwc1 $f0, fp1($zero)
        lwc1 $f1, fp2($zero)
        div.s $f0, $f0, $f1
        lwc1 $f1, fp3($zero)
        sub.s $f1, $f12, $f1
        mul.s $f0, $f0, $f1
        jr $ra
```

## processor

