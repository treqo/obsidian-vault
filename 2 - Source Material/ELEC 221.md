## 1 – Intro

A **Signal** is something that contains information.
A **System** is a process that can transform or generate a signal.
### Continuous vs Discrete

A **continuous-time** signal is denoted as $x(t)$. A discrete-time signal is denoted as $x_d[n]$.

---

$$
x_d[n]=x(nT_s)=x(t)|_{t=nT_s}
$$
*Mathematical definition of a discrete signal*

$T_s$ : The sampling period.
$n$ : Some integer $n \in \mathbb{Z}$. 

---
$$
D[x(t)]=\frac{dx(t)}{dt}=\lim_{h \to 0}{\frac{x(t+h)-x(t)}{h}}
$$
*Derivative of continuous-time signal*

**Continuous-time Systems** lead to differential equations.

---
$$
\Delta[x[n]]=x[n+1]-x[n]
$$
*Forward-finite Difference equation*

**Discrete-time Systems** lead to finite difference equations

---

### Complex Numbers ($\mathbb{C}$)

Basic Imaginary unit : $j = \sqrt{-1}$ 

**Representations**
(a) Cartesian Form : $z=x+jy$
(b) Polar Form : $z=r \angle \theta$
(c) Exponential Form : $z = re^{j \theta}$

**Concepts**
Modulus or Magnitude : $|z|=r=\sqrt{x^2+y^2}$
Principal Argument : $arg(z) = \theta$, where $\theta \in (-\pi,\pi]$

**Conversions**
$$
x = r\cos{\theta}
$$
$$
y = r\cos{\theta}
$$
$$
r=\sqrt{x^2+y^2}
$$
$$
\theta = \arctan{\frac{y}{x}}
$$
**Euler's Identity**

$$
e^{j \theta}=\cos{\theta}+j\sin{\theta}
$$
$$
e^{j \theta}=c_{\theta}+js_{\theta}
$$
*Short hand of Euler's formula*

### Practice Problems

#### Lecture Examples

Polar Representations

1. $z=1$ 
Solution:
$$
z=1=x+jy=1\angle0\degree
$$
2. $z=-1$
Solution:
$$
z=-1 = e^{j \pi} = 1 \angle 180 \degree
$$
3. $z=j$
Solution:
$$
z = j = e^{j \frac{\pi}{2}} = 1 \angle 90 \degree
$$

4. $z=1-2j$
	$$|z|=\sqrt{(1)^2+(-2)^2}=\sqrt{5}$$
	$$\theta=\arctan(\frac{-2}{1})$$
## 2 – CT Signals

Mathematical Definition of a **Signal**, is a function that maps a domain, $D$, to a range, $R$.
$$
X = [D \rightarrow R]
$$
*where $X$ is the **signal space**, the set of all eligible signals*

### Signal Classifications
1. Continuous-Time
2. Continuous-Amplitude
3. Analog : both continuous time and amplitude.
4. Discrete-Time
5. Discrete-Amplitude
6. Digital : both discrete time and amplitude.
7. *Even* vs *Odd* vs *Neither*
	
	An even signal is defined as
	$$
	x(t)=x(-t), \forall t \in D
	$$
	An odd signal is defined as
	$$
	x(t)=-x(-t),\forall t \in D
	$$
	Any signal can be decomposed into an even and odd component.
	$$
	y(t)=y_e(t)+y_o(t)
	$$
	$$y_e(t)=\frac{y(t)+y(-t)}{2}$$
	$$y_o(t)=\frac{y(t)-y(-t)}{2}$$
8. *Periodic* or *Aperiodic*
	
	A *periodic* signal is a signal that repeats itself. It must:
	1. Be defined for all possible values of $t$, $-\infty < t <\infty$.
	2. There is a positive real value $T_0$, the **fundamental period** of $x(t)$, such that $$x(t+kT_0)=x(t)$$ for any integer $k$. Note that the **fundamental period** is the smallest non-zero value that makes periodicity possible.
9. *Finite-Energy* vs *Finite-Power* vs *Infinite-Power*
	
	The **energy** and **power** of a continuous-time signals $x(t)$ are defined for either finite or infinite support signals as:
	$$E_X=\int_{-\infty}^{\infty}|x(t)|^2dt$$
	$$P_X=\lim_{T \rightarrow \infty}\frac{1}{2T}\int_{-T}^{T}|x(t)|^2dt$$
	*Notice that implicitly, power of a signal, $P_X$, refers to the **average power**.*
	
	Some things to realize are this:
	1. Notice the absolute value or modulus surrounding the signal $x(t)$. This is for when x(t) is complex.
	2. Notice the use of $T$ in the power equation. This is there because if the signal is periodic, it would be much easier to find the solution by considering one fundamental period.
	3. If the signal is **finite-energy** (i.e. $E_X<\infty$ ) then the average power, $P_X$ must be 0, by this substitution: $P_X=\lim_{T \rightarrow \infty}\frac{1}{2T} E_X$.
	4. The instantaneous power at some time, $t$, is $|x(t)|^2$
	
	A signal is said to be **finite-energy** or **square integrable** when $E_X<\infty$. This makes $x$ an **Energy Signal**.
	
	A signal is said to be **finite-power** when $P_X<\infty$. This can only occur if $E_X =\infty$. This makes $x$ an **Power Signal**.
10. *Causal* vs *Acausal* vs *Anti-causal*
	
	An **Causal** signal is defined as some signal $x(t)$ such that $x(t)=0, \forall t<0$. A way to remember this is to think that the cause of the signal starts at $t=0$, and anything before that point we don't consider. 
	
	![[Screenshot 2024-07-06 at 6.59.49 PM.png|300]]
	*figure of a causal signal*
	
	An **acausal** signal is defined as some signal $x(t)$ such that $\exists t<0$ where $x(t)\neq0$. A way to think about this is the prefix "A", meaning not. So it is essential saying "not causal". Thus, it does not abide of a causal signal.
	
	![[Screenshot 2024-07-06 at 7.00.31 PM.png|300]]
	*figure of an acausal signal*
	
	An  **anti-causal** signal is defined as some signal $x(t)$ such that $x(t)=0, \forall t \geq 0$ *and* $\exists t<0$ where $x(t)\neq0$. Notice this is both an *negation* of the causal signals definition, as well as the acausal's signal definition.
	
	![[Screenshot 2024-07-06 at 7.00.58 PM.png|300]]
	*figure of an anti-causal signal*
	
	$$\mathcal{X} = \text{Set of all signals} = \text{Causal signals} \cup \text{Acausal signals} ; \text{Anti-causal signals} \subseteq \text{Acausal signals}$$
	*All possible signals fall under either a causal or acausal signal. This means that the set of all possible signals is the union of causal and acausal signals.*
	
	*The set of anti-causal signals are a subset of the set of acausal signals.*
	
	A causal signal is one which is potentially a valid impulse response for a causal system. More on this when we get to systems.
### Basic Signal Operations

Consider the analog signal $x(t)$ shown below.
![[Screenshot 2024-07-06 at 12.26.57 PM.png|300]]

The following are **Unary** signal operations (i.e. one signal)
1. **Scaling**
	
	Notation
	![[Screenshot 2024-07-06 at 12.32.50 PM.png|300]]
	
	Graphical Interpretation
	 ![[Screenshot 2024-07-06 at 12.34.22 PM.png|300]]

2. **Time Shift (Delay)**
	
	A time shift *delay* occurs when the input signal into the delay operation has $\tau<0$. Mathematically, this makes sense because say we have an output $a=x(0)$ then before the delay this occurs when $t=0$, after the delay it occurs when $t=-\tau$. Graphically, the signal is **right-shifted** by $\tau$.
	
	Notation
	![[Screenshot 2024-07-06 at 12.35.01 PM.png|300]]
	
	Graphical Interpretation
	![[Screenshot 2024-07-06 at 12.35.27 PM.png|300]]

3. **Time Shift (Advance)**
	
	A time shift *advance* occurs when the input signal into the delay operation has $\tau>0$. Mathematically, this makes sense because say we have an output $a=x(\tau)$ then before the advance this occurs when $t=\tau$, after the advance it occurs when $t=0$. Graphically, the signal is **left-shifted** by $\tau$.
	
	Notation 
	In the box, it will either indicate "delay" or "advance."
	
	Graphical Interpretation
	![[Screenshot 2024-07-06 at 12.44.54 PM.png|300]]

4. **Time Scaling (stretch)**
	
	Time scaling is a stretch, compression or reversal along the *domain*. A time *stretch* indicates that $|a|<1$. This makes sense because since now the time to get from point $A$ to point $B$ increases. In $x(t)$ say $A=x(1)$ and $B=x(2)$. Now in the new signal $x(at)$ where $a=0.5$, the distance between $A$ and $B$ in the time-domain is doubled to $2$.
	
	Note that a time stretch causes a *lower* signal frequency.
	
	Notation 
	![[Screenshot 2024-07-06 at 12.46.08 PM.png|300]]
	
	Graphical Interpretation
	![[Screenshot 2024-07-06 at 12.52.01 PM.png|300]]

5. **Time Scaling (Contraction)**
	
	Note that a time contraction causes a *higher* signal frequency.
	
	Notation 
	![[Screenshot 2024-07-06 at 12.46.08 PM.png|300]]
	
	Graphical Interpretation
	![[Screenshot 2024-07-06 at 12.54.04 PM.png]]

6. **Time Scaling (Reversal)**
	
	A time reversal or reflection graphically flips the signal across the vertical axis. 
	
	Graphical Interpretation
	![[Screenshot 2024-07-06 at 12.56.09 PM.png|300]]

7. **Differentiation**
	
	Differentiation of an analog signal results in a differential equation. Below, you can see equivalent expressions, one in the $t$ domain, and one in the $s$ domain.
	
	Notation 
	![[Screenshot 2024-07-06 at 12.57.44 PM.png|300]]
	![[Screenshot 2024-07-06 at 12.58.03 PM.png|300]]
	
	Graphical Interpretation
	![[Screenshot 2024-07-06 at 12.57.00 PM.png|300]]
	*the arrow indicates an impulse function*
	
8. **Integration**
	
	Integration of an analog signal results in a differential equation. Below, you can see equivalent expressions, one in the $t$ domain, and one in the $s$ domain.
	
	Notation 
	![[Screenshot 2024-07-06 at 12.59.55 PM.png|400]]
	
	Graphical Interpretation
	![[Screenshot 2024-07-06 at 1.01.56 PM.png|300]]

