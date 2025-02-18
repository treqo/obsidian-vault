**Type:** [[course]]  
**Topics:** #electrical-engineering  
**Date:** 2024-08-30  
**Source/Author:** Kuang-Wei Cheng kwcheng@ece.ubc.ca
- **Tags**
	- 

---
# Notes

## Section 1 – Sinusoids and Phasors
*mon, sept 9, 2024 - wed, sept 11, 2024*

```ad-summary
title: General Sinusoid
Consider a general expression sinusoidal voltage
$$
\displaystyle
v(t)=V_{m}\sin(\omega t + \phi)
$$
- $V_m$ : amplitude of the sinusoid
- $\omega$ : angular frequency
- $\phi$ : phase of the sinusoid
The *period* of a sinusoid is given as 
$$
\displaystyle
T = \frac{2\pi}{\omega} = \frac{1}{f}
$$
```

```ad-tip
title: Lagging vs Leading
Consider two sinusoids of the same period, $v_1(t)=V_{m}\sin(\omega t + \phi_1)$ and $v_2(t)=V_{m}\sin(\omega t + \phi_2)$, such that $v_1$ has a *phase* of $\phi_1$ and $v_2$ has a phase $\phi_2$, where $\phi_{1} \neq \phi_{2}$.

Supposing $\phi \in [180 \degree, -180 \degree)$, then
- The sinusoids are **in phase** for $\Delta \phi = \phi_{2}-\phi_{1} =0$
- The sinusoids are **out of phase** for $\Delta \phi \neq 0$. 

Additionally, for out of phase sinusoids, if $\phi_{2} > \phi_{1}$, then we can say that $v_2$ **leads** $v_1$ or that $v_1$ **lags** $v_2$.

For positive $\omega$, a positive phase results in an *advance* or *left-shift*, whereas a negative phase results in an *delay* or *right-shift* of the sinusoid.

![[Screenshot 2024-09-15 at 10.11.20 AM.png|300]]
*graphical representation of leading vs lagging sinusoids*

![[Pasted image 20240915102858.png|300]]
*phasor diagram representation, leading vs lagging*
```

```ad-summary
title: Sinusoids and Trigonometric Identities
Through trigonometric identities and manipulations, we can interpret any sinusoid with frequency $\omega$ and phase $\phi$, as a sum of scaled $\sin$ and $\cos$ sinusoids of the same frequency, with phases $\phi = 0$.

$$
\displaystyle
\begin{aligned}
&A\cos(\omega t) + B \sin(\omega t) = C \cos(\omega t + \phi) \\ \\
&\text{where} \qquad C = \sqrt{A^{2}+B^{2}}, \quad \phi = \tan^{-1}{\frac{B}{A}}
\end{aligned}
$$

![[Pasted image 20240915101358.png|300]]
*Phasor diagram with sinusoids*
```

```ad-summary
title: Phasors
Phasor representation is based on *Eurler's identity*. It is used to represent (visually and mathematically), the manitude and phase of complex sinusoids of the same frequency.
$$
\displaystyle
\begin{aligned}
&e^{\pm j \phi} = \cos(\phi) \pm j \sin(\phi) \\ \\
&\implies \cos(\phi) = \Re(e^{j \phi}), \quad \sin(\phi) = \Im(e^{j \phi})
\end{aligned}
$$
In phasor notation, a sinusoidal signal such as $A \cos(wt + \phi)$ is represented as a rotating vector (phasor), with magnitude $A$ and phase $\phi$.

$$
A \cos(wt + \phi) \leftrightarrow A \ \angle \phi
$$

![[Pasted image 20240915102955.png|300]]

A phasor may be regarded as a mathematical equivalent to a sinusoid with the time-dependence dropped. To transform a sinusoid from time-domain to phasor-domain, convert the sinusoid into euler's form and then drop the $e^{j \omega t}$ term.

$$
\displaystyle
\begin{aligned}
V(t) = V_{m}cos(\omega t + \phi) &= \Re(V_{m}e^{j(\omega t + \phi)}) \\ \\
&= \Re(V_{m}e^{j \omega t} e^{j \phi}) \\ \\
&\implies \Re(\mathbf{V} e^{j \omega t}) \\ \\ \\

\text{where }\mathbf{V}\text{ is the phasor} \qquad &\mathbf{V} = V_m e^{j\phi} = V_{m} \ \angle \phi
\end{aligned}
$$
```
## For final

