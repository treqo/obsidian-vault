**Type:** [[textbook]]  
**Topics:** #electrical-engineering #signals #systems
**Date:** 2024-07-16  
**Source/Author:** Luis F. Chaparro, Aydin Akan
- **Tags**
	- 

---
# Notes

## Chapter 0 – From the Ground Up

> In theory, there is no difference between theory and practice. In practice, there is.

```ad-quote
title: $0.1$ – Introduction – pg $4$
The use of *digital signal processors* (DSPs) and of *field-programmable gate arrays* (FPGAs) have been **replacing** the use of *application-specific integrated circuits* (ASICs) in industrial, medical and military applications.

The abundance of *algorithms* for *processing digital signals*, and the pervasive presence of DSPs and FPGAs in thousands of applications make *digital signal processing theory* a necessary tool not only for engineers but for anybody who would be dealing with digital data
```
### Examples of Signal Processing

```ad-quote
title: $0.2.1$ – Compact-Disc Player – pg. 5
![[Screenshot 2024-07-16 at 10.41.20 AM.png]]

When playing a CD, the CD-player *follows the tracks in the disc*, focusing a laser on them, as the CD is spun. The laser shines a light which is *reflected* by the *pits* and *bumps* put on the surface of the disc and corresponding to the *coded digital signal* from an acoustic signal. A sensor detects the *reflected light* and converts it into a digital signal, which is then converted into an analog signal by the digital-to-analog converter (DAC). When amplified and fed to the speakers such a signal sounds like the originally recorded acoustic signal.
```

```ad-summary
title: Compact-Disc (CD) Player
The CD Player is a very interesting *control system*. The player must
1. rotate the disc at different speeds depending on the location of the track within the CD being read
2. focus a laser and a lens system to read the pits and bumps (which is encoded as 0's and 1's) on the disc.
3. move the laser to follow the track being read.
```

```ad-check
title: Connection and Understanding
The CD player reminds me of a more advanced version of the classic, *vinyl player*, probably because that's exactly what it is. The vinyl player rotates a disc as well, but reads with a *stylus* – the needle –  instead of a *laser*. It is also much larger, the disc is more compact and must be able to hold more information, as it can even encode movies (that's audio and visual data)! 

![[Pasted image 20240716113304.png|200]]
```

```ad-quote
title: SOFTWARE-DEFINED RADIO (SDR) AND COGNITIVE RADIO (CR)
![[Screenshot 2024-07-16 at 11.39.29 AM.png]]

Schematics of a voice *SDR mobile two-way radio*. *Transmitter*: the voice signal is inputted by means of a microphone, amplified by an audio amplifier, converted into a digital signal by an analog-to-digital converter (ADC) and then modulated using software, before being converted into analog by a digital-to-analog converter (DAC), amplified and sent as a radio frequency signal via an antenna. Receiver: the signal received by the antenna is processed by a superheterodyne front-end, converted into a digital signal by an ADC before being demodulated and converted into an analog signal by a DAC, amplified and fed to a speaker. The modulator and demodulator blocks indicate software processing.
```

```ad-quote
title: Control Systems
Typically, control systems are feedback systems where the dynamic response of a system is changed to make it follow a desirable behavior.
```

```ad-quote
title: Computer-Control System
![[Screenshot 2024-07-16 at 11.44.26 AM.png]]

Computer-control system for an analog plant (e.g., cruise control for a car). The reference signal is r(t) (e.g., desired speed) and the output is y(t) (e.g., car speed). The analog signals are converted to digital signals by an analog-to-digital converter (ADC), while the digital signal from the computer is converted into an analog signal (an actuator is probably needed to control the car) by a digital-to-analog converter (DAC). The signals v(t) and w(t) are disturbances or noise in the plant and the sensor (e.g., electronic noise in the sensor and undesirable vibration in the car).
```

