---
title: Signals And Systems Review
date: 2023-04-23 08:45:40
categories: Review
tags:
  - study
  - signals and systems
  - review
description: Review notes of Signals and Systems.
---

<p style="opacity: 0.7;">Start Notes: 

<small style="opacity: 0.7;">This course is the first part of EIE's "Information Processing" course but I prefer the name "Signals and Systems" which is the one we had together with EEE students. It established the basis for signal processing and was used in Control Systems course later.  </small>

---

## Introduction

|    | Continuous time     |       | Discrete time       |
| -- | ------------------- | ----- | ------------------- |
| t  | continuous time(s)  | n     | discrete time index |
|    |                     | $T_s$ | sampling period     |
|    |                     | $f_s$ |sampling frequency(s)|
|$\omega$| angular freq (rads/sec) |$\Omega = \omega T = \frac {2 \pi f} {f_s}$ | normalised angular freq (rads/sec) |
|$x(t)$| signal            | $x[n]$ | signal             |
|$X(j\omega)$| Fourier transform of $x(t)$ | $X(e^{j\Omega})$ | Discrete-time Fourier transform of $x[n]$ |
|    |   | $x[k]$ | Discrete Fourier transform of $x[n]$ |
|$X(s)$| Laplace transform of x(t)| $X(z)$ | z-transform of $x[n]$ |

Lower case used for time-domain functions. 

Upper case used for transform-domain functions. 

### Continuous-time and Discrete-time Signals

Discrete-time signals may be intrinsically discrete or may be obtained from continuous-time signals by sampling the amplitude at $t=nT_s$. 

$$ x[n] = x(nT_s) $$

### Symmetric Signals (even or odd)

Any signal may be written as the sum of an even-symmetric signal $x_e(t)$ and an odd-symmetric signal $x_o(t)$. 

$$ x(t) = x_e(t) + x_o(t) $$

$$ x_e(t) = \frac {1} {2} (x(t)+x(-t)) $$

$$ x_o(t) = \frac {1} {2} (x(t)-x(-t)) $$

### Periodic and Non-periodic Signals

$$ x(t) = x(t + T_0) \quad \forall t $$

$$ x(t) = x(t + \alpha T_0) \quad \forall \alpha \in \mathbb{Z} $$

$T_0$ : the **fundamental period** in seconds

$\omega_0 = \frac {2\pi} {T_0} $ : the **fundamental angular frequency** in rads/sec

$$ x[n] = x[n + N_0] \quad \forall n $$

$$ x[n] = x[n + \alpha N_0] \quad \forall \alpha \in \mathbb{Z} $$

$N_0$ : the **fundamental period** in samples

$\Omega_0 = \frac {2\pi} {N_0} $ : the **fundamental angular frequency** in rads/sample

### Deterministic and Random Ssignals

Uncertainty at any observation instant in time? 

### Energy and Power Signals

$$ E = \lim_{T \to \infty}\int_{-T/2}^{T/2}x^2(t)dt = \int_{-\infty}^{\infty}x^2(t)dt $$

$$ P = \lim_{T \to \infty}\frac{1}{T}\int_{-T/2}^{T/2}x^2(t)dt $$

$$ E = \sum_{n = -\infty}^\infty x^2[n] $$

$$ P = \lim_{N \to \infty}\frac{1}{N}\sum_{n = 0}^{N-1} x^2[n] $$

A signal is an **energy signal** iff $ 0 < E < \infty $. Energy signals have 0 average power. 

A signal is a **power signal** iff $ 0 < P < \infty $. Power signals have infinite energy. 

### Special Signals

<mark style="background-color: #9fc5e8;">Unit impulse function</mark> (discrete)

