---
tags: CSE
---
# 基礎訊號處理 Ch9~Ch10

----
> [TOC]
----

## Chapter 9: z-transforms
### definition of the z-transform
a finite-length signal $x[n]$ consists of a set of singal values $\{x[0], x[1],..., x[L-1]\}$ that can be represented by the relation $$x[n]=\sum^{L-1}_{k=0}x[k]\delta[n-k]$$ the <font color="#f59b42">**_z-transform_**</font> of the signal $x[n]$ is defined by the formula $$X(z)=\sum^{L-1}_{k=0}x[k]z^{-k}$$ where $z$, the independent variable of the $z$-transform $X(z)$, is a complex number.  
  
when we use $X(z)$ to determine the $z$-transform of the signal $x[n]$, we say that we <font color="f59b42">**_transform_**</font> $x[n]$ into a new representation $X(z)$.  
when we trans back from $X(z)$ to $x[n]$, we call we <font color="#f59b42">**_taking the inverse $z$-transform_**</font>.  
  
$$\begin{align}\color{#f59b42}{\text{n-domain}}&\iff\color{#f59b42}{\text{z-domain}}\\x[n]=\sum^{L-1}_{k=0}x[k]\delta[n-k]&\iff X(z)=\sum^{L-1}_{k=0}x[k]z^{-k}\end{align}$$  

note that <font color="#f59b42">__*n-domain*__</font> can be called <font color="#f59b42">__*time domain*__</font>.

as a simple example: $x[n]=\delta[n-n_0]\iff X(z)=z^{-n_0}$  

__EXAMPLE__:  
consider the sequence $x[n]$ given in the following table



| $n$ | $n<-1$ | $-1$ | $0$ | $1$ | $2$ | $3$ | $4$ | $5$ | $n>5$ |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| $x[n]$ | $0$ | $0$ | $2$ | $4$ | $6$ | $4$ | $2$ | $0$ | $0$ |

the $z$-transform is $X(z)=2+4z^{-1}+6z^{-2}+4z^{-3}+2z^{-4}$  
the nonzero values of the sequence $\{2,4,6,4,2\}$ become the coefficients of the polynomial $X(z)$  
  
__EXAMPLE__:  
consider the $z$-transform $X(z)$ given by the equation $$X(z)=1-2z^{-1}+3z^{-3}-z^{-5}$$ the corresponding sequence $x[n]$ is $x[n]=\delta[n]-2\delta[n-1]+3\delta[n-3]-\delta[n-5]$  

### basic z-transform properties
1. linearity
$$\color{#f59b42}{\text{linearity of the $z$-transform}}\\ax_1[n]+bx_2[n]\iff aX_1(z)+bX_2(z)$$

2. time delay
$$\color{#f59b42}{\text{delay of }n_0\text{ samples}}\\x[n-n_0]\iff z^{-n_0}X(z)$$

### a general z-transform formula
$X(z)=\sum^\infty_{n=-\infty} x[n]z^{-n}$  
  
an example:  
$x[n]=(\frac12)^nu[n]\iff X(z)=\sum^\infty_{n=0}(\frac12)^nz^{-n}=\frac{1}{1-\frac12z^{-1}},\text{ if }|\frac12z^{-1}|<1$  
the $z$-transform $X(z)$ is defined for only part of the complex $z$-plane, called the <font color="#f59b42">**_region of convergence_**</font>  
  
### the z-transform and LIS
1. unit-delay  
$y[n]=x[n-1]\iff Y(z)=z^{-1}X(z)$  
2. FIR filter  
$y[n]=\sum^{M}_{k=0}b_kx[n-k]\iff Y(z)=\sum^{M}_{k=0}b_k(z^{-k}X(z))=(\sum^{M}_{k=0}b_kz^{-k})X(z)=H(z)X(z)$  
the $z$-transform function $H(z)$ is called the <font color="#f59b42">**_system function_**</font> of the FIR filter.
3. impulse response  
$y[n]=\sum^M_{k=0}b_kx[n-k]=(\sum^M_{k=0}h[k]z^{-k})X(z)$
$$\color{#f59b42}{\text{system function of FIR system}}\\h[n]=\sum^M_{k=0}b_k\delta[n-k]\iff H(z)=\sum^{M}_{k=0}h[k]z^{-k}$$ __EXAMPLE__:  
determine the system function $H(z)$ of an FIR filter whose impulse response is $h[n]=\delta[n]-7\delta[n-2]-3\delta[n-3]$  
$H(z)=1-7z^{-2}-3z^{-3}$  

### roots of a z-transform polynomial
$H(z)=G\prod^M_{k=1}(1-z_kz^{-1})=G\prod^M_{k=1}z^{-1}(z-z_k)$  
_! $G$ is a constant_  

__EXAMPLE__:  
consider a FIR filter defined by the difference equation $$y[n]=6x[n]-5x[n-1]+x[n-2]$$ the $z$-transform system function is $H(z)=6-5z^{-1}+z^{-2}$ which factors as $H(z)=(z^{-1}-2)(z^{-1}-3)=2\cdot3(1/2-z)(1/3-z)z^{-2}$  
thus, the roots of $H(z)$ are $\frac13$ and $\frac12$.  

__EXAMPLE__:  
suppose the system function $H(z)$ has two roots at $\{-1,\frac12\}$.  
$H(z)=Gz^{-2}(z+1)(z-\frac12)=G(1+\frac12z^{-1}-\frac12z^{-2})$  
in order to find $H(z)$ we need to find $G$, which requires that we know an addition fact. in this example, we assume that the value of $H(3)=4$.  
$G(1+\frac16-\frac1{18})=\frac{10}{9}G=4,\,G=\frac{18}{5}=3.6$  
$H(z)=3.6+1.8z^{-1}-1.8z^{-2}$  
finally, $y[n]=3.6+1.8x[n-1]-1.8x[n-2]$  
### convolution and the z-transform
$y[n]=x[n]\star h[n]=\sum^{M}_{k=0}h[k]x[n-k]\iff Y(z)=\sum^M_{k=0}(h[k](z^{-k}X(z)))=(\sum^M_{k=0}h[k]z^{-k})X(z)=H(z)X(z)$.  
$$\color{#f59b42}{\text{convolution maps to multiplication of $z$-transforms}}\\y[n]=h[n]\star x[n]\iff Y(z)=H(z)X(z)$$

__EXAMPLE__:  
![](https://i.imgur.com/LnRNnel.jpg)

### cascading systems
$$\color{#f59b42}{\text{cascade of LTI systems}}\\h[n]=h_1[n]\star h_2[n]\iff H(z)=H_1(z)H_2(z)$$  __EXAMPLE__:  
consider a system described by the difference equations $w[n]=3x[n]-x[n-1]\\y[n]=2w[n]-w[n-1]$ which define a cascade of two first-order systems.
$H_1(z)=3-z^{-1}\text{ and }H_2(z)=2-z^{-1}\Rightarrow H(z)=H_1(z)H_2(z)=6-5z^{-1}+z^{-2}$  
so $y[n]=6x[n]-5x[n-1]+x[n-2]$.  

### factoring z-polynomials
__EXAMPLE__:  
consider the following third-order system $H(z)=1-2z^{-1}+2z^{-2}-z^{-3}$.   
by observing, we can know $z^{-1}=1$ is a root of $H(z)$.
$H(z)=(1-z^{-1})(z^{-2}-z^{-1}+1)=(1-z^{-1})(1-e^{j\pi/3}z^{-1})(1-e^{-j\pi/3}z^{-1})$  
the resulting difference equations for the cascade are obtained by inverse transforming the factors of $H(z)$  
$w[n]=x[n]-x[n-1]\\y[n]=w[n]-w[n-1]+w[n-2]$

### deconvolution
suppose that we have the cascade of two filters $H_1(z)$ and $H_2(z)$, and $H_1(z)$ is known. is it possible to find $H_2(z)$ so that the overall system has its output equal to the input? if so, the $z$-transform analysis tells us that its system function would have to be $H(z)=1$, so that $H_1(z)H_2(z)=1$.  
  
the process is called <font color="#f59b42">__*deconvolution*__</font>. another term for this is <font color="#f59b42">__*inverse filter*__</font>.

### relationship between the z-domain and the omega-domain
let $z=e^{j\hat\omega}$,
$$\begin{align}\color{#f59b42}{\text{z-domain}}&\iff\color{#f59b42}{\hat\omega\text{-domain}}\\H(z)=\sum^M_{k=0}b_kz^{-k}&\iff H(e^{j\hat\omega})=\sum^M_{k=0}b_ke^{-j\hat\omega k}\\X(z)=\sum^{L-1}_{n=0}x[n]z^{-n}&\iff X(e^{j\hat\omega})=\sum^{L-1}_{n=0}x[n]e^{-j\hat\omega n}\end{align}$$, so the conclusion is
$X(e^{j\hat\omega})=X(z)\mid_{z=e^{j\hat\omega}}$  

__EXAMPLE__:  
$H(e^{j\hat\omega})=(1+cos2\hat\omega)e^{-j3\hat\omega}=e^{-j3\hat\omega}+\frac12e^{-j\hat\omega}+\frac12e^{-j5\hat\omega}$, $H(z)=\frac12z^{-1}+z^{-3}+\frac12z^{-5}$, so the impulse response of the system is $h[n]=\frac12\delta[n-1]+\delta[n-3]+\frac12\delta[n-5]$  
  
### the z-plane and the unit circle  
the values of $z=e^{j\hat\omega}$ define a circle of radius one in the $z$-plane, which is called the <font color="#f59b42">**_unit circle_**</font>.

> the $z$-transform $X(z)=1+\frac12z^{-1}-\frac12z^{-2}=(1+z^{-1})(1-\frac12z^{-1})$ evaluate over the region $\{|z|\leq1.5\}$ of the $z$-plane that includes the unit circle.   
> the evaulation of $H(z)$ on the unit circle is the orange line.
> ![](https://i.imgur.com/wfQ0LGM.jpg)

### the zeros and poles of H(z)
__EXAMPLE__:  
$H(z)=1-2z^{-1}+2z^{-2}-z^{-3}=(1-z^{-1})(1-e^{j\pi/3}z^{-1})(1-e^{-j\pi/3}z^{-1})=z^{-3}(z-1)(z-e^{j\pi/3})(z-e^{-j\pi/3})$  
the roots of $H(z)$ are locations where $H(z)=0$, so these locations, which come from the factors in the numberator of the equation above, are called the <font color="#f59b42">_**zeros of the system function**_</font>.  
obviously, the zeros of the system function above are located at $z_1=1,\,z_2=e^{j\pi/3},\,z_3=e^{-j\pi/3}$ in the $z$-plane.  
equation above also show that $H(z)\to\infty$ when $z\to0$. values of $z$ for which $H(z)$ is infinite (and might be undefined) are called <font color="#f59b42">**_poles_**</font>.  
in this case, we say that the denominator term $z^3$ represents three poles at $z=0$.

### pole-zero plot
the plot below shows the three zeros and the three poles for the last example.  
such a plot is called a <font color="#f59b42">**_pole-zero plot_**</font>.  
each zero location is denoted by a small circle, and the three repeated poles at $z=0$ are indicated by a single x.
![](https://i.imgur.com/ISaCq0V.jpg)

### nulling
when $x[n]=z_0^n$, and $H(z_0)=0$. this zero-output case is called <font color="#f59b42">**_nulling_**</font>.  

$\begin{align}y[n]&=\sum^M_{k=0}b_kx[n-k]\\&=\sum^M_{k=0}b_kz_0^{n-k}\\&=\sum^M_{k=0}b_kz^n_0z_0^{-k}=(\sum^M_{k=0}b_kz^{-k}_0)z^n_0=H(z_0)z^{n}_0\end{align}$

### nulling filters
with the knowledge above, we can now eliminate $x[n]=z_0^n$ by a filter with a proper system function.  
more over, we can eliminate $x[n]=cos(\hat\omega_0 n)$. since $cos(\hat\omega_0 n)=\frac12(e^{j\hat\omega n}+e^{-j\hat\omega n})$.  
__EXAMPLE__:  
let $H_1(z)=1-z_1z^{-1},\,H_2(z)=1-z_2z^{-1}$, we need $H_1(e^{j\hat\omega})=0$ and $H_2(e^{-j\hat\omega})=0$.  
so $H_1(z)=1-e^{j\hat\omega}z^{-1},\,H_2(z)=1-e^{-j\hat\omega}z^{-1}$.  
$H(z)=H_1(z)H_2(z)=1-(e^{j\hat\omega}+e^{-j\hat\omega})z^{-1}+z^{-2}=1-2cos(\hat\omega)z^{-1}+z^{-2}$  
now, if we want to remove the sigal $cos(0.25\pi n)$ from a FIR filter input.  
$H(z)=1-2cos(\pi/4)z^{-1}+z^{-2}$  
$y[n]=x[n]-\sqrt2x[n-1]+x[n-2]$  

### ch 9 exercise

P-9.1  
find the $z$-transforms of the following signals:  
$x_1[n]=6\delta[n]$  
$x_2[n]=7\delta[n-3]$  
$x_3[n]=2\delta[n]+9\delta[n-5]$  
$x_4[n]=6\delta[n]-7\delta[n-3]-2\delta[n]-9\delta[n-5]$  

ans:  
$X_1(z)=6$  
$X_2(z)=7z^{-3}$  
$X_3(z)=2+9z^{-5}$  
$X_4(z)=4-7z^{-3}-9z^{-5}$  

P-9.3  
suppose that an LTI system has a system function $$H(z)=1-3z^{-2}+3z^{-4}+4z^{-6}+7z^{-7}$$ (a) determine the difference equation that relates the output $y[n]$ of the system to the input $x[n]$  
(b) determine and plot the output sequence $y[n]$ when the input is $x[n]=\delta[n]$.  

ans:  
(a) $y[n]=x[n]-3x[n-2]+3x[n-4]+4x[n-6]+7x[n-7]$
(b) $y[n]=\delta[n]-3\delta[n-2]+3\delta[n-4]+4\delta[n-6]+7\delta[n-7]$
(skip the plotting)  

P-9.5  
consider an LTI system whose system function is the product of five terms $H(z)=(1-z^{-1})(1-0.7e^{-j\pi/2}z^{-1})(1-0.7e^{j\pi/2}z^{-1})(1+e^{j2\pi/3}z^{-1})(1+e^{-j2\pi/3}z^{-1})$  
(a) write the difference equation that gives the relation between the input $x[n]$ and the output $y[n]$  
(b) plot the poles and zeros of $H(z)$ in the complex $z$-plane.  
(c\) if the input is of the form $x[n]=Ae^{j\phi}e^{jn\hat\omega}$, for what values of $-\pi<\hat\omega\leq\pi$ is it true that $y[n]=0$?  

ans:  
(a) $H(z)=(1-z^{-1})(1+0.49z^{-2})(1+2cos(2\pi/3)z^{-1}+z^{-2})=1-2z^{-1}+2.49z^{-2}-1.98z^{-3}+0.98z^{-4}-0.49z^{-5}$  
$y[n]=x[n]-2x[n-1]+2.49x[n-2]-1.98x[n-3]+0.98x[n-4]-0.49x[n-5]$  
(b) $5$ of zeros at $z=1,\,0.7e^{\pm j\pi/2},\,-e^{\pm j2\pi/3}=e^{j(\pm2\pi/3\mp\pi)}$, and $5$ of poles at $z=0$.  
![](https://i.imgur.com/dk1TpTL.png)
(c\) we need $H(z_0)=0$, where $x=Xz_0^n$, on the unit circle, $z_0$ must be $1,\,e^{\pm j\pi/3}$, so $\hat\omega=1,\,\pm \pi/3$  

P-9.6  
the diagram below depicts a cascade connection of two LTI systems, where the output of the first system is the input to the second system and the overall output is the output of the second system.  
![](https://i.imgur.com/QAips7W.jpg)

(a) suppose that LTI 1 is a 3-point averager described by the difference equation $y_1[n]=\frac13(x[n]+x[n-1]+x[n-2])$ and LTI 2 is a 4-point averager described by the system function $H_2(z)=\frac14(1+z^{-1}+z^{-2}+z^{-3})$.  
determine the system function of the overall cascade system in Fig. (a).

(b) for the cascade system in Fig. (b), assume that LTI 1 is a 4-point averager and LTI 2 is a 3-point averager.  
determine the $z$-transforms, $Y_2(z)$ and $W(z)$ when $x[n]=\delta[n]-\delta[n-1]$.  

(c\) obtain a single difference equation that relates $y[n]$ to $x[n]$ in Fig. (a). is the cascade of the two averagers the same as a 7-point averager?

(d) plot the poles and zeros of $H(z)=H_1(z)H_2(z)$ in the complex $z$-plane.  

![](https://i.imgur.com/kB7yeIe.jpg)

ans:  
(a) $H(z)=H_1(z)H_2(z)=\frac1{12}(1+2z^{-1}+3z^{-2}+3z^{-3}+2z^{-4}+z^{-5})$  
(b)  
$H_1(z)=\frac14(1+z^{-1}+z^{-2}+z^{-3})\\H_2(z)=\frac13(1+z^{-1}+z^{-2})$  
$Y_2(z)=X(z)H_2(z)=\frac13(1-z^{-3})\\W(z)=Y_2(z)H_1(z)=\frac1{12}(1+z^{-1}+z^{-2}-z^{-4}-z^{-5}-z^{-6})$  
(c\) $y[n]=\frac1{12}(x[n]+2x[n-1]+3x[n-2]+3x[n-3]+2x[n-4]+x[n-5])$.  
no, it's not the same as a 7-point averager.

(d)  
$\begin{align}H(z)&=\frac1{12}(1+2z^{-1}+3z^{-2}+3z^{-3}+2z^{-4}+z^{-5})=\frac1{12}z^{-5}(z+1)(z^2+1)(z^2+z+1)\\&=\frac1{12}z^{-5}(z+1)(z-e^{j\pi/2})(z-e^{-j\pi/2})(z-e^{j2\pi/3})(z-e^{-j2\pi/3})\end{align}$
![](https://i.imgur.com/HIB020q.jpg)

P-9.12  
a system is defined by the system function $$H(z)=(1-z^{-1})(1+z^{-1}+z^{-2})(1+z^{-3})$$ (a) write the time-domain description of this system in the form of a difference equation.  

(b) write a formula for the frequency response of the system.  

(c\) derive simple formulas for the magnitude response versus $\hat\omega$ and the phase response versus $\hat\omega$. these formulas must contain no complex terms and no square roots.  

(d) this system can "null" certain input signals. for which input frequencies $\hat\omega_0$ is the response to $x[n]=cos(\hat\omega_0 n)$ equal to zero?  

(e) when the input to the system is $x[n]=cos(\pi n/4)$, determine the output signal $y[n]$ in the form: $$Acos(\hat\omega_0 n+\phi)$$ give numerical values for the constants $A,\hat\omega_0,\phi$.

ans:  
(a) $H(z)=1-z^{-6}\iff y[n]=x[n]-x[n-6]$  
(b) $H(z)\mid_{z=e^{j\hat\omega}}=1-e^{-j6\hat\omega}$  
(c\) $|H(e^{j\hat\omega})|=|2sin(3\hat\omega)|$  
$\angle H(e^{j\hat\omega})=\begin{cases}\pi/2-3\hat\omega&,0<\hat\omega<\pi/2\\5\pi/2-3\hat\omega&,\pi/2<\hat\omega<\pi\end{cases}$  
$\angle H(e^{j\hat\omega})=\begin{cases}\pi/2-3\hat\omega&,0<\hat\omega<\pi/3\\3\pi/2-3\hat\omega&,\pi/3<\hat\omega<2\pi/3\\5\pi/2-3\hat\omega&,2\pi/3<\hat\omega<\pi\end{cases}$  
(TBD which is correct.)  
(d) $\hat\omega_0=0,\pm\pi/3,\pm2\pi/3,\pm\pi$  
(e) $H(e^{j\pi/4})=2sin(3\pi/4)e^{j(\pi/2-3\pi/4)}\Rightarrow y[n]=\sqrt2cos(\pi n/4-\pi/4)$  
$A=1.41421,\hat\omega=0.78540,\phi=-0.78540$  

P-9.13  
a LTI system has zeros at $\pm1$ and $\pm3$, and four poles at $z=0$. the input to this system is $$x[n]=50-60\delta[n]+20cos(\pi n/2+\pi/3)$$ for $-\infty<n<\infty$. if $h[0]=1$, determine the output of the system $y[n]$ corresponding to the above input $x[n]$. give an equation for $y[n]$ that is valid for all $n$.  

ans:  
$H(z)=Gz^{-4}(z^2-1)(z^2-9)=G(1-10z^{-2}+9z^{-4}),\,y[n]=Gx[n]-10Gx[n-2]+9Gx[n-4],\,\\h[n]=G\delta[n]-10G\delta[n-2]+9G\delta[n-4],G=1$  
$H(e^{j\pi/2})=20$, so $y[n]=50-60(\delta[n]-10\delta[n-2]+9\delta[n-4])+400cos(\pi n/2+\pi/3)$

P-9.14  
the input to the C-to-D converter in Fig. below is $$x(t)=3+cos(600\pi 5-\pi/6)-2cos(1200\pi t)$$ the system function for the LTI system is $$H(z)=1-z^{-1}+z^{-4}-z^{-5}$$ if $f_s=2400$ samples/s, determine an expression for $y(t)$, the output of the D-to-C converter.  
![](https://i.imgur.com/3p0Pp6Y.jpg)

ans:  
$x[n]=3+cos(\pi n/4-\pi/6)-2cos(\pi n/2)$  
$H(e^{j0})=0\\H(e^{j\pi/4})=0\\H(e^{j\pi/2})=2\sqrt2e^{j\pi /4}$
$y[n]=-4\sqrt2cos(\pi n/2+\pi/4)$, by using $-cos(x+\pi)=cos(x)$, $y[n]=4\sqrt2cos(\pi n/2-3\pi/4)$  
$y(t)=4\sqrt2cos(1200\pi t-3\pi/4)$

P-9.16  
in the cascade system, it's known that the system function $H(z)$ of the overall system is the product of four terms $$H(z)=(1-z^{-2})(1-0.8e^{j\pi/4}z^{-1})(1-0.8e^{-j\pi/4}z^{-1})(1+z^{-2})$$ (a) determine the zeros and poles of $H(z)$ and plot them in the complex $z$-plane.  

(b) it's possible to determine two system functions $H_1(z)$ and $H_2(z)$ so that (1) the overall cascade system has the given system function $H(z)$, and (2) the output of the first system is $y_1[n]=x[n]-x[n-4]$. find $H_1(z)$ and $H_2(z)$.  

(c\) determine the difference equation that relates $y[n]$ to $y_1[n]$ for your answer in (b).

ans:  
(a) $6$ of zeros at $z=\pm1,e^{\pm j\pi/2},0.8e^{\pm j\pi/4}$, $6$ of poles at $z=0$
(wait for pic.)[reference link]  
(b) $H_1(z)=1-z^{-4},\,H_2(z)=1-1.6cos(\pi/4)z^{-1}+0.64z^{-2}=1-0.8\sqrt2z^{-1}+0.64z^{-2}$  
(c\) $y[n]=y_1[n]-0.8\sqrt2y_1[n-1]+0.64y_1[n-2]$  

P-9.17  
a causal FIR filter is known to have four zeros: $\{0.8e^{\pm j\pi/3},1.25e^{\pm j\pi/3}\}$, but the zeros do not completely define the system.  
(a) if the first sample of the impluse response is $h[0]=7$, determine the impulse response of the filter.  

(b) if the DC value of the frequency response is $H(e^{j0})=2$, determine the system function of the filter.  

ans:  
(a) $H(z)=G(1-2.05z^{-1}+3.2025z^{-2}-2.05z^{-3}+z^{-4})$, since $h[0]=7, G=7$  
$h[n]=7(\delta[n]-2.05\delta[n-1]+3.2025\delta[n-2]-2.05\delta[n-3]+\delta[n-4])$  

(b) $H(z)=G(1-2.05z^{-1}+3.2025z^{-2}-2.05z^{-3}+z^{-4})$, $H(e^{j0})=(1-2.05+3.2025-2.05+1)G=1.1025G=2,G=1.81406$, $H(z)=1.81406(1-2.05z^{-1}+3.2025z^{-2}-2.05z^{-3}+z^{-4})$  

----

## chapter 10: IIR filters
### the general IIR difference equation
$$\color{#f59b42}{\text{difference equation with feedback}}\\y[n]=\sum^N_{l=1}a_ly[n-l]+\sum^M_{k=0}b_kx[n-k]$$