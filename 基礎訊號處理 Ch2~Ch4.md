---
tags: CSE
---
# 基礎訊號處理 Ch2~Ch4

----
> [TOC]
----

## Chapter 2: Sinusoids
### __singal represented by cosine__

$x(t) = Acos(\omega_0 t + \phi)$, $A$ is **_amplitude_**, $\omega$ is **_radian frequency_**, $\phi$ is **_phase_**.
! there's the other form: $x(t) = Acos(2\pi f_0 t+\phi)$, $f_0$ is called the **_cyclic frequency_**, which $f_0 = \frac{\omega_0}{2\pi}$, and $f_0$ must have units of $s^{-1}$ or, _hertz_.


### __how to determine period__

$x(t)=x(t+T_0) \Rightarrow Acos(\omega_0(t+T_0)+\phi)=Acos(\omega_0t+\phi)\Rightarrow \omega_0T_0=2\pi\Rightarrow T_0=\frac{2\pi}{\omega_0}$
hence, $T$ is **_period_** which can be determined by $\frac{2\pi}{\omega}=\frac{1}{f}$

### __time delay__

whenever a signal can be expressed in the form $x_1(t)=s(t-t_1)$, we say that $x_1(t)$ is a time-shifted version of $s(t)$.

$t_d$ is the **_time delay_**, and the new formula is $x_d(t) = Acos(\omega_0(t - t_d) + \phi_0) = Acos(\omega_0t - \omega t_d + \phi_0)$
the phase of new formula is $- \omega t_d + \phi_0 (mod\, 2\pi)$

### __find special location__

__*peak location*__ is determined by solving $\omega t_p + \phi = 0$ and __*peak period*__ is $T$.
__*zero crossing location*__ is deetermined by solving $\omega t+\phi=\pm\frac\pi2$, and __*zero crossing period*__ is $\frac T2$

*time-delay technique:*
replace $t$ with $t - t_m$:
$x(t - t_m) = A cos(\omega (t - t_m))$
peak value now appears at $t = t_m$ which is $-\frac \phi \omega$. $\phi = -\omega t_m$

### __amplitude expoential decay__

amplitude can decay expoentially
$A(t) = Ae^{-\frac t \alpha}$
rewrite the singal formula above:
$Ae^{-\frac t \alpha} cos(\omega t + \phi)$

### __complex number__

a complex number $z$ can be formed by:
**_polar form_**: $r\, arg\, \theta$
**_exponential form_**: $re^{j\theta}$
$r=|z|$ is called _magnitude_ of z, $\theta$ is called _argument_ of z(denoted $arg\,z$)

### __complex exponential signals__

the _complex exponential signal_ is defined as
$z(t)=Ae^{j(\omega_0 t+\phi)}$
$x(t) = \Re\{Ae^{j\omega t}e^{j \phi}\} = Acos(\omega t + \phi)$
_using $re^{jt}$ to avoid all trigonometric manipulation_

$X:=Ae^{j\phi}$, then $z(t)=Xe^{j\omega_0 t}$
the complex number $X$ is called the **_complex amplitude_**

### __inverse Euler formulas__

$cos\theta=\frac{e^{j\theta}+e^{-j\theta}}2$
$sin\theta=\frac{e^{j\theta}-e^{-j\theta}}{2j}$

based on this, $x(t)=Acos(\omega_0t+\phi)=\frac12A(e^{j(\omega_0t+\phi)}+e^{-j(\omega_0t+\phi)})=\frac12Xe^{j\omega_0t}+\frac12X^*e^{-j\omega_0t}=\frac12z(t)+\frac12z^*(t)=\Re\{z(t)\}$
$Asin(\omega_0t+\phi)=\frac12(Xe^{-j\pi/2})e^{j\omega_ot}+\frac12(X^*e^{j\pi/2})e^{-j\omega_0t}$ (using $sin\theta = cos(\theta-\frac\pi2)$ )

### __phasor addition__
there's many situations where it's necessary to add two or more sinusoidal signal.
general form: $\sum^N_{k=1} A_kcos(\omega_kt+\phi_k)$ .
but now we focus on _all signals have the same frequency_, our goal is to prove that:
$$\sum^{N}_{k=1}A_kcos(\omega_0t+\phi_k)=Acos(\omega_0t+\phi)$$

> proof:
>
> $\sum^N_{k=1}\Re\{A_ke^{j(\omega_0t+\phi_k)}\}=\Re\{e^{j\omega_0t}(\sum^N_{k=1}A_ke^{j\phi_k})\}$
>
> notice that $\sum^N_{k=1}A_ke^{j\phi_k}$ is just lots complex numbers adding together.
> assume $z = \sum^N_{k=1}A_ke^{j\phi_k} = Ae^{j\phi}$
> then rewrite the equation: $\Re\{e^{j\omega_0t}(\sum^N_{k=1}A_ke^{j\phi_k})\}=\Re\{e^{j\omega_0t}Ae^{j\phi}\}=Acos(\omega_0t+\phi)$
> Q.E.D.

EX:
$x_1(t)=1.7cos(20\pi t+70\pi/180)$
$x_2(t)=1.9cos(20\pi t+200\pi/180)$
$x_3(t)=x_1(t)+x_2(t)=?$

$X_1=1.7e^{j(70\pi/180)}\approx0.5814+1.5975j$ , $X_2=1.9e^{j(200\pi/180)}\approx-1.7854-0.6498j$
$x_3(t)=\Re\{e^{20\pi t}(\sum X_k)\}\approx\Re\{1.5322e^{20\pi t+141.79\pi/180}\}=1.5322cos(20\pi t+141.79\pi/180)$