The following are **Binary** signal operations (i.e. usually two signals)
1. **Adder**
	![[Screenshot 2024-07-06 at 1.02.26 PM.png|300]]
2. **Modulation / Time Windowing**
	![[Screenshot 2024-07-06 at 1.02.55 PM.png|300]]

### Basic Signals

![[Screenshot 2024-07-07 at 2.35.47 AM.png]]

A **complex exponential signal** is a signal, $x(t)$, of the form $x(t)=Ae^{st}$ where $s=\sigma+j\omega$.

```ad-note
title: Takeaways
1. $x(t) \in \mathbb{C}$ and A is a complex constant
2. $\sigma$ tells about the stability of the signal
3. $\omega$ tells us about the frequency of the signal
4. $x(t)=Ae^{st}=|A|e^{\sigma t}[\cos(\omega t + \theta)+j\sin(\omega t + \theta)]$, $t \in \mathbb{R}$

	Note that we suppose $A=|A| \angle \theta$
	Steps
	
	$|A|e^{j \theta}(e^{\sigma t}e^{j \omega t})$
	$|A|(e^{\sigma t})(c_{\omega t + \theta} + js_{\omega t + \theta})$
```

A **sinusoid** is of general form $A\cos(\omega_0t+\theta)=A\sin(\omega_0t+\theta+\pi/2)$.

```ad-tip
title: Comprehension Check
Cosine leads sine by 90 degrees.

![[Screenshot 2024-07-07 at 2.18.34 AM.png|300]]

Notice how sine is time shifted, or delayed by $\pi/2$

$A\cos(\omega_0t+\theta)=A\sin(\omega_0t+\theta+\pi/2)$
1. $A$ is the amplitude
2. $\omega_0$ is the analog, or fundamental angular frequency
3. $\theta$ is the phase shift
4. The fundamental period
$$\omega_0= 2 \pi f_0 = \frac{2 \pi}{T_0}$$
$$T_0=\frac{2 \pi}{\omega_0}$$
```

```ad-info
title: Convention Note
Most textbooks use
$\omega \rightarrow$ continuous frequency variable in radians/sec
$\Omega \rightarrow$ discrete frequency variable in radians/sample

But C&A uses $\omega$ for discrete and $\Omega$ for continuous.
```

The **Unit-impulse signal** $\delta (t)$
1. is $0$ everywhere except for the origin where its value is *not well defined*
2. has the integral $\int_{-\infty}^{t}\delta (\tau) d\tau = 1$ when $t>0$, but $0$ if $t<0$

The **shifting property** of the impulse function $\delta (t)$ on any signal $x(t)$ can be represented by
$$x(t)=\int_{-\infty}^{\infty}x(\tau)\delta(t-\tau) d\tau$$
### Practice Problems

#### Lecture Examples

1. C&A 1.1
2. C&A 1.2
3. C&A 1.3

## 3.0 – ODE Review

### 1st Order Linear ODEs

**Big Picture**

We have some System, which is mathematically a combination (*linear* or *non-linear*) of **Signal Operations**, thus it transforms a signal in some manner. We define the input as $f(t)$, the *forcing function*, and the output $y(t)$ is the **signal response**.

```ad-important
title: Chapter 1 Connection
Recall that *differentiating* continuous-time signals, and analog signals **result in differential equations**. Later on we will see that discrete-time signals, and digital signals result in finite difference equations.

![[Screenshot 2024-07-07 at 5.04.05 PM.png]]
```

A first order differential equation is of the form
$$
\begin{aligned}
\text{Given} \quad
&\begin{cases}
\frac{dy(t)}{dt} + ay(t) = f(t) \quad &(*) \\
y(t_0) = y_0 \quad &(**)
\end{cases}
\\
\text{find } y(t).
\end{aligned}
$$
1. if $f(t)$ is $0$, then the differential equation is just the **homogenous equation** and the system is said to be **source-free**
2. $y_0$ is the initial condition. 

**Fundamental Theorem of Linear DEs**

1. $y_p(t)$ is the **particular solution**, satisfying for the *forcing function* $f(t)$. so $\frac{dy_p(t)}{dt} + ay_p(t) = f(t)$
2. $y_c(t)$ is the **complementary solution**, satisfying the *homogenous equation*, where $f(t)=0$. $\frac{dy_c(t)}{dt} + ay_c(t) = 0$
3. $y(t)$ is the **complete solution** to the differential system, satisfying both $(*)$ and $(**)$, the differential equation, and the initial condition (I.C.) – $y(t)=y_c(t)+y_p(t)$
#### Complete Response

1. Particular integral solution $y_p(t)$
	- Sometimes, use **forced response**, also known as the **zero-state response**. The unique particular solution *satisfying an I.C. of $0$*. Denoted as $y_{zsr}(t)$
	- Sometimes more convenient to use **steady-state (permanent) response**. This essentially means, what does the input signal converge to as $t \rightarrow \infty$. Denoted as $y_{ss}(t)$.
2. Complementary solution $y_c(t)$
	- Sometimes use **natural response**, or **zero-input response**, the unique complementary solution satisfying *the given* initial condition. Denoted as $y_{zir}(t)$.
	- Sometimes more convenient to use **transient response**. Disappears, or converges to $0$ as $t \rightarrow \infty$. Denoted as $y_{tr}(t)$
3. We define the **complete response** as $y(t)$, the sum or combination of the particular and complementary solution

This leads to 2 different methods of finding the complete solutions

$$
\begin{flalign}
&(*) \quad y(t)=y_{p}(t)+y_{c}(t) \\ \\
&(1) \quad y(t)=y_{zsr}(t)+y_{zir}(t) \\
&(2) \quad y(t)=y_{ss}(t)+y_{tr}(t)
\end{flalign}
$$
```ad-tip
![[Screenshot 2024-07-07 at 5.42.20 PM.png]]
*Transcription*

- [[Forced Response]]  : Due to $f(t)$; compute assuming $y_p(0)=0$
- [[Natural Response]] : Due to IC $y_c(0)=y_0$. Take $f(t)=0$
- [[Steady-state]]     : Periodic (note that this includes DC, i.e. constant responses). Constant if DC input, Sinusoidal input then sinusoid of same frequency.
- [[Transient]]        : Usually combination of decaying exponentials. **CAN'T** be used for *unstable systems*
```

### 2nd Order Linear ODEs


A second order differential equation is of the form
$$
\begin{aligned}
\text{Given} \quad
&\begin{cases}
\frac{d^2y(t)}{dt^2}+a_1\frac{dy(t)}{dt} + a_0y(t) = f(t) \quad &(*) \\
y(t_0) = y_0 \quad \text{and} \frac{dy(t)}{dt}|_{t=t_0}=y_0'\quad &(**)
\end{cases}
\\
\text{find } y(t).
\end{aligned}
$$
*Notice we have two I.C.'s now.*

It is preferable to write the *homogenous differential equation* in the following form

$$
\frac{d^2y_c(t)}{dt^2}+2\zeta\omega_0\frac{dy_c(t)}{dt} + \omega_0^2y(t) = 0 \quad (***)
$$
In this equation we have
- $w_0$ : the *natural (undamped) frequency*
- $\zeta$ : the *damping ratio*

We assume the solution to the homogenous differential equation is of the form
$$
\begin{aligned}
y_c(t)=Ke^{st} \quad \implies \quad &s^2(Ke^{st})+2s\zeta\omega_0(Ke^{st})+\omega_0^2(Ke^{st})=0 \\\\
&s^2+2\zeta\omega_0s+\omega_0^2=0 \quad (****)
\end{aligned}
$$
- The last equation is known as the **characteristic equation**. 

#### Characteristic equation

The roots of the characteristic equation, $s^2+2\zeta\omega_0s+\omega_0^2=0$, provides useful information on the *behaviour* of the solution, $y(t)$, mainly for the *transient* characteristics, since the transient part of the *complete response* is due to the *homogenous equation*.

The solution yields two eigenvalues
$$
\begin{aligned}
s_{1,2}&=\frac{-2\zeta\omega_0\pm\sqrt{4\zeta^2\omega_0^2-4\omega_0^2}}{2}\\ \\
&=\frac{-2\zeta\omega_0\pm2\omega_0\sqrt{\zeta^2-1}}{2} \\ \\
&=\boxed{-\zeta\omega_0\pm\omega_0\sqrt{\zeta^2-1}}
\end{aligned}
$$
For **stable** systems, there are 3 cases which depend on the value of the damping ratio $\zeta$

1. **Over-damped** : $\zeta>1$
	- $s_{1,2}$ : negative, real and unequal ($s_1 \neq s_2$) roots.
	- Solution has form: $y_c(t) = Ae^{s_1t}+Be^{s_2t}$
	
	![[Screenshot 2024-07-08 at 11.39.00 AM.png|300]]
	*Note that this is a solution that satisfies the I.C.'s. If the I.C.'s are unknown, the result would be a slope field.*

2. **Critically damped** : $\zeta=1$
	- $s_{1,2}$ : single, *repeated* and *negative* *root*.
	- $\boxed{s=-\omega_0}$
	- Solution has form: $y_c(t) = [A+Bt]e^{-\omega_0t}$
	
	![[Screenshot 2024-07-08 at 11.44.57 AM.png|300]]

1. **Under-damped** : $0<\zeta<1$
	- $s_{1,2}$ : *complex-conjugate pair of roots*
	- $s_{1},s_{2}\in\mathbb{C} \quad \text{and} \quad s_{1} = \overline{s_{2}}$
	
	- $s_{1,2}=\quad-\zeta\omega_0 \pm j\omega_0\sqrt{1-\zeta^2}\quad=\quad \sigma \pm j \omega_d$
		- $\omega_d$ : The *damped* frequency. $\omega_d=\omega_0\sqrt{1-\zeta^2}$
		- $\sigma$ : The *exponential decay rate*. $\sigma = -\zeta \omega_0$.
	
	- Solution has form: $y_c(t) = \quad e^{\sigma t}C\cos(w_dt+\phi) \quad= \quad e^{\sigma t}[A\cos(w_dt)+B\sin(w_dt)]$
	
	![[Screenshot 2024-07-08 at 11.46.07 AM.png|300]]
## 3.1 – CT Systems

### System Mathematical Representation

A **system** – in systems and signals analysis (electrical engineering) – is a *function* that maps a *domain signal* to a *range signal*. In other words, it is a function that takes one signal as input, and produces one signal as output. Recall that a signal is actually a function, so another way of thinking about it is, it maps one function to another function.

![[Screenshot 2024-07-08 at 12.05.59 PM.png]]

