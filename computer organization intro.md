---
tags: CSE
---

# computer organization intro
---
> [toc]
---
## logic design
### basic concept
asserted = ture = 1, deasserted = false = 0.  
The values 0 and 1 are called _complements_ or _inverses_ of one another.  

logic blocks are categorized as two types, depending on whether they contain memory.  
blocks without memory are called _combinational_; the output of a combinational block depends only on the current input.  
in blocks with memory, the outputs can depend on both the inputs and the value stored in memory, which
is called the _state_ of the logic block.  

a logic system whose blocks don't contain memory is __combinational logic__, which compute the same output when giving the same input.  
a group of logic elements that contain memory is __sequential logic__, whose output depends on the inputs and current contents of memory.  

#### boolean algebra
in boolean algebra, all values are either 0 or 1, and there're three operation:  
* OR, which is represented by $+$  
* AND, which is represented by $\cdot$  
* NOT, which is represented by a line on operand.  

with that, several rules can be induced:  
* identity law: $A+0=A$ and $A\cdot 1=A$  
* zero and one laws: $A\cdot1=1$ and $A\cdot0=0$  
* inverse laws: $A+\overline{A}=1$ and $A\cdot\overline{A}=0$  
* commutative laws: $A+B=B+A$ and $A\cdot B=B\cdot A$  
* associative laws: $A+(B+C)=(A+B)+C$ and $A\cdot(B\cdot C)=(A\cdot B)\cdot C$  
* distributive laws: $A\cdot(B+C)=(A\cdot B)=(A\cdot C)$ and $A+(B\cdot C)=(A+B)\cdot(A+C)$
* DeMorgan’s laws: $\overline{A+B}=\overline{A}\cdot\overline{B}$ and $\overline{A\cdot B}=\overline{A}+\overline{B}$  