----

### __chap 2 exercise__

P-2.10
define $x(t)$ as
$$x(t)=2sin(\omega_0t+\pi/4)+cos(\omega_0t)$$

(a) express $x(t)$ in the form $x(t)=Acos(\omega_0t+\phi)$
(b) find a complex-valued signal $z(t)$ such that $x(t)=\Re\{z(t)\}$

ans:
(a) $2sin(\omega_0t+\pi/4)+cos(\omega_0t)=2cos(\omega_0t-\pi/4)+cos(\omega_0t)=$
$\Re\{2e^{-(\pi/4)j}e^{j\omega_0t}+e^{j\omega_0t}\}=\Re\{(2e^{-(\pi/4)j}+1)e^{j\omega_0t}\}=\Re\{2.79799e^{-0.5299j}e^{j\omega_0t}\}=$
$\Re\{2.79799e^{j(\omega_0t-0.5299)}\}=2.79799cos(\omega_0t-0.5299)$

(b)
based on above, $z(t)=2.79799e^{j(\omega_0t-0.5299)}$

P-2.12
solve the following equation for $\theta$ :
$$\Re\{(1+j)e^{j\theta}\}=-1$$

find all possible answers in radians.

ans:
$1+j=1.4142e^{j0.7854}$
$\Re\{(1+j)e^{j\theta}\}=\Re\{1.4142e^{j(\theta+0.7854)}\}=1.4142cos(\theta+0.7854)=-1$
$cos(\theta+0.7854)=-1/1.4142\Rightarrow \theta+0.7854=cos^{-1}(-0.7071)=\pm2.35620$$\Rightarrow\theta=\pm2.35620-0.7854 + 2n\pi,\, n\in \Bbb Z$

P-2.14
two sinusoidal signals are defined as
$x_1(t)=\sqrt5cos(7t-\pi/3)$
$x_2(t)=\sqrt5cos(7t+\pi)$
and their sum is denoted by $x(t)=x_1(t)+x_2(t)$.
(a) find a complex-valued signal $z_1(t)$ such that $x_1(t)=\Re\{z_1(t)\}$
(b) find a complex-valued signal $z_2(t)$ such that $x_2(t)=\Re\{z_2(t)\}$
\(c\) find a complex-valued signal $z(t)$ such that $x(t)=\Re\{z(t)\}$

ans:
(a) $z_1(t)=\sqrt5e^{j(7t-\pi/3)}$
(b) $z_2(t)=\sqrt5e^{j(7t+\pi)}$
\(c\) $\Re\{z(t)\}=\Re\{\sqrt5e^{j(7t-\pi/3)}+\sqrt5e^{j(7t+\pi)}\}=\Re\{(\sqrt5e^{j(-\pi/3)}+\sqrt5e^{j\pi})e^{j7t}\}=\Re\{\sqrt5e^{j(7t-2.0944)}\}=x(t)$
$z(t)=\sqrt5e^{j(7t-2.0944)}$

P-2.18
determine the values of the parameters $\{\omega,\phi,A\}$ in the equations below.
(a) $9cos(\omega t+\phi)=Ae^{j(8t+\phi)}+Ae^{-j(8t-2\pi/3)}$
(b) $10cos(9t-\pi/3)=Acos(\omega t-\pi/2)+5cos(\omega t +\phi)$

ans:
(a) $9cos(\omega t+\phi)=2A(e^{j(8t+\phi)}+e^{-j(8t-2\pi/3)})/2=2Acos(8t-2\pi/3)$
$\{\omega,\phi,A\}=\{8,-\frac23\pi,\frac92\}$
(b)
$\Re\{10e^{j(-\pi/3)}e^{j9t}\}=\Re\{Ae^{j(-\pi/2)}e^{j\omega t}+5e^{j\phi}e^{j\omega t}\}=\Re\{(Ae^{j(-\pi/2)}+5e^{j\phi})e^{j\omega t}\}$
$\omega=9$
$\Re\{10e^{j(-\pi/3)}\}=\Re\{Ae^{j(-\pi/2)}+5e^{j\phi}\}=\Re\{5-j5\sqrt3\}=\Re\{j(5sin\phi-A)+5cos\phi\}$
$\phi=2n\pi,\,n\in\Bbb Z$
$A=5\sqrt3$
$\{\omega,\phi,A\}=\{9,2n\pi\mid n\in \Bbb Z,5\sqrt3\}$

P-2.20
simultaneous sinusoidal equations can be solved as simultaneous algebraic equation once the sinusoids are converted to phasors. consider the following two equations with unknowns $A_1,\phi_1,A_2,\phi_2$
$$cos(7t)=A_1cos(7t+\phi_1)+A_2cos(7t+\phi_2)$$

$$sin(7t)=2A_1cos(7t+\phi_1)+A_2cos(7t+\phi_2)$$

let $z_1=A_1e^{j\phi_1},\,z_2=A_2e^{j\phi_2}$, use algebra to determine the values for $A_1,\phi_1,A_2,\phi_2$ .

ans:
$sin(7t)=cos(7t-\pi/2)$
$\Re\{e^{j7t}\}=\Re\{A_1e^{j\phi_1}e^{j7t}+A_2e^{j\phi_2}e^{j7t}\}$
$\Re\{e^{j(-\pi/2)}e^{j7t}\}=\Re\{2A_1e^{j\phi_1}e^{j7t}+A_2e^{j\phi_2}e^{j7t}\}$
$\Rightarrow1=z_1+z_2$
$\Rightarrow-j=2z_1+z_2$
$z_1=-1-j=1.4142e^{j(-2.3562)}$
$z_2=2+j=2.2361e^{j0.4636}$