- $y=S(x)$ : the *range signal* $y$ produced by the output of the System taking *domain signal* $x$ as input.

### System Classifications

![[Screenshot 2024-07-08 at 12.09.14 PM.png]]

![[Screenshot 2024-07-08 at 12.09.27 PM.png]]

1. *Memoryless* vs *Nonmemoryless*
	A **Memoryless System** is one in which the *instantaneous* output at any given time, is *independent* of the *input* at any other time.
	$$
	\begin{aligned}
	\text{Let} \quad &y_i=S(x_i), \quad i\in \{1,2\} \\ \\
	&\forall x_1, x_2 \in X, \forall t_0 \in\mathbb{R} \quad \text{then} \quad 
	\bigl( x_1(t_0)=x_2(t_0) \bigr) \implies \bigl( y_1(t_0)=y_2(t_0) \bigr)
	
	\end{aligned}
	$$
2. *Causal* vs *Acausal*
	A **Causal System** is one in which the *instantaneous* output at any given time, is *independent* of any *future inputs* (i.e. it only depends on the input up to that time).
	$$
	\begin{aligned} \text{Let} \quad &y_i = S(x_i), \quad i \in \{1, 2\} \\ \\ &\forall x_1, x_2 \in X, \forall t_0 \in \mathbb{R}, \quad \text{then} \quad \bigl( x_1(t) = x_2(t) \forall t \leq t_0 \bigr) \implies \bigl( y_1(t_0) = y_2(t_0) \bigr) \end{aligned}
	$$
	
	**Differentiating Causal and Memoryless**
	
	**Every** *memoryless* system is *causal*, but not every *causal* system is *memoryless*. Recall the definitions mentioned earlier. In a memoryless system, the instantaneous output is independent from any input at any other time. In a causal system, the instantaneous output is only independent of *future* inputs – **implying** that *past inputs can effect the instantaneous output*. This gives us the important expression.
	$$\text{Memoryless System} \subset \text{Causal System}$$

3. *Linear* vs *Non-Linear*
	A system is **Linear** if it has both *homogeneity (scaling)* and *additivity*. 
	$$
	\begin{aligned}
	\text{Linear System} \quad &\iff \quad (\text{Homogeneity}) \quad \wedge \quad (\text{Additive}) \\ \\
	&\iff \quad (\forall x \in [\mathbb{R} \rightarrow \mathbb{C}], \: \forall a \in \mathbb{C}, \; \; S(ax)=aS(x)) \quad \wedge \\ \\
	&\qquad\qquad \forall \ (x_1, \, x_2 \in [\mathbb{R} \rightarrow \mathbb{C}], \ S(x_1+x_2)=S(x_1)+S(x_2))
	\end{aligned}$$
	Alternatively, use the **combined** test 
	$$\forall x_1, x_2 \in [\mathbb{R} \to \mathbb{C}], \forall a, b \in \mathbb{C}, S(ax_1 + bx_2) = aS(x_1) + bS(x_2)$$
	![[Screenshot 2024-07-09 at 11.08.40 AM.png|300]]
	*Homogeneity **iff** behaviour of the red boxed section is **indistinguishable***

	![[Screenshot 2024-07-09 at 11.09.43 AM.png|300]]
	*Additivity **iff** behaviour of the red boxed section is **indistinguishable***

```ad-summary
title: Linearity C&A
A System $\mathcal{S}$ is said to be **linear** if, for its inputs $x(t)$ and $v(t)$, **superposition** holds:
$$
\mathcal{S}[\alpha x(t)+ \beta v(t)] = \alpha\mathcal{S}[x(t)] + \beta \mathcal{S}[v(t)]
$$
![[Screenshot 2024-07-09 at 11.15.14 AM.png]]
```

4. *Time invariant* vs *Time Varying*
```ad-summary
title: Time-Invariance C&A
A continuous-time system $\mathcal{S}$ is said to be **time-invariant** if whenever for an input $x(t)$, with a corresponding output $y(t)=\mathcal{S}[x(t)]$, the output corresponding to a time-shifted input $x(t \ \mp \ \tau)$ – delayed or advanced – is the *original* output, equally time-shifted, $y(t \ \mp \ \tau)=\mathcal{S}[x(t \ \mp \ \tau)]$.

$$
\begin{aligned}
x(t) \quad &\implies \quad y(t)=\mathcal{S}[x(t)] \\ \\
x(t \ \mp \ \tau) \quad &\implies \quad y(t \ \mp \ \tau)=\mathcal{S}[x(t \ \mp \ \tau)] \\ \\
\end{aligned}
$$

In other words, the system does not age – its parameters are constant.

![[Screenshot 2024-07-09 at 12.32.46 PM.png]]
```

#### Modelling Systems

In ELEC 221, most systems we model are **LTI (Linear, Time-Invariant)** systems, in order to simplify calculations. In the real world, this is not the case.

### Ideal Circuit Passive Components

```ad-note
title: Passive Component
A passive comonent, in the scope of electrical engineering, is a component that does not generate power, it can only store or disipate it.

Characteristics
- Do not generate power
- Two-terminal (input & output)
- Energy storage/dissipation
- **Linear** behaviour

![[Screenshot 2024-07-09 at 12.39.27 PM.png]]
*We regard phasor analysis as a special case of LT analysis where we set $s=j\omega$ (i.e. only one frequency) and ICs are ignored.*
```
#### Aside

![[Screenshot 2024-07-09 at 2.49.46 PM.png]]
### Active Components

![[Screenshot 2024-07-09 at 2.50.40 PM.png]]

### ODE System Representation

Most *CT dynamic* systems with *lumped parameters* can be represented by linear ODEs with constant coefficients. 

```ad-abstract
title: ODE System C&A
A System represented by a linear ordinary differential equation, of any order $N$, with constant coefficients, and having $x$ as input and $y$ as output

$$
a_0y(t)+a_1 \frac{dy(t)}{dt}+ \ ... \ + a_N \frac{d^Ny(t)}{dt^N} = b_0x(t)+b_1 \frac{dx(t)}{dt}+ \ ... \ + a_M \frac{d^Mx(t)}{dt^M} \quad t \geq 0
$$

is *linear time-invariant* if the system is **not initially energized** 
- i.e. the initial conditions are $0$, and the input $x(t)=0$ for $t<0$.

This allows for easy determination of the *characteristic polynomial*
$$
a_0 + a_1 s + \cdots + s^N = \prod_{k=1}^{N} (s - p_k)
$$
where the polynomial roots, $p_k$, are the systems **natural frequencies** or **eigenvalues**. This provides insight into the intrinsic dynamics of the system: Damping classification, stability, time constants, etc.
```

```ad-warning
The requirement of being “relaxed” (unenergized) to be LTI is mildly “troubling”. IC’s shouldn’t affect classification and can be remedied with ZIR (with **superposition**)
```
#### ODE System Superposition

```ad-abstract
title: ODE System Superposition C&A
Given a dynamic system represented by a linear ODE with constant coefficients,

$$
a_0y(t)+a_1 \frac{dy(t)}{dt}+ \ ... \ + a_N \frac{d^Ny(t)}{dt^N} = b_0x(t)+b_1 \frac{dx(t)}{dt}+ \ ... \ + a_M \frac{d^Mx(t)}{dt^M} \quad t \geq 0
$$

with $N$ initial conditions $y(0), \: \frac{d^ky(t)}{dt^k}|_{t=0}$ for $k \in \{1,2, \ ... \, N-1\}$ *and* input $x(t)=0$ for $t<0$, the system's **complete response** $y(t), \ t \geq 0$ has two components.
- the **zero-state response** aka **forced response**, $y_{zs}(t)$, due *exclusively* to the **input**, as the *inital conditions are zero*.
- the **zero-input response** aka **natural response**, $y_{zi}(t)$, due *exclusively* to the **initial conditions**, as the *input is $0$*.

Thus, the complete response is
$$
y(t)=y_{zs}(t)+y_{zi}(t)
$$

![[Screenshot 2024-07-09 at 3.15.00 PM.png]]
```
#### Convolution Integral

```ad-abstract
title: Convolution Integral C&A
If $S$ is the transformation corresponding to a **LTI System**, so that the response of the system is $y(t)=\mathcal{S}[x(t)]$ then we have that by *superposition* and *time-invariance*.
$$
\begin{aligned}
\mathcal{S}\left[\sum_{k} A_k x(t - \tau_k) \right] \quad = \quad \sum_{k} A_k \mathcal{S}[x(t - \tau_k)] \quad = \quad \boxed{\sum_{k} A_k y(t - \tau_k)} \quad &(1) \\ \\
\mathcal{S}\left[\int g(\tau) x(t - \tau) d\tau \right] \quad = \quad \int g(\tau) \mathcal{S}[x(t - \tau)] d\tau \quad = \quad \boxed{\int g(\tau) y(t - \tau) d\tau} \quad &(2) \\ \\
\end{aligned} 
$$
Takeaway
- This is a product only of *LTI Systems* (we mostly deal with this in ELEC 221)
- `(1)` corresponds to the property of *superposition* and *time-invariance*. The new input is a **linear combination** of *time-shifted versions* of the input, then the output is also a linear combination, equally time shifted.
- `(2)` This extension allows for a more general new input made of a continuum of differentials of the original. Remember an integral is just an infinite sum.
```
#### Impulse Response

```ad-summary
title: Impulse Response C&A
The **impulse response** of a *continuous-time LTI system*, $h(t)$, is the output of the system corresponding to an impulse $\delta (t)$, as the input signal, $x(t)$, and *initial conditions* equal to $0$.
```

```ad-important
An LTI system’s impulse response *gives us everything* needed to compute the response to *any input signal*. In fact, h(t) is a valid representation of the system.
```
#### Convolution with Impulse Response

```ad-summary
title: Convolution Integral C&A
The response of an *LTI System*, $\mathcal{S}$, represented by its *impulse response*, $h(t)$, to any signal, $x(t)$, is the **convolution integral**
$$
\begin{aligned}
y(t) \quad &= \quad \int_{-\infty}^{\infty}x(\tau) \, h(t-\tau) \ d\tau \\ \\
&= \quad \int_{-\infty}^{\infty}x(t-\tau) \, h(t) \ d\tau \\ \\
&= \quad [x*h](t) \ = \ [h*x](t)
\end{aligned}
$$
- Sanity check: if $x$ is $\delta$ then the *convolution integral* simply evaluates to the *impulse response* $h(t)$ as we would expect.

![[Screenshot 2024-07-10 at 2.09.44 PM.png]]
```