```ad-summary
title: Implementation of Digital Signal Processing Algorithms
Usually developed in a high-level programming language, like MATLAB, C/C++, then after testing, simulations, and debugging, implemented on hardware.

*hardware implementation options*
- General-purpose microprocessors (μPs) and micro-controllers (μCs).
- General-purpose digital signal processors (DSPs).
- Field-programmable gate arrays (FPGAs).

**MICROPROCESSORS and MICRO-CONTROLLERS**
- **General-purpose microprocessors** are capable of handling many digital signal processing applications but are often not optimized for complex operations like multiplication and division, making them less efficient and cost-effective.
- **Micro-controllers** are application-specific micro-computers with built-in components such as CPU, memory, and I/O ports, making them suitable for various consumer and industrial applications due to their small size, low cost, and integrated features.
- The **Arduino platform** is a popular, user-friendly micro-controller platform for digital signal processing, known for its powerful arithmetic logic unit, ease of use, and open-source environment.

**DIGITAL SIGNAL PROCESSORS**
- **Digital Signal Processors (DSPs)** are special-purpose microprocessors optimized for digital signal processing algorithms, widely used in consumer electronics like mobile phones, HDTVs, and digital cameras due to their cost-effectiveness and flexibility.
- **DSP Software Development** is facilitated by specialized tools, enabling easy reprogramming for upgrades or bug fixes, with features like a project build environment, code editor, C/C++ compiler, debugger, profiler, simulator, and real-time operating system.

**FIELD PROGRAMMABLE GATE ARRAYS**
- **FPGAs** are programmable devices consisting of fields of small logic blocks (typically NAND gates) that can be configured to implement digital signal processing algorithms.
- The **granularity** of FPGAs, which affects the complexity of wiring between blocks, can be categorized into three classes: fine granularity (sea of gates), medium granularity, and large granularity (Complex Programmable Logic Devices).
- FPGAs are **reprogrammable** using various memory technologies, offering short programming times and protection against unauthorized use, making them ideal for high-bandwidth signal processing applications like wireless, multimedia, and satellite communications due to their efficiency and flexibility.
```

```ad-summary
title: Continuous vs Discrete Summary
In studying signals and systems theory, we will deal with both **continuous** and **discrete** representations, which requires different approaches to solving.

- **Calculus**: Deals with continuously changing variables, using derivatives and integrals to measure rates of change and areas under curves.
    
    - **Continuous-time signals**: Measured as functions of time with amplitudes in terms of voltage, current, etc. These need to be converted into binary sequences for computer processing.
    - **Analog-to-Digital Converter (ADC)**: Converts continuous-time signals into discrete-time signals (sequences of samples), each sample represented by a binary string.
    - **Digital-to-Analog Converter (DAC)**: Converts digital signals back into continuous-time signals.
- **Finite Calculus**: Deals with sequences, replacing derivatives and integrals with differences and summations, and differential equations with difference equations.
    
    - **Discrete-time signals**: Naturally represented as sequences, suitable for processing by computers using Finite Calculus.
- **Numerical Methods**: Used in digital computers to approximate differentiation, integration, and solve differential equations through discretization.
    
- **Importance of Conversion**: Ensuring minimal information loss during the conversion between continuous and discrete forms is crucial, and understanding the working of ADCs and DACs is essential.
```

```ad-quote
title: Notation and Definitions
*Discrete-time* signals are a sequence of measurements, usually made at equal time intervals (the *sampling period* $T_s$). *Continuous-time* signals depends continuously on time.
$$
\begin{aligned}
\displaystyle x[n] \ &= \ x(nT_s) \ = \ x(t)|_{t=nT_s} \\ \\
\text{resulting sequence for $x[n]$ } \quad &\{\ ... \ \ x(-T_s) \ \ x(0) \ \ x(T_s)  \ \ ... \} \\ \\
\text{equivalently } \quad &\{\ ... \ \ x[-1] \ \ x[0] \ \ x[1]  \ \ ... \}
\end{aligned}
$$
*This process is called sampling or discretization of a continuous-time signal $x(t)$*

Differentiation is an operation that is approximated in Finite Calculus. The derivative operator

$$
D[x(t)] = \frac{dx(t)}{dt} = \lim_{h \to 0} \frac{x(t + h) - x(t)}{h}
$$

measures the rate of change of a signal \( x(t) \). In Finite Calculus, the forward finite-difference operator

$$
\begin{aligned}
\Delta[x(nT_s)] &= x((n + 1)T_s) - x(nT_s) \\ \\
\Delta[x[n]] &= x[n+1] - x[n]
\end{aligned}
$$

is used as an approximation.

```