![$$\delta (t) = \left\{\begin{matrix} 0, t \not= 0 \\∞, t = 0 \end{matrix}\right.$$](https://github.com/thisisamor/blog_pic/blob/main/year2/Signals_and_Systems/unit-impulse.jpg?raw=true)

<mark style="background-color: #9fc5e8;">Dirac delta fucntion</mark> (continuous)

$$ \int_{-\infty}^{\infty} \delta(t)dt = 1 \quad and \quad \delta (t) = 0 \ for \ t \not= 0 $$

<mark style="background-color: #9fc5e8;">Unit step function</mark> 

![unit step](https://github.com/thisisamor/blog_pic/blob/main/year2/Signals_and_Systems/unit-step.jpg?raw=true)


Relationship: 

$$ \delta (t) = \frac {d} {dt} u(t) $$

$$ u(t) = \int_{-\infty}^{t} \delta(\tau) d\tau $$

![graph1](https://github.com/thisisamor/blog_pic/blob/main/year2/Signals_and_Systems/graph1.jpg?raw=true)

<mark style="background-color: #9fc5e8;">Complex exponential</mark> 

$$ x(t) = e^{j\omega_0t} $$

<mark style="background-color: #9fc5e8;">Cosine and sine</mark> 

$$ cos(\omega_0 t) = \frac {1} {2} ( e^{j\omega_0t} + e^{-j\omega_0t} ) $$

$$ sin(\omega_0 t) = \frac {1} {2} ( e^{j\omega_0t} - e^{-j\omega_0t} ) $$

![graph2](https://github.com/thisisamor/blog_pic/blob/main/year2/Signals_and_Systems/graph2.jpg?raw=true)


### Signal Operators

- Amplification
  $$ y(t) = cx(t) \qquad y[n] = cx[n] $$

- Addition
  $$ y(t) = x_1(t) + x_2(t)  \qquad y[n] = x_1[n] + x_2[n] $$

- Multiplication (Modulation)
  $$ y(t) = x_1(t)x_2(t) \qquad y[n] = x_1[n]x_2[n] $$

- Differentiation
  $$ y(t) = \frac {d}{dt} x(t) $$

- Integration
  $$ y(t) = \int_{-\infty}^{t}x(\tau)d\tau $$

- Time scaling
  $$ y(t) = x(at) \qquad y[n] = x[kn] $$

- Time shifting 
  $$ y(t) = x(t-t_0) $$

- Time reflection
  $$ y(t) = x(-t) $$

### Precedence Rule

Consider the operation on $x(t)$ denoted by $y(t)=x(at+t_0)$, <mark style="background-color: #9fc5e8;">the precedence rule states that time-shift is done first, then time-scaling. 

### System

System id the operation, or combination of operations applied to an input signal. The result of applying the operations to the input signal is termed the output signal. 

$$ y(t) = H \lbrace x(t) \rbrace $$

$$ y[n] = H \lbrace x[n] \rbrace $$

H is the **system function**. 

H may be linear or non-linear function. 

H may be time-invariant or time-varying function. 

- eg: Moving average system. 

### Properties of systems

#### Linearity

iff it satisfies the **superposistion and homogeneity** properties. 

- Superposition
  
  iff $y_1(t)+y_2(t)$ is the output of input $x_1(t)+x_2(t)$. 

- Homogeneity

  iff $ay(t)$ is the output of $ax(t)$. 

- Linearity 

  when input $ x(t) = \sum_{i=1}^{N} a_i x_i(t) $, output $ y(t) = \sum_{i=1}^{N} a_i y_i(t) $

$$ y(t) = H \lbrace \sum_{i=1}^{N} a_i x_i(t) \rbrace = \sum_{i=1}^{N} a_i H \lbrace x_i(t)\rbrace = \sum_{i=1}^{N} a_i y_i(t) $$

H must commute with scaling and summation. 

#### Time Invariance

iff H does not change over time. 

*For discrete signals, it is often called "linear shift invariance".*

$$ let \ x_2(t) = x_1(t-\tau) = S^\tau \lbrace x_1(t) \rbrace $$

$$ y_1(t-\tau) = S^\tau \lbrace y_1(t) \rbrace = S^\tau H \lbrace x_1(t) \rbrace$$

$$ y_2(t) = H \lbrace x_2(t) \rbrace = \lbrace x_1(t-\tau) \rbrace = H S^\tau \lbrace x_1(t) \rbrace $$

iff $ y_2(t) = y1(t-\tau) $ when $ x_2(t) = x_1(t-\tau) $

that is, $ HS^\tau = S^\tau H $

#### Memory

Memoryless: iff the output signal is a function of **only the present value** of the input signal. 

With memory: depends on values of the input signal at any past (or future) time. 

#### Stability

iff **the output signal is bounded when the input signal is bounded**. 

**Bounded input, bounded output (BIBO) stable**

$$ \vert y(t) \vert \leq M_y < \infty \ given \ \vert x(t) \vert \leq M_x < \infty , \forall t $$

#### Causality 因果

iff the output signal at the present time **depends only on the input signal at the present or past times**. 

- Causal (right-sided)
  only present and past are required

- Anticausal (left-sided)
  only future are required

- Noncausal (two-sided)
  present, past and future are all required

### System architecture

- Single-input-single-output (SISO)

- Multiple-input-multiple-output (MIMO)


---

## LTI Systems

### Impulse Response

The impulse responde **completely** characterizes the behaviour of an LTI system. 

- discrete time
  input $\delta[n]$, output $h[n] = H\lbrace \delta[n]\rbrace$

- continuous time
  input $\delta(t)$, output $h(t) = H\lbrace \delta(t)\rbrace$

The impulse response of an unknown system can be measured by applying an impulse to the input and measure the output response. 
- Discrete: unit impulse function
- Continuous: approx. Dirac delta function

### Convolution

The **output of an LTI system** is given by <mark style="background-color: #9fc5e8;">the convolution of the input signal with the system's impulse response</mark>. 

$$ y(t) = x(t) * h(t) $$

$$ y[n] = x[n] * h[n] $$

#### Convolution Sum

In terms of delayed and scaled unit impulse functions: 
$$ x[n] = \sum_{k=-\infty}^{\infty} x[k]\delta[n-k] $$

$$ y[n] = H \lbrace \sum_{k=-\infty}^{\infty} x[k]\delta[n-k] \rbrace = \sum_{k=-\infty}^{\infty} x[k]H \lbrace \delta[n-k] \rbrace $$

$$ y[n] = \sum_{k=-\infty}^{\infty} x[k]h[n-k] $$

#### Convolution Integral

In terms of delayed and scaled unit impulse functions: 
$$ x(t) = \int_{-\infty}^{\infty} x(\tau)\delta(t-\tau) d\tau $$

$$ y(t) = x(t)*h(t) = \int_{-\infty}^{\infty} x(\tau)h(t-\tau) d\tau $$

#### Step Response

The output in response to a unit step input. 

$$ s[n] = \sum_{k=-\infty}^{n} h[k] $$

$$ s(t) = \int_{-\infty}^{t} h(\tau)d\tau $$


### Differential and Difference Representations

#### Differential equation (continuous)

$$ \sum_{k=0}^{N} a_k \frac {d^k y(t)}{dt^k} = \sum_{k=0}^{M} b_k \frac {d^k x(t)}{dt^k} $$

#### Difference equation (discrete)

$$ \sum_{k=0}^{N} a_k y[n-k] = \sum_{k=0}^{M} b_k x[n-k] $$

*$a_k$ and $b_k$ are constant coefficients, N and M describe the order.*

$$ y[n] = \frac{1}{a_0}\sum_{k=0}^{M} b_k x[n-k] - \frac{1}{a_0} \sum_{k=0}^{N} a_k y[n-k] $$

#### Roots of the characteristic equation 

|           | Homogeneous equation     |  Solution form   |  Characteristic equation |
|-----------|--------------------------|------------------|--------------------------|
|Continuous |$\sum_{k=0}^{N}a_k\frac{d^ky^{(h)}(t)}{dt^k}=0$|$y^{(h)}(t)=\sum_{i=1}^{N}c_ie^{r_it}$|$\sum_{k=0}^{N}a_kr^k=0$|
|Discrete |$\sum_{k=0}^{N}a_ky^{(h)}[n-k]=0$|$y^{(h)}[n]=\sum_{i=1}^{N}c_ir_i^n$|$\sum_{k=0}^{N}a_kr^{N-k}=0$|

*$r_i$ are the roots of the characteristic equation.*

#### Natural response

Components due only to the initial conditions: $y^{(nr)}$

#### Forced response

Component due only to the input signal: $y^{(fr)}$

### Block Diagrams

![block diagram](https://github.com/thisisamor/blog_pic/blob/main/year2/Signals_and_Systems/block-diagram.jpg?raw=true)

### Frequency Response

- the system's **gain** as a continuous function of frequency
- the systems's effect on the **amplitude and phase** of a sinusoidal input with frequency varied across the frequency range of interest

For $ x(t) = e^{j\omega t}$: 

$$ y(t) = \int_{-\infty}^{\infty} x(\tau)h(t-\tau) d\tau = \int_{-\infty}^{\infty} h(\tau)x(t-\tau) d\tau = \int_{-\infty}^{\infty} h(\tau)e^{j\omega (t-\tau)}d\tau = e^{j\omega t}\int_{-\infty}^{\infty} h(\tau)e^{-j\omega \tau}d\tau = e^{j\omega t}H(j\omega) $$

$\int_{-\infty}^{\infty} h(\tau)e^{-j\omega \tau}d\tau$ is the gain, so:

$$ H(j\omega) = \int_{-\infty}^{\infty} h(\tau)e^{-j\omega \tau} d\tau$$

Similarly: 

$$ H(e^{j\Omega}) = \sum_{k=-\infty}^{\infty} h[k]e^{-j\Omega k} $$

- The output y is a complex sinusoid of same frequency as the input x. 
- Frequency response H is a function of **frequency**, not of time. 
- Frequency response H is a **complex value** at each frequency.

--- 

## Sampling

**Frequency spectrum**: $X(j\omega)$, describes the magnitude and phase of each frequency that the signal contains. 

$\omega_m$: the highest frequency contained in $x(t)$. 

$T_s$: the sampling interval (period) in seconds. 

$f_s = \frac {1}{T_s}$: the sampling frequency in samples per second. 

$\omega_s=\frac {2\pi}{T_s}$

$n$: the sample index

### Sampling Theorem

$x(t)$ is completely determined by $x(nT_s)$, if $\omega_s>2\omega_m$. 

- all the information is captured
- can be perfectly reconstructed

### Sampling Process

For a sampling function $p(t)$, with period $T_s$ and $\omega_s =\frac {2\pi}{T_s}$, its spectrum is: 
$$ P(j\omega) = \frac {2\pi}{T_s} \sum_{k=-\infty}^{\infty} \delta(\omega-k\omega_s) $$

<mark style="background-color: #9fc5e8;">The product of two signals $s(t)=q(t)r(t)$ in the time domain corresponds to **$\frac{1}{2\pi} \times $ the convolution** of their spectra in the frequency domain.</mark>

(see example 2 in problem sheet.)

### Aliasing 混叠

**Nyquist Sampling Criterion**: the samples uniquely represent a signal only when we impose the rule that <mark style="background-color: #9fc5e8;">the signal frequency is less than half of the sampling frequency</mark>. 

#### Anti-aliasing filter

Sometimes the sampling frequency is fixed so cannot choose it dependent on the signal. It is then necessary to **band-limit signals** prior to sampling to avoid aliasing. 

<mark style="background-color: #9fc5e8;">Band-limiting is achieved by lowpass filtering the continuous signal $x(t)$ using a continuous-time anti-aliasing filter, which removes all signal components at or above half the sample frequency.</mark>

![anti-aliasing filter](https://github.com/thisisamor/blog_pic/blob/main/year2/Signals_and_Systems/anti-aliasing-filter.jpg?raw=true)

### Signal Reconstruction

#### Ideal implementation

Consider: 

![inverse fourier of a rectangular function](https://github.com/thisisamor/blog_pic/blob/main/year2/Signals_and_Systems/rectangular-func-inverse-fourier.jpg?raw=true)

Multiplication in the frequency domain is equivalent to convolution in the time domain. 

Recall: 

$$ x_{\delta}(t) = \sum_{n=-\infty}^{\infty} x[n]\delta(t-nT_s) $$

So: 

$$ x(t) = \sum_{n=-\infty}^{\infty} x[n]sinc(\omega_s \frac{(t-nT_s)} {2\pi}) $$

#### Practical implementation 

*(Why the ideal approach is not practical:*
- *the filter is non-causal*
- *the filter requires infinite summation)*

More practical reconstruction: 

![signal reconstruction](https://github.com/thisisamor/blog_pic/blob/main/year2/Signals_and_Systems/signal-reconstruction.jpg?raw=true)

- **zero-order hold** (ZOH): converts samples into a staircase waveform. 
  Formed by convolution of $x[n]$ with the $h_o(t)$. 
  $$ H_o(j\omega)=2e^{-j\omega T_s/2} \frac {sin(\omega T_s /2)}{\omega} $$
![ZOH](https://github.com/thisisamor/blog_pic/blob/main/year2/Signals_and_Systems/ZOH.jpg?raw=true)

- **anti-imaging filter** (reconstruction filter): removes spectral images. 

![anti-imaging filter](https://github.com/thisisamor/blog_pic/blob/main/year2/Signals_and_Systems/ZOH-anti-imaging-combined.jpg?raw=true)

- untiy gain (0dB) over the frequency range $ -\omega_m < \omega < \omega_m $
- almost complete suppression of the spectual images
- can be implemented as a digital filter prior to D-to-A conversion 

### Oversampling

When the difference between $\omega_m$ and $\omega_s-\omega_m$ becomes smaller, at $\omega_m$ the filter must have unity gain, at $\omega_s-\omega_m$ the filter must have high attenuation. 

These requirements are relaxed by using oversampling. 
- Increases $\omega_s$ (compared to the minimum required value). 
- Increases the computation complexity (more samples per second to process). 
- Decreases the complexity of the anti-aliasing filter design (analog filtering). 

### Quantization

Sampling is discrete in **time**, quantization is discrete in **amplitude**. 

Define a set of amplitudes known as the **quantization levels**. 

Precision of the quantization is proportional to the number of bits in each codeword. 
- 8 bits gives $2^8=256$ quantization levels (for image)
- 16 bits gives $2^16=65536$ quantization levels (for music)

---

## Fourier











---

## Laplace




---


## Z-transforms




---


## Filters


