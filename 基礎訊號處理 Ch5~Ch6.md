---
tags: CSE
---
# 基礎訊號處理 Ch5~

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
> __EXAMPLE: pulse input to 3-point running average__
> consider a 3-point running average system expressed in the sliding window form of $$y[n]=\sum^n_{l=n-2}\frac13x[l]$$ with input $$x[n]=\begin{cases}1 &0\leq n\leq10\\0 &otherwise\end{cases}$$
> 
> ![](https://i.imgur.com/It835wm.jpg)

> __EXAMPLE: FIR filter coefficients__
> in general, the FIR filter is completely defined once the set of filter coefficients ${b_k}$ is known. for example, if the coefficients of a causal filter are $$\{b_k\}=\{3,-1,2,1\}$$ then we have a length-4 filter with $M=3$, and the difference equation expands into: $$y[n]=\sum^3_{k=0}b_kx[n-k]$$

the parameter $M$ is the __*order*__ of the FIR filter. the number of filter coefficients is also called the filter __*length*__.

### unit impluse sequence
the unit impulse is defined mathematically as Kronecker **_delta function_** $$\delta[n]=\begin{cases}1&n=0\\0&n\neq0\end{cases}$$ with that, the input signal can be written as $$x[n]=\sum_kx[k]\delta[n-k]$$

### unit impluse response sequence
the output from a filter is often called the _response_ to the input, so when the input is the unit impulse, $\delta[n]$, the output is called the __*unit impulse response*__.
that is: $$h[n]=\sum^M_{k=0}b_k\delta[n-k]$$by observing, the sum evaluates to $h[n]=b_n$ .
! later, we'll use $h[n]$ to represent unit impulse reponse to a system.

> __EXAMPLE:__ determine the impulse response of the FIR system $$y[n]=\sum^{10}_{k=0}(k-3)x[n-k]$$
> 
> let $x[n]=\delta[n]$, and the impulse response is $h[n]=(n-3),\,0\leq n\leq10$

### convolution
a general expression for the FIR filter's output can be dervied in terms of the impulse response. since the filter coefficients are identical to the impulse response values, we can replace $b_k$ by $h[k]$:
$$y[n]=\sum^M_{k=0}h[k]x[n-k]$$

the sum is called a finite *__convolution sum__*.
usually, we use $\star$ to represent infinite convolution sum: $y[n]=\sum^\infty_{k=-\infty}h[k]x[n-k]=h[n]\star x[n]$
though it's infinite, but it will reduces to finite when $h[n]=0,\,n<0\,or\, n>M$.
the length of $y[n]$ is $L_y=L_x+L_h-1$

#### commutative property of convolution
$y[n]=h[n]\star x[n]=\sum^M_{k=0}h[k]x[n-k]\iff y[n]=\sum^n_{k=n-M}h[n-k]x[k]$
if $x[n]$ is finite and nonzero only for $0\leq n\leq M_x$, then $y[n]$ will reduces to $y[n]=\sum^{M_x}_{k=0}x[k]h[n-k]=x[n]\star h[n]$.
so $x[n]\star h[n] = h[n]\star x[n]$.

### implementation of FIR filter

in order to compute the output of the FIR filter, we need three basic building-block systems: multiplier, adder, unit-delay operator.

1. multipier
$$y[n]=\beta x[n]$$

2. adder
$$y[n]=x_1[n]+x_2[n]$$

3. unit delay
$$y[n]=x[n-1]$$

here it's an example of $y[n]=b_0x[n]+b_1x[n-1]+b_2x[n-2]+b_3x[n-3]$:

![](https://i.imgur.com/GKB9GR9.jpg)

### time invariant system

a discrete-time system is said to be _time-invariant_ if, when an input is delayed (shifted) by $n_0$, the output is delayed by the same amount.
a system can be tested for time-invariance by checking whether or not $w[n]=y[n-n_0]$ below.

![](https://i.imgur.com/O2HfHP0.jpg)

for example, if system $\mathcal S$ is square-law system,
in upper branch:
$$\mathcal S(\mathcal D(x[n]))=\mathcal S(x[n-n_0])=(x[n-n_0])^2$$ lower one: $$\mathcal D(\mathcal S(x[n]))=\mathcal D(x[n]^2)=(x[n-n_0])^2$$

the output is same, so the square-law system is time-invariant.

another example, if system $\mathcal S$ is time-flip system($\mathcal F$), defined by $\mathcal F(x[n])=x[-n]$.
in upper branch: $$\mathcal F(\mathcal D(x[n]))=\mathcal F(x[n-n_0])=x[-n-n_0]$$ lower one: $$\mathcal D(\mathcal F(x[n]))=\mathcal D(x[-n])=x[-(n-n_0)]$$

the result isn't equal, so time-filp system isn't time-invariant.

### linear system
linear systems have the property that if $x_1[n]\rightarrow y_1[n]$ and $x_2[n]\rightarrow y_2$, then $$x[n]=\alpha x_1[n]+\beta x_2[n]\rightarrow y[n]=\alpha y_1[n]+\beta y_2[n]$$

whether a system is linear can be test by:
![](https://i.imgur.com/8uKZJJN.jpg)

take square-law system as an example, the output $w[n]$ for the system is $$w[n]=\alpha(x_1[n])^2+\beta(x_2[n])^2$$ while, in the lower part is $$y[n]=(\alpha x_1[n]+\beta x_2[n])^2$$

thus $y[n]\neq w[n]$, the square-law system is not linear.

take another example, time-flip system $y[n]=x[-n]$. $$w[n]=\alpha x_1[-n]+\beta x_2[-n]$$ the lower branch, $$y[n]=\alpha x_1[-n]+\beta x_2[-n]$$ they are equal, so the time-flip system is linear.

> __corollary: FIR is LTI system__
proof:
FIR formula: $$y[n]=\sum^M_{k=0} b_kx[n-k]$$
> 1. time-invarinat test
$x[n]\rightarrow y[n]\rightarrow y[n-n_0]$
$x[n]\rightarrow x[n-n_0]\rightarrow y[n-n_0]$
> 2. linear
let $x[n]=\alpha x_1[n]+\beta x_2[n]$
$y[n]=\sum^M_{k=0} b_kx[n]=\sum^M_{k=0} b_k(\alpha x_1[n]+\beta x_2[n])=\alpha\sum^M_{k=0}b_kx_1[n]+\beta\sum^M_{k=0}b_kx_2[n]$$=\alpha y_1[n]+\beta y_2[n]$
>
> Q.E.D

### derivation of the convolution
as we know, $x[n]$ can be written as $\sum_l x[l]\delta[n-l]$.
when input signal $x[n]$ goes into a LTI system, the time-invariant property implies:
$\delta[n-l]\rightarrow h[n-l],\,\forall l\in \Bbb Z$
and due to the linearity: 
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

> __EXAMPLE:__
> let $h_1[n],h_2[n]$ be the unit impluse response of LTI1 and LTI2.
>
> $$h_1[n]=\begin{cases}1&0\leq n\leq3\\0&otherwise\end{cases}$$ $$h_2[n]=\begin{cases}1&1\leq n\leq3\\0&otherwise\end{cases}$$
> 
> the system in (a):
> $v[n]=\sum^3_{k=0} x[n-k]$
> $y[n]=\sum^3_{k=1} v[n-k]$
> 
> the equivalent LTI in \(c\):
> $h[n]=h_1[n]\star h_2[n]=\sum^6_{k=0} b_k\delta[n-k]=\{b_k\}=\{0,1,2,3,3,2,1\}$
> $y[n]=\sum^6_{k=0}b_kx[n-k]$