```ad-tip
title: But what is a convolution
This is a personal section I will create to supplement learning by trying to provide clarity on the ambiguous and scary, *convolution operation*.
- Goal: Make a manim animation
- Use 3Blue1Brown, try to make a similar video but for ELEC 221

![[Screenshot 2024-07-10 at 2.22.58 PM.png|400]]
```
#### Responses to Impulse, Step and Ramp

```ad-summary
title: Responses
1. The *impulse response*, $h(t)$
2. The *step response*, $s(t)$
3. The *ramp response*, $\rho (t)$

These responses are related by
$$
h(t) \ = \
\begin{cases}
\displaystyle \frac{ds(t)}{dt} \\ \\
\displaystyle \frac{d^2\rho (t)}{dt^2}
\end{cases}
$$
*This makes sense because from before we remember that* $\displaystyle \delta(t)=\frac{d[u(t)]}{dt}=\frac{d^2[r(t)]}{dt^2}$. Therefore, it follows that for an *LTI System*, $\mathcal{S}$, we have that $\displaystyle \mathcal{S}[\delta(t)]=h(t)=\frac{d[\mathcal{S}(u(t))]}{dt}=\frac{d^2[\mathcal{S}(r(t))]}{dt^2}$

![[Screenshot 2024-07-10 at 2.17.27 PM.png]]
```

### Block Diagrams

```ad-summary
title: Parallel / Series connected LTI Systems C&A
Two LTI Systems with impulse responses $h_1(t)$ and $h_2(t)$ connected in **series** have an overall impulse response 
$$
\begin{aligned}
\displaystyle h(t) \quad = \quad \bigl[h_1*h_2\bigr](t) \quad = \quad \bigl[h_2*h_1\bigr](t)
\end{aligned}
$$
![[Screenshot 2024-07-11 at 11.59.24 AM.png|400]]

Two LTI Systems with impulse responses $h_1(t)$ and $h_2(t)$ connected in **parallel** have an overall impulse response 
$$
\begin{aligned}
\displaystyle h(t) \quad = \quad h_1(t) \ + \ h_2(t)
\end{aligned}
$$
![[Screenshot 2024-07-11 at 11.59.34 AM.png|300]]
```

```ad-abstract
title: LTI System Feedback Control
Given two LTI systems with impulse responses $h_1(t)$ and $h_2(t)$, a negative feedback connection is such that the output is 
$$
y(t) \quad = \quad [h_1(t)*e](t)
$$
where $e(t)$ denotes the *error signal* given by
$$
e(t) \quad = \quad x(t) - [y*h_2](t)
$$
The *overall impulse response* $h(t)$, or the impulse response of the **closed-loop** system, is given by the following implicit expression
$$
h(t) = [h_1-h*h_1*h_2](t)
$$
If $h_2(t)=0$, then there is *no feedback*, and the system is called **open-loop**; $h(t)=h_1(t)$.
- [!] The *implicit expression is not very useful*; the main take-away is that solving in the time-domain is **much harder** than solving in the $s$-domain

![[Screenshot 2024-07-11 at 12.09.27 PM.png]]
```
#### Causal System

```ad-summary
title: Causal System
A continuous-time system $\mathcal{S}$ is called **causal** if
- whenever its input $x(t)=0$, and there are *no initial conditions*, the output is $y(t)=0$
- The output $y(t)$ *does not* depend on *future inputs*

A LTI System represented by its impulse response, $h(t)$, is **causal** if
$$
h(t) \ = \ 0 \quad \text{for} \ \ t \, < \, 0
$$
The output of a causal LTI system *with a causal input* $x(t)$ (i.e. $x(t)=0 \ \ \text{for} \ \ t \, < \, 0$) is given by
$$
\displaystyle y(t) \quad = \quad \int_{0}^{\infty}x(\tau) \, h(t-\tau) \ d\tau
$$
```
#### BIBO Stability

```ad-summary
In general, practical systems are designed to be *stable*. There are several important stability concepts.

**Bounded-input bounded output (BIBO) stability** establishes that for a bounded (well-behaved) input $x(t)$ the output of a BIBO stable system $y(t)$ is also bounded. This means that if there is $M< \infty$ such that $|x(t)| \leq M$ then the output is also bounded. That is, there is $L< \infty$ such that $|y(t)| \leq L < \infty$.

An LTI System that is **BIBO Stable** is also **absolutely integrable** (and vice versa). 
$$
\int_{-\infty}^{\infty}|h(t)| \ dt < \infty
$$
$$
\text{BIBO stable} \quad \iff \quad \text{$h(t)$ absolutely integrable}
$$
```
![[Screenshot 2024-07-11 at 12.47.51 PM.png]]

I got WW2 Q3 wrong because I didn't recognize that for the LTI system S', because it must be time invariant it could not have been x(0.5t). So infact, time scaling or compression is not allowed so it must have been basic addition. I should work back on this question, and also pose the question to myself, what operations from the **Basic Signal Operations** (as well as others like absolute value) are linear, which ones are time invariant.

GO back and do WW2 Q4-6

## 4 – Laplace Transform

```ad-summary
title: One-sided Laplace Tranforms
For causal signals or systems, the **one-sided laplace transform** is used.

For any function, $f(t)$, $-\infty < t < \infty$, its one-sided LT is defined as
$$
\displaystyle F(s) \quad = \quad \mathcal{L}[f(t)u(t)] \quad = \quad \int_{0^-}^{\infty}f(t)e^{-st} \ dt, \quad ROC
$$
- $u(t)$ ensures the signal is *causal*
- ROC (**Region of Convergence**) : area in $\mathbb{C}$-values, $s$-domain *for which the integral exists*. Recall $\displaystyle s=\sigma + j \omega$, so $e^{-st}=e^{-\sigma t}e^{-j \omega t}$
```
### Domain Changing

The Laplace transform converts a signal in the *t-domain* to the *s-domain*.

![[Screenshot 2024-07-12 at 11.45.35 AM.png]]

- In the *s-domain*, a solution is only valid in the **RoC**
- *t-domain* convolution is equivalent to *s-domain* multiplication.
- *I.C's* are incorporated when using Laplace Transform
- Output provides the *total response* (natural + forced response).

```ad-warning
title: Additional Comments
- Not all functions have a LT (e.g. $f(t)=1/t$) 
- $\bigl(\ f_1(t) = f_2(t) \ \bigr) \implies \bigl(\ F_1(s) = F_2(s) \ \bigr)$, but the converse is not necessarily true because of values at *discontinuities * and where $t<0$ (for 1-sided LT)
- $f(t) \ : \ \mathbb{R} \rightarrow \mathbb{R}$, whereas $F(s) \ : \ \mathbb{C} \rightarrow \mathbb{C}$
```

![[Screenshot 2024-07-12 at 11.54.34 AM.png]]
![[Screenshot 2024-07-12 at 11.56.14 AM.png]]
### 1-Sided Inverse LT

```ad-info
title: Extra
This information is not necessary to know as it is out of the scope of the course, and too mathematically complex to analyze.
If the *ROC* for $F(s)$ is $Re(s)> \sigma_c$ , then the inverse LT is given by
$$
\mathcal{L}^{-1} \left[ F(s) \right] = f(t) = \frac{1}{2\pi j} \int_{\sigma_1 - j\infty}^{\sigma_1 + j\infty} F(s) e^{st} \, ds
$$
![[Screenshot 2024-07-12 at 12.00.41 PM.png|400]]
```
#### Algorithm to find 1-sided inverse LT

1. Find all the poles of $F(s)$ and classify them as *simple*, *complex*, or *repeated*.
2. Find the partial fraction expansion in basic terms.
3. Look up inverse of each basic term in the look-up table.

Consider $F(s)=N(s)/D(s)$ where $N$ and $D$ are polynomials of $s$ with degrees $degree[N(s)] <degree[D(s)] = n$. The *poles* are values $p_i$ such that $\displaystyle D(s)=\prod_{i=1}^{n}(s-p_i$)
```ad-tip
title: Poles of $\displaystyle F(s)$
**Simple**: $p_i$ is *real*, *negative* ($p_i < 0$), and has degree of $1$.

**Repeated**: $p_i < 0$ and has degree $m \geq 2$

**Complex-conjugate Pair**: $p_i$ is *complex*, and comes in a pair $p_i= \sigma + j \omega$ and $p_i^*=\sigma - j \omega$ with $\sigma < 0$.

![[Screenshot 2024-07-12 at 12.20.14 PM.png]]
```

```ad-abstract
title: Partial Fraction Expansion
Given $\displaystyle F(s)=\frac{N(s)}{D(s)}$, and the $n$ poles of $D(s)$, as $p_i$, we need to find the coefficents in the PFE. There are two methods to go about doing this:
1. *Residue Method*
Recommended for finding the coefficent for a pole's *highest degree*
$$
\displaystyle \text{For simple pole $p_i$ : } \quad k_i = \lim_{s \rightarrow p_i}{(s-p_i)F(s)} 
$$
$$
\displaystyle \text{For repeated pole $p_i$ of degree $m$ : } \quad k_m = \lim_{s \rightarrow p_i}{(s-p_i)^mF(s)} 
$$

2. *Algebraic Method*
Recommended for finding the coefficients of *complex* and *lower degree* poles.

- [!] Poles must be in the Left-hand plane (LHP), otherwise signals in the time-domain grow exponentially (unstable).
```
#### Transfer Function

```ad-summary
title: Transfer Function C&A
A Transfer Function (TF) is a *ratio* of two $s$-domain signals, **assuming** the system is *relaxed* (IC's are $0$). The transfer function $H(s)$ related the output response $Y(s)$ to some input excitation $X(s)$. 

The transfer function $H(s)=\mathcal{L}[h(t)]$, is the laplace transform of the impulse response of an LTI system.
$$
H(s) \quad = \quad  \frac{\mathcal{L}[y(t)]}{\mathcal{L}[x(t)]} \quad = \quad \frac{Y(s)}{X(s)} = \quad \frac{\mathcal{L}[\text{output}]}{\mathcal{L}[\text{input}]}
$$

If the LTI system does have initial conditions we need to consider $I(s)$


```
#### Complete Response

```ad-summary
title: Complete Response C&A
![[Screenshot 2024-07-12 at 10.50.20 PM.png]]
![[Screenshot 2024-07-12 at 10.52.14 PM.png]]
![[Screenshot 2024-07-12 at 10.54.00 PM.png]]
```
### 2-Sided LT

The 1-sided LT is used for *causal signals*. For more general signals, *that include non-zero values for $t<0$*, (i.e. acausal signals), it is more appropriate to use the 2-sided LT.