- [ ] leading and lagging

## Section 2 – Sinusoidal Steady-State Analysis
*mon, sept 16, 2024 - wed, sept 18, 2024*

```ad-tip
title: Cramer's Rule

```

## Chapter 11 – AC Power Analysis

```ad-summary
title: Instantaneous Power
*Instantaneous Power* is the power at any moment of time, $t$, and is given by the product of the instantaneous voltage, $v(t)$, and instantaneous current $i(t)$.

$$
\displaystyle
\begin{aligned}
&\text{Power in the time domain} \qquad p(t) = v(t) \ i(t) \\ \\
&v(t) = V_{m} \cos(\omega t + \theta_{v}) \quad i(t) = I_{m}\cos(\omega t + \theta_{i}) \\ \\

&\text{With some math manipulation} \\ 
&p(t) = \frac{1}{2}V_{m}I_{m}\cos(\theta_{v}-\theta_{i})[1+\cos(2 \omega t + 2 \theta_{v})] + \frac{1}{2}V_{m}I_{m}\sin(\theta_{v}-\theta_{i})[\sin(2 \omega t + 2 \theta_{v})] \\
\end{aligned}
$$

- $V_m$ and $I_m$ are the magnitudes of the voltage and current signal respectively.
- $\theta_{v}-\theta_{i}$ is the phase difference between the voltage and current signal.
- The average power $P$ is therefore $\frac{1}{2}V_{m}I_{m}\cos(\theta_{v}-\theta_{i})$

![[Screenshot 2024-10-08 at 5.57.52 PM.png]]
```

```ad-summary
title: Average Power (Phasor Domain)
We have that $V=V_{m}\angle \theta_{v}$ and $I=I_{m}\angle \theta_{i}$. Then the **Complex Power** is

$$

$$
```


![[Pasted image 20241010215406.png]]

$$
\displaystyle
\begin{aligned}
&R_1=0.3 \\ &R_2=0.5  \\ &L_1=0.11 j \\ &L_2=0.23 j \\ &P_{1}=23 \ W \\ &P_{2}=15 \ W \\ &pf1=0.55 \\ & pf2 = 0.4 \\ &v_{0}=110 \ V_{rms}
\end{aligned}
$$

$$
\begin{aligned}
&V_{1}= V_{0}+ I_{2_{rms}}(R2+L2) \\
&pf2= \frac{P2}{|S_{2|}} \\
& |S_{2}|=\frac{P2}{pf2} \\
&S_{2}= P2 + j |S_{2}| sin(\theta_{v} - \theta_{i}) \\ \\
& \theta_{v}-\theta_{i} = \cos^{-1}(pf2) \\ \\
&\frac{S_{2}}{V_{rms}} = I_{2_{rms}}^{*} \\
&V_{1}= V_{0}+ I_{2_{rms}}(R2+L2) \\
\end{aligned}
$$

$$
\begin{aligned}
&I = I_{2_{rms}} + I_{1_{rms}} \\
&pf1= \frac{P1}{|S_{1}|} \\
& |S_{1}|=\frac{P1}{pf1} \\
&S_{1}= P1 + j |S_{1}| sin(\theta_{v1} - \theta_{i2}) \\ \\
& \theta_{v1}-\theta_{i2} = \cos^{-1}(pf1) \\ \\
&\frac{S_{1}}{V_1} = I_{1_{rms}}^{*} \\
&V_{s}= V_{1}+ I(R1+L1) \\
\end{aligned}
$$

---