```ad-summary
title: Differential and Difference Equations
*Connection Between Circuit Diagram, Mathematical Equation, and Block Diagram Representation*

In the study of differential and difference equations, it's crucial to understand the relationship between physical systems, their mathematical models, and their block diagram representations. Let's explore this connection using an example of a simple RC circuit.

#### Circuit Diagram

![[Screenshot 2024-07-16 at 12.26.44 PM.png|400]]

- The circuit consists of a resistor $R = 1 \Omega$ and a capacitor $C = 1 \text{F}$ in *series* with an input voltage source $v_i(t)$.
- **Initial Condition**: The capacitor has an initial voltage $v_c(0)$.

#### Mathematical Model

**Differential Equation**
$$
\begin{aligned}
v_i(t) \ &= \ v_c(t) \ + \ \frac{dv_c(t)}{dt}, \quad t \geq 0 \\ \\
v_c(0) \  &= \ v_{c_0}
\end{aligned}
$$
- This equation characterizes the dynamics of the RC circuit, relating the input voltage to the capacitor voltage.

#### Block Diagram

![[Screenshot 2024-07-16 at 12.36.03 PM.png]]

**Using Differentiators**

The block diagram on the left side of the figure shows the realization of the first-order ordinary differential equation using differentiators. This approach is analytically correct but prone to noise, as differentiation can amplify noise present in the signals.

**Using Integrators**

The block diagram on the right side of the figure illustrates the realization using integrators. Integrators smooth out the signal, making the system less sensitive to noise. The integration also allows the inclusion of the initial condition.

$$
v_c(t) = \int_{0}^{t} [v_i(\tau) - v_c(\tau)] d\tau + v_c(0) \quad t \geq 0
$$
*This is more practical than using differentiators because of noise, spikes which result in large derivatives.*
```

```ad-summary
title: Complex Variables for Signal Processing and Systems Theory

In signal processing and systems theory, complex variables play a crucial role despite signals being functions of real variables like time or space. Here are the key points:

- **Complex Variables in Signal Representation**:
    
    - Continuous-time signals: Represented using the *Laplace transform* with the complex variable $s=σ+jω$, where $\sigma$ is the *damping factor* and $\omega$ is the frequency.
    - Discrete-time signals: Represented using the *Z-transform* with the complex variable $re^{j\omega}$, where $r$ is the *damping factor* and $ω$ is the *discrete frequency*.
- **Linear System Response**:
    
    - Linear systems' response to sinusoids involves complex variables, which are fundamental in the analysis and synthesis of signals and systems.
- **Connection to Vectors and Phasors**:
    
    - Complex variables are related to vectors and phasors, commonly used in the sinusoidal steady-state analysis of linear circuits.

Understanding complex variables and their functions is essential for analyzing and processing both continuous and discrete signals in engineering.
```

```ad-quote
title:Trigonometric identities
All trigonometric identities can be obtained using *Euler’s identity*. For instance,
$$
\begin{align*} \sin(-\theta) &= \frac{e^{-j\theta} - e^{j\theta}}{2j} = -\sin(\theta), \\ \\ \cos(\pi + \theta) &= e^{j\pi} \frac{e^{j\theta} + e^{-j\theta}}{2} = -\cos(\theta), \\ \\ \cos^2(\theta) &= \left( \frac{e^{j\theta} + e^{-j\theta}}{2} \right)^2 = \frac{1}{4} \left[ 2 + e^{j2\theta} + e^{-j2\theta} \right] = \frac{1}{2} + \frac{1}{2} \cos(2\theta), \\ \\ \sin(\theta) \cos(\theta) &= \frac{e^{j\theta} - e^{-j\theta}}{2j} \cdot \frac{e^{j\theta} + e^{-j\theta}}{2} = \frac{e^{j2\theta} - e^{-j2\theta}}{4j} = \frac{1}{2} \sin(2\theta). \end{align*}
$$
```

### MATLAB

Resources for MATLAB will be in a new document linked here: [[Learning MATLAB]]


## Chapter 1 – Continuous-time Signals

```ad-abstract
title: Classification of time-dependent Signals
1. Predictability of signals as *random* or *deterministic*
2. Discrete vs continuous in *time* or *amplitude*.
3. Repetitive as *periodic* or *aperiodic*
4. Their energy content as *finite energy* or *finite power* or *infinite energy*, *infinite power*.
5. Their symmetry w.r.t the *time-origin* as *even* or *odd* or *neither*
6. dimensions of their *support*, as *finite* or *infinite* support. Support as in a finite interval where the signal is non-zero. If the signal is always non-zero then it is *infinite* support.
```

