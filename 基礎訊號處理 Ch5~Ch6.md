---
tags: CSE
---
# 基礎訊號處理 Ch5~Ch6

----
> [TOC]
----

## Chapter 5: FIR Filters
### the running-average filter
> (a) finite-length input signal, $x[n]$
> (b) output of running-average filter, $y[n]$
![](https://i.imgur.com/iqMVxk0.jpg)

the pictrue above shows the output of $y[n]=\frac13(x[n]+x[n+1]+x[n+2])=\frac13\sum^{n+2}_{l=n}x[l]$.
the euqation given above is called a __*difference equation*__.

a filter uses only the present and past values of the input is called __*causal*__ fiter. therefore a filter uses future values of the input is called __*noncausal*__. it can't be implemented in a a real-time application.

### the general FIR filter
the general causal difference equation is $$y[n]=\sum^M_{k=0}b_kx[n-k]$$ we say that $b_k$ defines a __*weighted running average*__ of $M+1$ samples.
> __EXAMPLE: pulse input to 3-point running average__\
> consider a 3-point running average system expressed in the sliding window form of $$y[n]=\sum^n_{l=n-2}\frac13x[l]$$ with input $$x[n]=\begin{cases}1 &0\leq n\leq10\\0 &otherwise\end{cases}$$
> 
> ![](https://i.imgur.com/It835wm.jpg)

> __EXAMPLE: FIR filter coefficients__\
> in general, the FIR filter is completely defined once the set of filter coefficients ${b_k}$ is known. for example, if the coefficients of a causal filter are $$\{b_k\}=\{3,-1,2,1\}$$ then we have a length-4 filter with $M=3$, and the difference equation expands into: $$y[n]=\sum^3_{k=0}b_kx[n-k]$$

the parameter $M$ is the __*order*__ of the FIR filter. the number of filter coefficients is also called the filter __*length*__.

### unit impluse sequence
the unit impulse is defined mathematically as Kronecker **_delta function_** $$\delta[n]=\begin{cases}1&n=0\\0&n\neq0\end{cases}$$ with that, the input signal can be written as $$x[n]=\sum_kx[k]\delta[n-k]$$

### unit impluse response sequence
the output from a filter is often called the _response_ to the input, so when the input is the unit impulse, $\delta[n]$, the output is called the __*unit impulse response*__.\
that is: $$h[n]=\sum^M_{k=0}b_k\delta[n-k]$$by observing, the sum evaluates to $h[n]=b_n$ .\
! later, we'll use $h[n]$ to represent unit impulse reponse to a system.

> __EXAMPLE:__ determine the impulse response of the FIR system $$y[n]=\sum^{10}_{k=0}(k-3)x[n-k]$$
> 
> let $x[n]=\delta[n]$, and the impulse response is $h[n]=(n-3),\,0\leq n\leq10$

### convolution
a general expression for the FIR filter's output can be dervied in terms of the impulse response. since the filter coefficients are identical to the impulse response values, we can replace $b_k$ by $h[k]$:\
$$y[n]=\sum^M_{k=0}h[k]x[n-k]$$

the sum is called a finite *__convolution sum__*.\
usually, we use $\star$ to represent infinite convolution sum: $y[n]=\sum^\infty_{k=-\infty}h[k]x[n-k]=h[n]\star x[n]$\
though it's infinite, but it will reduces to finite when $h[n]=0,\,n<0\,or\, n>M$.\
the length of $y[n]$ is $L_y=L_x+L_h-1$

#### commutative property of convolution
$y[n]=h[n]\star x[n]=\sum^M_{k=0}h[k]x[n-k]\iff y[n]=\sum^n_{k=n-M}h[n-k]x[k]$\
if $x[n]$ is finite and nonzero only for $0\leq n\leq M_x$, then $y[n]$ will reduces to $y[n]=\sum^{M_x}_{k=0}x[k]h[n-k]=x[n]\star h[n]$.\
so $x[n]\star h[n] = h[n]\star x[n]$.

### implementation of FIR filter

in order to compute the output of the FIR filter, we need three basic building-block systems: multiplier, adder, unit-delay operator.

1. multipier\
$$y[n]=\beta x[n]$$

2. adder\
$$y[n]=x_1[n]+x_2[n]$$

3. unit delay\
$$y[n]=x[n-1]$$

here it's an example of $y[n]=b_0x[n]+b_1x[n-1]+b_2x[n-2]+b_3x[n-3]$:

![](https://i.imgur.com/GKB9GR9.jpg)

### time invariant system

a discrete-time system is said to be _time-invariant_ if, when an input is delayed (shifted) by $n_0$, the output is delayed by the same amount.\
a system can be tested for time-invariance by checking whether or not $w[n]=y[n-n_0]$ below.

![](https://i.imgur.com/O2HfHP0.jpg)

for example, if system $\mathcal S$ is square-law system,\
in upper branch:
$$\mathcal S(\mathcal D(x[n]))=\mathcal S(x[n-n_0])=(x[n-n_0])^2$$ lower one: $$\mathcal D(\mathcal S(x[n]))=\mathcal D(x[n]^2)=(x[n-n_0])^2$$

the output is same, so the square-law system is time-invariant.

another example, if system $\mathcal S$ is time-flip system($\mathcal F$), defined by $\mathcal F(x[n])=x[-n]$.\
in upper branch: $$\mathcal F(\mathcal D(x[n]))=\mathcal F(x[n-n_0])=x[-n-n_0]$$ lower one: $$\mathcal D(\mathcal F(x[n]))=\mathcal D(x[-n])=x[-(n-n_0)]$$

the result isn't equal, so time-filp system isn't time-invariant.

### linear system
linear systems have the property that if $x_1[n]\rightarrow y_1[n]$ and $x_2[n]\rightarrow y_2$, then $$x[n]=\alpha x_1[n]+\beta x_2[n]\rightarrow y[n]=\alpha y_1[n]+\beta y_2[n]$$

whether a system is linear can be test by:\
![](https://i.imgur.com/8uKZJJN.jpg)

take square-law system as an example, the output $w[n]$ for the system is $$w[n]=\alpha(x_1[n])^2+\beta(x_2[n])^2$$ while, in the lower part is $$y[n]=(\alpha x_1[n]+\beta x_2[n])^2$$

thus $y[n]\neq w[n]$, the square-law system is not linear.

take another example, time-flip system $y[n]=x[-n]$. $$w[n]=\alpha x_1[-n]+\beta x_2[-n]$$ the lower branch, $$y[n]=\alpha x_1[-n]+\beta x_2[-n]$$ they are equal, so the time-flip system is linear.

> __corollary: FIR is LTI system__\
_proof_:
FIR formula: $$y[n]=\sum^M_{k=0} b_kx[n-k]$$
> 1. time-invarinat test\
$x[n]\rightarrow y[n]\rightarrow y[n-n_0]$\
$x[n]\rightarrow x[n-n_0]\rightarrow y[n-n_0]$
> 2. linear\
let $x[n]=\alpha x_1[n]+\beta x_2[n]$\
$y[n]=\sum^M_{k=0} b_kx[n]=\sum^M_{k=0} b_k(\alpha x_1[n]+\beta x_2[n])=\alpha\sum^M_{k=0}b_kx_1[n]+\beta\sum^M_{k=0}b_kx_2[n]$$=\alpha y_1[n]+\beta y_2[n]$
>
> Q.E.D

### derivation of the convolution
as we know, $x[n]$ can be written as $\sum_l x[l]\delta[n-l]$.\
when input signal $x[n]$ goes into a LTI system, the time-invariant property implies:\
$\delta[n-l]\rightarrow h[n-l],\,\forall l\in \Bbb Z$\
and due to the linearity:\
$x[l]\delta[n-l]\rightarrow x[l]h[n-l],\,\forall l\in \Bbb Z$

then we have $$x[n]=\sum_lx[l]\delta[n-l]\rightarrow y[n]=\sum_lx[l]h[n-l]$$

$$y[n]=x[n]\star h[n]$$

the expression shows that __*all LTI systems can be represented by a convolution sum*__.

### some properties of convolution
1. commutative
$$y[n]=h[n]\star x[n]=x[n]\star h[n]$$
2. identity element
$$x[n]\star\delta[n]=x[n]$$ more generalized, $$x[n]\star \delta[n-n_0]=x[n-n_0]$$
3. associative
$$(x_1[n]\star x_2[n])\star x_3[n]=x_1[n]\star (x_2[n]\star x_3[n])$$
4. distributive
$$(x_1[n]+x_2[n])\star h[n]=x_1[n]\star h[n]+x_2[n]\star h[n]$$

### cascaded LTI system
LTI systems have the remarkable property that _two LTI system in cascade can be implemented in either order_. this property is a direct consequence of the commutative property of convolution.

![](https://i.imgur.com/LvqU8PB.jpg)

> __EXAMPLE:__\
> let $h_1[n],h_2[n]$ be the unit impluse response of LTI1 and LTI2.
>
> $$h_1[n]=\begin{cases}1&0\leq n\leq3\\0&otherwise\end{cases}$$ $$h_2[n]=\begin{cases}1&1\leq n\leq3\\0&otherwise\end{cases}$$
> 
> the system in (a):\
> $v[n]=\sum^3_{k=0} x[n-k]$\
> $y[n]=\sum^3_{k=1} v[n-k]$
> 
> the equivalent LTI in \(c\):\
> $h[n]=h_1[n]\star h_2[n]=\sum^6_{k=0} b_k\delta[n-k]=\{b_k\}=\{0,1,2,3,3,2,1\}$\
> $y[n]=\sum^6_{k=0}b_kx[n-k]$

----
### chap 5 exercise

P-5.1\
if the impulse response $h[n]$ of an FIR filter is $$h[n]=7\delta[n]+\delta[n-2]-6\delta[n-5]$$ write the difference equation for the FIR filter.

ans:
$y[n]=7x[n]+x[n-2]-6x[n-5]$

P-5.3\
the length-$L$ running sum is defined as $$y[n]=\frac1L\sum_{k=0}^{L-1}x[n-k]$$ when the input $x[n]$ is the _unit-step_ signal, determine the output signal as follows:\
(a) compute the numerical values of $y[n]$ over the time interval $-5\leq n\leq10$, assuming that $L=5$\
\(c\) finally, derive a general formula for $y[n]$ that will apply for any length $L$ and for the range $n\geq0$

ans: \
(a) $y[n]=\begin{cases}0&,-5\leq n\leq-1\\(n+1)/5&,0\leq n\leq4\\1&,5\leq n\leq10\end{cases}$\
\(c\) $y[n]\begin{cases}(n+1)/L&,0\leq n\leq L-1\\1&,L\leq n\end{cases}$

P-5.4\
an LTI system is described by the difference equation $$y[n]=2x[n]-3x[n-1]+2x[n-2]$$ (a) when the input to this system os $$x[n]=\begin{cases}0&,n<0\\n+1&,n=0,1,2\\5-n&,n=3,4\\1&,n\geq5\end{cases}$$ compute the values of $y[n]$, over the index range $0\leq n\leq10$.\
(c\) determine the response of this system to a unit impulse input $h[n]$.

ans:\
(a)
| n | 0 | 1 | 2 | 3 | 4 | 5 | 6~10 |
| - | - | - | - | - | - | - | - |
| __y[n]__ | 2 | 1 | 2 | -1 | 2 | 3 | 1|

(b) $h[n]=2\delta[n]-3\delta[n-1]+2\delta[n-2]$

P-5.5\
an LTI system is describe by the difference equation $$y[n]=4x[n]-5x[n-1]+1.2x[n-2]$$ (a) draw the implementation of this system as a block diagram in direct form.\
(b) give the implementation as a block diagram in transposed direct form.

ans:

P-5.6\
consider a system defined by $$y[n]=\sum^M_{k=0}b_kx[n-k]$$ (a) suppose that the input $x[n]$ is nonzero only for $0\leq n\leq N-1$. show that $y[n]$ is nonzero at most over a finite interval of the form $0\leq n\leq P-1$, which is the support of $y[n]$. determine $P$ in terms of the filter order $M$ and the input signal length $N$.\
(b) suppose that the input $x[n]$ is nonzero only for $N_1\leq n\leq N_2$. what is the length of $x[n]$? show that $y[n]$ is nonzero at most over a finite interval of the form $N_3\leq n\leq N_4$, which is the support of $y[n]$. determine $N_3$ and $N_4$ and also the length of $y[n]$ in terms of $N_1,N_2$, and $M$.

ans:\
(a) $P=N+M$\
(b) $N_3=N_1,N_4=N_2+M$

P-5.7\
the unit-step $u[n]$ can be used to represent sequences that are zero for $n<0$.\
(b) the $L$-point running sum is defined as $$y[n]=\sum^{L-1}_{k=0}x[n-k]$$ for the input sequence $x[n]=(-0.5)^nu[n]$, compute the values of $y[n]$ over the index range $-2\leq n\leq8$, assuming that $L=2$.\
(c\) for the input sequence $x[n]=a^nu[n]$, derive a general formula for the output of the running sum $y[n]$ that applies for any value $a$, for any length $L$, and for the index range $n\geq-2$. in doing so, you may have use this formula: $$\sum^N_{k=M}\alpha^k=\frac{\alpha^M-\alpha^{N+1}}{1-\alpha}$$

ans:\
(a) $y[n]=\begin{cases}0&,-2\leq n\leq-1\\1&,n=0\\\frac23[(-\frac12)^{n-1}-(-\frac12)^{n+1}]&,1\leq n\leq8\end{cases}$

(b) $y[n]=\begin{cases}0&,-2\leq n\leq -1\\\frac{1-\alpha^{n+1}}{1-\alpha}&,0\leq n\leq L-1\\\frac{\alpha^{n-L+1}-\alpha^{n+1}}{1-\alpha}&,L\leq n\end{cases}$

P-5.9\
if the filter coefficients of an FIR system are $\{b_k\}=\{-10,10,10\}$ and the input signal is $$x[n]=\begin{cases}0&,n\pmod 2=0\\1&,n\pmod 2=1\end{cases}$$ determine the output signal $y[n]$ for all $n$.

ans: $y[n]=\begin{cases}0&,n=0\\-10&,n=1\\10(n-1\pmod 2)&,2\leq n\end{cases}$

P-5.11\
suppose that $\mathcal S$ is an LTI system whose exact form is unknown. it's tested by running some inputs into the system, and then observing the output signals. suppose that the following input-output pairs are the result of the tests:

__input1__: $\delta[n]-\delta[n-1]$\
__output1__: $\delta[n]-\delta[n-1]+2\delta[n-3]$

__input2__: $cos(\pi n/2)$\
__output2__: $2cos(\pi n/2-\pi/4)$

(b) use linearity and time invariance to find the output of the system when the input is $$x[n]=7\delta[n]-7\delta[n-2]$$

ans:\
(b) $7\delta[n]-7\delta[n-2]=7\delta[n]-7\delta[n-1] + 7\delta[n-1]-7\delta[n-2]$, so the output is $7\delta[n]-7\delta[n-1]+14\delta[n-3]+7\delta[n-1]-7\delta[n-2]+14\delta[n-4]=7\delta[n]-7\delta[n-2]+14\delta[n-3]+14\delta[n-4]$

P-5.12\
an LTI system has impulse response $$h[n]=2\delta[n]-3\delta[n-1]+\delta[n-2]+4\delta[n-5]$$ (a) implement the above system in direct form and transposed direct form.\
(b) if the difference equation was given instead, how would the implementations differ?

ans:\
(a)\
(b) no difference.

P-5.13\
for a particular LTI system, when the input is the unit step, $x_1[n]=u[n]$, the corresponding output is $y_1[n]=\delta[n]+2\delta[n-1]-\delta[n-2]$

(c\) determine the impulse response $h[n]$ of the system.

ans:\
(c\) $\delta[n]=u[n]-u[n-1]$, so the impulse response is $h[n]=\delta[n]+\delta[n-1]-3\delta[n-2]+\delta[n-3]$