$A_1=1.4142,\phi_1=-2.3562$
$A_2=2.2361,\phi_2=0.4636$

---

## Chapter 3: Spectrum
### __spectrum of a sum of sinusoids__
a sum of sinusoids can be represented by
$$x(t)=A_0+\sum^{N}_{k=1}A_kcos(2\pi f_kt+\phi_k)$$

which is equal to:
$$x(t)=X_0+\sum^{N}_{k=1}\Re\{X_ke^{j2\pi f_kt}\}$$

note that $X_0=A_0\in \Bbb R$ .

the _inverse Euler formula_ gives another way to represent $x(t)$:
$$x(t)=X_0+\sum^{N}_{k=1}\{\frac{X_k}2e^{j2\pi f_kt}+\frac{X_k^*}2e^{-j2\pi f_kt}\}$$

and this singal representation is called the **_two-sided spectrum_**, since it uses $2N+1$ _positive and negative frequencies_.

our definition of the spectrum is the set of pairs
$$\{(0,X_0), (f_1,\frac12X_1),(-f_1,\frac12X_1^*),...,(f_k,\frac12X_k),...\}$$

! when the number of components is small and the frequencies are spaced widely, in such cases, it's common to refer to the spectrum as being **_sparse_**.

! there's a more compact representation of $x(t)$: $$x(t)=\sum^{N}_{k=-N} a_ke^{j2\pi f_kt}$$

where $a_k=\begin{cases}A_0 &,k=0 \\ \frac12A_ke^{j\phi} &,k>0 \\ \frac12A_{|k|}e^{-j\phi} &, k<0\end{cases}$ 

,and $f_k=\begin{cases}0 &,k=0 \\ f_k &,k>0 \\ -f_{|k|} &,k<0\end{cases}$

! with that representation above, we can know $x(t)=A_0+\sum^N_{k=1} 2\Re\{a_ke^{j2\pi f_kt}\}$

### __beat note__

when multiply two sinusoids having different frequencies, we can create an audio effect called a **_beat note_**.
the multiplication is called **_amplitude modulation_**(AM)

EX:
$x(t)=cos(\pi t)sin(10\pi t)$
use the inverse Euler formula:
$x(t)=(\frac{e^{j\pi t}+e^{-j\pi t}}2)(\frac{e^{j10\pi t}-e^{-j10\pi t}}2)=\frac12cos(11\pi t-\pi/2)+\frac12cos(9\pi t-\pi/2)$
in this derivation, there are four spectrum components at frequencies $\pm11\pi$ and $\pm9\pi$ rad/s with $\frac14$ magnitude.

### __beat note waveform__

with the example above, we can guess a beat note waveform is $x(t)=cos(2\pi f_1t)+cos(2\pi f_2t)$
and now we gonna prove it:
assume $f_2>f_1$, let $f_c=\frac12(f_1+f_2),\,f_\Delta=f_2-f_c=f_c-f_1$ .
$x(t)=cos(2\pi f_1t)+cos(2\pi f_2t)
 =\Re\{e^{j2\pi(f_c-f_\Delta)t}+e^{j2\pi(f_c+f_\Delta)t}\}=\Re\{e^{j2\pi f_ct}(e^{-j2\pi f_\Delta t}+e^{j2\pi f_\Delta t})\}$
$=\Re\{e^{j2\pi f_ct}(2cos(2\pi f_\Delta t))\}=2cos(2\pi f_\Delta t)cos(2\pi f_ct)$

>! there's more general form:
$$x(t)=cos(2\pi f_1t+\phi_1)+cos(2\pi f_2+\phi_2)\iff$$
>
> $$x(t)=2cos(2\pi f_\Delta t+\phi_\Delta)cos(2\pi f_ct+\phi_c)$$
>
> where $\phi_c=\frac12(\phi_1+\phi_2),\, \phi_\Delta=|\phi_1-\phi_c|$
> ! we can prove that it's ok to swap $f_c$ and $f_\Delta$

### __amplitude modulation__
the AM signal is a product of the form
$$x(t)=v(t)cos(2\pi f_ct)$$

in radio, $v(t)$ is represents the voice or music signal to be transmitted. the cosine wave $cos(2\pi f_ct)$ is called the _carrier signal_, and its frequency $f_c$ is called the _carrier frequency_.

### __operations on the spectrum__

#### __scaling or adding a constant__

$$\gamma x(t)=\gamma\sum^M_{k=-M}a_ke^{j2\pi f_kt}=\sum^M_{k=-M}(\gamma a_k)e^{j2\pi f_kt}$$

$$x(t)+c=(a_0+c)+\sum_{f_k\neq0}a_ke^{j2\pi f_kt}$$

#### __adding signals__

$$x_1(t)+x_2(t)=\sum_{f_{1k}}a_{1k}e^{j2\pi f_{1k}t}+\sum_{f_{2l}}a_{2l}e^{j2\pi f_{2l}t}$$

