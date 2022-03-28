---
tags: CSE
---
# mircoelectronics ch3

---
> [TOC]
---
## ideal diode
![](https://i.imgur.com/KFVBdYO.png)

__forward and reverse bias:__  
the diode turn “on” if $V_{anode} > V_{cathode }$and “off” if $V_{anode}<V_{cathode}$.  
Defining $V_{anode}-V_{cathode} = V_D$, we say the diode is “forward-biased” if $V_D$ tends to exceed zero and “reverse-biased” if $V_D <0$.  

__I/V characteristics:__  
![](https://i.imgur.com/LnIfq3Z.png)

__EXAMPLE:__  
plot the I/V characteristic for the “antiparallel” diodes shown below.  
![](https://i.imgur.com/NtkefUm.png)

ans:  
![](https://i.imgur.com/t6JIRlR.png)

__EXAMPLE:__  
Plot the I/V characteristic for the diode-resistor combination of figure below.  
![](https://i.imgur.com/A7f2xhS.png)

ans:  
![](https://i.imgur.com/GTtRlz8.png)

__EXAMPLE:__  
In the circuit of figure below, each input can assume a value of either zero or $+3 V$. Determine the response observed at the output.  
![](https://i.imgur.com/bgOPokE.png)

ans:  
if one of the input has the output voltage $3V$, the output would have the output of $3V$, too.  

__I/O characteristics:__  
electronic circuits process an input and generate a corresponding output. It is therefore instructive to construct the input/output characteristics of a circuit by varying the input across an allowable range and plotting the resulting output.  

as an example, consider the circuit depicted below.  
![](https://i.imgur.com/3VyHekE.png)  

if $V_{in}< 0$, $D_1$ is reverse biased, reducing the circuit to  
![](https://i.imgur.com/vPsP8Jc.png)  
since no current flows through $R_1$, we have $V_{out} = V_{in}$  

If $V_{in} > 0$, then $D_1$ is forward biased, shorting the output and forcing $V_{out} = 0$.  
![](https://i.imgur.com/BYd9aEk.png)

the overall characteristic:  
![](https://i.imgur.com/GT5nLfb.png)

## application examples
Let us now design a circuit that performs this function.  
We may naturally construct the circuit as shown below  
![](https://i.imgur.com/4mHrvKR.png)  

Unfortunately, however, the cathode of the diode is “floating,”(no loop) the output current is always equal to zero, and the state of the diode is ambiguous.  

We therefore modify the circuit to:  
![](https://i.imgur.com/rwrhjHw.png)

Since $R_1$ has a tendency to maintain the cathode of $D_1$ near zero, as $V_{in}$ rises, $D_1$ is forward biased, shorting the output to the input.  
This state holds for the positive half cycle. When $V_{in}$ falls below zero, $D_1$ turns off and $R_1$ ensures that $V_{out} = 0$ because $I_DR_1 = 0$.

suppose $V_{in}=V_psin(\omega t)$ where $\omega=2\pi/T$, we have
\\$$V_{out}=\begin{cases}V_psin(\omega t)&,0\leq t\leq T/2\\0&,T/2\leq t\leq T\end{cases}$$

and
\\$$V_{out}(t+T)=V_{out}(t)$$

to compute the average(DC), 
\\$$V_{out,avg}=\frac{1}{T}\int^T_0V_{out}(t)dt=V_p/\pi$$


![](https://i.imgur.com/sWstogO.png)

the circuit of (b) is called a "rectifier."  
It is instructive to plot the input/output characteristic of the circuit as well.  
The rectifier is a nonlinear circuit because if $V_{in} \to −V_{in}$ then $\require{cancel}V_{out} \cancel{\to} -V_{out}$.  

In our effort toward understanding the role of diodes, we examine another circuit that
will eventually lead to some important applications.  
consider the circuit below:  
![](https://i.imgur.com/ego9cR8.png)  

How does this circuit behave? If $V_1< 0$, the cathode voltage is higher than the anode voltage, placing $D_1$ in reverse bias. Even if $V_1$ is slightly greater than zero, e.g., equal to $0.9 V$, the anode is not positive enough to forward bias $D_1$. Thus, $V_1$ must approach $+1 V$ for $D_1$ to turn on.  
I/V characteristic:  
![](https://i.imgur.com/eX1O85S.png)

now let examine the circuit below:  
![](https://i.imgur.com/4UuiOab.png)

the behavior:  
![](https://i.imgur.com/Ktj3b1P.png)

the result can be obtained by solving $\begin{cases}V_{in}-iR_1-V_D-V_B=0\\V_B-V_D-V_{out}=0\\V_{in}-iR_1-V_{out}=0\end{cases}\iff V_D=0,V_{out}=V_B$  


as depicted, the modification indeed guarantees
$V_{out} ≤ +1 V$ for any input level.  
We say the circuit “clips” or “limits” at $+1 V$. “Limiters” prove useful in many applications.  

__EXAMPLE:__  
Sketch the time average of $V_{out}$ in circuit above for a sinusoidal input as the battery voltage, $V_B$, varies from $−∞$ to $+∞$.  

ans:  
![](https://i.imgur.com/Tq8CGWx.png)

## pn junction as a diode
![](https://i.imgur.com/a9W32tu.png)

Shown in figure \(c\) above is the constant-voltage model developed in Chapter 2, providing a simple approximation of the exponential function and also resembling the characteristic just examined.  

Given a circuit topology, how do we choose one of the above models for the diodes?  
We may utilize the ideal model so as to develop a quick, rough understanding of the circuit’s operation. Upon performing this exercise, we may discover that this idealization is inadequate and hence employ the constant-voltage model. This model suffices in most cases, but we may need to resort to the exponential model for some circuits. The following examples illustrate these points.  

It is important to bear in mind two peinciples:   
1. if a diode is at the edge of turning on or off, then $I_D ≈ 0$ and $V_D ≈ V_{D,on}$; 
2. if a diode is on $I_D$ must flow from anode to cathode.  

__EXAMPLE:__  
Plot the input/output characteristic of the circuit shown below by using  
1. the ideal model
2. the constant-voltage model  

![](https://i.imgur.com/xc8f10Y.png)

ans:  
1. the ideal model  

![](https://i.imgur.com/WsImRCh.png)  

2. the constant-voltage model  

![](https://i.imgur.com/WwjvWmr.png)

the equivalent circuit when $V_{out}>V_{D,on}$ in constant-voltage model:  
![](https://i.imgur.com/MKOvZB5.png)  

In this case, $D_1$ is reverse biased for $V_{in} < V_{D,on}$, yielding $V_{out} = V_{in}$.  
As $V_{in}$ exceeds $V_{D,on}$, $D_1$ turns on, operating as a constant voltage source with a value $V_{D,on}$. Reducing the circuit to that above, we apply Kirchoff’s current law to the output node: 
\\$$\frac{V_{in}-V_{out}}{R_1}=\frac{V_{out}-V_{D,on}}{R_2}$$

follows,
\\$$V_{out}=\frac{\frac{R_2}{R_1}V_{in}+V_{D,on}}{1+\frac{R_2}{R_1}}$$

__EXAMPLE:__  
In the circuit below, $D_1$ and $D_2$ have different cross section areas but are otherwise identical. Determine the current flowing through each diode.  
![](https://i.imgur.com/9FeSLZf.png)  

ans:  
that the current of $D_1$ is $i_1$ and $D_2$ is $i_2$.  

by KCL, $I_{in}=i_{1}+i_{2}$.  

by KVL, $\begin{cases}V_1-R_1I_{in}-V_{D_1}=0\\V_1-R_1I_{in}-V_{D_2}=0\end{cases}\Rightarrow V_{D_1}=V_{D_2}$.  
so $V_{D_1}=V_{D_2}=V_Tln(i_1/I_{S1})=V_Tln(i_2/I_{S2})\Rightarrow \frac{i_1}{I_{S1}}=\frac{i_2}{I_{S2}}$  

bring back to $I_{in}=i_{1}+i_{2}$, yield $i_1=\frac{I_{in}}{1+\frac{I_{S2}}{I_{S1}}}$ and $i_2=\frac{I_{in}}{1+\frac{I_{S1}}{I_{S2}}}$.  

__EXAMPLE:__  
Using the constant-voltage model, plot the input/output characteristics of the circuit
depicted below.  
Note that a diode about to turn on carries zero current but sustains $V_{D,on}$.  
![](https://i.imgur.com/IPTw5v9.png)

ans:  
when $V_{in}<V_{D,on}$, $\frac{R_2}{R_1+R_2}V_{in}=V_D=V_{out}$.  
![](https://i.imgur.com/U5X7oM7.png)  

and the voltage imposed on $D_1$ is $V_D$ which is equal to $\frac{R_2}{R_1+R_2}V_{in}$.  

when $V_D=V_{D,on}$, there's no current go through the diode but it sustain the voltage $V_{D,on}$. so $V_{out}=V_{D,on}$  
![](https://i.imgur.com/UJPc6li.png)  

when $V_D$ increase, the diode is forward-bias, so there's current go through it, namely $i_2$ decrease, which leads to $i_2R_2=V_D=V_{D,on}=V_{out}$  
![](https://i.imgur.com/WwRuSjx.png)  

so the I/O characteristic is  
![](https://i.imgur.com/ifmQiG2.png)

__EXAMPLE:__  
Plot the input/output characteristic for the circuit shown below. Assume a constant-voltage model for the diode.  
![](https://i.imgur.com/kef9iF1.png)  

ans:  
$V_D=-\frac{R_2}{R_1+R_2}V_{in}$  

when $V_D<V_{D,on}\iff-\frac{R_2}{R_1+R_2}V_{in}<V_{D,on}\iff V_{in}>-\frac{R_1+R_2}{R_2}V_{D,on}$, the diode is equivalent as open switch. $V_{out}=\frac{R_1}{R_1+R_2}V_{in}$.  

when $V_D=V_{D,on}\iff V_{in}=-\frac{R_1+R_2}{R_2}V_{D,on}$, there's no current go through the diode but it sustain a voltage $V_{D,on}$.  
so $V_{out}=-\frac{R_1}{R_2}V_{D,on}=\frac{R_1}{R_1+R_2}V_{in}$  

when $V_D>V_{D,on}\iff V_{in}<-\frac{R_1+R_2}{R_2}V_{D,on}$, there's current go through ($V_{R_2}=V_{D,on}$), let the point $X$ has a voltage $V_{in}+V_{D,on}$.  

the I/O characteristic:  
![](https://i.imgur.com/hk2KlsS.png)  

__EXAMPLE:__  
Plot the input/output characteristic of the circuit shown below using the constant-voltage diode model.  
![](https://i.imgur.com/NasIiTl.png)  
ans:  
first assume $V_{in}\geq0$, so that we can focus on the under one.  

when $V_{in}\geq0$, the voltage imposed on $D_2$ is $V_{D_2}=V_{in}-V_B$.  
the condition, $V_{D_2}\geq V_{D_2,on}$, is satisfied when $V_{in}\geq V_{D_2,on}+V_B$.  
so when $V_{D_2,on}+V_B>V_{in}\geq0$, $V_{out}=0$.  
when $V_{in}=V_{D_2,on}+V_B$, $V_{out}=0$.  
when $V_{in}>V_{D_2,on}+V_B$, $V_{out}=V_{in}-V_{D_2,on}-V_B$.  

second part, assume $V_{in}<0$.  

the voltage imposed on $D_1$ is $V_{D_1}=0-V_{in}$.  
the condition, $V_{D_1}\geq V_{D_1,on}$, is satisfied when $V_{in}\leq-V_{D_1,on}$.  
so when $-V_{D_1,on}< V_{in}<0$, $V_{out}=0$.  
when $V_{in}=-V_{D_1,on}$, $V_{out}=0$.  
when $V_{in}<-V_{D_1,on}$, $V_{out}=\frac{R_1}{R_1+R_1}(V_{in}+V_{D_1,on})=\frac12(V_{in}+V_{D_1,on})$. (since the voltage on the two resistances is $\frac{R_1}{R_1+R_1}(0-(V_{in}+V_{D_1,on}))$ each.)  

the I/O characteristic:  
![](https://i.imgur.com/fNBoEDp.png)

## large-signal and small-signal operation
Our treatment of diodes thus far has allowed arbitrarily large voltage and current changes, thereby requiring a “general” model such as the exponential I/V characteristic.  
We call this regime “large-signal operation” and the exponential characteristic the “large-signal model” to emphasize that the model can accommodate arbitrary signal levels.  
However, this model often complicates the analysis, making it difficult to develop an intuitive understanding of the circuit’s operation.  
The ideal and constant-voltage diode models resolve the issues to some extent, but the sharp nonlinearity at the turn-on point still proves problematic. The following example illustrates the general difficulty.  

__EXAMPLE:__  
three identical diodes in forward bias produce a total voltage of $V_{out} = 3V_D ≈ 2.4 V$ and resistor $R_1$ sustains the remaining $600 mV$.   
Neglect the current drawn by the cellphone.  
(a) Determine the reverse saturation current, $I_{S1}$ so that $V_{out} = 2.4 V$.  
(b) Compute $V_{out}$ if the adaptor voltage is in fact $3.1 V$.

![](https://i.imgur.com/73PRaOG.png)

ans:  
(a) $I_D=0.6/100=6mA=I_Se^{800mV/26mV}\Rightarrow I_S=2.6016\times10^{-16}A$  
(b) first suppose Vout remains constant and equal to $2.4 V$. Then, the additional $0.1 V$ must drop across $R_1$, raising $I_X$ to $7 mA$.  
then we can iterate by these two equation:  
1. $V_{out}=3V_D=3V_Tln(I_D/I_S)=3V_Tln(I_X/I_S)$
2. $I_X=(3.1-V_{out})/100$

finally we will get $V_{out}=2.411V$.

Noting the very small difference between them, we conclude that $V_{out} = 2.411 V$ with good accuracy.  
The constant-voltage diode model would not be useful in this case.  

The situation described above is an example of small “perturbations” in circuits.  
The change inVad from 3 V to 3.1 V results in a small change in the circuit’s voltages and currents, motivating us to seek a simpler analysis method that can replace the nonlinear equations and the inevitable iterative procedure.  

To develop our understanding of small-signal operation, let us consider diode $D_1$ below.  
![](https://i.imgur.com/4gjmYsq.png)  

it sustains a voltage $V_{D1}$ and carries a current $I_{D1}$.  
Now suppose a perturbation in the circuit changes the diode voltage by a small amount $\Delta V_D$. How do we predict the change in the diode current, $I_D$? We can begin with the nonlinear characteristic:
\\$$\begin{align}I_{D2}&=I_Se^{(V_{D1}+\Delta V)/V_T}\\&=I_Se^{V_{D1}/V_T}e^{\Delta V_{D}/V_T}\end{align}$$

if $\Delta V<<V_T$, then $e^{\Delta V/V_T}\approx1+\Delta V/V_T$ and
\\$$\begin{align}I_{D2}&=I_Se^{V_{D1}/V_T}+(\Delta V_{D}/V_T)I_Se^{V_{D1}/V_T}\\&=I_{D1}+\frac{\Delta V}{V_T}I_{D1}\end{align}$$

that's $\Delta I_D=\frac{\Delta V}{V_T}I_{D1}$.  
![](https://i.imgur.com/hHnUX9t.png)

in other words, 
\\$$\begin{align}\frac{\Delta I_D}{\Delta V_D}&=\frac{dI_D}{dV_{D}}|_{V_D=V_{D1}}\\&=\frac{I_S}{V_T}e^{V_{D1}/V_T}\\&=\frac{I_{D1}}{V_T}\end{align}$$

which yields the same result.  

__EXAMPLE:__  
A diode is biased at a current of $1 mA$.  
(a) Determine the current change if $V_D$ changes by $1 mV$.  
(b) Determine the voltage change if $I_D$ changes by $10\%$.  

ans:  
(a) $\Delta I_D=\frac{I_D}{V_T}\Delta V_D=\frac{1mA}{26mV}1mV=\frac{1}{26}mA$  
(b) $\frac{V_T}{I_D}\Delta I_D=\Delta V_D=\frac{26mV}{1mA}0.1\times1mA=2.6mV$  

the above example reveals an interesting aspect of small-signal operation: as far as (small) changes in the diode current and voltage are concerned, the device behaves as a linear resistor. In analogy with Ohm’s Law, we define the “small-signal resistance” of the diode as: 
\\$$r_d=\frac{V_T}{I_D}$$

conclusion:  
![](https://i.imgur.com/SiC5U8I.png)  

__EXAMPLE:__  
A sinusoidal signal having a peak amplitude of $V_p$ and a dc value of $V_0$ can be expressed as $V(t) = V_0 + V_p cos(ωt)$. If this signal is applied across a diode and $V_p<<V_T$, determine the resulting diode current.  
![](https://i.imgur.com/dFtZ5UL.png)

ans:  
since $V_p<<V_T$, the equation can be simplified to $\Delta I_D=\Delta V_D/r_d$.  
$\Delta V_D=V_pcos(\omega t)\iff I(t)=I_0+V_pcos(\omega t)/r_d$ where $I_0=I_Se^{V_0/V_T}$.  
$I(t)=I_0+\frac{V_pI_0}{V_T} cos(\omega t)=I_Se^{V_0/V_T}+\frac{V_pI_0}{V_T} cos(\omega t)$.  

The above example demonstrates the utility of small-signal analysis.  

If $V_p$ were large, we would still need to solve the following equation: 
\\$$I_D(t)=I_Se^{(V_0+V_pcos(\omega t))/V_T}$$


__EXAMPLE:__  
Beginning with $V_D = V_T ln(I_D/I_S)$, investigate that $I_D$ changes by a small amount and we wish to compute the change in $V_D$.  

ans:  
$V_D+\Delta V_D=V_Tln((I_D+\Delta I_D)/I_S)=V_Tln(\frac{I_D}{I_S}(1+\Delta I_D/I_D))=V_Tln(I_D/I_S)+V_Tln(1+\Delta I_D/I_D)\\\Rightarrow \Delta V_D=V_Tln(1+\Delta I_D/I_D)$  
since $ln(1+\epsilon)\approx\epsilon$ if $\epsilon<<1$ and $\Delta I_D<<I_{D1}$, $\Delta V_D=V_T(\Delta I_D/I_D)$  

__EXAMPLE:__  
three identical diodes in forward bias produce a total voltage of $V_{out} = 3V_D ≈ 2.4 V$ and resistor $R_1$ sustains the remaining $600 mV$.   
Neglect the current drawn by the cellphone.  
(a) Determine the reverse saturation current, $I_{S1}$ so that $V_{out} = 2.4 V$.  
(b) Compute $V_{out}$ if the adaptor voltage is in fact $3.1 V$.

![](https://i.imgur.com/73PRaOG.png)

ans:  
(a) $I_D=0.6/100=6mA=I_Se^{800mV/26mV}\Rightarrow I_S=2.6016\times10^{-16}A$  
(b) ![](https://i.imgur.com/RCiOstL.png)  

$\Delta V_{out}=\frac{3r_d}{R_1+3r_d}\Delta V_{ad}=0.1150V=11.5mV$ where $r_d=\frac{V_T}{I_X}=26mV/6mA=13/3\,\Omega$  
so the new $V_{out}=2.4115V$  

another sol.:  
$V_{ad}-(I_x+\Delta I_D)R_1-3(V_D+\Delta V_D)=0$.  
replace $\Delta I_D$ by $\Delta V_D/r_d$:  
$V_{ad}-(I_x+\Delta V_D/r_d)R_1-3(V_D+\Delta V_D)=0$  
solve this equation and come out $\Delta V_D=0.0038348$, $V_{out}=3(V_D+\Delta V_D)=2.4115V$

__EXAMPLE:__  
in the previous example, the current drawn by the cellphone is neglected. Now suppose, as shown below, the load pulls a current of $0.5 mA$ and determine $V_{out}$.  
![](https://i.imgur.com/p6eFAtR.png)

ans:  
$\Delta V_D=r_d\Delta I_D=(V_T/6mA)\times-0.5mA=-13/6mV$  
$3(V_D+\Delta V_D)=V_{out}=2.3935V$

In summary, the analysis of circuits containing diodes (and other nonlinear devices such as transistors) proceeds in three steps:  
1. determine—perhaps with the aid of the constant-voltage model—the initial values of voltages and currents (before an input change is applied)
2. develop the small-signal model for each diode (i.e., calculate $r_d$)
3.  replace each diode with its small-signal model and compute the effect of the input change.