```ad-summary
title: Two-Sided LT C&A
$$
F(s) = \mathcal{L}[f(t)] = \int_{-\infty}^{\infty}f(t)e^{-st}dt \quad s \in \text{ROC}
$$
with the variable $s=j \omega$, with a damping factor $\sigma$, and a frequency $\omega$ in $rad/sec$. 
```

```ad-summary
title: LTI System Eigenfunctions C&A
An input $x(t)=e^{s_0t}$ with $s_0=\sigma_0 + j \omega_0$, is called an **eigenfunction** of an LTI system with impulse response $h(t)$ if the corresponding output of the system is
$$
y(t)=x(t)\int_{-\infty}^{\infty}h(t)e^{-s_0t}dt=x(t)H(s_0)
$$
where $H(s_0)$ is the *Laplace Transform* of the impulse response, evaluated at $s=s_0$. This property is only valid for LTI systems.

The key takeaway is that when inputting an eigenfunction, the output is a scaled version of the input.
```
#### Existence of LT

```ad-summary
For a LT $F(s)$ to exist, we need that
$$
\begin{aligned}
\displaystyle
\left| \int_{-\infty}^{\infty} f(t)e^{-st} \, dt \right| &= \left| \int_{-\infty}^{\infty} f(t)e^{-\sigma t} e^{-j\omega t} \, dt \right| \\ \\
&\leq \int_{-\infty}^{\infty} \left| f(t)e^{-\sigma t} \right| \, dt < \infty
\end{aligned}
$$
Or that $f(t)e^{-\sigma t}$ be absolutely integrable.

- [!] The value chosen for $\sigma$ **determines** the ROC. The frequency **does not** affect the ROC.
```
#### Region of Convergence

If the LT has poles, the ROC may appear as one of 3 forms, *depending on the signals **causality***.

1. Causal : $x(t)=0, \forall t < 0$
	*RoC* is to the **right** of the **right-most** pole.
	![[Screenshot 2024-07-13 at 3.25.11 PM.png|300]]
	*Notice that $\sigma$ is on the horizontal axis and $j \omega$ on the vertical axis. `X`'s represent poles and `O`'s represent zeroes.*
2. Acausal : $\exists t<0, \text{ st } \ x(t) \neq 0$
	1. Anti-causal : $x(t)=0, \forall t>0$
		Region to the *Left* of *left-most* pole.
		![[Screenshot 2024-07-13 at 3.29.12 PM.png|300]]
	2. Two-sided
		Combination of the two: the right most of the LHP and the left most of the RHP. split up into a causal and acausal component and then the intersection of the two regions.
		![[Screenshot 2024-07-13 at 3.30.04 PM.png|300]]
		

Steps to find *RoC*:
1. determine causality
2. plot the poles
3. RoC is bordered by poles, but *must not contain any*.
#### 1-Sided LT to solve 2-Sided LT

```ad-summary
title: C&A
Laplace transform of a
1. Finite support function $f(t)$: $f(t)=0 \text{ for } t<t_1 \text{ and } t>t_2, \ t_1<t_2$
	$$
	F(s) = \mathcal{L} \{ f(t)[u(t - t_1) - u(t - t_2)] \} \quad \text{ROC: whole } s\text{-plane}
	$$
	
2. Causal function $g(t)$: $g(t)=0 \text{ for } t<0$
	$$
	G(s) = \mathcal{L} \{ g(t) u(t) \} \quad \mathcal{R}_c = \{ (\sigma, \omega) : \sigma > \max \{ \sigma_i \}, -\infty < \omega < \infty \}
	$$
	*where $\{\sigma_i \}$ are the real parts of the poles of $G(s)$*
	
3. Anti-causal function $h(t)$: $h(t)=0 \text{ for } t>0$
	$$
	H(s) = \mathcal{L} \{ h(-t) u(t) \}_{-s} \quad \mathcal{R}_{ac} = \{ (\sigma, \omega) : \sigma < \min \{ \sigma_i \}, -\infty < \omega < \infty \}
	$$
	- *signal $h(-t)u(t)$ is causal, so can use 1-sided LT.*
	- *the subscript "-s" (i.e. $_{(-s)}$), means use $-s$ in place of $s$* 
	
4. Two-sided function $p(t)$: $p_{ac}(t)+p_c(t)=p(t)u(-t)+p(t)u(t)$
	$$
	P(s) = \mathcal{L} [p(t)] = \mathcal{L} [p_{ac}(-t) u(t)]_{(-s)} + \mathcal{L} [p_c(t) u(t)] \quad \mathcal{R}_c \cap \mathcal{R}_{ac}
	$$
	*Notice that $p_{ac}$ represents the acausal and $p_c$ represents the causal component.*
```
#### Revisit System Interconnections

Determine the overall TF given $H_i(s) = \mathcal{L}(h_i(t))$

1. Cascade (Series)
	![[Screenshot 2024-07-13 at 4.03.17 PM.png|300]]
	$$
	\begin{aligned}
	y(t) &= (x * h)(t) \quad \text{where} \quad h(t) = (h_1 * h_2)(t) \\ \\
	Y(s) &= H(s) X(s) \quad \text{where} \quad H(s) = H_1(s) H_2(s)
	\end{aligned}
	$$
2. Parallel
	![[Screenshot 2024-07-13 at 4.04.02 PM.png|300]]
	$$
	\begin{aligned}
	y(t) &= (x * h)(t) \quad \text{where} \quad h(t) = h_1(t) + h_2(t) \\ \\
	Y(s) &= H(s) X(s) \quad \text{where} \quad H(s) = H_1(s) + H_2(s)
	\end{aligned}
	$$
3. Feedback
	![[Screenshot 2024-07-13 at 4.02.59 PM.png||300]]
	$$
	\begin{aligned}
	Y(s) &= H_1(s) \left[ X(s) - H_2(s) Y(s) \right] \\ \\ &\Rightarrow \left( 1 + H_1(s) H_2(s) \right) Y(s) = H_1(s) X(s) \\ \\ &\Rightarrow Y(s) = H(s) X(s) \quad \text{where} \quad H(s) = \frac{H_1(s)}{1 + H_1(s) H_2(s)}
	\end{aligned}
	$$
#### Revisit BIBO Stability

1. A LTI with transfer function $H(s)$ and region of convergence $\mathcal{R}$ is *BIBO stable* if the $j \omega$ axis is *contained* in the RoC.
2. A causal LTI system with TF $H(s)$ is BIBO stable if the following **equivalent** conditions are satisfied.
	1. $\displaystyle H(s)= \mathcal{L}[h(t)] = \frac{N(s)}{D(s)} \quad$ $j \omega$-axis in RoC
	2. $\displaystyle \int_{-\infty}^{\infty}|h(t)| \ dt < \infty \quad$ $h(t)$ is absolutely integrable
	3. Poles of $H(s)$ are in the **open** *LHP* of the $s$-plane. (not including $j \omega$-axis).

## 5 – Frequency Analysis: Fourier Series

```ad-info
title: Recall
A periodic signal $x(t)$ is a signal that
1. Is defined for $-\infty < t < \infty$
2. $x(t+kT_0) = x(t), \ \forall k \in \mathbb{Z}$ where $\displaystyle T_0= \frac{2 \pi}{\omega_0}$

All periodic signals can be represented by a corresponding **Fourier Series**.
```
### Complex Exponential Representation

```ad-summary
title: Complex Exponential FS
The Fourier Series representation of a periodic signal $x(t)$ with *fundamental frequency* $\omega_0$ is given by an infinite sum of *weighted* complex exponentials (sines and cosines), with frequencies which are multiples of $\omega_0$.
$$
\displaystyle x(t)= \sum_{k=-\infty}^{\infty}X_k \, e^{j \, k \, \omega_0 t}
$$
where the **Fourier Coefficients** $\{X_k\}$ are found according to
$$
\displaystyle X_k = \frac{1}{T_0}\int_{t_0}^{t_0+T_0}x(t)e^{-j \, k \, \omega_0 t} \ \ dt
$$
For $k= 0, \pm1, \pm2, ...$

Notice that $x(t)e^{j \, k \, \omega_0 t} \equiv x(t)e^{-s t}|_{s=jk\omega_0}$. This is similar to Laplace but now we are just considering the frequency part.
```

```ad-tip
title: Remarks
1. $X_0$ is the DC value, or the average of $x(t)$
2. The function set $\displaystyle \{e^{jk\omega_0 t}\}$, $k \in \mathbb{Z}$, provides an **orthonormal basis** for all *periodic functions with fundamental period $T_0$*.
	$$
	\left\langle e^{jk\omega_0 t}, e^{jl\omega_0 t} \right\rangle = \frac{1}{T_0} \int_{t_0}^{t_0 + T_0} e^{jk\omega_0 t} \left[ e^{jl\omega_0 t} \right]^* dt = \begin{cases} 1 & \text{if } k = l \\ 0 & \text{if } k \ne l \end{cases} \quad k, l \in \mathbb{Z}
3. The square magnitude of the **Fourier Coefficients**, provides the **power spectrum** – the power distribution showing each frequencies contribution. 
	$$
	\displaystyle \{|X_k|^2\} = \{X_kX_k^*\}
	$$
4. **Parseval’s power relation**: The orthonormality of the basis functions results in superposition of power. The power of a periodic function can be calculated in either the time, or frequency domain:
	$$
	P_X = \frac{1}{T_0} \int_{t_0}^{t_0+T_0}|x(t)|^2 \ dt = \sum_{k=-\infty}^{\infty} |X_k|^2 \quad \text{for any } t_0
	$$
5. For a *real-valued* periodic signal $x(t)$, with fundamental period $T_0$ represented in the frequency domain by the fourier coefficients *$\displaystyle \{X_k=|X_k|e^{j \angle X_k}\}$* we have that
	$$
	X_k = X_{-k}^*
	$$
	Or equivalently
	$$
	\displaystyle
	\begin{aligned}
	(1)& \quad |X_k| = |X_{-k}| \quad \text{Magnitude $|X_k|$ is an even function of $k \omega_0$} \\ \\
	(2)& \quad \angle X_k = -\angle X_{-k} \quad \text{Phase $\angle X_k$ is an odd function of $k \omega_0$} \\ \\
	\end{aligned}
	$$
	**Takeaway**
	
	For *real-valued signals*, we only need to display for $k \geq 0$ the *magnitude line spectrum* (i.e. a plot of $|X_k| \text{ vs } k \omega_0$), as well as the *phase line spectrum* (i.e. a plot of $\angle X_k \text{ vs } k \omega_0$).
	$$ $$
6. Suppose we are given a magnitude and phase spectra. When $|X_k|=0$ the **phase isn't important**.
	
	![[Screenshot 2024-07-14 at 2.29.20 PM.png]]
	
	*The highlighted vertical lines aren't important in the frequency domain because $|X_k|=0$ at that point*
	
```