> i.e.
![](https://i.imgur.com/6QfwMUI.jpg)

#### __time-shifting__

$$y(t)=x(t-\tau_d)=\sum_ka_ke^{j2\pi f_k(t-\tau_d)}=\sum_k(a_ke^{-j2\pi f_k\tau_d})e^{j2\pi f_kt}$$

let $b_k=a_ke^{-j2\pi f_k\tau_d}$ ,
$$y(t)=x(t-\tau_d)=\sum_kb_ke^{j2\pi f_kt}$$

#### __differentiating__

$$y(t)=\frac{d}{dt}x(t)=\sum_k (a_kj2\pi f_k)e^{j2\pi f_kt}=\sum_k b_ke^{j2\pi f_kt}$$

#### __frequency shifting__

$$y(t)=Ae^{j\phi}e^{j2\pi f_ct}x(t)=\sum_k(a_kAe^{j\phi})e^{j2\pi(f_c+f_k)t}$$

### __periodic waveforms__

the time interval $T_0$ is called the **_period_** of $x(t)$, which satisfies the condition that $x(t+T_0)=x(t)$, and the smallest interval is **_fundamental period_**.

we can construct a period signal easily:
$$x(t)=A_0+\sum^N_{k=1}A_kcos(2\pi kF_0t+\phi_k)$$

where $f_k=kF_0,\,k\in\Bbb Z$ , $f_k$ is called the $k^{th}$ **_harmonic_** of $F_0$, which is **_fundamental frequency_**.

! $F_0=\frac1\lambda gcd\{\lambda f_k\}$

and this singal form is called **_harmonic_**.

> prove it's a periodic singal:
> we know $\frac1{F_0}=T_0$, $x(t+T_0)=A_0+\sum^N_{k=1}A_kcos(2\pi kF_0(t+T_0)+\phi_k)=A_0+\sum^N_{k=1}A_kcos(2\pi kF_0t+2\pi kF_0T_0+\phi_k)=$
> $A_0+\sum^N_{k=1}A_kcos(2\pi kF_0t+\phi_k)=x(t)$

### __fourier series__
> _every periodic signal can be synthesized as a sum of harmonically related sinusoids --- Fourier, might say that_

**_Fourier Synthesis Summation_** :
$$x(t)=\sum^{\infty}_{k=-\infty}a_ke^{j(2\pi/T_0)kt}$$

**_Fourier Analysis Integral_** :
$$a_k=\frac1{T_0}\int^{T_0}_0x(t)e^{-j(2\pi/T_0)kt}\,dt$$

> prove Fourier Analysis Integral works:
> $x(t)$ is comprised of $a_ke^{j2\pi kF_0t}$, 
> $$\frac1{T_0}\int_0^{T_0}a_ke^{j2\pi k'F_0t}e^{-j2\pi kF_0t}=0,\,k'\neq k$$
>
> $$\frac1{T_0}\int_0^{T_0}a_ke^{j2\pi k'F_0t}e^{-j2\pi kF_0t}=a_k,\,k'=k$$
> 
> Q.E.D.

EXAMPLE:
$x(t)=|sin(2\pi t/T_1)|$

> $sin(2\pi t/T_1)$ :
![](https://i.imgur.com/jRd2DFJ.jpg)

> $|sin(2\pi t/T_1)|$ :
![](https://i.imgur.com/iCBW0rP.jpg)

based on the picture, $T_0=\frac12T_1$

$a_k=\frac1{T_0}\int^{T_0}_0 |sin(2\pi t/T_1)|e^{-j(2\pi/T_0)kt}$

since we integral on the interval $[0, T_0]$, the absolute function is dispensable:

$$a_k=\frac1{T_0}\int^{T_0}_0 sin(2\pi t/T_1)e^{-j(2\pi/T_0)kt}=\frac1{\pi(1-2k)}+\frac1{\pi(1+2k)}=\frac2{\pi(1-4k^2)}$$

$$x(t)=\sum^{\infty}_{k=-\infty}\frac2{\pi(1-4k^2)}e^{j(2\pi/T_0)kt}=\frac2\pi+\sum^{\infty}_{k=1}\frac4{\pi(1-4k^2)}cos(2\pi kt/T_0)$$

where $T_0=\frac12T_1$

---

### chap 3 exercise

P-3.1
a signal $x(t)$ has the two-sided spectrum representation shown below.
![](https://i.imgur.com/tLC3HXh.jpg)

(a) write an equation for $x(t)$ as a sum of cosines.
(b) is $x(t)$ a periodic signal? if so, determine its fundamental period and its fundamental frequency.

ans:
(a) $8cos(2\pi(175)t-\pi/2)+14cos(2\pi(50)t-\pi/3)+11$
(b) yes, $gcd(50,175)=25(Hz)=F_0$, $T_0=0.04(s)$

P-3.8
consider the signal $x(t)=8+10cos(2\pi(100)t+\frac14\pi)+20cos(2\pi(250)t)$

(a) using Euler's relation, the signal $x(t)$ defined above can be expressed as a sum of complex exponential signals using the finite Fourier synthesis summation.
determine values for $f_0,N$, and all the complex amplitudes,$a_k$.
(b) is the signal $x(t)$ periodic? if so, what's the fundamental period?

ans:
(a) $f_0=50$, $N=5$, $a_{\pm2}=5e^{\pm j\pi/4},a_{\pm5}=10$
(b) yes, $0.02(s)$

P-3.9
an AM cosine wave is represented by the formula
$$x(t)=[12+9cos(\pi t-\pi/3)]sin(14\pi t)$$

(a) use phasors to show that $x(t)$ can be expressed in the form
$$x(t)=A_1cos(\omega_1t+\phi_1)+A_2cos(\omega_2t+\phi_2)+A_3cos(\omega_3t+\phi_3)$$

where $\omega_1<\omega_2<\omega_3$; that is, find values of the parameters $A_1, A_2, A_3, \phi_1, \phi_2, \phi_3, \omega_1, \omega_2, \omega_3$.

ans:
(a) $\omega_1=13\pi, \omega_2=14\pi, \omega_3=15\pi, \phi_1=-\frac16\pi, \phi_2=-\frac12\pi, \phi_3=-\frac56\pi, A_1=\frac92, A_2=12, A_3=\frac92$

----

## Chapter 4: Sampling and Aliasing

### __sample__
a _discrete-time signal_ is represented: $$x[n]=x(nT_s),\,-\infty<n<+\infty$$

$x[n]$ are called **_samples_** of the continuous-time signal.

the time interval between samples, $T_s$, can also be expressed as a fixed **_sampling rate_**, $f_s$, in samples/s: $$f_s=\frac1{T_s}\,samples/s$$

therefore, $x[n]$ can be rewritten as $x[n]=x(\frac n{f_s})$

if we sample a sinusoid of the form $Acos(\omega t+\phi)$, we obtain $x[n]=x(nT_s)=Acos(\omega nT_s+\phi)=Acos(\hat{\omega} n+\phi)$
where we have defined $\hat\omega=\omega T_s=\omega/f_s$ , $\hat\omega$ is called **_normalized radian frequency_**.

### __alias__
the samples is the form like this: $x[n]=cos(\hat\omega n+\phi)$
since $n$ is a integer, we know $cos(\hat\omega n + \phi)=cos(\hat\omega n+2\pi n+\phi),\,\forall n\in \Bbb Z$
we call $cos(\hat\omega n+2\pi n+\phi)$ an **_alias_** of $cos(\hat\omega n+\phi)$.

__EXAMPLE__: find all alias of $7cos(0.4n\pi-0.2\pi)$.
ans:
$7cos(0.4n\pi-0.2\pi+2n\pi),\,n\in\Bbb Z$
$7cos(-0.4n\pi+0.2\pi+2n\pi),\,n\in\Bbb Z$

from the example above, we can notice that all aliases of frequency $\hat\omega$ are $\hat\omega$, $\hat\omega+2n\pi$, $-\hat\omega+2n\pi$, $n\in\Bbb Z$

in order not to be confused which frequency we should use, we call a frequency, $\omega$, **_principal alias_** if a frequency, $-\pi<\omega\leq+\pi$

### __Shannon sampling theorem__
a continuous-time signal $x(t)$ with frequencies no higher than $f_{max}$ can be reconstructed exactly from its samples $x[n]=x(nT_s)$, if the samples are taken at a rate $f_s=1/T_s$ that is greater than $2f_{max}$

! the minimum sampling rate of $2f_{max}$ is called the **_Nyquist rate_**

### __ideal reconstruction__
the reconstruction process would undo the C-to-D conversion, so it's called **_D-to-C conversion_**

since the ideal D-to-C converter is defined by the mathematical substitution $t=nT_s=n/f_s$, we'd expect the same relationship to govern the ideal D-to-C converted, that is, $$x(t)=x[n]\mid_{n=tf_s},\,t\in \Bbb R\land t\in(-\infty,+\infty)$$

derivation: $x[n]=x(nT_s)$, let $nT_s=t$, then $n=tf_s$, $x[tf_s]=x(t)$.

however, this substitution is _only_ true when $x[n]$ is expressed as a sum of sinusoids formula.

in real world, $x[n]$ is just a sequence of number. an actual hardware digital-to-analog converter shall "fill in" the signal values between the sampling times, $t_n=nT_s$, which can be done by useing **_interpolation_**.

but hey, we're now talking "ideal" reconstruction, so it's fine to do that.

### __ideal reconstruction example__
the spectrum of a sampled discrete-time signal also has two spectrum lines at $\hat\omega=\pm\omega_0/f_s$ :
$x[n]=x(nT_s)=Acos((\omega_0T_s)n+\phi)=Acos((\omega_0/f_s)n+\phi)$

however, it also must contain all the aliases at the following discrete-time frequencies:
$$\hat\omega=\omega_0/f_s+2n\pi,\,n\in\Bbb Z$$

$$\hat\omega=-\omega_0/f_s+2n\pi,\,n\in\Bbb Z$$

now we consider a singal $x(t)=cos(2\pi(100)t+\pi/3)$, and we'll sample it at different frequency, see how's its outcome.

#### __over-sampling__

if $f_s=500(Hz)$, $x[n]=x(n/f_s)=x(2/5\pi t+\pi/3)$, we know the spectrum of $x[n]$ is:
$\hat\omega+2n\pi,\,n\in\Bbb Z$ and
$-\hat\omega+2n\pi,\,n\in\Bbb Z$
which is $\pm0.4\pi,\pm1.6\pi,\pm2.4\pi...$
> the spectrum of original singal:
![](https://i.imgur.com/a9CWYpt.png)

> the spectrum of samples:
![](https://i.imgur.com/iqQz0h3.png)
! the star up the spectrum line means that the magnitude is the conjugate to another one.

we can simplely choose the _principal alias_, which is $\pm0.4\pi$.
so we use the formula $\hat\omega f_s=\omega=\pm0.4\pi\times 500=\pm200\pi=\pm2\pi(100)$

however, things get wrong sometimes:

#### __under-sampling__
if we choose $f_s=80 (Hz)$, $x[n]=cos(5/2n\pi+\pi/3)$
the spectrum of samples is $\pm2.5\pi,\pm0.5\pi,\pm1.5\pi...$

this time, we can't just simplely choose the principal alias, since it's worng when we reconstruct by it, $\omega'=\hat\omega f_s=\pm0.5\pi\times80=\pm40\pi=\pm2\pi(20)$

![](https://i.imgur.com/LSWERfG.jpg)

notice that if we can let $\hat\omega$ be principal alias everytime, the outcome'll be right everytime.

based on the definition of principal alias $-\pi<\hat\omega\leq+\pi$, we can derive $-\pi f_s<\omega_0=\hat\omega f_s\leq +\pi f_s\iff -\frac{f_s}2<\omega_0/2\pi=f_0\leq +\frac{f_s}2$, which means if $\hat\omega$ is principal alias, the output frequency always **_lies between $-f_s/2$, and $+f_s/2$_**.

that's why _shannon sampling theorem_ says the sample frequency, $f_s$, must greater than $2f_{max}$ .

#### __when $f_s$ equal the original frequency__

$x[n]=cos(2n\pi+\pi/3)=cos(\pi/3)$
$\hat\omega=2n\pi,\,n\in\Bbb Z$
$\omega'=0$
$x'(t)=cos(\pi/3)$

### __folding__
we want to know the graph with two axis, x-axis is input frequency and y-axis is the output after-reconstructing frequency.
and define the sampling rate $f_s$ .

based on the example above, we know when $x$, which is input frequency, less or equal than $\frac12f_s$ , $y$ is equal to $x$.

when $f_s\geq x>\frac12f_s$, an alias of $\pm\hat\omega$ occurs, which is $\pm\hat\omega\mp2\pi$, since $2\pi\geq|\hat\omega|>\pi$.
now the negative frequency becomes the positive one, the output frequency is $y=\frac1{2\pi}(-\hat\omega+2\pi)f_s=-\frac1{2\pi}\omega+f_s=-x+f_s$

by intuition, the final graph shall be like:

![](https://i.imgur.com/kqnxEoK.png)

### __interpolation__
since the ideal reconstructor(D-to-C) isn't practical, it need another way to approximate the ideal result. there actual haraware systems, called D-to-A converters.

one way to approximate is interpolation with pulse, which can be written as:
$$y(t)=\sum^\infty_{n=-\infty}y[n]p(t-nT_s)$$

where $p(t)$ is the characteristic pulse shape of the converter.

#### __zero-order hold interpolation__

example:
![](https://i.imgur.com/h0CNLcF.jpg)

formula:
$p(t)=\begin{cases}1 &-\frac12T_s<t\leq\frac12T_s\\0 &else\end{cases}$

#### __linear interpolation__

example:
![](https://i.imgur.com/awc2vpK.jpg)

formula:
$p(t)=\begin{cases}1-|t|/T_s &-T_s\leq t\leq T_s\\ 0 &else\end{cases}$

! notice that every intervals is comprised of two pulse.

#### __cubic-spline interpolation__

example:
![](https://i.imgur.com/e5R3u06.jpg)

#### __ideal bandlimited interpolation__

formula:
$p(t)=sinc(t/T_s)=\frac{sin\pi t/T_s}{\pi t/T_s},\,-\infty<t<+\infty$

---
### chap 4 exercise

P-4.1
let $x(t)=10cos(9\pi t-\pi/5)$. in each of the following parts, the discrete-time signal $x[n]$ is obtained by sampling $x(t)$ at a rate $f_s$ samples/s, and the resultant $x[n]$ can be written as $$x[n]=Acos(\hat\omega_1n+\phi)$$

for each part below, determine the values of $A,\phi,$ and $\hat\omega_1$ such that $0\leq\hat\omega\leq \pi$. in addition, state whether or not the signal has been over-sampled or under-sampled.
(a) sampling frequency is $f_s=11$ samples/s.
(b) sampling frequency is $f_s=7$ samples/s.
\(c\) sampling frequency is $f_s=4$ samples/s.

ans:
(a) $\hat\omega=9\pi/11$, over-sampled.
(b) $\hat\omega=-9\pi/7+2\pi$, under-sampled.
\(c\) $\hat\omega=9\pi/4-2\pi$, under-sampled.

P-4.2
determine which of the following discrete-time cosine signals are equal (i.e., plots of $x_i[n]$ versus $n$ will be identical):
$x_1[n]=cos(0.8\pi n+0.2\pi)$
$x_2[n]=cos(-2.8\pi n+0.2\pi)$
$x_3[n]=x_1[n]+x_2[n]$
$x_4[n]=cos(5.2\pi n-0.2\pi)$
$x_5[n]=x_1[n]+cos(-7.1\pi n-0.1\pi)$

ans:
$x_1[n]=x_4[n]$

P-4.3
for each of the discrete-time signals $y[n]$ below, determine the continuous-time output signal $y(t)$ from an ideal D-to-C converter whose smapling rate is $f_s=3200(Hz)$
(a) $y[n]=8cos(\frac\pi4n-\pi/3)$
(b) $y[n]=cos(1.4\pi n)$
\(c\) $y[n]=2cos(2.5\pi n-\pi/4)+sin(1.4\pi n)$

ans:
(a) $8cos(800\pi t-\pi/3)$
(b) $cos(4480\pi t)$
\(c\) $2cos(8000\pi t-\pi/4)+cos(4480\pi t-\pi/2)=2cos(8000\pi t-\pi/4)+sin(4480\pi t)$

P-4.4
(a) $x(t)=cos(3000\pi t)cos(6000\pi t)$, draw a sketch of its spectrum. lebel the frequencies and complex amplitudes of each component spectral line.
(b) determine the minimum sampling rate that can be used to sample $x(t)$ without aliasing for any of its components.
\(c\) $r(t)=cos(2\times10^6\pi t)sin(5\times 10^6\pi t)sin(9\times10^6\pi t)$, determine the minimum sampling rate to avoid aliasing for any of its components.
(d) $v(t)=cos(2\times10^6\pi t)+sin(5\times10^6\pi t)+sin(9\times10^6\pi t)$, determine the minimum sampling rate to avoid aliasing for any of its components.

ans:
(a) 
![](https://i.imgur.com/5IgmcEK.png)
correct: the complex amplitudes should be $\frac14$ _(date: 12.4.21)_
(b) $9000$
\(c\) $1.6\times10^7$
(d) $9\times10^6$

P-4.6
(a) the input $x(t)=26cos(2\pi(200)t+0.7\pi)+14cos(2\pi(500)t-0.3\pi)$, and the sampling rate of the C-to-D converter is $f_s=2500$ samples/s, sketch the spectrum of the discrete-time signal $x[n]$
(b) use the spectrum for $x[n]$ found in the previous part to determine a simple formula for the output signal $y(t)$ when the rate of the D-to-C converter is $f_s=1500$ samples/s

(a)
$x[n]=26cos(\frac4{25}\pi n+0.7\pi)+14cos(\frac25\pi n-0.3\pi)$
! skip the sketch
(b)
$y(t)=26cos(240\pi t+0.7\pi)+14cos(600\pi t-0.3\pi)$

P-4.7
given the input signal $x(t)=26cos(2\pi(200)t+0.7\pi)+14cos(2\pi(500)t-0.3\pi)$
in the system with ideal C-to-D and D-to-C converters, assume that the sampling rate of both converters is $f_s=475$ samples/s.

(a) is the output signal $y(t)$ equal to the input signal $x(t)$?
\(c\) determine a simple formula for the output signal $y(t)$ when $f_s=475(Hz)$

ans:
(a) $x[n]=26cos(\frac{16}{19}\pi n+0.7\pi)+14cos(\frac{40}{19}\pi n-0.3\pi)=26cos(\frac{16}{19}\pi n+0.7\pi)+14cos(-\frac{40}{19}\pi n+0.3\pi+2\pi n)$
$=26cos(\frac{16}{19}\pi n+0.7\pi)+14cos(-\frac{2}{19}\pi n+0.3\pi)=26cos(\frac{16}{19}\pi n+0.7\pi)+14cos(\frac{2}{19}\pi n-0.3\pi)$
$\Rightarrow y(t)=26cos(400\pi t+0.7\pi)+14cos(50\pi t-0.3\pi)$
no.
\(c\) $y(t)=26cos(400\pi t+0.7\pi)+14cos(50\pi t-0.3\pi)$

P-4.9
(a) suppose that the output from the C-to-D converter is $x[n]=cos(0.4\pi n)$, $f_s=7000$ samples/s. determine a formula for the continuous-time sinusoidal input $x(t)$ using the smallest frequency greater than $9000(Hz)$

(b) suppose the output from the C-to-D converter is $x[n]=cos(0.2\pi n)$, the input signal is $x(t)=cos(525\pi t)$, $f_s<135$ samples/s . determine the largest possible sampling rate satisfying theres three conditions.

\(c\) suppoes the output from the D-to-C converter is $y(t)=cos(325\pi t)$, and the input to the C-to-D converter is $x(t)=cos(75\pi t)$. if both converters are operating at the $f_s$, determine the smallest possible sampling rate $f_s$ satisfying these conditions.

ans:
(a) aliases: $0.4\pi, 1.6\pi, 2.4\pi, 3.6\pi...$
! notice $\phi$ need to be negative when it's derived by negative frequency.
$f$ greater than $9000(Hz)\iff$ $\omega$ greater than $18000\pi(Rad/s)$
$\omega=3.6\pi\times7000=25200\pi$
$x(t)=cos(25200\pi t)$

(b) alias: $0.2\pi,1.8\pi,2.2\pi,3.8\pi,4.2\pi,5.8\pi...$
alias of $\hat\omega=525\pi/f_s$
alias of $\hat\omega f_s=525\pi$
$f_s=525/4.2=125$ samples/s

\(c\) not exists.

P-4.11
$x[n]=3cos(800\pi n/f_s)$, for $-\infty<n<\infty$. in the following parts, assume that $f_s=3600(Hz)$

(a) how many samples are taken in one period of the cosine wave $x(t)$

(b) consider $y(t)=3cos(\omega_0 t)$. find $\omega_0$, which between $7000\pi$ and $9999\pi$ rad/s, such that the signal samples are identical to $x[n]$ above.

\(c\) determine the average number of samples taken in one period of $y(t)$.

ans:
(a) $T=1/400(s)$, $f_sT=9$
(b) aliases: $\frac29\pi, \frac{16}9\pi,\frac{20}9\pi,\frac{34}9\pi,\frac{38}9\pi...$
$\omega_0/f_s=$ one of aliases $=k\pi$
$7000\pi\leq\omega_0=k\pi f_s\leq9999\pi\iff7000/f_s\leq k\leq9999/f_s\Rightarrow17.5/9\leq k\leq24.9975/9$
$\Rightarrow\omega_0=\frac{20}9\pi\times3600=8000\pi$
\(c\) $T=1/4000(s)$, $f_sT=0.9$

P-4.12
suppose that a discrete-time signal $x[n]$ given by the formula $$x[n]=11.3cos(0.3\pi n+0.6\pi)$$ was obtained by sampling a continuous-time signal $x(t)=Acos(2\pi f_0t+\phi)$ at a sampling rate of $f_s=4000$ samples/s. determine three different continuous-time signals that could have produced $x[n]$. all these continuous-time signals should be positive and less than $7000$ Hz.

ans:
alias: $0.3\pi,1.7\pi,2.3\pi,3.7\pi,4.3\pi,...$
$x_1(t)=11.3cos(2\pi(600)t+0.6\pi)$
$x_2(t)=11.3cos(2\pi(3400)t-0.6\pi)$
$x_3(t)=11.3cos(2\pi(4600)t+0.6\pi)$

P-4.16
suppose that a discrete-time signal $x[n]$ is given by the formula $$x[n]=cos(0.7\pi n+0.2\pi)$$ and that it was obtained by sampling a continuous-time signal at a sampling rate of $f_s=2000$ samples/s

(a) determine two different continuous-time signals $x_1(t)$ and $x_2(t)$ with frequencies between $7000$ and $9000Hz$, and whose samples are equal to $x[n]$.

(b) determine the continuous-time signal $x(t)$ whose frequency lies between $7000$ and $8000Hz$ and whose samples are equal to $x[n]$.

\(c\) suppose that $x[n]=cos(\hat{\omega}n+0.2\pi)$, where $\hat\omega\in[0,\pi]$ is the discrete-time signal obtained by sampling $x(t)=cos(2\pi ft+\phi)$ at a sampling rate of $f_s=2000Hz$. determine the general formula fot the frequency $f$ in terms of $\hat\omega$ such that the frquency satisfies $f\in[7000,8000]$ Hz. also, determine the $\phi$.

ans:
(a) 
$7000\leq f \leq 9000 \iff 14000\pi\leq \omega=\hat\omega f_s\leq18000\pi\iff7\pi\leq\hat\omega\leq 9\pi$
alias: $0.7\pi,1.3\pi,2.7\pi,3.3\pi,...,7.3\pi,8.7\pi,...$
$x_1(t)=cos(2\pi(7300)t-0.2\pi)$
$x_2(t)=cos(2\pi(8700)t+0.2\pi)$

(b) $x(t)=cos(2\pi(7300)t-0.2\pi)$
\(c\) $7000\leq f \leq 8000 \iff 14000\pi\leq \omega=\hat\omega f_s\leq16000\pi\iff7\pi\leq\hat\omega\leq 8\pi$
$7\pi\leq2n\pi\leq\hat\omega+2n\pi\leq\pi+2n\pi\leq8\pi\Rightarrow7\leq2n\leq2n+1\leq8\Rightarrow$ no sol.

$7\pi\leq-\pi+2\pi+2n\pi\leq(-\hat\omega+2\pi)+2n\pi\leq2\pi+2n\pi\leq8\pi\Rightarrow7\leq2n+1\leq2n+2\leq8\Rightarrow n=3$
$(-\hat\omega+8\pi)f_s=\omega=2\pi f\iff f=1000(-\hat\omega+8\pi)/\pi$
$\phi=-0.2\pi$
$x(t)=cos(2000(-\hat\omega+8\pi)t-0.2\pi)$

P-4.21 an AM wave is represented by the formula $$x(t)=[7+cos(\pi t)]sin(15\pi t+\pi/2)$$ (a) use _phasors_ to show that $x(t)$ can be expressed in the form $$x(t)=A_1cos(\omega_1t+\phi_1)+A_2cos(\omega_2t+\phi_2)+A_3cos(\omega_3t+\phi_3)$$ where $\omega_1<\omega_2<\omega_3$.

\(c\) determine the minimum sampling rate that can be used to sample $x(t)$ without aliases.

ans:
(a) $x(t)=[7+cos(\pi t)]cos(15\pi t)=(7+\frac{e^{j\pi t}+e^{-j\pi t}}2)(\frac{e^{j15\pi t}+e^{-j15\pi t}}2)=\frac12cos(14\pi t)+7cos(15\pi t)+\frac12cos(16\pi t)$

\(c\) $f_s\geq16Hz$

P-4.22
assume that the sampling rates of the C-to-D and D-to-C converters are equal, and the input to the ideal C-to-D converter is $$x(t)=2cos(2\pi(50)t+\pi/2)+cos(2\pi(150)t)$$ (a) if the output of the ideal D-to-C converter is equal to the input $x(t)$, that is, $$y(t)=2cos(2\pi(50)t+\pi/2)+cos(2\pi(150)t)$$ what general statement can you make about the sampling frequency $f_s$ in this case?

(b) if $f_s=250$ samples/s, determine the discrete-time signal $x[n]$, and give an expression for $x[n]$ as a sum of consine. _make sure all frequencies shall be postive and less than $\pi$ radians_.

(d) if the output of the ideal D-to-C converter is $$y(t)=2cos(2\pi(50)t+\pi/2)+1$$ determine the value of the sapmling frequency $f_s$.

ans:
(a) $f_s\geq300Hz$
(b) $x[n]=2cos((2/5)\pi n+\pi/2)+cos(-(6/5)\pi n+2\pi)=2cos((2/5)\pi n+\pi/2)+cos((4/5)\pi n)$
(d) ![](https://i.imgur.com/cdndgDk.png)
since $50Hz$ can be reconstructed, so $50\leq\frac12f_s$ .
$150Hz$ will be folded to $0Hz$, so $nf_s=150Hz$ and $\frac12f_s\leq150$.
based on the condition above, $f_s=150Hz$.