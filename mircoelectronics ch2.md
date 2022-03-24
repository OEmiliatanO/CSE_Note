---
tags: CSE
---
# mircoelectronics ch2

---
> [TOC]
---

## basic physics of semiconductors
### semiconductor materials
#### charge carries
![](https://i.imgur.com/CHEE9Bm.png)  
![](https://i.imgur.com/7mWCEwK.png)

__hole__:  
hole is a imaginary particle, whose charge is positive. it's created when a electron is freed from a covalent bond and leave a "void" behind.  
called a “hole,” such a void can readily absorb a free electron if one becomes available.
thus, we say an “electron-hole pair” is generated when an electron is freed, and an “electron-hole recombination” occurs when an electron “falls” into a hole.  

the movement of holes is actually the movement of current. so concept of hole is useful.  

__bandgap enery__:  
bandgap enery, denoted by $E_g$, is a minimum energy which is required to dislodge an electron from a covalent bond.  
this energy is a fundamental porperty of the material. for silicon, $E_g=1.12eV$.  

density of free electrons: $n_i=5.2\times10^{15}T^{3/2}e^{-E_g/(2kT)} \text{electrons/cm}^3$ where $k=1.38\times10^{-23}\text{J/K}$ is called the Boltzmann constant.  

__EXAMPLE:__  
detemine the density of electrons in silicon at $T=300\text{K}$ (room temperature) and $T=600\text{K}$.  
($1eV = 1.6\times10^{-19}J$)

ans:  
$E_g = 1.12eV=1.792\times10^{-19}J$.  
$n_i(T=300K)=5.2\times10^{15}(300)^{3/2}e^{-1.792\times10^{-19}/(2\times1.38\times10^{-23}\times300)}=1.078\times10^{10}$  
$n_i(T=600K)=5.2\times10^{15}(600)^{3/2}e^{-1.792\times10^{-19}/(2\times1.38\times10^{-23}\times600)}=1.526\times10^{15}$  

the $n_i$ values obtained above may appear quite high, but silicon has $5\times10^{22}\text{atoms/cm}^3$, only one in $5\times10^{12}$ atoms benefit from a free electron at room temperature, i.e. pure silicon is a very poor condutor.  

### modification of carrier densities
__intrinsic and extrinsic semiconductors__:  
it's possible to modify the resistivity of silicon by replacing some of the atoms in the crystal with atoms of another material.  
in an intrinsic semiconductor, the electron density, $n(=n_i)$, is equal to the hole density, $p$, thus 
\\$$np=n_i^2$$  

__n-type semiconductor:__

when some P atoms are added into a silicon crystal, there will be an extra free electron since P contains five valence electrons.  

![](https://i.imgur.com/AcPIUKl.png)  

the controlled addition of an “impurity” such as phosphorus to an intrinsic semiconductor is called “doping,” and phosphorus itself a “dopant.”  
providing many more free electrons than in the intrinsic state, the doped silicon crystal is now called “extrinsic,” more specifically, an “n-type” semiconductor to emphasize the abundance of free electrons.

surprisingly, n-type semiconductor still follows the rule, $np=n_i^2$ where $n$ and $p$ respectively denote the electron and hole densities in the extrinsic semiconductor.  
The quantity $n_i$ represents the densities in the intrinsic semiconductor (hence the subscript i) and is therefore independent of the doping level.  

__EXAMPLE:__  
a piece of crystalline silicon is doped uniformly with phosphorus atoms. The doping density is $10^{16} \text{atoms/cm}^3$. Determine the electron and hole densities in this material at the room temperature.  

ans:  
the addition of $10^{16}$ P atoms bring the same number of electrons per cubic cm.  
with the result in the last example, the density of electrons in intrinsic silicon is about $10^{10}$. since $10^{16} >> 10^{10}$, we simply let $n=10^{16}$.  
so $p = n_i^2/n=10^{20}/10^{16}=10^4$.  
(notice the amount of hole is dramatically decreasing.)  

this example justifies the reason for calling electrons the “majority carriers” and holes the “minority carriers” in an n-type semiconductor.

__p-type semiconductor:__  
now we add some B atoms instead of P atoms.  

![](https://i.imgur.com/EsIaGiP.png)

there will be a lack of an free electrons, however, with an extra hole. so holes in here are the majority carriers.  
The boron atom is called an “acceptor” dopant.  

let conclude the result so far:  
1. if an intrinsic semiconductor is doped with a
density of $N_D(>>n_i)$ donor atoms per cubic centimeter, then the mobile charge densities
are given by  

majority carries: $n\approx N_D$  
minority carries: $p\approx \frac{n_i^2}{N_D}$

2. if doped with a density of $N_A(>>n_i)$ acceptor atoms per cubic centimeter:  

majority carries: $p\approx N_A$  
minority carries: $n\approx \frac{n_i^2}{N_A}$

> summary:
![](https://i.imgur.com/oUTgzRc.png)

### transport of carriers
__dift__:  
The electric field accelerates the charge carriers in the material, forcing some to flow from one end to the other. Movement of charge carriers due to an electric field is called "drift".  

![](https://i.imgur.com/7KRvlzF.png)

semiconductors behave in a similar manner.  
the charge carriers are accelerated by the field and accidentally collide with the atoms in the crystal, eventually reaching the other end and flowing into the battery.  
The acceleration due to the field and the collision with the crystal counteract, leading to a _constant_ velocity for the carriers.  

the constant velocity is $v=\mu E$ where $\mu$ is called the mobility and usually expressed in $\text{cm}^2/V\cdot s$.  
For example in silicon, the mobility of electrons, $\mu_n = 1350 cm^2/(V\cdot s)$, and that of holes, $\mu_p = 480 cm^2/(V \cdot s)$.  
since electrons move in a direction opposite to the electric field, we must express
the velocity vector as $\vec{v}_e=-\mu_n\vec{E}$ and for holes $\vec{v}_h=\mu_p\vec{E}$.  

__EXAMPLE:__  
a uniform piece of n-type of silicon that is $1 \mu m$ long senses a voltage of $1V$. determine
the velocity of the electrons.  

ans:  
since the material is uniform, we have $E=V/L$, where $L$ is the length. thus, $E=10^4V/cm$ and hence $v=\mu_n E=1350\times10^4=1.35\times10^7$.  

with the velocity of carriers known, we can calculate the current.  
![](https://i.imgur.com/FhBP6Wh.png)  

first note that $q=1.6\times10^{-19}C$ where electron have negative value of it and the hole are reversed. now suppose a voltage $V_1$ is applied across a uniform semiconductor bar having a free electron density of $n$. assuming the electrons move with the velocity of $v\,m/s$, considering a cross section of the bar at $x=x_1$ and taking two snapshots at $t=t_1$ and $t=t_1+1$ second, we note that the total charge in $v$ meters passes the cross section in 1 second. since the bar has width of $W$, we have 
\\$$I = v\cdot W\cdot h\cdot n\cdot -q$$

where $I$ is the current passing throgh the cross section. and we assume the current passing through are steady and average, we have 
\\$$J_n=\frac{I}{Wh}=-vnq=\mu_nEnq$$  

where $J_n$ denotes the "current density", i.e. the current passing through a _unit_ cross section area, and is expressed in $A/cm^2$.  

in the presence of both electrons and holes, 
\\$$J_{tot}=\mu_nEnq+\mu_pEpq = Eq(\mu_nn+\mu_pp)$$  

this equation gives the drift current density in response to an electric field $E$ in a semiconductor having uniform electron and hole densities.  

__EXAMPLE:__  
in an experiment, it is desired to obtain equal electron and hole drift currents. how should the carrier densities be chosen?  

ans:  
so the equation, $\mu_nn=\mu_pp$, must be satisified.  
by using $np=n_i$, we derive $\sqrt{(\mu_n/\mu_p)}n_i=p$ and $\sqrt{(\mu_p/\mu_n)}n_i=n$.  
so $n = 0.596n_i$ and $p=1.68n_i$.  

__velocity saturation:__  
In reality, if the electric field approaches sufficiently high levels, $v$ no longer
follows E linearly (not $v = μE$). This is because the carriers collide with the lattice so frequently and the time between the collisions is so short that they cannot accelerate much.  
as a result, $v$ varies “sublinearly” at high electric fields, eventually reaching a saturated level, $v_{sat}$, called velocity saturation. this effect manifests itself in some modern transistors, limiting the performance of circuits.  

![](https://i.imgur.com/BXwxNbD.png)
so the velocity must be modified:
\\$$v=\frac{\mu_0}{1+bE}E$$  

where $\mu_0$ is the "low-field" mobility and $b$ a proportionality factor.  
since
\\$$\lim_{E\to\infty}\,v=v_{sat}=\frac{\mu_0}{b}$$  

and hence $b=\frac{\mu_0}{v_{sat}}$, 
\\$$v=\frac{\mu_0}{1+\frac{\mu_0E}{v_{sat}}}E$$  

__EXAMPLE:__  
a uniform piece of semiconductor $0.2 μm$ long sustains a voltage of $1 V$. If the low-field mobility is equal to $1350 cm^2/(V \cdot s)$ and the saturation velocity of the carriers $10^7 cm/s$, determine the effective mobility. also, calculate the maximum allowable voltage such that the effective mobility is only $10\%$ lower than $μ_0$.  

ans:  
$E=V/L=1/(0.2\times10^{-4})=5\times10^4\,V/cm$  
$\mu=\frac{\mu_0}{1+\frac{\mu_0E}{v_{sat}}}=174.1935\,cm^2/(V\cdot s)$  
there's the effective mobility.  

next, the effective mobility is only $10\%$ lower than $μ_0$.  
$\mu=0.9\mu_0=\frac{\mu_0}{1+\frac{\mu_0E}{v_{sat}}}\iff0.9(1+\frac{\mu_0E}{v_{sat}})=1\iff E=\frac{v_{sat}}{9\mu_0}=823.0453\,V/cm$  
and then the voltage to sustain is $V=EL=0.0165=16.5\,mV$  

__diffusion:__  
In addition to drift, another mechanism can lead to current flow.  
it's diffusion, movement of particle from high concentration to low one.  

![](https://i.imgur.com/HIAuOYJ.png)
![](https://i.imgur.com/OcXeJoG.png)

our qualitative study of diffusion suggests that the more nonuniform the concentration, the larger the current. More specifically, we can write: 
\\$$I\propto\frac{dn}{dx}$$

if each carrier has a charge equal to $q$, and the semiconductor has a cross section area of $A$, it can be written as:
\\$$I\propto Aq\frac{dn}{dx}$$

thus, 
\\$$I=AqD_n\frac{dn}{dx}$$

where $D_n$ is a proportionality factor called the “diffusion constant” and expressed in $cm^2/s$.  
For example, in intrinsic silicon, $D_n = 34 cm^2/s$ (for electrons), and $D_p = 12 cm^2/s$ (for holes).  

and thus current density is 
\\$$J_n=qD_n\frac{dn}{dx}$$

and 
\\$$J_p=-qD_p\frac{dp}{dx}$$

(notice that $J_p$ has a negative sign, since the current is passing the same way as hole.)  

with both electron and hole, 
\\$$J_{tot}=q(D_n\frac{dn}{dx}-D_p\frac{dp}{dx})$$

__EXAMPLE:__  
suppose the electron concentration is equal to $N$ at $x=0$ and falls linearly to $0$ at $x=L$.  
![](https://i.imgur.com/KsQ6J2Q.png)  
determine the diffusion current.  

ans:  
$I=AqD_n\frac{dn}{dx}=AqD_n(-\frac{N}{L})$  
and  
$J_n=qD_n(-\frac{N}{L})$  

__Einstein relation:__  
\\$$\frac{D}{\mu}=\frac{kT}{q}$$
note that $kT/q\approx26mV$ at $T=300K$.

### pn junction
![](https://i.imgur.com/NVUe6HX.png)

__pn junction in equilibrium:__
let us first study the pn junction with no external connections, i.e., the terminals are open and no voltage is applied across the device. We say the junction is in “equilibrium.”  
We begin by examining the interface between the n and p sections 
. The sharp concentration gradient for both electrons and holes across the junction leads to two large diffusion currents: electrons flow from the n side to the p side, and holes flow in the opposite direction.  

![](https://i.imgur.com/EAjPhcn.png)  

the diffusion currents transport a great deal of charge from each side to the other, but they must eventually decay to zero.  
there're two reasons why it will stop.  
first, which is minor factor, enough free carriers have moved across the junction so as to equalize the concentrations on the two sides.  
second, which is major factor, for every electron that departs from the n side, a positive ion is left behind.  consequently, the immediate vicinity of the junction is depleted of free carriers and hence called the “depletion region.”  

![](https://i.imgur.com/hFxQhFN.png)  

now recall from basic physics that a particle or object carrying a net (nonzero) charge creates an electric field around it.  
thus, with the formation of the depletion region, an electric field emerges, which is shown below.  

![](https://i.imgur.com/cX9JPbv.png)  

interestingly, the field tends to force positive charge flow from left to right whereas the concentration gradients necessitate the flow of holes from right to left (and electrons from left to right).  
we therefore surmise that the junction reaches _equilibrium_ once the electric field is strong enough to completely stop the diffusion currents.  

from our observation regarding the drift and diffusion currents under equilibrium,
we may be tempted to write: 
\\$$|I_{\text{drift},\,p}+I_{\text{drift},\,n}|=|I_{\text{diff},\,p} + I_{\text{drift},\,n}|$$

and $|I_{\text{drift},\,p}|=|I_{\text{diff},\,p}|$, $|I_{\text{drift},\,n}|=|I_{\text{diff},\,n}|$.  

__built-in potential:__  
the existence of an electric field within the depletion region suggests that the junction may exhibit a “built-in potential.”  
by replacing $|I_{\text{drift},\,p}|$ and $|I_{\text{diff},\,p}|$ as $Aq\mu_pEp$ and $AD_p\frac{dp}{dx}$, we can come out the potential. 
\\$$|I_{\text{drift},\,p}|=|I_{\text{diff},\,p}|\iff Aq\mu_pEp=AD_p\frac{dp}{dx}\iff \mu_pEp=D_p\frac{dp}{dx}$$

since $E=-\frac{dV}{dx}$,
\\$$-\mu_pp\frac{dV}{dx}=D_p\frac{dp}{dx}\iff-\mu_ppdV=D_pdp\iff -\mu_p\int dV=D_p\int\frac{dp}{p}\iff -\mu_pV=D_pln(p)$$

![](https://i.imgur.com/20KKlbM.png)

$V$ is a variable which is dependent on $x$, so as $p$.  
to compute potential, $|V_0|$, 
\\$$|V_0|=|V(x_2)-V(x_1)|=|\frac{D_p}{\mu_p}ln(\frac{p_n}{p_p})|$$

and accroding to Einstein's relation, 
\\$$|V_0|=|\frac{kT}{q}ln(\frac{p_n}{p_p})|=|-\frac{kT}{q}ln(\frac{p_p}{p_n})|=|\frac{kT}{q}ln(\frac{p_p}{p_n})|$$

then 
\\$$V_0=\frac{kT}{q}ln(\frac{p_p}{p_n})=\frac{kT}{q}ln(\frac{N_A}{\frac{n_i^2}{N_D}})=\frac{kT}{q}ln(\frac{N_AN_D}{n_i^2})$$

this equation plays a central role in many semiconductor devices.  

__EXAMPLE:__  
a silicon pn junction employs $N_A = 2 × 10^{16} cm^{−3}$ and $N_D = 4 × 10^{16} cm^{−3}$. determine the built-in potential at room temperature ($T = 300 K$).  

ans:  
note $n_i\approx1.08\times10^{10}$ at $T=300K$  

$\begin{align}V_0&=\frac{kT}{q}ln(\frac{N_AN_D}{n_i^2})\\&=26\times10^{-3}ln(2\times10^{16}\times4\times10^{16}/(1.08\times10^{10})^2)\\&=26\times10^{-3}\times29.5565=0.7685V\end{align}$

__EXAMPLE:__  
equation of potential reveal that $V_0$ is a weak function of the doping levels. how much does $V_0$ change if $N_A$ or $N_D$ is increased by one order of magnitude?  

ans:  
$\Delta V=\frac{kT}{q}ln(\frac{10N_AN_D}{n_i^2})-\frac{kT}{q}ln(\frac{N_AN_D}{n_i^2})=\frac{kT}{q}ln10$  
at $T=300K$, $\Delta V=26\times10^{-3}ln(10)=0.0599V$  

an interesting question may arise at this point. the junction carries no net current (because its terminals remain open), but it sustains a voltage. How is that possible? We observe that the built-in potential is developed to _oppose_ the flow of diffusion currents (and is, in fact, sometimes called the “potential barrier”). This phenomenon is in contrast to the behavior of a uniform conducting material, which exhibits no tendency for diffusion and hence no need to create a built-in voltage.  

### pn junction under reverse bias
![](https://i.imgur.com/4B1g0PQ.png)  

![](https://i.imgur.com/Sr09aba.png)

since under equilibrium, $\vec{E}$ is directed from the n side to the p side, $V_R$ enhances the field. But, a higher electric field can be sustained only if a larger amount of fixed charge is provided, requiring that more acceptor and donor ions be exposed and, therefore, the depletion region be widened.  
since the external voltage has strengthened the field, the barrier rises even higher than that in equilibrium, thus prohibiting the flow of current. in other words, the junction carries a negligible current under reverse bias.  
we note that as $V_R$ increases, more positive charge appears on the n side and more negative charge on the p side.  
Thus, the device operates as a _capacitor_. In essence, we can view the conductive n and p sections as the two plates of the capacitor.  

but there's a spark difference from a common capacitor. the capacitor is voltage-dependent.  
since as $V_R$ increase, the capacitance of the structure _decreases_ as the two plates move away from each other.  

it can be proved that the capacitance of the junction per unit area is equal to 
\\$$C_j=\frac{C_{j0}}{\sqrt{1-\frac{V_R}{V_0}}}$$

where $C_{j0}$ denotes the capacitance corresponding to zero bias ($V_R = 0$) and $V_0$ is the builtin potential. (this equation assume $V_R$ shall be negative when reverse bias.)  

the value of 
\\$$C_{j0} = \sqrt{\frac{\epsilon_{si}q}{2}\frac{N_AN_D}{N_A+N_D}\frac1{V_0}}$$

where $\epsilon_{si}$ represents the dielectric constant of silicon and is equal to $11.7\times8.85\times10^{-14}F/cm$.  

![](https://i.imgur.com/SNrmyop.png)

__EXAMPLE:__  
a pn junction is doped with $N_A = 2 × 10^{16} cm^{−3}$ and $N_D = 9 × 10^{15} cm^{−3}$. Determine the capacitance of the device with $V_R = 0V$ and $V_R = 1 V$.  

ans:  
$V_0=\frac{kT}{q}ln(\frac{N_AN_D}{n_i^2})=26\times10^{-3}\times28.0649=0.7297V$  

for $V_R=0$, $C_{j0}=2.65\times10^{-8}F/cm^2=0.265\text{ fF/}\mu\text{m}^2$  

for $V_R=1V$, $C_j=\frac{C_{j0}}{\sqrt{1-\frac{-V_R}{V_0}}}=0.172\text{ fF/}\mu\text{m}^2$  

the variation of the capacitance with the applied voltage makes the device a “nonlinear” capacitor because it does not satisfy $Q = CV$.  
nonetheless, as demonstrated by the following example, a voltage-dependent capacitor leads to interesting circuit topologies.  

__EXAMPLE:__  
a cellphone incorporates a 2-GHz oscillator whose frequency is defined by the resonance frequency of an LC tank (shown as below).  
if the tank capacitance is realized as the pn junction of last example, calculate the change in the oscillation frequency while the reverse voltage goes from $0$ to $2 V$.  
assume the circuit operates at 2 GHz at a reverse voltage of $0 V$, and the junction area is $2000 μm^2$.  

![](https://i.imgur.com/IMsk1hw.png)

ans:  
from the property of [LC circuit](https://en.wikipedia.org/wiki/LC_circuit), we know $T=2\pi\sqrt{LC}$, so $f=\frac{1}{2\pi\sqrt{LC}}$  

$V_0=0.7297V$, $C_{j0}=2.65\times10^{-8}F/cm^2=0.265\text{ fF/}\mu\text{m}^2$  
the total capacitance is $C_{j,tot}=C_{j0}\times2000=530\text{ fF}$  
so at $V_R=0$, $f=\frac{1}{2\pi\sqrt{LC_{j,tot}}}=2\times10^{9}Hz$, then $L=11.9\times10^{-9}H$

$C_j=\frac{0.265}{\sqrt{1-\frac{-2}{0.7297}}}=0.1370\text{ fF/}\mu\text{m}^2$, $C_{j,tot}=0.1370\times2000=274\text{fF}$  

so at $V_R=2V$, $f=\frac{1}{2\pi\sqrt{LC_{j,tot}}}=2.79GHz$  

an oscillator whose frequency can be varied by an external voltage (VR in this case) is called a “voltage-controlled oscillator” and used extensively in cellphones, microprocessors, personal computers, etc.  

### pn junction under forward bias
![](https://i.imgur.com/VYFjItz.png)

in forward bias, the external voltage, $V_F$, tends to create a field directed from the p side toward the n side—opposite to the built-in field that was developed to stop the diffusion currents. we therefore surmise that $V_F$ in fact lowers the potential barrier by weakening the field, thus allowing greater diffusion currents.  

To derive the I/V characteristic in forward bias, we begin with $V_0=\frac{kT}{q}ln(\frac{p_p}{p_n})$, rewrite it as 
\\$$p_{n,e}=\frac{p_{p,e}}{e^{\frac{V_0}{V_T}}}$$

where the subscript $e$ emphasizes equilibrium conditions and $V_T = kT/q$ is called the “thermal voltage” ($\approx26 mV$ at $T = 300 K$).  
In forward bias, the potential barrier is lowered by an amount equal to the applied voltage: 
\\$$p_{n,f}=\frac{p_{p,f}}{e^{\frac{V_0-V_F}{V_T}}}$$

where the subscript $f$ denotes forward bias.  
we expect $p_{n,f}$ to be much higher than $p_{n,e}$. (it can be proved that $p_{p,f} ≈ p_{p,e} ≈ N_A$)  
in other words, the _minority_ carrier concentration on the p side rises rapidly with the forward bias voltage while the majority carrier concentration remains relatively constant. This statement applies to the n side as well.  

![](https://i.imgur.com/Po1Ewao.png)

as the junction goes from equilibrium to forward bias, $n_p$ and $p_n$ increase dramatically, leading to a proportional change in the diffusion currents.  
We can express the change in the hole concentration on the n side as: 
\\$$\begin{align}\Delta p_n&=p_{n,f}-p_{n,e}\\&=\frac{p_{p,f}}{e^{(V_0-V_F)/V_T}}-\frac{p_{p,e}}{e^{V_0/V_T}}\\&\approx\frac{N_A}{e^{V_0/V_T}}(e^{V_F/V_T}-1)\end{align}$$

similarly, for the electron concentration on the p side: 
\\$$\Delta n_p\approx\frac{N_D}{e^{V_0/V_T}}(e^{V_F/V_T}-1)$$

note that $e^{V_0/V_T}=N_AN_D/n_i^2$.  
the increase in the minority carrier concentration suggests that the diffusion currents must rise by a proportional amount above their equilibrium value, i.e., 
\\$$I_{tot}\propto\Delta p_n+\Delta n_p$$

it can be proved that 
\\$$I_{tot}=I_s(e^{V_F/V_T}-1)$$

where $I_S=Aqn^2_i(\frac{D_n}{N_AL_n}+\frac{D_p}{N_DL_p})$, which is called the "reverse saturation current".  
in this equation, $A$ is the cross section area of the device, and $L_n$ and $L_p$ are electron and hole “diffusion lengths,” respectively.  

__EXAMPLE:__  
determine $I_S$ for the junction whose $N_A=2\times10^{16}cm^{-3}$ and $N_D=4\times10^{16}cm^{-3}$ at $T = 300K$ if $A = 100 μm^2$, $L_n = 20 μm$,
and $L_p = 30 μm$.  

ans:  
$n_i$(at $T=300K$) is $1.08\times 10^{10}cm^{-3}$.  
thus, $I_S=1.77\times10^{-17}A$  

since $I_S$ is extremely small, the exponential term in $I_{tot}$ must assume very large values so as to yield a useful amount (e.g., 1 mA) for $I_{tot}$.  

### I/V characteristic  
\\$$I_D=I_S(e^{V_D/V_T}-1)$$

and 
\\$$I_D\propto A$$

where $I_D$ and $V_D$ is the diode current and voltage, and $A$ is cross section area.  

![](https://i.imgur.com/u4RUHyL.png)

the figure above plot the overall I/V characteristic of the junction, revealing why $I_S$ is called the “reverse saturation current.”.  
note that the current under reverse bias is independent on voltage imposing on. since the current is caused by that the minority carriers cross the pn junction, $I_S$ is dependent on the doping level.  
the last example reveals that $I_S$ is typically very small. we therefore view the current under reverse bias as “leakage.”  
note that $I_S$ and hence the junction current are proportional to the device cross section area. the figure below represents this phenomenon.  
![](https://i.imgur.com/spbFzDl.png)

__EXAMPLE:__  
each junction in the figure above employs the doping levels, $N_A=2\times10^{16}cm^{-3}$ and $N_D=4\times10^{16}cm^{-3}$. Determine the forward bias current of the composite device for $V_D = 300 mV$ and $800 mV$ at $T = 300 K$.  

ans:  
$I_S=1.77\times10^{-17}A$.  
at $V_D=300mV$, $I_{D,tot}=2I_S(e^{V_D/V_T}-1)=1.77\times10^{-17}(e^{300/26}-1)\times2=3.6315 \text{pA}$  

at $V_D=800mV$, $I_{D,tot}=2I_S(e^{V_D/V_T}-1)=1.77\times10^{-17}(e^{800/26}-1)\times2=820\mu A$  

__EXAMPLE:__  
a diode operates in the forward bias region with a typical current level (i.e., $I_D ≈ I_S\cdot e^{V_D/V_T}$). suppose we wish to increase the current by a factor of 10. How much change in $V_D$ is required?  

ans:  
$e^{(V_D'-V_D)/V_T}=10\iff \Delta V_D=V_D'-V_D=V_T\cdot ln(10)=0.0596$  

__EXAMPLE:__  
the cross section area of a diode operating in the forward bias region is increased by a factor of 10.  
(a) determine the change in $I_D$ if $V_D$ is maintained constant.  
(b) determine the change in $V_D$ if $I_D$ is maintained constant.  
Assume $I_D ≈ I_S e^{V_D/V_T}$.  

ans:  
(a) $I_{D1}=10I_D$  
(b) $I_D=10I_Se^{V_{D1}/V_T}\iff V_{D1}=V_T\cdot ln(I_D/(10I_S))=V_T\cdot ln(I_D/I_S)-V_T\cdot ln(10)$  

__constant-voltage model:__  
the exponential I/V characteristic of the diode results in nonlinear equations, making the analysis of circuits quite difficult.  
fortunately, the above examples imply that the diode voltage is a relatively weak function of the device current and cross section area.  
with typical current levels and areas, $V_D$ falls in the range of $[700,800] mV$. For this reason, we often approximate the forward bias voltage by a constant value of
$800 mV$ (like an ideal battery), considering the device fully off if $V_D < 800 mV$.  

![](https://i.imgur.com/Ks8Bufl.png)

note that the current goes to infinity (actually is equal to $I_D$) as $V_D$ tends to exceed $V_{D,\,on}$ because we assume the forward-biased diode operates as an ideal voltage source.  
neglecting the leakage current in reverse
bias, we derive the circuit model shown above.  

__EXAMPLE:__  
consider the circuit below. calculate $I_X$ for $V_X = 3 V$ and $V_X = 1 V$ using  
(a) an exponential model with $I_S = 10^{−16} A$  
(b) a constant-voltage model with
$V_{D,on} = 800 mV$.  

![](https://i.imgur.com/cpwzOyS.png)

ans:  
(a)  
$I_D=I_X=I_S(e^{V_D/V_T}-1)\iff V_D=V_T\cdot ln(\frac{I_X}{I_S}+1)$  
by KVL, $V_X-I_X\times1k-V_D=0$.  

we can use iteration to solve these equations. guess a value for $V_D$, compute $I_X=(V_D-V_X)/1k$ then determine the new $V_D$ from $V_D=V_T\cdot ln(\frac{I_X}{I_S}+1)$. (the reason why is a complex problem.)  

the figure of iteration:  
![](https://i.imgur.com/zB8HC3W.png)

another sol. :  
$V_X-I_X-V_T\cdot ln(\frac{I_X}{I_S}+1)=0$  
we can use binary search, since the plot of the equation above is like,  
![](https://i.imgur.com/VFxj3rv.png)

hence, $I_X=2.201mA$ and $V_D=0.7994=799.4mV$ (when $V_X=3V$)

following the same procedure for $V_X=1V$, $I_X=0.258mA$ and $V_D=0.742V$.  

(b)  
since $V_X>V_{D,on}$, the circuit can be turn into a equivalent circuit, by replacing the diodo with a ideal voltage source.  
then, solve the equation, $V_X-I_X\cdot1k-V_D=0$  
for $V_X=3$, $I_X=2.2mA$.  
for $V_X=1$, $I_X=0.2mA$.  

## reverse breakdown
![](https://i.imgur.com/SwOuC95.png)

### zener breakdown
the depletion region in a pn junction contains atoms that have lost an electron or a hole and, therefore, provide no loosely-connected carriers. however, a high electric field in this region may impart enough energy to the remaining covalent electrons to tear them from their bonds.  
once freed, the electrons are accelerated by the field and swept to the n side of the junction. This effect occurs at a field strength of about $106 V/cm (1 V/μm)$.  
![](https://i.imgur.com/oDw5h4q.png)

in order to create such high fields with reasonable voltages, a narrow depletion region is required, which $V_0=\frac{kT}{q}ln(\frac{N_AN_D}{n_i^2})$ implies high doping levels on both sides of the junction. Called the “Zener effect,” this type of breakdown appears for reverse bias voltages on the order of $[3,8] V$.  

### avalanche breakdown
junctions with moderate or low doping levels ($<10^{15} cm^3$) generally exhibit no Zener
breakdown.  
but, as the reverse bias voltage across such devices increases, an avalanche effect takes place. even though the leakage current is very small, each carrier entering the depletion region experiences a very high electric field and hence a large acceleration, thus gaining enough energy to break the electrons from their covalent bonds. called “impact ionization,” this phenomenon can lead to avalanche:  
each electron freed by the impact may itself speed up so much in the field as to collide with another atom with sufficient energy, thereby freeing one more covalent-bond electron.  