```ad-summary
title: Power Line Spectrum & Symmetry
The periodic signal $x(t)$, with fundamental period $T_0$ is represented in the *frequency domain* by its:
$$
\text{Magnitude Line Spectrum} \quad |X_k| \text{ vs } k \omega_0
$$
$$
\text{Phase Line Spectrum} \quad \angle X_k \text{ vs } k \omega_0
$$
The **Power Line Spectrum** $|X_k|^2 \text{ vs } k \omega_0$ displays the distribution of the power of the signal over frequency.
```


```ad-summary
title: Convergence of Fourier Series
The *Fourier Series* of a *piecewise smooth* (continuous or discontinuos) periodic signal $x(t)$ **converges for all values of $t$**. The mathematician Dirichlet showed that for the Fourier series to converge to the periodic signal $x(t)$, the signal should satisfy the following *sufficient* (not necessary) conditions *over a period*.
1. be absolutely integrable
2. have a finite number of maxima, minima and discontinuities. 

The infinite series equals $x(t)$ at every continuity point, and equals the average of the right-hand limit, and left-hand limit at every discontinuity point $\displaystyle 0.5[x(t+0_+)+x(t+0_-)]$.
```

### Trigonometric Representation

```ad-summary
title: Trigonometric Fourier Series
The **Trigonometric Fourier Series** representation of a *real-valued*, *periodic* signal $x(t)$, is an equivalent Fourier representation, using *sinusoids* instead of *complex exponentials*, as the **basis functions**.
$$
\begin{aligned}
x(t) \quad &= \quad X_0 + 2 \sum_{k=1}^{\infty}|X_k|\cos(k \omega_0 t + \theta_k) \\ \\
&= \quad c_0 + 2 \sum_{k=1}^{\infty}c_k\cos(k \omega_0 t)+d_k\sin(k \omega_0 t) \\ \\
\text{Where we have } \quad & \omega_0=\frac{2\pi}{T_0} \quad \text{and} \quad X_k=|X_k|e^{j \theta_k}
\end{aligned} 
$$
Again, *$X_0$ is called the **DC-component**,* and  $\{2|X_k|\cos(k \omega_0 t + \theta_k)\}$ are the **$k$th harmonics** for $k=1,2,3,...$

The coefficients $\{c_k,d_k\}$ are obtained from the following
$$
\begin{aligned}
\displaystyle
c_k \quad &= \quad \frac{1}{T_0} \int_{t_0}^{t_0+T_0}x(t)\cos(k \omega_0 t) dt \quad \text{for } k=0,1,2,...\\ \\
d_k \quad &= \quad \frac{1}{T_0} \int_{t_0}^{t_0+T_0}x(t)\sin(k \omega_0 t) dt \quad \text{for } k=1,2,...
\end{aligned}
$$
**Notice**
- $c_k$ corresponds to $Re(X_k)$. These account for the *even component of $x(t)$*.
- $d_k$ corresponds to $-Im(X_k)$. These account for the *odd component of $x(t)$*.

**Connection**
The coefficients $X_k=|X_k|e^{j \theta_k}$ are connected with the coefficients of $c_k$ and $d_k$ by
$$
\begin{aligned}
|X_k| &= \sqrt{{c_k}^2+{d_k}^2} \\ \\ 
\theta_k &= \tan^{-1}\Big[\frac{d_k}{c_k}\Big]
\end{aligned}
$$
- The sinusoidal basis function $\{\sqrt{2}\cos(k \omega_0 t),\sqrt{2}\sin(k \omega_0 t)\}$, $k=0,1,2,...$ are *orthonormal* in $[0,T_0]$.
```


### FS Coefficients from LT

For a periodic signal $x(t)$ with period $T_0$, if we know, or can easily compute the LT of a *period* of $x(t)$
$$
x_1(t)=x(t)[u(t-t_0)-u(t-t_0-T_0)]
$$
Then the *Fourier Coefficients* are given by
$$
\displaystyle X_k=\frac{1}{T_0}\mathcal{L}[x_1(t)]_{s=jk \omega_0} \quad \text{for } k=0,\pm1,\pm2,...
$$

### Pure Sinusoid from Eigenfunction

```ad-summary
For a *stable LTI system*, any input signal of the form $e^{j\omega t}$ is an eigenfunction. This means for any function $f(t)\in\{e^{j\omega t}\}$ where $\omega = k \omega_0$ for $k=1,2,3,...$.

Consider representations of a pure sinusoid in both the $t$-domain and $s$-domain:
$$
\begin{aligned}
x(t) \quad &= \quad A\cos(\omega t + \theta) \quad \text{$t$-domain eigenfunction} \\ \\
\displaystyle &= \quad A \Big(\frac{e^{j (\omega t+\theta)} + e^{-j (\omega t + \theta)}}{2}\Big) \quad = \quad \frac{A}{2}e^{j \theta} e^{j \omega t} + \frac{A}{2}e^{-j \theta} e^{-j \omega t} \\ \\
& \qquad\qquad\qquad\qquad\qquad \qquad \quad \ \   = \quad X_1 e^{j \omega t} + X_{-1} e^{-j \omega t}
\end{aligned}
$$
*Notice that $e^{\pm j\omega t}$ are the **Eigenfunctions**, and $\frac{A}{2}e^{\pm j \theta}$ are the **Fourier Coefficients**.* 

![[Screenshot 2024-07-14 at 5.13.57 PM.png|600]]
```

### Frequency Response

```ad-summary
title: Frequency Response
**Eigenfunction Property**
In *steady state*, the response to a *complex exponential (or a sinusoid)* $x(t)$, of a certain frequency is the same complex exponential (or sinusoid), but its *amplitude* and *phase* are affected by the **frequency response** of the system at that frequency.

If the input $x(t)$ to a *causal* and *stable* LTI System, with impulse response $h(t)$, is periodic, of fundamental period $T_0$ and has the Fourier Series
$$
x(t) \quad = \quad X_0+2\sum_{k=1}^{\infty}|X_k| \cos(k \omega_0 t + \angle X_k)
$$
The **Steady-state response** of the system is
$$
\displaystyle y(t)  =  X_0|H(j0)| +2\sum_{k=1}^{\infty}|X_k||H(jk\omega_0)| \cos\Big(k \omega_0 t + \angle X_k + \angle H(jk\omega_0)\Big)
$$
where
$$
\displaystyle H(jk\omega_0) \ = \ |H(jk\omega_0)|e^{j \angle H(jk\omega_0)} \ = \ \int_{0}^{\infty}h(\tau)e^{-jk\omega_0 \tau} \ d\tau \ = \ H(s)|_{s=jk \omega_0}
$$
*Notice that the **Transfer Function** $H(s)$ gives us the **Frequency Response** of the system, $H(jk \omega_0)$, at $k \omega_0$.*
```


```ad-check
title: Freq Response from Eigenfunctions
- If we have eigenfunction $x_1(t)=e^{j \omega t}$ as input into the system, we get $\displaystyle y_1(t)=|H(j \omega)|e^{j \omega t + \angle H(j \omega)}$.
- If we have eigenfunction $x_2(t)=e^{-j \omega t}$ as input into the system, we get $\displaystyle y_2(t)=|H(j \omega)|e^{-j \Big(\omega t + \angle H(j \omega)\Big)}$.

In general we have
$$
\begin{aligned}
\text{For $j \omega$} \qquad &x(t)=e^{j \omega t} \quad \rightarrow \quad y(t) \ = \ H(j \omega)e^{j \omega t} \ = \ |H(j \omega)|e^{j (\omega t + \angle H(j \omega))} \\ \\
\text{For $-j \omega$} \qquad &x(t)=e^{-j \omega t} \quad \rightarrow \quad z(t) \ = \ H(-j \omega)e^{-j \omega t} \ = \ |H(-j \omega)|e^{-j (\omega t - \angle H(-j \omega))} \\ \\ & \qquad \qquad \qquad \qquad \qquad \qquad \qquad \qquad \qquad \, \,  = \ |H(j \omega)|e^{-j (\omega t + \angle H(j \omega))}
\end{aligned}
$$
For negative frequencies, by the *even property* of FS coefficient magnitudes, and the *odd* property of FS coefficent phases, this is the reason we get the result $y(t)=z^*(t)$.

![[Screenshot 2024-07-14 at 5.29.59 PM.png]]

![[Screenshot 2024-07-14 at 5.45.01 PM.png]]
```

### Reflection & Even / Odd Decomposition

```ad-summary
title: Reflection C&A
If the fourier coefficients of a signal $x(t)$ are $\{X_k\}$ then those of $x(-t)$, the time-reversed signal with the same period as $x(t)$, are $\{X_{-k}\}$.

**Even periodic signal** *$x(t)$*
Its Fourier Coefficients $X_k$ are **real**, and its trigonometric Fourier Series is
$$
\displaystyle x(t)=X_0+2\sum_{k=1}^{\infty}X_k \cos(k \omega_0 t)
$$

**Odd periodic signal** *$x(t)$*
Its Fourier Coefficients $X_k$ are **imaginary**, and its trigonometric Fourier Series is
$$
\displaystyle x(t)=2\sum_{k=1}^{\infty}jX_k \sin(k \omega_0 t)
$$

Recall that any periodic signal can be written as $x(t)=x_e(t)+x_o(t)$, the even-odd decomposition of a periodic signal. Then we have that
$$
X_k=X_{ek}+X_{ok}
$$
where $\{X_{ek}\}$ are the *even* FS coefficients to the fourier series of the function $x_e(t)$ and $\{X_{ok}\}$ are the *odd* FS coefficients to the fourier series of the function $x_o(t)$
$$
\begin{aligned}
X_{ek} = 0.5[X_k+X_{-k}] \\ \\
X_{ok} = 0.5[X_k-X_{-k}] 
\end{aligned}
$$
```

### Operations on Periodic Signals