```ad-quote
title: Time Windowing
Time windowing is an operation where a signal $x(t)$ is multiplied by a window function $p(t)$ to create a finite-support signal $x(t)p(t)$. The window function is typically designed to be *unity* within a specific range and *zero* outside that range. This operation provides information on $x(t)$ only within the range where $p(t)$ is unity, effectively ignoring the rest of the signal outside the support of $p(t)$.

In short, time windowing is the product of a unity *finite-support signal* with an *infinite-support signal*.
```

```ad-abstract
title: Even and Odd Signals

Even and odd signals are defined as follows:
$$
\begin{aligned}
x(t) \text{ even}: \quad x(t) = x(-t) \\ \\
x(t) \text{ odd}: \quad x(t) = -x(-t)
\end{aligned}
$$
Any signal $y(t)$ is representable as a sum of an even component $y_e(t)$ and an odd component $y_o(t)$
$$
\begin{aligned}
y(t) &= y_e(t) + y_o(t) \\ \\
y_e(t) &= 0.5 \left[ y(t) + y(-t) \right] \\
y_o(t) &= 0.5 \left[ y(t) - y(-t) \right]
\end{aligned}
$$
```

```ad-abstract
title: Periodic Signals
Consider periodic signals $x(t)$ with period $T_0$ and $v(t)$ with period $T_1=NT_0$. Then it is true that for the expression
$$
z(t)=\alpha x(t) \pm \beta v(t) \quad 
$$
The *fundamental period* of $z(t)$ will be $T_1$.

---

Consider periodic signals $x(t)$ with period $T_0$ and $v(t)$ with period $T_1$. Then it is true that for the expression
$$
z(t)=\alpha x(t) \pm \beta v(t) \quad 
$$
The condition for $z(t)$ to be periodic is as follows
$$
\frac{T_1}{T_0}= \frac{N}{M}
$$
Where $N$ and $M$ are *positive integers, not divisible by each other*, then the fundamental period of $z(t)$ is $MT_1=NT_0=T$
```

```ad-note
*Instantaneous power* is given by
$$
p(t)=v(t)i(t)
$$
```

```ad-summary
title: Energy and Power of Continuous-Time Signals
The energy and the power of a continuous-time signal $x(t)$ are defined for either finite- or infinite-support signals by

$$
E_x = \int_{-\infty}^{\infty} |x(t)|^2 dt, \quad P_x = \lim_{T \to \infty} \frac{1}{2T} \int_{-T}^{T} |x(t)|^2 dt. \quad (1.8)
$$

A signal $x(t)$ is then said to be finite-energy, or square integrable, whenever

$$
E_x < \infty. \quad (1.9)
$$

A signal $x(t)$ is said to be finite-power if

$$
P_x < \infty. \quad (1.10)
$$

### Remarks

1. The above definitions of energy and power are valid for any signal of finite or infinite support, since a finite-support signal is zero outside its support.
2. In the formulas for energy and power, we are considering the possibility that the signal might be complex, and so we are squaring its magnitude. If the signal being considered is real, this simply is equivalent to squaring the signal.
3. According to the above definitions, a finite-energy signal has zero power. Indeed, if the energy of the signal is some constant $E_x < \infty$ then

$$
P_x = \left[ \lim_{T \to \infty} \frac{1}{2T} \right] \left[ \int_{-\infty}^{\infty} |x(t)|^2 dt \right] = \lim_{T \to \infty} \frac{E_x}{2T} = 0.
$$

4. A signal $x(t)$ is said to be absolutely integrable if $x(t)$ satisfies the condition

$$
\int_{-\infty}^{\infty} |x(t)| dt < \infty. \quad (1.11)
$$

### Power of Periodic Signals

The power of a periodic signal $x(t)$ of fundamental period $T_0$ is

$$
P_x = \frac{1}{T_0} \int_{t_0}^{t_0 + T_0} x^2(t) dt \quad (1.12)
$$

for any value of $t_0$, i.e., the average energy in a period of the signal.
```

```ad-note
title: Power of a Sum of Sinusoids
The power of a sum of sinusoids,

$$
x(t) = \sum_k A_k \cos(\Omega_k t) = \sum_k x_k(t) \quad (1.13)
$$

with harmonically or non-harmonically related frequencies $\{\Omega_k\}$, is the sum of the power of each of the sinusoidal components,

$$
P_x = \sum_k P_{x_k} \quad (1.14)
$$
```