#### gates
![](https://i.imgur.com/tMbMXJR.png)

## combinational Logic
### decoder
> a 3-to-8 decoder
![](https://i.imgur.com/LUxxthu.png)

*there is _encoder_ which performs the inverse function of decoder.

### multiplexors
a multiplexor might more properly be called a _selector_, since its output is one of the inputs that is selected by a control.  
The left side of Figure below shows this multiplexor has three inputs: two __data values__ and a __selector__ (or __control__) __value__.  
as shown below, the multiplexor can be represented as logic equation: $C=(A\cdot 
\overline{S})+(B\cdot S)$.
![](https://i.imgur.com/MesJjma.png)

note that multiplexors can be created with an arbitrary number of data inputs.  
If there are n data inputs, there will need to be $\lceil log_2n\rceil$ selector inputs.  
in this case, the multiplexor basically consists of three parts:  
1. a decoder that generates $n$ signals, each indicating a different input value.  
2. an array of $n$ AND gates, each combining one of the inputs with a signal from the decoder.  
3. a single large OR gate that incorporates the outputs of the AND gates.

### two-level logic and PLAs
any logic function can be implemented with only AND, OR, and NOT functions.  
in fact, a much stronger result is true: any logic function can be written in a canonical form, where every input is either a true or complemented variable and there are only two levels of gates (AND and OR) with a possible inversion on the final output.  
Such a representation is called a _two-level representation_, and there are two forms, called __sum of products__ and __product of sums__.  
(two level logic means that the logic design uses maximum two logic gates between input and output. 
This does not mean that the whole design will contain only two logic gates but the single path from input to output may contain no more than two logic gates.)

a sum-of-products representation is a logical sum (OR) of products (terms using the AND operator); a product of sums is just the opposite.  

for example, $E = (A\cdot B\cdot \overline{C})+(A\cdot \overline{B}\cdot C)+(\overline{A}\cdot B\cdot C)$ is sum of products.  

In this text, we use the sum-of-products form. It is easy to see that any logic function can be represented as a sum of products by constructing such a representation from the truth table for the function.  
Each truth table entry for which the function is true corresponds to a product term. The product term consists of a logical product of all the inputs or the complements of the inputs, depending on whether the entry in the truth table has a 0 or 1 corresponding to
this variable.  
The logic function is the logical sum of the product terms where the function is true.  

__EXAMPLE:__  
A, B and C is inputs. D is output.
![](https://i.imgur.com/PYKFJDv.png)

D is true for four input combinations:  
1. $\overline{A}\cdot\overline{B}\cdot C$
2. $\overline{A}\cdot B\cdot C$
3. $A\cdot\overline{B}\cdot\overline{C}$
4. $A\cdot B\cdot C$

thus, $D=(\overline{A}\cdot\overline{B}\cdot C)+(\overline{A}\cdot B\cdot C)+(A\cdot\overline{B}\cdot\overline{C})+(A\cdot B\cdot C)$

----
the sum-of-products representation corresponds to a common structured-logic implementation called a __programmable logic array(PLA)__.  
a PLA has a set of inputs and corresponding input complements (which can be implemented with a set of inverters), and two stages of logic.  
The first stage is an array of AND gates that form a set of __product terms__ (sometimes called __minterms__); each product term can consist of any of the inputs or their complements.  
The second stage is an array of OR gates, each of which forms a logical sum of any number of the product terms.  
> a basic form of PLA:  
![](https://i.imgur.com/dnGWf6l.png)

A PLA can directly implement the truth table of a set of logic functions with multiple inputs and outputs.  
Since each entry where the output is true requires a product term, there will be a corresponding row in the PLA. Each output corresponds to a potential row of OR gates in the second stage. The number of OR gates corresponds to the number of truth table entries for which the output is true.  

__EXAMPLE:__  
> the truth table:  
> ![](https://i.imgur.com/V2UD6N9.png)

since there are seven unique product terms with at least one true value in the output section, there will be seven columns in the AND plane.  
The number of rows in the AND plane is three (since there are three inputs), and there are also three rows in the OR plane (since there are three outputs).  
> the PLA for implementing the logic function:  
> ![](https://i.imgur.com/0U8ChBP.png)

Rather than drawing all the gates, designers often show just the position of AND gates and OR gates.  
Dots are used on the intersection of a product term signal line and an input line or an output line when a corresponding AND gate or OR gate is required.  
as shown below:  
![](https://i.imgur.com/JBdkkoY.png)

### ROMs
Another form of structured logic that can be used to implement a set of logic functions is a __read-only memory (ROM)__.  
A ROM is called a memory because it has a set of locations that can be read; however, the contents of these locations are fixed, usually at the time the ROM is manufactured.  

A ROM has a set of input address lines and a set of outputs.  
The number of addressable entries in the ROM determines the number of address lines: if the ROM contains $2^m$ addressable entries, called the _height_, then there are $m$ input lines.  
The number of bits in each addressable entry is equal to the number of output bits and is sometimes called the _width_ of the ROM.  

A ROM can encode a collection of logic functions directly from the truth table.  
For example, if there are $n$ functions with $m$ inputs, we need a ROM with $m$ address lines (and $2^m$ entries), with each entry being $n$ bits wide.  
The entries in the input portion of the truth table represent the addresses of the entries in the ROM, while the contents of the output portion of the truth table constitute the contents of the ROM.  

comparing PLA and ROM, PLA is partially decoded while ROM is fully decoded, which means ROM contains more entries. this differece makes PLA generally more efficient for implementing combinational logic function, however, in the other hand, ROM have the advantage of being able to implement any logic function with the matching number of inputs and outputs, making it easier to change the ROM contents if the logic function changes.  

### don't care
Oft en in implementing some combinational logic, there are situations where we do not care what the value of some output is.  
Don’t cares are important because they make it easier to optimize the implementation of a logic function.  

There are two types of don’t cares: output don’t cares and input don’t cares, both of which can be represented in a truth table.  
_Output don’t cares_ arise when we don’t care about the value of an output for some input combination. They appear as X in the output portion of a truth table. When an output is a don’t care for some input combination, the designer or logic optimization program is free to make the output true or false for that input combination.  
_Input don’t cares_ arise when an output depends on only some of the inputs, and they are also shown as X, though in the input portion of the truth table.  

__EXAMPLE:__  
Consider a logic function with inputs A, B, and C defi ned as follows:
* If A or C is true, then output D is true, whatever the value of B.
* If A or B is true, then output E is true, whatever the value of C.
* Output F is true if exactly one of the inputs is true, although we don’t care about the value of F, whenever D and E are both true.  

> truth table without don't care:  
> ![](https://i.imgur.com/Aib8em6.png)
> 
> truth table with output don't care:  
> ![](https://i.imgur.com/Ffxg6B9.png)
> 
> truth table with both don't care:  
> ![](https://i.imgur.com/5e25C2u.png)

### arrays of logic elements
Many of the combinational operations to be performed on data have to be done to an entire word (32 bits) of data. Thus we often want to build an array of logic elements, which we can represent simply by showing that a given operation will happen to an entire collection of inputs.  

Inside a machine, much of the time we want to select between a pair of _buses_. A __bus__ is a collection of data lines that is treated together as a single logical signal.  
For example, in the MIPS instruction set, the result of an instruction that is written into a register can come from one of two sources. A multiplexor is used to choose which of the two buses (each 32 bits wide) will be written into the Result register. The 1-bit multiplexor, which we showed earlier, will need to be replicated 32 times.  

When we show a logic unit whose inputs and outputs are buses, this means that the unit must be replicated a sufficient number of times to accommodate the width of the input.  
![](https://i.imgur.com/EyYuPJ5.png)

## constructing a basic ALU
### 1-bit ALU
The 1-bit logical unit for AND and OR looks like:  
![](https://i.imgur.com/e0C29Je.png)

The next function to include is addition. An adder must have two inputs for the operands and a single-bit output for the sum. There must be a second output to pass on the carry, called _CarryOut_. Since the CarryOut from the neighbor adder must be included as an input, we need a third input, called _CarryIn_. (if don't it will be a half adder)  

We can express the output functions CarryOut and Sum as logical equations, and these equations can in turn be implemented with logic gates.  
$\text{CarryOut} = (\text{b}\cdot\text{CarryIn})+(\text{a}\cdot\text{CarryIn})+(\text{a}\cdot\text{b})$  
$\text{Sum} = (\text{a}\cdot\overline{\text{b}}\cdot\overline{\text{CarryIn}})+(\overline{\text{a}}\cdot\text{b}\cdot\overline{\text{CarryIn}})+(\overline{\text{a}}\cdot\overline{\text{b}}\cdot\text{CarryIn})+(\text{a}\cdot\text{b}\cdot\text{CarryIn})$  

we will ignore the detail of gate implementation, but draw an adder as a block:  
![](https://i.imgur.com/znO8vH3.png)

the figure shown below is a 1-bit ALU derived by combining the adder with logic unit:  
![](https://i.imgur.com/2eRdqc4.png)

### a 32-bit ALU
![](https://i.imgur.com/1tKC13H.png)

now we only implement the addition function. we  need substraction and NOR gate.  
since $A-B=A+(-B)$ so we implement the 1-bit ALU like:  
![](https://i.imgur.com/1HKvmaH.png)

when the operation is substraction, Binvert will be set to 1 with the CarryIn of the first ALU is 1.  

now consider NOR gate: $\overline{A+B}=\overline{A}\cdot\overline{B}$, so we only need to add an Ainvert:  
![](https://i.imgur.com/u2JNxYe.png)

there's one instruction that still needs support, that is ```slt```.  
since $a<b\iff a-b<0$, so we need only detect the value in the last ALU whether it's one (less) or zero:  
> 1-bit ALU with overflow detection:  
> ![](https://i.imgur.com/vyYzXhN.png)

then we can construct 32-bit ALU now:  
> 32-bit ALU:  
![](https://i.imgur.com/L2jNZXY.png)

Notice that every time we want the ALU to subtract, we set both CarryIn and Binvert to 1. For adds or logical operations, we want both control lines to be 0. We can therefore simplify control of the ALU by combining the CarryIn and Binvert to a single control line called _Bnegate_.  

To further tailor the ALU to the MIPS instruction set, we need support compare whether two variable is equal or not. (```beq, bne```).  
since $a=b\iff a-b=0$, we only need to compute the substraction and determine the value $\overline{(\sum^{31}_{i=0}\text{Result}\,i)}$ is $1$ or $0$.  
figure below shows the implementation:  
![](https://i.imgur.com/kiaqWk5.png)

We can think of the combination of the 1-bit Ainvert line, the 1-bit Binvert line, and the 2-bit Operation lines as 4-bit control lines for the ALU, telling it to perform add, subtract, AND, OR, or set on less than.  

## addition optimization carry lookahead
in the last section, we implement the addition by using ripple carry. however, the performence of ripple carry isn't really good, since the "carry bit" is computed slowly from the lowest to the highest whose complexity is $O(n)$, $n$ represents $n$-bit variable.  

the key to optimize the ripple addition is to let the carry bit independent from the "last result".  
![](https://i.imgur.com/RX7dpJC.png)

we know that carry bit depends on input operand and last carry bit, so we may imagine there's a circuit compute that.
![](https://i.imgur.com/AA357HV.png)

assume $c_i$ is carryIn bit at $i$-th place, and $a_i,b_i$ are the operands at $i$-th place.  
we can derive the formula:  
$c_{i+1}=(a_i\cdot b_i)+(a_i\cdot c_i)+(b_i\cdot c_i)=(a_i\cdot b_i)+(a_i+b_i)\cdot c_i$  

to simplify the formula, let $g_i=a_i\cdot b_i,\,p_i=a_i+b_i$. then we get $c_{i+1}=g_i+p_i\cdot c_i$.  

so,  
$c_1=g_0+(p_0\cdot c_0)$  
$c_2=g_1+(p_1\cdot g_0)+(p_1\cdot p_0\cdot c_0)$  
$c_3=g_2+(p_2\cdot g_1)+(p_2\cdot p_1\cdot g_0)+(p_2\cdot p_1\cdot p_0\cdot c_0)$  
$c_4=g_3+(p_3\cdot g_2)+(p_3\cdot p_2\cdot g_1)+(p_3\cdot p_2\cdot p_1\cdot g_0)+(p_3\cdot p_2\cdot p_1\cdot p_0\cdot c_0)$  
we can know that even this simplified form still leads to a large equations, which having bad performence still, so we now use only 4-bit carry computation:  
![](https://i.imgur.com/r2Kr4rU.png)

the complexity is $O(n/4)$.  

however, if we now consider 4-bit carry computation unit as a kind of add, we eventually come up a idea:  
![](https://i.imgur.com/7QTYiTr.png)

seems like the first complexity reducing idea, isn't it?  
focus on the math formula,  
$c_4=g_3+(p_3\cdot g_2)+(p_3\cdot p_2\cdot g_1)+(p_3\cdot p_2\cdot p_1\cdot g_0)+(p_3\cdot p_2\cdot p_1\cdot p_0\cdot c_0)$  
$c_8=g_7+(p_7\cdot g_6)+(p_7\cdot p_6\cdot g_5)+(p_7\cdot p_6\cdot p_5\cdot g_4)+(p_7\cdot p_6\cdot p_5\cdot p_4\cdot c_4)$...  
we notice that if let $G_i=g_{4i+3}+(p_{4i+3}\cdot g_{4i+2})+(p_{4i+3}\cdot p_{4i+2}\cdot g_{4i+1})+(p_{4i+3}\cdot p_{4i+2}\cdot p_{4i+1}\cdot g_{4i})$, $P_i=p_{4i+3}\cdot p_{4i+2}\cdot p_{4i+1}\cdot p_{4i}$, and $C_i=c_{4i}$.  
the carry bit of 4-bit carry computation unit is actually $C_{i+1}=G_i+P_i\cdot C_{i}\iff c_{4(i+1)}=G_i+P_i\cdot c_{4i}$.  
thus, we can write down the expansion formula of $C_i$ just like $c_i$, and reuse the 4-bit carry computation unit:  
![](https://i.imgur.com/gF4jj1R.png)

since we can keep stacking up the carry computation unit when the computaion bit increasing, which eventually constructs a tree structure, the complexity is $O(log_2 n)$.  

__EXAMPLE:__  
determine the $g_i$, $p_i$, $P_i$, and $G_i$ values of these two 16-bit numbers:  
$a=\text{0b0001 1010 0011 0011}$  
$b=\text{0b1110 0101 1110 1011}$  
also, determine carryOut15 (which is actually $c_{16}=C_4$).  

$g_i=a\cdot b=\text{0b0000 0000 0010 0011}$  
$p_i=a+b=\text{0b1111 1111 1111 1011}$  

$P_0=1\cdot0\cdot1\cdot1=0$  
$P_1=P_2=P_3=1\cdot1\cdot1\cdot1$  

$G_0=0+1\cdot0+1\cdot0\cdot1+1\cdot0\cdot1\cdot1=0$  
$G_1=1$  
$G_2=0$  
$G_3=0$  

$C_4=G_3+P_3\cdot C_3=G_3+P_3\cdot G_2+P_3\cdot P_2\cdot C_2=...=G_3+P_3\cdot G_2+P_3\cdot P_2\cdot G_1+P_3\cdot P_2\cdot P_1\cdot G_0+P_3\cdot P_2\cdot P_1\cdot P_0\cdot C_0\\=0+0+1+0+0=1$  

hence, $C_4$ which is carryOut15 is 1.  

## clocks
_Clocks_ are needed in sequential logic to decide when an element that contains state should be updated.  
As shown in figure below, the _clock cycle time_ or _clock period_ is divided into two portions: when the clock is high and when the clock is low.  
![](https://i.imgur.com/2Z9tYqI.png)

in later section, we use only __edge-triggered clocking__. This means that all state changes occur on a clock edge.  

In an edge-triggered methodology, either the rising edge or the falling edge of the clock is _active_ and causes state changes to occur. As we will see in the next section, the state elements in an edge-triggered design are implemented so that the contents of the state elements only change on the active clock edge.  

The major constraint in a __synchronous system__ is
that the signals that are written into state elements must be _valid_ when the active clock edge occurs.  
A signal is valid if it is stable (i.e., not changing), and the value will not change again until the inputs change. Since combinational circuits cannot have feedback, if the inputs to a combinational logic unit are not changed, the outputs will eventually become valid.  

figure below shows the relationship among the state elements and the combinational logic blocks in a synchronous, sequential logic design.  
The state elements, whose __outputs change only after the clock edge__, provide valid inputs to the combinational logic block. To ensure that the values written into the state elements on the active clock edge are valid, the clock must have a long enough period so that all the signals in the combinational logic block stabilize.  
This constraint sets a lower bound on the length of the clock period, which must be long enough for all state element inputs to be valid.  
![](https://i.imgur.com/lJ6luzM.png)

One other advantage of an edge-triggered methodology is that it is possible to have a state element that is used as both an input and output to the same combinational logic block, as shown:  
![](https://i.imgur.com/yv6i0yp.png)

In practice, care must be taken to prevent races in such situations and to ensure that the clock period is long enough.  

## memory elements: flip-flops, latches, and registers

### S-R latch
what comes to the first is RS-latch. it's _unclock_, that is, no clock signal input.

> S-R latch(set-reset latch): when $\text{S}$ is asserted, $\text{Q}$ will be asserted; when $\text{R}$ is asserted, $\overline{\text{Q}}$ is asserted; __when both__ $\text{S}$ __and__ $\text{R}$ __are not assert, the state will be hold__.  
> *note that it's not allow that both $S$ and $R$ are asserted.  
![](https://i.imgur.com/9RP8A65.png)

### flip-flop and latch
Flip-flops and latches are the simplest memory elements. In both __flip-flops__ and __latches__, the output is equal to the value of the stored state inside the element.  
Furthermore, unlike the S-R latch described above, all the latches and flip-flops we will use are clocked.  

The difference between a fip-flop and a latch is the point at which the clock causes the state to actually change.  
In a clocked latch, the state is changed whenever the appropriate inputs change and the clock is asserted, whereas in a flip-flop, the state is changed only on a clock edge.  

> a D-latch (or D flip-flop, but it's actually not a flip-flop):  
> when the clock input \(C\) is asserted, the latch is _open_, and the value of the ouput (Q) becomes the value of input (D).  
> when it's deasserted, the latch is _closed_, and the value of output will be hold.  
> ![](https://i.imgur.com/XWy94iS.png)  
> 
> the operation of a D-latch:
> ![](https://i.imgur.com/14cIHCE.png)


Since when the latch is open the value of Q changes as D changes, this structure is sometimes called a _transparent latch_.  
Flip-flops are not transparent: their outputs change only on the clock edge. A flip-flop can be built so that it triggers on either the rising (positive) or falling (negative) clock edge; for our designs we can use either type.  
figure below shows a falling-edge D flip-flop is constructed from a pair of D latch.  
> a D flip-flop with a falling-edge trigger:
> ![](https://i.imgur.com/f6FJJkF.png)
> 
> operation of a D flip-flop:
> ![](https://i.imgur.com/r6JEyoy.png)

Because the D input is sampled on the clock edge, it must be valid for a period of time immediately before and immediately aft er the clock edge. The minimum
time that the input must be valid before the clock edge is called the __setup time__; the minimum time during which it must be valid aft er the clock edge is called the __hold time__.  
![](https://i.imgur.com/AkizH4v.png)

### register files
One structure that is central to our datapath is a _register file_. A register file consists of a set of registers that can be read and written by supplying a register number to be accessed.  

A register file can be implemented with a decoder for each read or write port and an array of registers built from D flip-flops with a falling-edge trigger.  

Because reading a register does not change any state, we need only supply a register number as an input, and the only output will be the data contained in that register.  
The read ports can be implemented with a multiplexor, which is as wide as the number of bits in each register of the register file.  

For writing a register we will need three inputs: a register number, the data to write, and a clock that controls the writing into the register.  
We can write only one selected register by using a decoder to generate a signal that can be used to determine which register to write.  

note that read port can be implemented as mutilple.  
here is an example of two read port register file:  
![](https://i.imgur.com/SYhPmtf.png)

, the implementation of read ports:  
![](https://i.imgur.com/p9dYD5G.png)

, and the implementaion of write port:  
![](https://i.imgur.com/eaXHPeP.png)

What happens if the same register is read and written during a clock cycle?  
The value returned will be the value written in an earlier clock cycle, since the output of D flip-flops  changes only after clock edge falling.  
If we want a read to return the value currently being written, additional logic in the register file or outside of it is needed.  

## memory elements: SRAMs and DRAMs
Registers and register files provide the basic building blocks for small memories, but larger amounts of memory are built using either __SRAMs (static random access memories)__ or __DRAMs (dynamic random access memories)__.  

### SRAMs
SRAMs are simply integrated circuits that are memory arrays with (usually) a single access port that can provide either a read or a write. SRAMs have a fixed access time to any address, though the read and write access characteristics often differ.  
An SRAM chip has a specific configuration in terms of the number of addressable locations, as well as the width of each addressable location.  
for example, a 4M $\times$ 8 SRAM provides 4M entries, each of which is 8 bits wide. Thus it will have 22 address lines (since 4M=2^22), an 8-bit data output line, and an 8-bit single data input line.  
> a 2M $\times$ 16 SRAM:
![](https://i.imgur.com/Ng6etLQ.png)

To initiate a read or write access, the Chip select signal must be made active.  
For reads, we must also activate the Output enable signal that controls whether or not the address selected by the address is actually driven on the pins.  
The Output enable is useful for connecting multiple memories to a single-output bus and using Output enable to determine which memory drives the bus.  

For writes, we must supply the data to be written and the address, as well as signals to cause the write to occur. When both the Write enable and Chip select are true, the data on the data input lines is written into the cell specifi ed by the address. There are setup-time and hold-time requirements for the address and data lines, just as there were for D flip-flops and latches.  
In addition, the Write enable signal is not a clock edge but a pulse with a minimum width requirement. Th e time to complete a write is specifi ed by the combination of the setup times, the hold times, and the Write enable pulse width.  

Large SRAMs cannot be built in the same way we build a register file, since a 2M-to-1(one 16-bit bus) multiplexor is totally impractical.  
Rather than use a giant multiplexor, large memories are implemented with a shared output line, called a _bit line_, which multiple memory cells in the memory array can assert.  
To allow multiple sources to drive a single line, a __three-state buffer__ (or __tri-state buffer__) is used.  

#### three-state buffer
A three-state buffer has two inputs, a data signal and an Output enable, and a single output, which is in one of three states: asserted, deasserted, or high impedance.  
The output of a tristate buffer is equal to the data input signal, either asserted or deasserted, if the Output enable is asserted.  
otherwise, in a _high-impedance state_ that allows another three-state buffer whose Output enable is asserted to determine the value of a shared output.  

> a set of tri-state buffers wired to form a multiplexor:
> ![](https://i.imgur.com/0I4E8Q0.png)

The three-state buffers are incorporated into the flip-flops that form the basic cells of the SRAM. figure below shows how a small $4\times2$ SRAM might be built, using D latches with an input called Enable that controls the three-state output.  
![](https://i.imgur.com/1T9z21H.png)

The design eliminates the need for an enormous multiplexor; however, it still requires a very large decoder and a correspondingly large number of word lines. For example, in a 4M $\times$ 8 SRAM, we would need a 22-to-4M decoder and 4M word lines.  
To circumvent this problem, large memories are organized as rectangular arrays and use a two-step decoding process. here is an example:  
![](https://i.imgur.com/nSLbreX.png)

### DRAMs
In a static RAM (SRAM), the value stored in a cell is kept on a pair of inverting gates, and as long as power is applied, the value can be kept indefinitely.  
In a dynamic RAM, the value kept in a cell is stored as a charge in a capacitor. A single transistor is then used to access this stored charge. since that, DRAM is much denser and cheaper per bit. (SRAMs require four to six transistors per bit.)  
however, because DRAMs store the charge on a capacitor, it must periodically be _refreshed_ (read its contents and write it back). That is why this memory structure is called _dynamic_.  

How does a DRAM read and write the signal stored in a cell?  
![](https://i.imgur.com/WY7sqmg.png)  
The pass transistor acts like a switch: when the signal
on the word line is asserted, the switch is closed, connecting the capacitor to the bit line.  
If the operation is a write, then the value to be written is placed on the bit line. If
the value is a 1, the capacitor will be charged. If the value is a 0, then the capacitor will
be discharged.  
Reading is slightly more complex, since the DRAM must detect a very small charge stored in the capacitor. Before activating the word line for a read, the bit line is charged to the voltage that is halfway between the low and high voltage. Then, by activating the word line, the charge on the capacitor is read out onto the bit line. This causes the bit line to move slightly toward the high or low direction, and this change is detected with a sense amplifier, which can detect small changes in voltage.  

### error correction
[hamming code](https://en.wikipedia.org/wiki/Hamming_code)

## finite state machine
Moore machine: the output funcstion only depends on current state.  
Mealy machine: the output funcstion depends on both current state and input.  

## timing methodologies
In an edge-triggered design, the clock cycle must be long enough to accommodate the path from one flip-flop through the combinational logic to another flip-flop.  
the clock period (or cycle time) of such a system must be at least as large as:  
$t_{\text{prop}}+t_{\text{combinational}}+t_{\text{setup}}$  
* $t_{\text{prop}}$ is the time for a signal to propagate through a flip-flop; it is also sometimes called clock-to-Q.  
* $t_{\text{combinational}}$ is the longest delay for any combinational logic (which by definition is surrounded by two flip-flops).  
* $t_{text{setup}}$ is the time before the rising clock edge that the input to a flip-flop must be valid.  

One additional complication that must be considered in edge-triggered designs is __clock skew__.  
Clock skew is the difference in absolute time between when two state elements see a clock edge. it arises because the clock signal will often use two different paths which cause slightly different delays.  

![](https://i.imgur.com/6LvLwQS.png)

To avoid incorrect operation, the clock period is increased to allow for the maximum clock skew. Thus, the clock period must be longer than  
$t_{\text{prop}}+t_{\text{combinational}}+t_{\text{setup}}+t_{skew}$  

Edge-triggered designs have two drawbacks: they require extra logic and they may sometimes be slower.  
An alternative is to use __level-sensitive clocking__.  

### level-sensitive timing
In level-sensitive timing, the state changes occur at either high or low levels, but they are not instantaneous as they are in an edge-triggered methodology.  
Because of the noninstantaneous change in state, races can easily occur. To ensure that a level-sensitive design will also work correctly if the clock is slow enough, designers use _two-phase clocking_.  
Two-phase clocking is a scheme that makes use of two nonoverlapping clock signals.  
![](https://i.imgur.com/G17mWYr.png)  

![](https://i.imgur.com/3lxUCwu.png)  

As in an edge-triggered design, we must pay attention to clock skew, particularly between the two clock phases.  

### asynchronous inputs and synchronizers
By using a single clock or a two-phase clock, we can eliminate race conditions if clock-skew problems are avoided.  
Unfortunately, it is impractical to make an entire system function with a single clock and still keep the clock skew small.  
While the CPU may use a single clock, I/O devices will probably have their own clock. An asynchronous device may communicate with the CPU through a series of handshaking steps.  
To translate the asynchronous input to a synchronous signal that can be used to change the state of a system, we need to use a _synchronizer_, whose inputs are the asynchronous signal and a clock and whose output is a signal synchronous with the input clock.  

assume we communicate with a handshaking protocol, namely, it does not matter whether we detect the asserted state of the asynchronous signal on one clock or the next, since the signal will be held asserted until it is acknowledged.  
so the first attempt is to build a synchronizer by a D flip-flop:  
![](https://i.imgur.com/0kMhZ92.png)

however, the simple structure still has a problem, __metastability__.  


In metastable states, the output will not have a legitimate high or low value, but will be in the indeterminate region between them. Furthermore, the flip-flop is not guaranteed to exit this state in any bounded amount of time.  
Some logic blocks that look at the output of the flip-flop may see its output as 0, while others may see it as 1. This situation is called a __synchronizer failure__.  

the only solution possible is to wait long enough before looking at the output of the flip-flop to ensure that its output is stable, and that it has exited the metastable state, if it ever entered it.  
the probability that the flip-flop will stay in the metastable state decreases exponentially, so after a very short time the probability that the flip-flop is in the metastable state is very low, but never reach to 0.  

For most flip-flop designs, waiting for a period that is several times longer than the setup time makes the probability of synchronization failure very low.  
If the clock rate is longer than the potential metastability period (which is likely), then a safe synchronizer can be built with two D flip-flops:  
![](https://i.imgur.com/szzQljj.png)

this is an example of how it work:
![](https://i.imgur.com/ZrEDZdw.png)