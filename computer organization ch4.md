---
tags: CSE
---

# computer organzation ch4

---
> [toc]

---
## overview of the implementation

we will intro the implementation of a processor by implement a subset of the core MIPS instruction set:  
1. memory-reference instructions: ```lw,sw```  
2. arithmetic-logic instructions: ```add,sub,AND,OR,slt```  
3. control flow instructions: ```beq,j```  

### outlook of what to do
for instructions mentioned above, the skelton of step are:  
1. fetch the instrcution in memory accroding to the address inside PC.
2. for memory-reference instruction, one of registers must be read/written and the other one will add offset to compose a address in memory.  
for arithmetic-logic instructions, two of the registers must be read and one of them will be written.  
for control flow instruction, we need to check whether two values in specific registers are equal (by using subtract) and decide jump or not. for ```j```, we will need to use PC and shift unit, which will be discuss later.  

> basic implementation:  
> ![](https://i.imgur.com/9Q8lWob.png)

## basic components of a datapath
a datapath unit used to operate on or hold data within a processor.  
In the MIPS implementation, the datapath elements include the instruction and data memories, the register file, the ALU, and adders.  

### fetch instruction
first, we need a datapath to access instructions:  
![](https://i.imgur.com/4fCHNdX.png)

connect them together:  
![](https://i.imgur.com/4VsXMY3.png)

### register file
then, the processor's 32 general-purpose registers(GPR) are stored in a structure called a [register file](https://hackmd.io/0dsvPijiTH2cVMTozWAeRg?view#register-files).  
since there are 32 GPRs, the input for select register is 5 bits wide.  
recall [R-type](https://hackmd.io/l9ugIxRSR9Kqf_-qLd54Sg#instruction-formats) instructions in MIPS, they contain two source registers and one destination register. for that, we need two read ports and one write port in register file.  
![](https://i.imgur.com/XcTgNnx.png)
(the register file always outputs the contents of the registers corresponding to the Read register inputs on the outputs; no other control inputs are needed.  
In contrast, a register write must be explicitly indicated by asserting the write control signal.)  

### ALU
next is ALU:  
![](https://i.imgur.com/CoVjvIh.png)  
(the operation to be performed is controlled with the ALU operation signal, which will be 4 bits wide)

### data memory
consider ```lw $rt, offset($rs)``` and ```sw $rt, offset($rs)```.  
we need to compute ```$rs + offset```, however, ```offset``` is a 16-bit value, inside a register is a 32-bit value. so we will need a [sign-extend](https://en.wikipedia.org/wiki/Sign_extension) unit:  
![](https://i.imgur.com/UiG1vEU.png)

### flow control
now we implement ```beq``` instruction.  
first, use ALU to test equallity of two value of registers.  
second, the ```offset``` is sign-extended to 32-bit. and the new ```offset``` will shift left by 2. then add it with PC + 4 to form __branch target address__.  

![](https://i.imgur.com/DHfuHUo.png)

```j``` is kinda different. it operates by replacing the lower 28 bits of the PC with the  lower 26 bits of the instruction shift ed left by 2 bits.  
the specific implementation will be mentioned later.  

## create a signle datapath
this simple datapath will attempt to execute all instructions in one clock cycle.  
(This means that no datapath resource can be used more than once per instruction, so any element needed more than once must be duplicated.  
we therefore need a memory for instructions separate from one for data.)  

![](https://i.imgur.com/vcciaoh.png)

## control unit

### ALU control
the MIPS ALU in [intro](https://hackmd.io/0dsvPijiTH2cVMTozWAeRg#constructing-a-basic-ALU) defines the 6 following combination of four control inputs:  
![](https://i.imgur.com/6Nu3UEy.png)

depending on the instruction class, the ALU will need to perform one of these operations.  
we can generate the 4-bit ALU control input by using a small control unit that has input from the _funct field_ and 2-bit control field which is call ALUOp.  
the truth table:  
![](https://i.imgur.com/FRqlNRc.png)  

This is a common implementation 
technique of using multiple levels of decoding, that is, the main control unit generates the ALUOp bits, which then are used as input to the ALU control that generates the actual signals to control the ALU unit.  

Using multiple levels of control can reduce the size of the main control 
unit and that may also potentially increase the speed of the control unit.  

by using don't-care term, the truth table can be reduced:  
![](https://i.imgur.com/kUAKh6F.png)  

### design the main control unit
now that we have described how to design an ALU that uses the function code and a 2-bit signal as its control inputs, we can return to looking at the rest of the control.  

by using the format of instructions,  
![](https://i.imgur.com/8A9OqFE.png)  

we can construct the datapath with all necessary multiplexors and all control lines identified:  
![](https://i.imgur.com/q6fYBb2.png)  

and then we can add an main control unit:  
![](https://i.imgur.com/VFBxSlq.png)  
![](https://i.imgur.com/2fXNMc6.png)  

these nine control signals (seven from Figure 4.16 and two for ALUOp) can now be set based on the six input signals to the control unit, which are the opcode bits 31 to 26.  
as below:  
![](https://i.imgur.com/vWADS6T.png)  

we can complete the truth table.
![](https://i.imgur.com/OuN5x7E.png)

### operation of the datapath for R-type instruction
these steps show the operation of the datapath for an R-type instruction:  
1. the instruction is fetched, and the PC is incremented.
2. two registers, ```$rs```, ```$rt```, are read. also, the main control unit computes the setting of the control lines during this step.
3. the ALU operates on the data read from the register file, using the output of ALU control to generate the specific function.

![](https://i.imgur.com/krHGtat.png)

### operation of the datapath for memory-reference instruction
take ```lw $rt, offset($rs)``` as example:  
1. the instruction is fetched, and the PC is incremented.
2. a register(```$rs```) value is read.
3. the ALU computes the sum of the value of ```$rs + offset``` where ```offset``` is sign-extended.
4. this sum is used as the address for the data memory.
5. the data from memory unit is written into the register, which is specificed by the bits 20:16 (```$rt```)

![](https://i.imgur.com/UEdI1fY.png)

### operation of the datapath for flow control
take ```beq $rs, $rt, offset``` as example (```j``` will be implemented later):  
1. the instruction is fetched, and the PC is incremented.
2. two registers, ```$rs,$rt``` are read from the register file.
3. the ALU performs a subtract on the data values read from the register file. and the branch target address is computed.
4. the Zero result from the ALU is used to decide which adder result to store into the PC.

![](https://i.imgur.com/anNZpIK.png)

### single-cycle implementation
now we have a __single-cycle implementation__ of most of the MIPS core instrction set.  
(single-cycle implementation is an implementation in which an instruction is executed in one clock cycle.)

let’s add the jump instruction to show how the basic datapath and control can be extended to handle other instructions in the instruction set.  

__EXAMPLE:__  
implement jumps.  

![](https://i.imgur.com/MAZxzDs.png)

the steps are:  
1. the instruction is fetched, and the PC is incremented.
2. bits 0:25 shall shift left by 2, and the result is concatenated into the upper 4 bits of PC + 4.  

to meet the request, the control unit will need to add another control line for an additional mutiplexor, which is asserted when _opcode_ is 0b000010.  
![](https://i.imgur.com/mD6LsVa.png)

### why single-cycle implementation is obsolete
notice that the clock cycle must have the same length for every instruction in this single-cycle design.  
the longest possible path in the processor determines the clock cycle.  
This path is almost certainly a load instruction, which uses five functional units in series: the instruction memory, the register file, the ALU, the data memory, and the register file.  
Although the CPI is 1, the overall performance of a single-cycle implementation is likely to be poor, since the clock cycle is too long.  

for that, we need another implementation technique.  

## pipeline overview
### intro
__pipelining__ is now a universal technique to optimize the latency of each instructions, which means that it's used to optimize the the number of executing instructions per unit time, i.e. improve instruction throghput.  
take a look of the single-cycle implementation. we can actually break down the step of execution of an instruction to pieces:  
1. fetch instruction from memory
2. read registers while decoding the instruction
3. execute the operation or calculate an address
4. access an operand in data memory
5. write the result into a register

if we implement without pipelining, the picture of execution will be like:  

![](https://i.imgur.com/h7HJOvK.png)

the CPU can only execute an instruction at a single clock cycle. however, with pipelining:  

![](https://i.imgur.com/tvo30VD.png)

after we break down the step and increase the frequency of clock rate, the executing instructions count per unit time has increased, namely, the throughput of CPU increases.  

notice that the clock time must be the least required time among all stages.
![](https://i.imgur.com/g8iBy3J.png)
take the above figure for example, the clock must be 200ps.


we can derive a formula to show how the speed-up of pipelining (in ideal condition):  

$\text{time between instructions}_\text{pipelined}=\frac{\text{time between instructions}_\text{nonpipelined}}{\text{number of pipe stages}}$

(pipe stages is the break-down steps above, e.g. the number of pipe stages of figure above is 5)  

### why MIPS is easy pipelining
1. the length of an instruction is fixed. it makes it much easier to fetch and decode.
2. the number of format kinds in MIPS is small, and the source register fields being located is always in the same place.  
this symmetry means that the second stage can begin reading the register when the time the hardware is determining what type of instruction was fetched.
3. memory operands only appear in loads or stores in MIPS. it's means we can use the execute stage to calculate the memory address and then access memory in the following stage.
4. operands must be aligned in memory. Hence, the requested data can be transferred between processor and memory in a single pipeline stage.

### pipeline hazards
there're situations in pipelining when the next instruction cannot execute in the following clock cycle. it's called __hazards__ divided by three types.  
#### structural hazards
__structural hazard__ means that the hardware cannot support the combination of instructions that we want to execute in the same clock cycle.  

take a situation for example, suppose that now we only have a single memory where puts the instructions and data together.  

![](https://i.imgur.com/tvo30VD.png)  

if there's fourth instruction in the above figure. when the time between 600ps and 800ps, the first instruction is accessing data from memory while the fourth one is fetching an instruction from the same memory. it will surely cause error.  
without two memory unit, the pipeline could have a structural hazard.  

#### data hazards
__data hazard__ occurs when the pipeline must be stalled because one step must wait for another to complete.  
for example, the instruction segment below can cause data hazard:  
```
add $s0, $t0, $t1
sub $t2, $s0, $t3
```
without any action, the second instruction will wait till the fifth stage of first instruction.  it surely waste lots of time and it's unacceptable.  

the primary solution to data hazards is __forwarding__.(or __bypassing__)  
when the stages of an instruction isn't fully completed, for example, as soon as ALU create the sum for the ```add```, we can supply the result as an input of following instructions.  

> an example of forwarding   
![](https://i.imgur.com/BgiOZcf.png)

from the above figure, we can use forwarding to avoid pipeline stalls. however, forwarding can't avoid all pipeline stalls.  

> an example that forwarding fails
![](https://i.imgur.com/Ds2lWUC.png)

since the data will be available only after fourth stage, even with forwarding, we would have to stall one stage for a __load-use data hazard__ as the figure shows.  
the figure shows an important pipeline concept, called a __pipeline stall__ or __bubble__.  
we will discuss how to deal with hard case like this by using either hardware detection and stalls or software that reorders code to try to avoid load-use pipeline stalls, as the example shows.  

__EXAMPLE:__  
consider the following C code segment:  
```
a = b + e;
c = b + f;
```
here is the generated MIPS code for the segment:  
```
lw  $t1, 0($t0)
lw  $t2, 4($t0)
add $t3, $t1, $t2
sw  $t3, 12($t0)
lw  $t4, 8($t0)
add $t5, $t1, $t4
sw  $t5, 16($t0)
```
find the hazards in the preceding code segment and reorder the instructions to avoid any pipeline stalls.

answer:  
the ```add``` instructions at line 3,6 suffer from load-use data hazard.  
reorder:  
```
lw  $t1, 0($t0)
lw  $t2, 4($t0)

lw  $t4, 8($t0)

add $t3, $t1, $t2
sw  $t3, 12($t0)
add $t5, $t1, $t4
sw  $t5, 16($t0)
```

#### control hazards
the third one is called a __control hazard__, arising from the need to make a decision based one the results of one instruction while others are executing.  

notice that the instruction following next will be fetched on the very next clock cycle.  
nevertheless, the pipeline can't know what the next instruction should be, since it only just receive the branch instruction from memory.  

one solution to this is to stall immediately after we fetch a branch, waiting until the pipeline determines the outcome of the branch.  
to see how the solution works, we have to make some ideal assumption: we put in enough extra hardware so that we can test registers, calculate the branch address, and update the PC during the second stage of the pipeline.  

take a MIPS segment as example,  
```
 0| add $4, $5, $6
 4| beq $1, $2, 40
 8| lw  $3, 300($0)
...
48| or  $7, $8, $9
```

if the branch is taken:
![](https://i.imgur.com/HmQkGry.png)

if not:
![](https://i.imgur.com/EF0xWvU.png)

you'll surprisingly find that it doesn't matter whether the branch be taken or not.  
so why don't you just assume that this branch will not be taken and start the stages of next instruction. (there's a restriction, the following instruction must have no side effect, i.e. is able to undo or ignore) after all, when the result of the branch comes, we can simply jump to the address or just keep going.  

this is how the idea looks like:  
![](https://i.imgur.com/r0YCf45.png)

the solution we discovered is actually a specific case of __branch prediction__, which predicts the branch is never taken.  

a more sophisticated version is that it would have some branches predicted as taken and some as untaken.  
this version is called __static prediciton__.  
for example, at the bottom of loops are branches that jump back to the top of the loop. since they are likely to be taken and they branch backward, we could always predict taken for branches that jump to an earlier address.  
such rigid way to prediction rely on stereotypical behavior and don't account for the individuality of a specific branch instruction.

__dynamic prediction__ make the guess depending on the behavior of each branch and may change predictions for a branch over the life of a program.  
it's used unversally nowaday, since the accuracy is 90% or higher.

in the end, we introduce an obesolete technique, __delayed branch__.  
it's actually used by MIPS architecture and hidden from the programmer because the assembler can automatically arrange the instructions to get the branch behavior desired by the progranner.  
MIPS software will place an instruction into a slot, called __branch delay slot__. after the delayed branch instruction, the instruction inside the slot will begin executing. then, the result of the delayed branch will be available with the proper instruction to be fetched.  

in the example above, the ```add``` instruction before the branch does not affect the branch and can be moved after the branch.

what is the advantage of this?  
in branch prediction, we will predict whether the branch be taken or not. if we predict wrong, there will be a pipeline stall. but what if we put in the instruction in branch delay slot?  
after the instruction in the slot begins, we will get the branch result and is able to keep going.  
what if we predict right at first? the cost time is still the same. so this technique make wrong predicitions with no penalty.

it sounds wonderful, [but why it's obsolete?](https://stackoverflow.com/questions/54724410/why-is-the-branch-delay-slot-deprecated-or-obsolete)  
Citing Henessy and Patterson (Computer architecture and design, 5th ed.):  
> Fallacy : You can design a flawless architecture.  
> All architecture design involves trade-offs made in the context of a set of hardware and software technologies. Over time those technologies are likely to change, and decisions that may have been correct at the time they were made look like mistakes. (...) An example in the RISC camp is delayed branch. It was a simple matter to control pipeline hazards with five-stage pipelines, but a challenge for processors with longer pipelines that issue multiple instructions per clock cycle.  

in terms of software, delayed branch only has drawbacks as it makes programs more difficult to read and less efficient as the slot is frequently filled by nops.  

In terms of hardware, it was a technological decision that has some sense in the eighties, when pipeline was 5 or 6 stages and there was no way to avoid the one cycle branch penalty.  

But presently, pipelines as much more complex. Branch penalty is 15-25 cycles on recent pentium μarchitectures. One instruction delayed branch is thus useless and it would be a nonsense and clearly impossible to try to hide this delay slot with a 15 instructions delayed branch (that would break instruction sets compatibility).  

And we have developed new technologies. Branch prediction is a very mature technology. With present branch predictors, misprediction is by far lower than the number of branches with a useless delay slot and is accordingly more efficient, even on a 6 cycles computer (like nios-f).  

So delayed branches are less efficient in hardware and software. No reason to keep them.

quotation from https://stackoverflow.com/questions/54724410/why-is-the-branch-delay-slot-deprecated-or-obsolete

## pipeline datapath and control
### pipelined datapath
now we try to build the pipeline version of datapath.  

if look at the illustration of instructions being execution:  
![](https://i.imgur.com/DDA5a0d.png)  
it may seems like that the instructions have their own datapath.  
however, if we build as many datapath as the instructions on executing, the cost will obviously be unacceptable.  
instead, we put a "pipeline registers" between each stages. as shown below.  
> the pipeline version of datapath:  
![](https://i.imgur.com/kTSmZ8Y.png)  

take ```lw``` as example:  
1. IF:
![](https://i.imgur.com/W9DPVZc.png)  
fetch the instrcution addressed by PC and increase PC by 4. store these two in IF/ID register. (the reason why store PC is for branch instruction, and etc...)
2. ID:
![](https://i.imgur.com/zjwDbbA.png)  
break down the instruction. access the register and sign-extend the immediate value. it's no matter whether this instruction is R-type or I-type, the three value will be stored in ID/EX register.  
in this case, what we need is the immediate value and the register for read and for write.
3. EX*:
![](https://i.imgur.com/LYR8kk1.png)  
control unit will determine the value of the multiplexers accroding to the type of instruction and perform the right execution (ALUSrc, ALUOp). in the same time, the immediate will be shifted by 2 and added by PC+4 for the branch instruction.  
these two result will throw into EX/MEM register.  
in this case, the multiplexer is asserted so that the ALU compute the result of \$rs + offset and store it in EX/MEM. (since it's not a branch instruction, the shifted immediate value doesn't matter.)
4. MEM*:
![](https://i.imgur.com/lBNIkdT.png)
accroding to EX/MEM, the control unit will store the result into MEM/WB and access the memory address (MemWrite, MemRead), or determine whether the branch is taken or not (Branch).  
5. WB*:
![](https://i.imgur.com/Q8mhTOq.png)
the control unit will determine which result shall be selected and write back(MemtoReg, RegWrite). if the type isn't involved in writing back to a register, this instruction doesn't actually do anything in this stage.  
in this case, the data in the memory will be write back into the register.

(the star in the stage means that the stage involves in control unit.)

but the method has a bug: which register to write back isn't supplied in the last stage, neither the fourth nor third, but the second.  
hence, we need to preserve the necessary information in pipeline registers for the later stage. as shown below. (for easy implementation, maybe preserve the whole instruction is best.)  
> the corrected pipelined datapath (only consider handling memory accessing):
![](https://i.imgur.com/I0HEOsQ.png)

### pipelined control
we can take the single-cycle datapath as reference.  
recall the figure:  
![](https://i.imgur.com/rgSc491.png)
we don't consider jump instruction for now. take as much as we can from the figure above and place them into the proper place in pipelined datapath.  
![](https://i.imgur.com/YMMIH7A.png)
as the single-cycle datapath, PC will be updated on each clock cycle, so PC won't have a separate write signal.  
same as PC, the pipeline registers will be updated on each clock cycle, so no separate write signal is needed.  

since each control line is associated with a component active in only a single pipeline stage, we need to only set the control value during each stage. we can divede the control lines into five groups based on stage.  
recall the stages which are marked star.  
3. EXE: RegDst, ALUOp, ALUSrc.  
4. MEM: Branch (influence PCSrc), MemRead, MemWrite  
5. WB: MemtoReg, RegWrite.

notice that changing the meaning of the control lines isn't necessary at all, so we can keep them as the same. as shown below.  
![](https://i.imgur.com/xQB0fXu.png)

![](https://i.imgur.com/3XxoxSG.png)

implement control means setting the nine value in each stage. the simplest way is to extend the pipeline registers:  
![](https://i.imgur.com/WbK10JJ.png)

then we come out the pipelined datapath with control signals:  
![](https://i.imgur.com/aFgHbKX.png)  

## data hazards
### forwarding
the example in the last section didn't consider any hazards. so let's bring the first hazard into consider: data hazard.  

first look at a sequence with many dependences:  
```
sub $2, $1, $3
and $12,$2, $5  # 1st operand($2) depends on sub
or  $13,$6, $2  # 2nd operand($2) depends on sub
add $14,$2, $2  # both operands depend on sub
sw  $15,100($2) # base ($2) depends on sub
```
if we don't do something, the initial value of \$2 will effect the later instruction instead of the "correct" one.  
as shown below, the instructions after must wait till WB of the first one, or output the wrong result.  
(there's a potential hazard, what if we write and read the same register in a clock cycle? if the register write in the first half of the clock cycle and read in the second, this hazard will be solved.)  

![](https://i.imgur.com/zejkHki.png)

as mentioned, __forwarding__ can solve this problem.  
we needn't necessarily wait till the WB stage, since the result is available at the end of the EXE stage(namely, clock cycle 3).  
notice that we have pipeline register to perserve the result, so we can simply forward the content in pipeline regitster:  

![](https://i.imgur.com/IjEdbE9.png)

however, some instructions don't write registers, e.g. ```sw```.  
we can simply see whether the RegWrite signal in WB control field of the pipeline registers is asserted or not to decide when to forward.

but there's still a bug here:  
consider a sequence of hacking code:  
```
add $0, $1, $2 
add $3, $0, $0 # shall it forwarding?
```
if we forwarding the result of \$1+\$2, the content in \$3 isn't correct.  
to solve this special problem, we need to modify forwarding condition: forwarding when RegWrite asserted and __EX/MEM.RegisterRd isn't 0 and MEM/WB.RegisterRd isn't 0__.  

for now, we will assume the only instruction we need to forward are the four R-type instructions: ```add,sub,and,or```.  
the figure below shows the partial pipelined datapath with forwarding.  
![](https://i.imgur.com/fQFpbRN.png)  

the forwarding unit will function in the EX stage, because the ALU forwarding multiplexors are in this stage.  

![](https://i.imgur.com/uk38lxA.png)

now we write the conditions for detecting hazards and the forwarding control signal based on above figure:  

1. _EX hazard:_  
```
if (\
   EX/MEM.RegWrite and \
   (EX/MEM.RegisterRd != 0) and \
   (EX/MEM.RegisterRd == ID/EX.RegisterRs)\
   ) 
    ForwardA = 0b10;  
if (\
   EX/MEM.RegWrite and \
   (EX/MEM.RegisterRd != 0) and \
   (EX/MEM.RegisterRd == ID/EX.RegisterRt)\
   )
    ForwardB = 0b10;
```
this case forwards the result from the previous instrcution to either input of ALU.

2. _MEM hazard:_  
```
if (\
   MEM/WB.RegWrite and \
   (MEM/WB.RegisterRd != 0) and \
   (MEM/WB.RegisterRd == ID/EX.RegisterRs)\
   )
    ForwardA = 0b01;
if (\
   MEM/WB.RegWrite and \
   (MEM/WB.RegisterRd != 0) and \
   (MEM/WB.RegisterRd == ID/EX.RegisterRt)\
   )
    ForwardB = 0b01;
```
this case forwads the result from data memory or an earlier ALU result.  

however(yep, another "however"), consider:  
```
add $1, $1, $2
add $1, $1, $3
add $1, $1, $4
```
the condition above will have a conflict, ForwardA shall be 0b10(EX hazard) but also 0b01(MEM hazard).  
obviously, the forwarding from MEM/WB is obsolete, so what we need is the forwarding from EX/MEM (if exists). we modify the _MEM hazard_ condition:  
```
if (\
   MEM/WB.RegWrite and \
   (MEM/WB.RegisterRd != 0) and \
   
   !(\
   MEM/WB.RegWrite and \
   (MEM/WB.RegisterRd != 0) and \
   (MEM/WB.RegisterRd == ID/EX.RegisterRs)\
   ) and \
   
   (MEM/WB.RegisterRd == ID/EX.RegisterRs)\
   )
    ForwardA = 0b01;
    
if (\
   MEM/WB.RegWrite and \
   (MEM/WB.RegisterRd != 0) and \
   
   !(\
   EX/MEM.RegWrite and \
   (EX/MEM.RegisterRd != 0) and \
   (EX/MEM.RegisterRd == ID/EX.RegisterRt)\
   ) and \
   
   (MEM/WB.RegisterRd == ID/EX.RegisterRt)\
   )
    ForwardB = 0b01;
```

figure below shows the hardware necessary to support forwarding for operations that use results during the EX stage.
![](https://i.imgur.com/8PtYa15.png)

**forwarding can also help with hazards when store instructions are dependent on other instructions. and it's actually easy to implement.*  
**do u notice that we forget some I-type instruction, e.g. ```addi, muli```, etc.... it can be solve by adding one more multiplexer. as shown below.*  
![](https://i.imgur.com/4dMOsBZ.png)

### stall
as we mentioned, there's one case where forwarding can't handle. it happens when an instruction tries to read a register following a load instruction that writes the same register. as shown below.  
![](https://i.imgur.com/n4dUDKd.png)

to solve the problem, we need to "stall" a pipeline, which means we do nothing and delay the following instruction by 1 clock.  

hence, we add one more unit, called _hazard detection unit_. it operates during ID stage so that it can insert stall betweem the load and its ues.  
the condition of hazard detection unit:
```
if (\
   ID/EX.MemRead and \
   (\
     (ID/EX.RegisterRt == IF/ID.RegisterRs) or \
     (ID/EX.RegisterRt == IF/ID.RegisterRt)\
   )\
   )
    stall the pipeline
```
after this 1-cycle stall, the forwarding logic will take care the rest part.  

let's talk about how to implement "stall", i.e. do nothing.  
if we stall a pipeline, we expect that every thing won't change, including PC. (so that we can repeat the instruction)  
how can we achieve that? recall the control line involve in the changing of state:  
1. Branch
2. MemWrite
3. RegWrite

if these control lines are deasserted during ID stage, none of state will be changed. to implement easier, we simply deasserted all the nine control lines.  
but PC and IF/ID? they will both be written just after the IF stage, while the hazard detection unit work on ID stage.  
well, we need to connect PC and IF/ID with a write signal (unlike before, we assume they will be updated every clock cycle.) which is usually asserted but when the hazard occurs, it deasserted.  
we call the instruction that doesn't change any state a __nop__.  
the figure shows what happens when stalling a pipeline:  
![](https://i.imgur.com/LFrSwm5.png)

finally, put the pieces of idea into implementation:  
![](https://i.imgur.com/416Zt0q.png)
**the control line, Branch, isn't matter asserted or not after we add a PCWrite signal.*  

## control hazards
as mentioned, there're also pipeline hazards involving branches.  
as shown below:  
![](https://i.imgur.com/X1HxnS7.png)  
the decision about whether to branch doesn't occur till the MEM pipeline stage. this delay in determining the proper instruction to fetch is called _control hazard_ or _branch hazard_.  

branch is a major problem when the pipeline being more deeper. if our prediction is wrong, all the pipeline will waste. eventually, it effect the performance of pipeline significantly. so branch prediction is an important topic we need to study.  
(to take advantage of long pipeline, some program in super computers will deliberately decrease the amount of branch.)

### reducing the delay of branches
before enter the section of branch prediction, we first introduce a simple technique to reduce the delay of branch.  
this technique is moving the branch execution earlier in the pipeline, then fewer instructions need to be flushed.  
moving the branch decision up requires two actions to occur earlier: 
1. computing the branch target address  
for the target address, since we already have the PC value and the immediate field in the IF/ID pipeline register, so we just move the branch adder from the EXE stage to the ID stage.
2. evaluating the branch decision.  
for branch equal, we would compare the two registers read during the ID stage to see if they are equal.  
equality can be tested by first XOR their respective bits and then OR all the results.  
moving the branch test to the ID stage implies additional forwarding and hazard detection hardware.  
for example: one of, or both, the operands in equality comparison is needed from fowarding.  
however, it is possible that a data hazard can occur and a stall will be needed.  
consider the code:  
```
add $1, $3, $4 
beq $1, $2, EQ # when this instruction on ID stage
               # the add instruction is on EXE stage (operating)
               # so a stall is required
```
futher more, a load is immediately followed by a conditional branch that is on the load result, two stall cycles will be needed.  

despite these difficulties, reducing the cost of branch is an improvement, because it reduces the penalty of a branch to only one instruction if the branch is taken.  

to flush instruction in the IF stage, we add a control line, called IF.Flush, which zeros the instruction field of the IF/ID pipeline register. clearing the pipeline register and transforming the fetched instruction into a _nop_.  

__EXAMPLE:__  
show what happens when the branch is taken.  
assuming the pipeline is optimized for branches that are not taken and that we moved the branch execution to the ID stage.
```
36 | sub $10, $4, $8
40 | beq $1,  $3,  7 # PC-relative branch to 40 + 4 + 7 * 4 = 72
44 | and $12, $2, $5
48 | or  $13, $2, $6
52 | add $14, $4, $2
56 | slt $15, $6, $7
. . .
72 | lw  $4, 50($7)
```

ans:  
at clock cycle 3:  
![](https://i.imgur.com/41GJF1f.png)

at clock cycle 4:  
![](https://i.imgur.com/51khtX7.png)

### branch predictor
> an skelton of pipeline in NetBurst(Pentium 4):  
![](https://i.imgur.com/cGBMMCc.png)  

the hardware unit to predict is called __branch predictor unit__ (abbreviation, __BPU__).  

### branch prediction implementation
#### static prediction
1. branch not taken

it's the simplest way to prediction. if the branch is not taken, the instruction will continue; if it is, the instruction that are being fetched and decoded must be discarded. and continue execution at the branch target.  
to discard (__flush__) instructions, we merely change the original control value to 0s, much as we did to stall a pipeline.  
the difference is, in stalling, we need to preserve the info in IF/ID and PC, but in flushing, we need to change the three instructions in the IF, ID, EXE stages.

2. advanced static branch prediction

we find that the branch's usually taken when it jumps to an address before this instruction (loop). so if we predict this branch is jump backward and assume it'll be taken, the performance will increase when in a loop.  
the code below shows how this algorithm works:
```
IF (condition) // prediction: forward branch are not taken
{
    // if condition is true, jump here. (forward)
}

LOOP
{
    // if condition is true, jump here. (backward)
}(condition); // prediction: backward branches are taken

JMP // unconditional jump
```

this strategy can be used when the processor is lack of the history of branch, e.g. Motorola MPC7450 (G4e) and Intel Pentium 4 do like this when predict first branch.  

3. hints static branch prediction

some processors allow inserting hints in instruction manually, telling processor take the branch or not.  
little the processor architecture use this. based on my search, only Intel Pentium 4 has this property.  

source: page 52 and 37 in https://www.agner.org/optimize/microarchitecture.pdf  

in gcc, it provides a way to add branch prediction hint:  
```unlikely(x)``` and ```likely(x)```.  
they're both marco, which is defined as  
```c
#define likely(x)       __builtin_expect(!!(x),1)
#define unlikely(x)     __builtin_expect(!!(x),0)
```

the builtin function, ```long __builtin_expect(long exp, long c)```, means we expect ```exp == c```. and it's will give a hint to the complier, so that the complier can arrange the order of instructions.  
**note that ```c``` in the parameter must be a complie-time constant*.

reference: https://stackoverflow.com/questions/7346929/what-is-the-advantage-of-gccs-builtin-expect-in-if-else-statements  
reference: https://stackoverflow.com/questions/109710/how-do-the-likely-unlikely-macros-in-the-linux-kernel-work-and-what-is-their-ben  
reference: https://www.ibm.com/docs/en/zos/2.2.0?topic=performance-builtin-expect

#### dynamic prediction
static prediction is useful for the simple five-stage pipeline with compiler-based prediction. however, with deeper and deeper pipeline coming out, the penalty of misprediction increasing.

unlike static prediction, __dynamic prediction__ will predict the branch based on the history.  

1. branch history table (BHT)

one simplest way to implement dynamic prediction is having an addition __branch history table__ which indexed by the lower portion of each branch address.  
the table has only one bit in each position, ```0``` means this branch isn't taken last time; ```1``` means this is taken last time.

the example below shows how it works:  

__EXAMPLE:__  
cosider the MIPS pesudocode below.  
```
0 | EQ0:# ...
4 |     # ...
8 |     beq REG1, REG2, EQ1
12| EQ1:# ...
16|     # ...
20|     beq REG1, REG2, EQ2
24| EQ2:# ...
28|     # ...
32|     beq REG1, REG2, EQ3
36| EQ3:# ...
40|     # ...
44|     j 0
```
at the beginning, we have a branch history table:  
```
8 | 0
20| 0
32| 0
```

assume that the first branch is taken, the table will changing into:  
```
8 | 1
20| 0
32| 0
```
after, if the second isn't taken:  
```
8 | 1
20| 0
32| 0
```
... return to first branch, if it's not taken:
```
8 | 0
20| 0
32| 0
```

to increase the accuracy of prediction, we can extend the bit number to n-bit in order to record the recent n-time branch.

2. saturating counter

let's reduce the idea just now. we don't want to use so many space to predict.  
we can simply use one bit to indicate that whether the last branch is taken or not. though this approach isn't good enough, but it can be extended to 2-bit, which's a finite-state machine:  

![](https://i.imgur.com/ZejzM6U.png)

the prefix "strongly" and "weakly" doesn't matter. if the state now is "weakly not taken", the prediction is still "not taken".

both 1-bit saturating counter and 2-bit saturating counter are _one-level branch prediction_.

3. two-level adaptive prediction (bimodal prediction)

there's flaw in the 2-bit saturating counter. if we have a branch instruction which is taken every second time, the 2-bit saturating counter'll have 0% accuracy.  
as shown:  
1. [state = strongly not taken] > the branch is taken
2. [state = weakly not taken] > the branch is taken
3. [state = weakly taken] > the branch isn't taken
4. [state = weakly not taken] > the branch isn't taken (match)
5. [state = strongly not taken] > the branch is taken
...  

since there's a period in the branch here, it cause the accuracy of 2-bit saturating get only 25%, which is unacceptable.  

to solve this problem, we combine two 2 approaches together.  
![](https://i.imgur.com/9SnotqE.png)
_branch history_ is a n-bit variable, which records the result of the last n-time branch.  
_pattern history table_ have 2^n entries, inside each entry has a saturating counter.  

when predict a branch, predictor will use branch history as a index on pattern history table.  

take the above situation as example:  
assume n=2, the length of branch history is 2 bits, and pattern history table has 4 entries.  
after 3 times of the period, i.e. 12 times of branch, the two-level adaptive predictor can perfectly predict the branch.  

but if the period of a branch is 3 or more, we will need more bigger branch history.  

the advantage of two-level adaptive predictor is able to learn a repeated pattern quickly. based on this predictor, many variants are implemented on many processor nowadays.  

4. local branch prediction (correlating prediction)  

extend the branch history to an array.  
every index of it are the addresses of the branches.  
![](https://i.imgur.com/iSPd6xr.png)

![](https://i.imgur.com/YqpNU2n.png)

the Intel Pentium MMX, Pentium II, and Pentium III have local branch predictors with a local 4-bit history and a local pattern history table with 16 entries for each conditional jump.  
on the SPEC'89 benchmarks, very large local predictors saturate at 97.1% correct.  

5. global branch prediction  

two-level adaptive prediction is global branch prediction. the adventage of global branch is it can find out the correlation between instructions. but local branch prediction can't.  
however, global branch prediction is slower than local branch.  

global branch prediction is used in AMD processors, and in Intel Pentium M, Core, Core 2, and Silvermont-based Atom processors.  

6. tournament prediction  

it uses multiple predictors, tracking, for each branch, which predictor yields the best results.  
a typical tournament predictor might contain two predictions for each branch index: one based on local information and one based on global branch behavior.  
a selector would choose which predictor to use for any given prediction. the selector can operate similarly to a 1- or 2-bit predictor, favoring whichever of the two predictors has been more accurate.  
some recent microprocessors use such elaborate predictors.

7. neural branch prediction  

machine learning for branch prediction using LVQ and multi-layer perceptrons, called "neural branch prediction".  
the main advantage is its ability to exploit long histories while requiring only linear resource growth. classical predictors require exponential resource growth.  

The AMD Ryzen multi-core processor's Infinity Fabric and the Samsung Exynos processor include a perceptron-based neural branch predictor.  

### summary
the final datapath and control for this section:  
![](https://i.imgur.com/MrqyvRA.png)

## exceptions
__exceptions__ and __interrupts__ are unexpected events which will disrupt the normal flow of execution of instruction.  
by MIPS convention, an exception, also called a _trap_, is an unexpected event from internal, and an interrupt is from external.  
![](https://i.imgur.com/8PL7FAd.png)

### exceptions in MIPS intro
first, let's look the overview of what happens when an exception occur:  
1. exception occurs
2. the control is transferred to a different program __exception handler__
3. exception handler handle the exception
4. return to the original control flow

from the overview above, we can see there's a require for exception handler. exception handler must preserve the state of the program that was interrupted such that its execution can continue at a later time.  
but that cause a problem, in MIPS, exception handler store the registers into a memory space, but addressing a memory space requires a register. (i don't know why it doesn't put on stack.)  
for that, $k0, $k1 registers are meant to reserve for exception handler. that means user program shall avoid using these two registers.  

### the MIPS exception machanism
the exception mechanism is implemented by the coprocessor 0 which is always present. (unlike coprocessor 1, the floating point unit, which may or may not be present)  

note that, __coprocessor__ is a processor, but it can't independently exist without a main processor. it's meant to support the main process to handle something.  

the CPU operates in one of the two possible modes, __user__ and __kernel__. user programs run in user mode. the CPU enters the kernel mode when an exception happens. coprocessor 0 can only be used in kernel mode.  

when running in kernel mode the registers of coprocessor 0 can be accessed using the following instructions:  
- ```mfc0 Rdest, C0src```  
move the content of coprocessor’s register ```C0src``` to ```Rdest```.  

- ```mtc0 Rsrc, C0dest```  
integer register ```Rsrc``` is moved to coprocessor’s register ```C0dest```.

- ```lwc0 C0dest, address```  
load word from address in register ```C0dest```.

- ```swc0 C0src, address```  
store the content of register ```C0src``` at address in memory.

the relevant registers for the exception handling, in coprocessor 0 are:  

| register number | register name | usage |
| ----- | -------- | -------- |
| 8     | BadVAddr | memory address where exception occurred |
| 12    | Status   | interrupt mask, enable bits, and status when exception occurred |
| 13    | Cause    | type of exception and pending interrupt bits |
| 14    | EPC      | address of instruction that caused exception |

#### BadVAddr
the BadVAddr register will contain the memory address where the exception has occurred. An unaligned memory access, for instance, will generate an exception and the address where the access was attempted will be stored in BadVAddr.  

#### Status register
The Status register contains an interrupt mask on bits 15-10 and status information on bits 5-0.  

#### Cause register
the Cause register provides information about what interrupts are pending and the cause of the exception.  

#### EPC register
the return address can not be saved in \$ra since it may clobber a return address that has been placed in that register before the exception. the Exception Program Counter (EPC) is used to store the address of the
instruction that was executing when the exception was generated.  

more detail: http://www.cs.iit.edu/~virgil/cs470/Labs/Lab7.pdf

instead of Cause register, some architecture use __vector interrupts__.  
in a vector interrupt, the address to which control is transferred is determined by the cause of the exception.  
for example:  
if confronts an undefined instrction, the control will be transferred to 0x8000 0000.  
if confronts an arithmetic overflow, the control will be transferred to 0x8000 0180.  

when the exception is not vectored, a single entry point for all exceptions can be used, and the operating system decodes the status register to find the cause.  

### implement exception in a pipeline
a pipelined implementation treats exceptions as another form of control hazard.  

but there's some difference between branch and exception handling.  

when handling mispredition, we only flush instructions in IF stage.  
however, when an exception occurs, we need to flush not only IF but also ID and EX. that's because we don't kown the dependency between the instruction which generate exception and the following instructions. to prevent the following instruction write the wrong result back to register, we have to flush IF, ID, and EX.  

to flush instructions in the ID stage, we use the multiplexor already in the ID stage that zeros control signals for stalls. a new control signal, called ID.Flush, is OR with the stall signal from the hazard detection unit to flush during ID.  

to flush the instruction in the EX, we use a new signal called EX.Flush to cause new multiplexors to zero the control lines.  

to start fetching instructions from location 0x8000 0180, which is the MIPS exception address, we simply add an additional input to the PC multiplexor that sends 0x8000 0180 to the PC.  
(Although MIPS uses the exception entry address 0x8000 0180 for almost all exceptions, it uses the address 0x8000 0000 to improve performance of the exception handler for TLB-miss exceptions)  

The final step is to save the address of the offending instruction in the EPC. In our implement, we save the address + 4. if it's an exception we encount, we don't need to substract by 4. and if it's an interrupt, we do need to substract by 4. (think about why?)  

![](https://i.imgur.com/KqxvWvH.png)

__EXAMPLE:__  
given this instruction sequence,  
```
40| sub $11, $2, $4
44| and $12, $2, $5
48| or $13, $2, $6
4C| add $1, $2, $1
50| slt $15, $6, $7
54| lw $16, 50($7)
...
```

assume the instructions to be invoked on the exception begin like this:  
```
80000180| sw $26, 1000($0)
80000184| sw $27, 1004($0)
```

show what happens in the pipeline if an overfl ow exception occurs in the ```add``` instruction.

ans:  
![](https://i.imgur.com/WeffOxY.png)  

![](https://i.imgur.com/qqNogPJ.png)

I/O device requests and hardware malfunctions are not associated with a specifi c
instruction, so the implementation has some fl exibility as to when to interrupt the
pipeline.  

the EPC captures the address of the interrupted instructions, and the MIPS Cause register records all possible exceptions in a clock cycle, so the exception soft ware must match the exception to the instruction. an important clue is knowing in which pipeline stage a type of exception can occur.  
for example, an undefined instruction is discovered in the ID stage, and invoking the operating system occurs in the EX stage. Exceptions are collected in the Cause register in a pending exception field so that the hardware can interrupt based on later exceptions, once the earliest one has been serviced.  

## parallelism via instructions
pipeline reveal the potential parallelism among instructions.  
this parallelism is called __instruction-level parallelism (ILP)__.  
it can be approached by two major methods:  
1. increase the depth of pipeline
2. replicate the internal components of the computer, so that it can launch multiple instructions in every pipeline stage, this's called __multiple issue__

launching multiple instructions per stage allows the instruction execution rate to exceed  the CPI to be less than 1.

there're two major ways to implement a multiple-issue processor:  
1. static multiple issue, where many decisions are made by the compiler before execution.
2. dynamic multiple issue, where many decisions are made by the processor during execution.

### the concept of speculation
one of the most important methods for finding and exploiting more ILP is speculation.  
based on the idea of prediction, speculation is an approach that allows the compiler or the processor to “guess” about the properties of an instruction, so as to enable execution to begin for other instructions that may depend on the speculated instruction.  

the difficulty with speculation is that it might go wrong. so any speculation mechanism must include the ability to check if the guess was right and to unroll the effects of the instructions.  

Speculation may be done in the compiler or by the hardware.  
For example, the compiler can use speculation to reorder instructions, moving an instruction across a branch or a load across a store. The processor hardware can perform the same transformation at runtime.  

the recovery mechanisms used by speculation are rather different.  
the compiler usually inserts additional instructions to check the accuracy and provide a fix-up routine.  
in hardware, the processor usually buffers the speculation results until it knows they're no longer speculative. if the speculation is correct, the flow continues. if not, the hardware flushes the buffer and re-execute the correct sequence.  

### static multiple issue
static multiple-issue processors all use the compiler to assist with packaging instructions and handling hazards.  
in a static issue processor, you can think of the set of instructions issued in a given clock cycle, which is called an __issue packet__, as one large instruction with multiple operations.  

since a static multiple-issue processor usually restricts what mix of instructions can be initiated in a given clock cycle, it is useful to think of the issue packet as a single instruction allowing several operations in certain predefined fields. This view led to the original name for this approach: __Very Long Instruction Word (VLIW)__.  

most static issue processors also rely on the compiler to take on some responsibility for handling data and control hazards. the compiler’s responsibilities may include static branch prediction and code scheduling to reduce or prevent all hazards.  

> a static two-issue datapath:  
> (notice that we replicate the datapath)
![](https://i.imgur.com/nk3ochy.png)

an important compiler technique, based on multiple issue, to get more performance from loops is __loop unrolling__, which let more instructions to be issued and overlap on the same clock.  

### dynamic multiple-issue processors
dynamic multiple-issue processors are also known as __superscalar__ processors, or simply superscalars.  
in the simplest superscalar processors, instructions issue in order, and the processor decides whether zero, one, or more instructions can issue in a given clock cycle.  

there is an important difference between this simple superscalar and a VLIW processor: the code, whether scheduled or not, is guaranteed by the hardware to execute correctly.  

![](https://i.imgur.com/ih8Si3B.png)