```ad-note
title: Generic Representation of Signals
By the sifting property of the impulse function $\delta(t)$, any signal $x(t)$ can be represented by the following generic representation:

$$
x(t) = \int_{-\infty}^{\infty} x(\tau) \delta(t - \tau) d\tau. \quad (1.32)
$$
![[Screenshot 2024-07-16 at 4.24.49 PM.png]]
```

## Chapter 2 – Continuous-Time Systems

## Chapter 5 – Frequency Analysis: Fourier Transform

We can extend the Fourier Series representation of *aperiodic* signals by using an *infinite fundamental period* $T_0$ on *BIBO* stable signals (otherwise would be unbounded). 

The Fourier Transform is a special Laplace Transform where $s=j\omega$, and RoC must include the $j\omega$ axis.

```ad-important
The fourier transform of a periodic signal cannot be obtained using the integral definition of a fourier transform (i.e. consider $x(t)=cos(t)$)
![[Screenshot 2024-07-20 at 4.10.13 PM.png]]

If a lapace transform has $j\omega$ axis in RoC then $X(\omega)=X(s)|_{s=j\omega}$

For something like the *sinc* function (the impulse response to a *low-pass filter*), we cannot use laplace or integral definition: we must use duality, and FT properties to solve.
```

```ad-note
title: FT Existence
Could be any of these (but consider $u(t)$ which have FT but don't match any of these.
1. $x(t)$ absolutely integrable
	![[Screenshot 2024-07-20 at 4.16.18 PM.png|400]]
2. Laplace $X(s)$ includes $j\omega$ axis.
3. Finite number of discontinuities and finite number of maxima and minima in any finite interval.

```

![[Screenshot 2024-07-20 at 4.18.56 PM.png]]

```ad-success
title: Easiest way to calculate FT of a signal $x(t)$
- $x(t)$ is a support signal and bounded, then FT exists. Use the **Integral definition** or **LT** of $x(t)$
- $x(t)$ is infinite support, and LT $X(s)$ has RoC including $j\omega$ axis, the FT is $X(s)|_{s=j\omega}$ (**Laplace Transform**)
- $x(t)$ is periodic then FT obtained using **Fourier Series**.
- If none of the above, if has discontinuities, or has discontinuities and is not finite energy, or it has possible discontinuities in freq domain $\omega$ even though it has finite energy sinc$(t)$ use **Properties of FT**
```

```ad-tip
- Fourier Transform is linear
![[Screenshot 2024-07-20 at 4.30.49 PM.png]]

- Duality
![[Screenshot 2024-07-20 at 4.53.54 PM.png]]
*suppose we want to get some function $F(\omega)$ in the frequency domain. To obtain it, first do the fourier transform of some $f(t)$, then do the fourier transform of $F(t)$
```

```ad-summary
title: Spectral Representation

```















































**Come back to 0.2 to look at the processes and get a general understanding of what's happening**
- [?] Think of another process that requires signal manipulation and draw a diagram explaining the transformations that occur in that process




---
## Vocabulary
- [[Digital Signal Processors]] (DSPs)
- [[Field Programmable Gate Arrays]] (FPGAs)
- [[Application Specific Integrated Circuits]] (ASICs)
- [[control system]]
- 

## Questions
- **Concept**: [[CT-Systems]] – Suppose $\mathcal{S}$ is the transformation corresponding to an *LTI system* so that the response to the system is $y(t)=\mathcal{S}[x(t)]$. Now suppose we provide as input a linear combination of scaled, time shifted dirac-delta functions (a sampling train) $\displaystyle x(t)=\sum_{k}{A_k\delta({t-\tau_k})}$. What is the response of the system?
		By the property of Time-invariance and Linearity, the output is a linear combination of equally time shifted outputs. 
		$$
		\begin{aligned}
		\displaystyle y(t)&=\mathcal{S}[x(t)] \\ \\
		&=\mathcal{S}\Big[\sum_{k}{A_k\delta({t-\tau_k})}\Big] \\ \\
		&=\sum_{k}{A_k\mathcal{S}\Big[\delta({t-\tau_k})}\Big] \\ \\
		&=\sum_{k}{A_ky(t-\tau_k)} \quad \text{or} \quad \sum_{k}{A_kh(t-\tau_k)} \\ \\&\text{if the impulse response is known}
		\end{aligned}
		$$
- **Concept**: [[CT-Systems]] – 

## Further Exploration
- Links to related articles, videos, or books

## Personal Insights
- Personal reflections or insights gained from the media