```ad-summary
title: Addition of Periodic Signals
*Same fundamental frequency*
If $x(t)$ and $y(t)$ are periodic functions with the same fundamental frequency, then the *FS Coefficients* of the linear superposition of these two functions $z(t)=\alpha x(t) + \beta y(t)$ is an equivalent superposition of its individual, scaled coefficients.
$$
Z_k = \alpha X_k + \beta Y_k
$$

*Different fundamental frequency*
If $x(t)$ is periodic with fundamental period $T_1$, and $y(t)$ is periodic with fundamental period $T_2$, such that $\displaystyle \frac{T_2}{T_1} \ = \ \frac{N}{M}$, for **non-divisible integers**, $N$ and $M$, then $z(t)=\alpha x(t) + \beta y(t)$ is periodic of fundamental period $T_0=MT_2=NT_1$, and its Fourier Coefficients are
$$
Z_k = \alpha X_{k/N} + \beta Y_{k/M} \quad \text{for } k=0,\pm1,... \quad \text{such that $k/N$ and $k/M$ are integers}
$$
where $X_k$ and $Y_k$ are FS coefficients of $x(t)$ and $y(t)$ respectively.
```

```ad-summary
title: Product of Periodic Signals
*Same fundamental period*
If $x(t)$ and $y(t)$ are periodic signals of the same fundamental period $T_0$, then their product $z(t) = x(t) y(t)$ is also periodic of fundamental period $T_0$ and with Fourier coefficients which are the convolution sum of the Fourier coefficients of $x(t)$ and $y(t)$:
$$
Z_k = \sum_m X_m Y_{k-m}
$$

- [?] Explore what happens if you multiply periodic signals of different fundamental periods.
```

![[Screenshot 2024-07-14 at 6.21.47 PM.png]]

![[Screenshot 2024-07-14 at 6.22.30 PM.png]]

## 6 – Fourier Transform

```ad-summary
title: Fourier Transform
*Aperiodic* signals **do not** have a *Fourier Series*, but *might* have a **Fourier Transform**.

An aperiodic signal $x(t)$ can be thought of as a periodic signal $\tilde{x}(t)$ with an *ifinite fundamental period*. Using the *Fourier Series* representation of this signal and a limiting process, we obtain a **Fourier Transform Pair**.
$$
x(t) \iff X(\omega)
$$
where the signal $x(t)$ is transformed into the function $X(\omega)$ in the frequency domain by the
$$
\textbf{Fourier Transform:} \quad X(\omega) = \int_{-\infty}^{\infty}x(t)e^{-j \omega t} \ dt
$$
*replacing $s$ by $j \omega$ in the 2-sided LT yields this.*

while $X(\omega)$ is transformed into a signal $x(t)$ in the time-domain by the
$$
\textbf{Inverse Fourier Transform:} \quad x(t) = \frac{1}{2\pi} \int_{-\infty}^{\infty}X(\omega)e^{j \omega t} \ dt
$$
*Notice that the FS involves a discrete sequence of frequencies (DC + fundamental + harmonics as $k\omega_0$), the FT involves a continuum of frequencies.*
```

```ad-tip
title: Deriving FT from FS
Define an aperiodic signal $x(t)$ as a periodic signal $\tilde{x}(t)$ with infinite period $T_0 \rightarrow \infty$.
$$
\begin{aligned}
\displaystyle
&x(t) = \lim_{T_0 \rightarrow \infty}{\tilde{x}(t)} \\ \\

&\tilde{x}(t)=\sum_{k=-\infty}^{\infty}{X_k e^{j k \omega_0 t}}, \ \omega_0=\frac{2\pi}{T_0}, \ X_k = \frac{1}{T_0} \int_{-T_0/2}^{T_0/2}{\tilde{x}(t) e^{-j k \omega_0 t} \ dt} \qquad \text{From FS definition} \\ \\

&X(\omega_k)|_{\omega_k=k \omega_0}=T_0 X_k, \ 
\end{aligned}
$$
*incomplete refer to slide 6.3*
```
### Properties of FT

![[Screenshot 2024-07-15 at 11.59.42 AM.png]]
![[Screenshot 2024-07-15 at 11.59.19 AM.png]]

```ad-note
title: A Closer Look + Duality
- **Temporal Scaling Property** 
	$$\displaystyle \mathcal{F}[x(a t)] \ = \ \frac{1}{|a|}X(\frac{\omega}{a})$$
	
- **Frequency Scaling**
	$$\displaystyle \mathcal{F}^{-1}[X(\beta \omega)] \ = \ \frac{1}{|\beta|}x(\frac{t}{\beta})$$
	
- **Temporal Shifting** dual to **Frequency Shifting**
	$$
	\displaystyle
	\mathcal{F}[x(t-a)] = e^{-ja\omega}X(\omega) \quad \text{ dual } \quad \mathcal{F}[X(\omega-a)] = e^{jat}x(t)
	$$
- **Temporal Differentiation**
$$
\mathcal{F} \left[ \frac{d^n x(t)}{dt^n} \right] = (j\omega)^n X(\omega)
$$
- **Frequency Differentiation**
$$\mathcal{F}^{-1} \left[ \frac{d^n X(\omega)}{d\omega^n} \right] = (-jt)^n x(t)$$
- **Duality**
$$\mathcal{F}[x(t)] = X(\omega) \implies \mathcal{F}[X(t)] = 2\pi x(-\omega)
$$
```

### Dirichlet Conditions for Convergence

```ad-summary
title: FT Existence C&A
The Fourier Transform
$$
X(\omega) = \int_{-\infty}^{\infty}x(t)e^{-j \omega t} \ dt
$$
of a signal $x(t)$ *exists* (i.e., we can calculate its Fourier transform via this integral) provided
- $x(t)$ is *absolutely integrable*, i.e. the area under $|x(t)|$ is finite.
- $x(t)$ has only a *finite* number of *discontinuities*, and a finite number of *minima* and *maxima*, in any finite interval.

*notice these conditions are identical to that of the **convergence of the FS**.*
- [!] These are *sufficient*, but **not necessary** conditions for the FT to exist. Table 5.2, signals 3, 4, 6, 10 & 11 are NOT absolutely integrable but have FTs (it’s no coincidence that these FTs include the frequency-domain impulse).
```

### FT from LT

```ad-summary
title: FT from LT
If the *RoC* of $\hat{X}(s)=\mathcal{L}[x(t)]$ **contains** the $j \omega$-axis, then the FT of $x(t)$ is given by $\displaystyle X(\omega) \ = \ \mathcal{F}[x(t)] \ = \ \hat{X}(s)|_{s=j \omega}$
$$
\Big( \hat{X}(s)= \mathcal{L}[x(t)] \ \text{  and  } \  \text{$j \omega$-axis $\in$ RoC} \Big)  \implies \Big( X(\omega) \ = \ \hat{X}(s)|_{s=j \omega} \Big)
$$
*Takeaways*
- The FT can be regarded as a *special case* of LT where $s=j \omega$
- The LT is a function of a *2-D*, *complex* parameter, whereas the FT is a function of a
	*1-D*, *real-valued* frequency.
- As with the *Dirichlet conditions*, this provides a *sufficient*, but *not necessary*, condition for the FT to exist. Again, Table 5.2, signals 3, 4, 6, 10 & 11 do not include the $j \omega$-axis in their RoC.
```

### FT Calculation

```ad-tip
title: "Rules of Thumb" for FT calculation
Depending on the signal $x(t)$, *there are many valid ways to find the FT*. The goal is to *understand each of these methods*, and know *which ones to apply* in diffferent scenarios.
1. *Bounded* signal with a *finite* time support
> Use *FT integral directly* or *LT*
2. *Infinite* time signals that *include $j \omega$-axis* in RoC
> Use LT, substituting $s=j \omega$
3. *Periodic Signals*
> Use $\delta(\omega-k \omega_0)$ scaled by FS coefficients
4. *Signals not classified by 1-3*
> Apply FT Properties, FT Pairs, Duality, etc.
```

### Fourier Transform from Fourier Series

```ad-summary
title: FT of periodic signals
Representing a periodic signal $x(t)$, of period $T_0$, by its *Fourier Series*, we have the following *Fourier pair*:
$$
\displaystyle \Big( x(t) = \sum_{k}{X_k e^{jk\omega_0 t}} \Big) \iff \Big( X(\omega) = \sum_{k}{2 \pi X_k \delta(\omega - k \omega_0)} \Big)
$$
```

### Duality

```ad-info
title: Duality of FT and IFT
Notice the similarity between the FT and the IFT integrals.

$$X(\omega) = \int_{-\infty}^{\infty} x(t)e^{-j\omega t} \, dt \quad \quad x(t) = \frac{1}{2\pi} \int_{-\infty}^{\infty} X(\omega)e^{j\omega t} \, d\omega$$

**Aside:** The similarity is even more striking when the frequency parameter is in Hz instead of rad/s: $f = \omega/2\pi$

$$\hat{X}(f) = \int_{-\infty}^{\infty} x(t)e^{-j2\pi ft} \, dt \quad \quad x(t) = \int_{-\infty}^{\infty} \hat{X}(f)e^{j2\pi ft} \, df$$

The FT and IFT are duals of each other, allowing "mirroring" of concepts and pairs of equations. This means that problems solved in one domain (time or frequency) often allow for ready solutions in the complementary domain (frequency or time).
```

### Spectral Representation

```ad-summary
title: Parseval's Energy Relation
For an aperiodic, finite energy signal, $x(t)$, with Fourier Transform pair $X(\omega)$, its energy $E_X$ is *convserved* by the transformation:
$$
E_X=\int_{-\infty}^{\infty}|x(t)|^2 \ dt = \frac{1}{2 \pi}\int_{-\infty}^{\infty}|X(\omega)|^2 \ d\omega
$$
- $|X(\omega)|^2$ is an *energy density* – indicating the amount of energy at a given frequency $\omega$.
- Plotting $|X(\omega)|^2$ vs $\omega$ is called the *energy spectrum* of $x(t)$, and displays how the energy of the signal is spread out over its frequencies.
- $|x(t)|^2$ gives *temporal energy density* (energy per unit time)
- $\frac{1}{2 \pi}|X(\omega)|^2$ gives *frequency energy density* (energy per unit Hz; $\displaystyle \Delta f = \frac{\Delta \omega}{2 \pi}$)
```

```ad-summary
title: Energy Spectrum
If $X(\omega)$ is the Fourier transform of a *real-valued* signal, $x(t)$, periodic or aperiodic, the magnitude $|X(\omega)|$ and the real part $\mathcal{R}e \Big[ X(\omega) \Big]$ are *even functions* of $\omega$.
$$
\begin{aligned}
|X(\omega)| &= |X(-\omega)| \\ \\
\mathcal{R}e \Big[ X(\omega) \Big] &= \mathcal{R}e \Big[ X(-\omega) \Big]
\end{aligned}
$$
The phase $\angle X(\omega)$ and the imaginary part $\mathcal{I}m \Big[ X(\omega) \Big]$ are *odd functions* of $\omega$.
$$
\begin{aligned}
\angle X(\omega) &= -\angle X(- \omega) \\ \\
\mathcal{I}m \Big[ X(\omega) \Big] &= -\mathcal{I}m \Big[ X(-\omega) \Big]
\end{aligned}
$$
We call the plots
$$
\begin{aligned}
|X(\omega)| \ \text{ vs } \ \omega &\qquad \textbf{Magnitude Spectrum} \\ \\
\angle X(\omega) \ \text{ vs } \ \omega &\qquad \textbf{Phase Spectrum} \\ \\
|X(\omega)|^2 \ \text{ vs } \ \omega &\qquad \textbf{Energy/Power Spectrum}
\end{aligned}
$$
**Takeaway**

$x(t)$ is real-valued $\implies$ $X(-\omega) = X^{*}(\omega)$
```

### Frequency Response Applied to FT

```ad-summary
If the input $x(t)$ (periodic or aperiodic) of a *stable LTI system* has Fourier transform $X(\omega)$, and the system has a *frequency response* $H(j\omega) = \mathcal{F}[h(t)]$, where $h(t)$ is the impulse response of the system, the output of the LTI system is the convolution integral $y(t) = (x * h)(t)$, with Fourier transform

$$ \displaystyle
Y(\omega) = X(\omega) H(j\omega)
$$

In particular, if the input signal $x(t)$ is periodic, the output is also periodic of the same fundamental period, and with Fourier transform

$$ \displaystyle
Y(\omega) = \sum_{k=-\infty}^{\infty} 2\pi \  X_k \ H(jk\omega_0)  \ \delta(\omega - k\omega_0)
$$

where $\{X_k\}$ are the Fourier series coefficients of $x(t)$ and $\omega_0$ its fundamental frequency.
```

## 7 – Sampling

Many *signals* in the real world are *analog* in nature. Digital hardware (i.e. computers) cannot directly use *analog signals* as it would require infinite memory. So the first step in an Analog-to-Digital (ADC) converter, is **sampling** to generate a **discrete-time signal** from a **continuous-time** one.

![[Screenshot 2024-07-20 at 1.48.24 PM.png]]
*a figure depicting an analog-to-digital converter. First, the analog signal is sampled, using a sampler, (thus having discrete domain), then quantizer for a finite set of values, then encoder for the computer (in bits)* 

![[Screenshot 2024-07-20 at 1.50.18 PM.png]]
*note that $\omega_0=30\pi$ so $f=15 \ Hz$*
### Uniform Sampling

```ad-summary
title: Sampler
![[Screenshot 2024-07-20 at 1.53.21 PM.png]]

**Concept**: The process of converting a continuous-time (CT) signal to a discrete-time (DT) signal.

**Sampler Transformation**:
- **Input**: $x : \mathbb{R} \to \mathbb{C}$
- **Output**: $y : \mathbb{Z} \to \mathbb{C}$

**Equation**:
$$
y = \text{Sampler}_T(x): \forall n \in \mathbb{Z}, y[n] = x(nT)
$$

**Parameters**:
- $T$: Sampling interval
- $f_s = \frac{1}{T}$: Sampling frequency or sampling rate

**Q1: Is \( y \) a digital signal?**
- **A1**: Not yet...values aren’t quantized yet.

**Q2: As the engineer, if you can specify the sampling rate, how should you do so?**
- higher sampling rate generally means better approximation of CT signal (less information loss) but requires more resources (in memory, processing power, cost, etc.)
- This depends on the highest frequencies expected. A remarkable fact is that certain sampling conditions allow for perfect reconstruction of the original signal (no loss of information)!

**Key Points**:
- **Bridge**: The sampling process acts as a bridge converting CT signals to DT signals.
- **Remarkable Fact**: Under certain conditions, perfect reconstruction of the original signal is possible.
```

```ad-summary
title: Aliasing
**Aliasing** is an effect that causes two different sampled signals to become indistinguishable in the output (after sampling).
$$
\text{Aliasing occurs when} \ x \neq v \quad \textbf{but} \quad Sampler_T(x) = Sampler_T(v)
$$
$$
\begin{aligned}
\textbf{Example} \qquad x(t)=\cos(2\pi f t) \quad \text{and} \quad v(t)=\cos(2\pi (f+Nf_s) t) \\ \\
y_1[n]=\cos(2\pi f nT_s) \equiv y_2[n]=\cos(2\pi (f+\frac{N}{T_s}) nT_s)
\end{aligned}
$$
```

```ad-info
title: Sampler System Property
The Sampler system $Sampler_T(x)$ is **Linear** but is **Time-varying**. Thus, it is *not an LTI system*.
```

### Reconstruction (from DT signal)

![[Screenshot 2024-07-20 at 3.22.52 PM.png]]
Given a signal, $x$, we want to try to reconstruct the analog signal after sampling.
- *Aliasing* results in the loss of information, often the reconstructed signal $x_r$ will not match the input signal $x$.

```ad-summary
title: Reconstruction Methods
Suppose we have the DT-signal $y[n]$ and we want to try to reconstruct the original signal $x_r$.
![[Screenshot 2024-07-20 at 3.28.34 PM.png]]

The reconstruction stages are:
1. input $y[n]$ into an *ideal impulse generator*
2. Use one of *Zero-Order Hold*, *First-Order Hold*, or *Ideal Interpolation*

![[Screenshot 2024-07-20 at 3.26.44 PM.png]]
*the figure shows common reconstruction methods. Taking the DT-signal, convoluting with an impulse train, then into an LTI system.*

![[Screenshot 2024-07-20 at 3.32.21 PM.png]]
*basically just unit step*

![[Screenshot 2024-07-20 at 3.32.45 PM.png]]
*Linear relation between points*

![[Screenshot 2024-07-20 at 3.33.16 PM.png]]
**

![[Screenshot 2024-07-20 at 3.33.59 PM.png]]
![[Screenshot 2024-07-20 at 3.34.08 PM.png]]
![[Screenshot 2024-07-20 at 3.35.13 PM.png]]
```

### Ideal Interpolator

```ad-summary
title: Ideal Interpolator
Note that the *Ideal Interpolator* can either refer to the system $DiscToCont_T: \ [y \rightarrow x_r]$ or the LTI system $\mathcal{S}: Sinc_T$

$$
\text{IdealInterpolator}_T = \text{Sinc}_T \circ \text{ImpulseGen}_T
$$
```

### Nyquist-Shannon Sampling Theorem

```ad-summary
title: Nyquist-Shannon Sampling Theorem C&A

If a *low-pass continuous-time signal* $x(t)$ is *band-limited* (i.e., it has a spectrum $X(\omega)$ such that $X(\omega) = 0$ for $|\omega| > \omega_{max}$, where $\omega_{max}$ is the *maximum frequency* in $x(t)$) we then have:

The information in $x(t)$ is *preserved* by a sampled signal $x_s[n]$, with samples $x_s[n] = x(nT_s)$ at $t = nT_s, n = 0, \pm 1, \pm 2, \ldots$, provided that the sampling frequency $\omega_s = \frac{2\pi}{T_s}$ (rad/sec) is such that

$$
\omega_s > 2\omega_{max} \quad \text{(Nyquist sampling rate condition)} \quad (8.17)
$$

or equivalently if the sampling rate $f_s$ (samples/sec) or the sampling period $T_s$ (sec/sample) are

$$
f_s = \frac{1}{T_s} > \frac{\omega_{max}}{\pi} \quad (8.18)
$$

**Note**: Often mistakenly includes equality. Equality doesn’t cause aliasing but likely results in lost information.

When the Nyquist sampling rate condition is satisfied, the original signal $x(t)$ can be reconstructed by passing the sampled signal $x_s[n]$ through *impulse generator cascaded* to an *ideal low-pass filter* with frequency response:

$$
H(j\omega) = 
\begin{cases} 
T_s \left[ u\left(\omega + \frac{\omega_s}{2}\right) - u\left(\omega - \frac{\omega_s}{2}\right) \right] & -\frac{\omega_s}{2} < \omega < \frac{\omega_s}{2} \\
0 & \text{otherwise}
\end{cases}
$$

The reconstructed signal is given by the following sinc interpolation from the samples:

$$
x_r(t) = \sum_{n=-\infty}^{\infty} x(nT_s) \frac{\sin(\pi (t - nT_s) / T_s)}{\pi (t - nT_s) / T_s} \quad (8.19)
$$

```

```ad-tip
title: Nyquist Frequency vs Nyquist Rate

The **Nyquist frequency** is a property of a *sampler*. It is the upper bound of an input signals frequency so that it can be completely recovered through ideal interpolation.
$$\displaystyle
f_{Nyquist}=\frac{f_s}{2} \qquad (X(f)=0 \ \text{i.e. band-limited signal} \ \forall f \geq f_{Nyquist} ) \implies x(t) \ \text{recoverable}
$$

The **Nyquist (sampling) rate** is a property of a signal, or set of signals. given a *bandlimited* signal with max frequency $f_{max}$, the Nyquist rate is the minimum sampling rate required for complete recovery of the signal through ideal interpolation.
$$
f_{Nrate} > 2 f_{max}
$$

For the edge case of $x(t)$ with freq $f_{Nyquist}$ exactly, there is no aliasing, but the signal is *not recoverable* (most of the time). This occurs where there is a phase shift $\phi$, which scales the perceived amplitude of the signal.
```

![[Screenshot 2024-07-21 at 9.01.46 PM.png]]






## Chapter 8 – DT Signals & Systems

```ad-abstract
title: DT Signal
$x[n]$, is a discrete-time signal, real or complex valued function of an integer sample index: $X: \mathbb{Z} \rightarrow \mathbb{R} (\mathbb{C})$. Note that $n$ has units of a *sample* or *step*
```

```ad-summary
title: Periodicity
A DT signal $x[n]$ is periodic if
- defined for all possible values of $n \in mathbb{Z}$
- there is a positive integer $N$ (of *steps* or *samples*) such that 
$$x[n+kN]=x[n]$$

Periodic DT sinusoids are of the form
$$
x[n]=Acos(\frac{2\pi m}{N}n+\theta) \qquad n \in \mathbb{Z}
$$
Where the *discrete frequency* is given by $\Omega_0=\frac{2 \pi m}{N}$ rad/step
```



