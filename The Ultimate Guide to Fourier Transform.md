# The Ultimate Guide to Fourier Transform: Learn from Scratch to Pro

_**Author:**_ [_pseudo\_Skye_](https://github.com/pseudo-Skye) _**Updated:** 2023-03-11_

Welcome to my tutorial on Fourier Transform! In this tutorial, I will be covering all the **fundamental content of Fourier Series**, including all the **mathematical proofs** you need to understand how Fourier Transform works.

Fourier Transform is a powerful mathematical tool that is widely used in various fields, such as signal processing, image analysis, and quantum mechanics. It allows us to break down complex signals into simpler components, which can be analyzed and manipulated to extract valuable information.

Whether you're a beginner or an experienced practitioner, this tutorial will provide you with a solid foundation in Fourier Transform. I will start by introducing the concept of periodic functions and Fourier Series, and then gradually build up to Fourier Transform. Throughout the tutorial, I will provide detailed explanations of the concepts and mathematical proofs, so you can follow along even if you don't have a strong background in math. By the end of this tutorial, you will have a deep understanding of Fourier Transform and how to apply it in practice.

---

### Content

1\. [Fundamentals](#1-fundamentals)

            1.1 [Sinusoid functions](#11-sinusoid-functions)

            1.2 [Trig function orthogonality](#12-trig-function-orthogonality)

            1.3 [Even and odd functions](#13-even-and-odd-functions)

            1.4 [Matrix-vector multiplication](#14-matrix-vector-multiplication)

2\. [Fourier Series](#2-fourier-series)

3\. [Complex Form of Fourier Series](#3-complex-form-of-fourier-series)

             3.1 [Euler's formula](#31-eulers-formula)

             3.2 [Complex Fourier series](#32-complex-fourier-series) 

             3.3 [Complex basis function](#33-complex-basis-function)

4\. [Discrete Fourier Transform](#4-discrete-fourier-transform)

             4.1 [Understanding DFT step by step](#41-understanding-dft-step-by-step)

             4.2 [The DFT matrix](#42-the-dft-matrix)

                          4.2.1 [Hermitian matrices](#421-hermitian-matrices)

                          4.2.2 [Unitary matrices](#422-unitary-matrices)

                          4.2.3 [Properties of DFT matrix](#423-properties-of-dft-matrix)

5\. [Fast Fourier Transform](#5-fast-fourier-transform)

6\. [The Continuous-time Fourier Transform](#6-the-continuous-time-fourier-transform)

7\. [Reference](#7-reference)

## 1\. Fundamentals

### 1.1 Sinusoid functions

*   For a detailed introduction to sinusoid functions, see: [fundamentals of sinusoid functions](https://www.mathsisfun.com/algebra/amplitude-period-frequency-phase-shift.html)
<p align="center" width="100%">
<img width="40%" src = "https://user-images.githubusercontent.com/117964124/218657054-d0961003-d4c7-46ba-9a71-dc2ce59dddb1.png">
</p>

*   Another general expression of the sinusoid function:  
    $$x(t)=a \\sin (\\omega t+\\phi)=a \\sin (\\frac{2 \\pi}{T} t+\\phi) = a \\sin (2 \\pi f t+\\phi)$$  
    where $\\omega$ denotes the angular speed $\\omega = 2 \\pi f$, which comes from the fact that 1 revolution = $2\\pi$ radians. For a period of time $T$ to complete one revolution, $f$ denotes frequency representing the number of revolutions in unit time, thus $f = 1/T$, and $w=\\frac{\\Delta \\theta}{\\Delta t}=\\frac{2 \\pi}{T}=2 \\pi f$.  
*   All sounds can be built up out of pure tones (sinusoids). Linear combination of signals: $x(t) = \\sin(2 \\pi t) + \\sin(4 \\pi t)$.

### 1.2 Trig function orthogonality

*   Definition of orthogonal functions: Two non-zero functions, $f(x)$ and $g(x)$ are said to be **orthogonal** on $a≤x≤b$ if $\\int\_a^b f(x) g(x) d x=0$. Like how we determine orthogonal vectors, we take the inner product of two functions. As a vector is an abstraction of a list of values, we can think of a function as a way to describe a list of values, but with infinite dimensions.

**Proof**:  
       Take a large integer $N$, put $h=(b-a)/N$ and partition the interval $a≤x≤b$ by defining $x\_1=a+h$, $x\_2=a+2h$, $\\ldots$, $x\_N=a+Nh=b$. Then $$\\int\_a^b f(x)g(x)d x \\approx f\\left(x\_1\\right) g\\left(x\_1\\right) h+\\cdots+f\\left(x\_N\\right) g\\left(x\_N\\right) h=\\left(u\_N \\cdot v\_N\\right) h$$  
       where $u\_N=\\left(f\\left(x\_1\\right), \\ldots, f\\left(x\_N\\right)\\right)$ and $v\_N=\\left(g\\left(x\_1\\right), \\ldots, g\\left(x\_N\\right)\\right)$ are vectors containing the values of $f(x)$ and $g(x)$. The vectors $u\_N$ and $v\_N$ are said to be orthogonal (or perpendicular) if their dot product equals zero $\\left(u\_N \\cdot v\_N=0\\right)$, and so it makes sense to say the functions $f(x)$ and $g(x)$ are orthogonal when the integral $\\int\_a^b f(x) g(x) d x = 0$.

*   Orthogonality of sines and cosines for $-\\pi \< x \< \\pi$

       Let $n,m \\ge 1$ be integers. Then:

$$  
\\begin{align\*}  
\\int\_{-\\pi}^\\pi \\cos (n x) \\cos (m x) d x & =0 & & \\text { if } n \\neq m \\\\  
& =\\pi & & \\text { if } n=m \\\\  
\\int\_{-\\pi}^\\pi \\sin (n x) \\sin (m x) d x & =0 & & \\text { if } n \\neq m \\\\  
& =\\pi & & \\text { if } n=m \\\\  
\\int\_{-\\pi}^\\pi \\cos (n x) \\sin (m x) d x & =0 & & \\text { if } n \\neq m \\\\  
& =0 & & \\text { if } n=m  
\\end{align\*}  
$$

       The **proof** can be found [here](https://www.stumblingrobot.com/2015/08/06/prove-the-orthogonality-relations-for-sine-and-cosine/). To simplify, for a trig function set $\\{0 (\\sin 0), 1 (\\cos 0), \\sin x, \\cos x, \\sin 2x, \\cos 2x, ..., \\sin nx, \\cos nx\\}$, if we take two **different** trig functions from the set, there are orthogonal between $\[-\\pi, \\pi\]$.

### 1.3 Even and odd functions

By [definitions of even and odd functions](https://www.mathsisfun.com/algebra/functions-odd-even.html), we can find $cosx$ is an even function while $sinx$ is an odd function. 

### 1.4 Matrix-vector multiplication

Matrix-vector multiplication is an operation between a matrix and a vector that produces a new vector. Notably, matrix-vector multiplication is only defined between a matrix and a vector where the length of the vector equals the number of columns of the matrix. More details including the visualization can be found [here](https://mbernste.github.io/posts/matrix_vector_mult/). 

Given a matrix $\\boldsymbol{A} \\in \\mathbb{R}^{m \\times n}$ and vector $\\boldsymbol{x} \\in  \\mathbb{R}^n$, the matrix-vector multiplication of $\\boldsymbol{A}$ and $\\boldsymbol{x}$ is defined as $$\\boldsymbol{A} \\boldsymbol{x}:=x\_1 \\boldsymbol{a}\_{\*, 1}+x\_2 \\boldsymbol{a}\_{\*, 2}+\\cdots+x\_n \\boldsymbol{a}\_{\*, n}$$

where $\\boldsymbol{a}\_{\*, i}$ is the $i$ th column vector of $\\boldsymbol{A}$. For example, it can also be written like 

$$  
\\left\[\\overrightarrow{\\boldsymbol{a}\_1}, \\overrightarrow{\\boldsymbol{a}\_2}, \\overrightarrow{\\boldsymbol{a}\_3}, \\overrightarrow{\\boldsymbol{a}\_4}\\right\]\\left\[\\begin{array}{l}  
x\_1 \\\\  
x\_2 \\\\  
x\_3 \\\\  
x\_4  
\\end{array}\\right\]=x\_1 \\overrightarrow{\\boldsymbol{a}\_{\\mathbf{1}}}+x\_2 \\overrightarrow{\\boldsymbol{a}\_{\\mathbf{2}}}+x\_3 \\overrightarrow{\\boldsymbol{a}\_{\\mathbf{3}}}+x\_4 \\overrightarrow{\\boldsymbol{a}\_{\\mathbf{4}}}  
$$

Thus, another important property is that: if we swap the columns of the matrix $\\boldsymbol{A}$, and then swap the corresponding rows of the vector $\\boldsymbol{x}$, the multiplication result remains the same. 

$$  
\\left\[\\overrightarrow{\\boldsymbol{a}\_1}, \\overrightarrow{\\boldsymbol{a}\_4}, \\overrightarrow{\\boldsymbol{a}\_3}, \\overrightarrow{\\boldsymbol{a}\_2}\\right\]\\left\[\\begin{array}{l}  
x\_1 \\\\  
x\_4 \\\\  
x\_3 \\\\  
x\_2  
\\end{array}\\right\]=x\_1 \\overrightarrow{\\boldsymbol{a}\_{\\mathbf{1}}}+x\_4 \\overrightarrow{\\boldsymbol{a}\_{\\mathbf{4}}}+x\_3 \\overrightarrow{\\boldsymbol{a}\_{\\mathbf{3}}}+x\_2 \\overrightarrow{\\boldsymbol{a}\_{\\mathbf{2}}}  
$$

## 2\. Fourier Series

*   **What is the difference between the Fourier series and Fourier transform?**

       Fourier transform is applicable for non-periodic signals, while the Fourier series is applicable to periodic signals.

*   **What is the Fourier series?**

        A Fourier series is an expansion of a **periodic** function $f(x)$ in terms of an infinite sum of sines and cosines. (Linear combination of sinusoids).

        For period **T**: $$f(x)=\\sum\_{n=0}^{\\infty}\\left\[a\_n \\cos \\left(\\frac{2 n \\pi}{T} x\\right)+b\_n \\sin \\left(\\frac{2 n \\pi}{T} x\\right)\\right\]$$

        or $$f(x)=a\_0 + \\sum\_{n=1}^{\\infty}\\left\[a\_n \\cos \\left(\\frac{2 n \\pi}{T} x\\right)+b\_n \\sin \\left(\\frac{2 n \\pi}{T} x\\right)\\right\]$$

        If $T = 2\\pi$, then the Fourier series can be written as:

        $$f(x)=\\sum\_{n=0}^{\\infty} a\_n \\cos (n x)+\\sum\_{n=0}^{\\infty} b\_n \\sin (n x)$$

        or $$f(x)=a\_0(\\mathrm{or} \\ \\frac{a\_0}{2} ) + \\sum\_{n=1}^{\\infty} a\_n \\cos (n x)+\\sum\_{n=1}^{\\infty} b\_n \\sin (n x)$$

        Here, $a\_0$, $a\_n$, and $b\_n$ are coefficients of the Fourier series.

*   For a periodic function $f(x) = f(x+2\\pi)$, based on the definition of the Fourier series, we have to find a linear combination of sinusoids to represent $f(x)$. **How to determine which sinusoid function to use?**

       (1) What **period** to choose: Intuitively, we can choose those sinusoids with period $T=2\\pi$, like $sinx$, $cosx$, $sin2x$, $cos2x$, ..., $sinnx$, $cosnx$, and constants $cos0x$. 

       (2) Which **type of function** to choose, $sin$, $cos$, or both: Any function can be written as the sum of an even function and an odd function, as $$f(x)=\\frac{f(x)+f(-x)}{2}+\\frac{f(x)-f(-x)}{2}$$ 

            where $\\frac{f(x)+f(-x)}{2}$ is even function, while $\\frac{f(x)-f(-x)}{2}$ is odd function. Thus, we determine the choice of sinusoids as follows:

<div align="center">

$f$ is even | $f$ is odd | $f$ is neither even nor odd
:---: | :---: | :---:
$cos$ only | $sin$ only | $cos+sin$ both
</div>

            Thus, we can obtain the Fourier series represented as:

$$f(x)=a\_0+\\sum\_{n=1}^{\\infty} a\_n \\cos (n x)+\\sum\_{n=1}^{\\infty} b\_n \\sin (n x), n \\in \\mathbb{N}, a\_n, b\_n \\in \\mathbb{R}$$

            where, $a\_0$ is the constant part, $a\_n$ and $b\_n$ are coefficients.

*   **How to compute the coefficients of the Fourier series?**

$$  
\\begin{align\*}  
& a\_0=\\frac{1}{2 \\pi} \\int\_{-\\pi}^\\pi f(x) d x \\\\  
& a\_n=\\frac{1}{\\pi} \\int\_{-\\pi}^\\pi f(x) \\cos (n x) d x \\\\  
& b\_n=\\frac{1}{\\pi} \\int\_{-\\pi}^\\pi f(x) \\sin (n x) d x  
\\end{align\*}  
$$

       **Proof**: To obtain $a\_0$, we take the integral of $f(x)$, $\\int\_{-\\pi}^\\pi f(x) dx=\\int\_{-\\pi}^\\pi a\_0 dx=2 a\_0 \\pi$, thus $a\_0=\\frac{1}{2 \\pi} \\int\_{-\\pi}^\\pi f(x) d x$. To derive the formula for the $a\_n$, observe that 

$$\\begin{aligned}  
& \\int\_{-\\pi}^\\pi f(x) \\cos k x d x \\\\  
& \\quad=a\_0 \\int\_{-\\pi}^\\pi \\cos k x d x+\\sum\_{n=1}^N a\_n \\int\_{-\\pi}^\\pi \\cos n x \\cos k x d x+\\sum\_{n=1}^N b\_n \\int\_{-\\pi}^\\pi \\sin n x \\cos k x d x .  
\\end{aligned}$$

       Applying the orthogonality of trig functions, we can get $\\int\_{-\\pi}^\\pi f(x) \\cos k x d x=a\_k \\pi$, thus $a\_k=\\frac{1}{\\pi} \\int\_{-\\pi}^\\pi f(x) \\cos (k x) d x $, same as $a\_n=\\frac{1}{\\pi} \\int\_{-\\pi}^\\pi f(x) \\cos (n x) d x$. Similarly, by multiply $f(x)$ with $\\sin kx$, we can obtain $b\_n=\\frac{1}{\\pi} \\int\_{-\\pi}^\\pi f(x) \\sin (n x) d x$.

*   Some exercises of the Fourier series can be found [here](http://people.brunel.ac.uk/~icstmkw/ma2715/exer_ma2715_chap5_some_answers.pdf).
*   Understanding the coefficients of the Fourier series by the orthogonality of the sinusoids.

       Orthogonal sinusoids: $\\{1, \\cos x, \\sin x, \\cos 2x, \\sin 2x, ...\\}$

       The length of a vector is the square root of the dot product of the vector with itself. Thus, based on $||f(x)||\_2=\\sqrt{\\int\_{-\\pi}^\\pi {f(x)}^2 dx}$, we obtain $\\sqrt{\\int\_{-\\pi}^\\pi \\sin ^2(n x) \\mathrm{dx}}=\\sqrt{\\pi}$, $\\sqrt{\\int\_{-\\pi}^\\pi \\cos ^2(n x) \\mathrm{dx}}=\\sqrt{\\pi} (n \\neq \\sqrt{\\pi})$, and $\\sqrt{\\int\_{-\\pi}^\\pi 1 \\mathrm{dx}}=\\sqrt{2\\pi}$.

       Orthonormal sinusoids: $\\{\\frac{1}{\\sqrt{2\\pi}}, \\frac{\\cos x}{\\sqrt{\\pi}}, \\frac{\\sin x}{\\sqrt{\\pi}}, \\frac{\\cos 2x}{\\sqrt{\\pi}}, \\frac{\\sin 2x}{\\sqrt{\\pi}}, ...\\}$

       For $T=2\\pi$, the projection length of $f(x)$ along the direction of $\\cos 2x$ is $$\\frac{\< f(x), \\cos (2 x) >}{||\\cos (2 x)||}=\\frac{\\int\_{-\\pi}^\\pi f(x) \\cos (2 x) \\mathrm{dx}}{\\sqrt{\\int\_{-\\pi}^\\pi \\cos ^2(2 x) \\mathrm{dx}}}=\\frac{1}{\\sqrt{\\pi}} \\int\_{-\\pi}^\\pi f(x) \\cos (n x) \\mathrm{dx}$$

       Thus, for the projection length relative to the length of $\\cos 2x$, we have $\\frac{\< f(x), \\cos (2 x) >}{||\\cos (2 x)||^2}=\\frac{1}{\\pi} \\int\_{-\\pi}^\\pi f(x) \\cos (2 x) \\mathrm{dx}$.

## 3\. Complex Form of Fourier Series

### 3.1 Euler's formula

<p align="center" width="100%">
<img width = "30%" src = "https://akuli.github.io/math-tutorial/asymptote/0a4dcab270a3599b446e8105d9311ecc.svg">
</p>

We can find proof of Euler's formula [here](https://en.wikipedia.org/wiki/Euler%27s_formula). 

Based on the Euler's formula, we have $$e^{i \\varphi}=\\cos\\varphi+i \\sin \\varphi,\\quad e^{-i \\varphi}=\\cos \\varphi-i \\sin \\varphi$$

Thus, we can obtain $$\\cos \\varphi=\\frac{e^{i \\varphi}+e^{-i \\varphi}}{2}, \\quad \\sin \\varphi=\\frac{e^{i \\varphi}-e^{-i \\varphi}}{2 i}$$

### 3.2 Complex Fourier series

Take the Euler's formula into the Fourier series, we have

$$  
\\begin{align\*}  
f(x) & =a\_0+\\sum\_{n=1}^{\\infty} a\_n \\cos (n x) \\mathrm{dx}+\\sum\_{n=1}^{\\infty} b\_n \\sin (n x) \\mathrm{dx} \\\\  
& =a\_0+\\sum\_{n=1}^{\\infty} \\frac{a\_n}{2}\\left(\\mathrm{e}^{i n x}+\\mathrm{e}^{-i n x}\\right)+\\sum\_{n=1}^{\\infty} \\frac{b\_n}{2 i}\\left(\\mathrm{e}^{i n x}-\\mathrm{e}^{-i n x}\\right) \\\\  
& =a\_0+\\sum\_{n=1}^{\\infty}\\left(\\frac{a\_n-i b\_n}{2}\\right) \\mathrm{e}^{i n x}+\\sum\_{n=1}^{\\infty}\\left(\\frac{a\_n+i b\_n}{2}\\right) \\mathrm{e}^{-i n x} \\\\  
& =a\_0+\\sum\_{n=1}^{\\infty} c\_n e^{inx}+\\sum\_{n=1}^{\\infty} c\_{-n} e^{-inx} \\\\  
& =a\_0+\\sum\_{n=1}^{\\infty} c\_n e^{inx}+\\sum\_{n=-\\infty}^{-1} c\_{n} e^{inx} \\\\  
& =\\sum\_{n=-\\infty}^{\\infty} c\_{n} e^{inx}  
\\end{align\*}  
$$

where $c\_0=a\_0, c\_n=\\frac{a\_n-i b\_n}{2}, c\_{-n}=\\frac{a\_n+i b\_n}{2} (n \\geq 1)$. The coefficients of complex Fourier series $c\_n$ can be calculated as:

$$\\begin{align\*}  
c\_n & =\\frac{a\_n-i b\_n}{2}=\\frac{\\frac{1}{\\pi} \\int\_{-\\pi}^\\pi f(x) \\cos (n x) \\mathrm{dx}-i \\frac{1}{\\pi} \\int\_{-\\pi}^\\pi f(x) \\sin (n x) \\mathrm{dx}}{2} \\\\  
& =\\frac{1}{2 \\pi} \\int\_{-\\pi}^\\pi f(x) \\cos (\\mathrm{n} x) \\mathrm{dx}-i \\frac{1}{2 \\pi} \\int\_{-\\pi}^\\pi f(x) \\sin (n x) \\mathrm{dx} \\\\  
& =\\frac{1}{2 \\pi} \\int\_{-\\pi}^\\pi f(x)\[\\cos (n x)-i \\sin (n x)\] \\mathrm{dx} \\\\  
c\_n &=\\frac{1}{2 \\pi} \\int\_{-\\pi}^\\pi f(x) \\mathrm{e}^{-i n x} \\mathrm{dx} \\quad(n \\geq 1) \\\\  
c\_{-n} &=\\frac{1}{2 \\pi} \\int\_{-\\pi}^\\pi f(x) \\mathrm{e}^{i n x} \\mathrm{dx} \\quad(n \\geq 1) \\\\  
c\_n&=\\frac{1}{2 \\pi} \\int\_{-\\pi}^\\pi f(x) \\mathrm{e}^{-i n x} \\mathrm{dx} \\quad(n \\leq-1) \\\\  
c\_0&=\\frac{1}{2 \\pi} \\int\_{-\\pi}^\\pi f(x) d x  
\\end{align\*}$$

Thus, we have $c\_n=\\frac{1}{2 \\pi} \\int\_{-\\pi}^\\pi f(x) e^{-i n x} d x, \\quad n=0, \\pm 1, \\pm 2, \\ldots$.

*   **The complex conjugate of a complex number**:

       The complex number with an equal real part and an imaginary part equal in magnitude but opposite in sign. That is, if $a$ and $b$ are real, then the complex conjugate of $a+bi$ equals to $a-bi$. The product of a complex number and its conjugate is a real number: $a^2+b^2$. Thus, if the function $f(x)$ here is the real function, the conjugate of complex Fourier coefficient $c\_n$ is $c\_{-n}$.

*   **What if $f(x)$ is a complex function**?

       For $f(x)=g(x)+ih(x)$, we have $$f(x)=\\sum\_{n=-\\infty}^{\\infty} g\_n e^{i n x}+\\mathrm{i} \\sum\_{n=-\\infty}^{\\infty} h\_n e^{i n x} =\\sum\_{n=-\\infty}^{\\infty}\\left(g\_n+\\mathrm{i} h\_n\\right) e^{i n x}$$

       where the conjugate of $g\_n$ is $g\_{-n}$, the conjugate of $h\_n$ is $h\_{-n}$. For this function $f(x)$, we have $\\left(g\_n+\\mathrm{i} h\_n\\right)$ as the coefficients $c\_n$. To see if the conjugate of $c\_n$ is still $c\_{-n}$, we have

$$\\begin{align\*}  
\\overline{c\_n}=\\overline{g\_n}-\\mathrm{i} \\overline{h\_n}=g\_{-n}-i h\_{-n} \\\\  
c\_{-n}=g\_{-n}+i h\_{-n} \\neq \\overline{c\_n}\\end{align\*}  
$$

       Thus, the conjugate of $c\_n$ is $c\_{-n}$ only when $h(x) = 0$.

### 3.3 Complex basis function

Function $e^{ix}$ in the two-dimensional complex space (here $||f(x)||$ **represents the norm of a certain value of the complex** **function**):

<p align="center" width="100%">
<img width = "30%" src = "https://user-images.githubusercontent.com/117964124/221768218-d1120f26-8997-4d8b-bbe5-64d74f70d707.png">
</p>

$$\\begin{align\*}  
n=0 \\quad & f(x)=e^0=1 \\\\  
n=1 \\quad & f(x)=e^{i x}, \\quad T = 2\\pi, \\quad ||f(x)|| = \\cos x+i \\sin x=\\sqrt{\\cos ^2 x+\\sin ^2 x}=1 \\\\  
n=2 \\quad & f(x)=e^{i 2 x},  \\quad T = \\pi, \\quad ||f(x)|| = 1 \\\\  
n=-1 \\quad & f(x)=e^{-i x},  \\quad T = 2\\pi, \\quad ||f(x)|| = 1  
\\end{align\*}$$

Thus $f(x)=\\cdots+c\_{-2} e^{-i 2 x}+c\_{-1} e^{-i x}+c\_0+c\_1 e^{i x}+c\_2 e^{i 2 x}+\\cdots$ is a linear combination of the basis function $e^{inx}$, except the constant $c\_0$, we have a pair of basis functions $e^{inx}$ with the same period $T$. 

*   **Complex inner product**:

       Unlike what we have learned about the **real inner product** in [1.2](#12-trig-function-orthogonality), the definition for the dot-product given earlier must be slightly modified for use with complex variables. The new definition is $$\\left\\langle x\_n, y\_n\\right\\rangle=\\sum\_{n=0}^{N-1} x\[n\] y^\*\[n\]$$

       where $y^\*\[n\]$ denotes the complex conjugate of $y\[n\]$. For a real number function, we can obtain the norm of the function by the formula $$||x\_n|| = \\sqrt{\\left\\langle x\_n,x\_n \\right\\rangle} =  \\sqrt{\\sum\_{n=0}^{N-1}|x\[n\]|^2}$$

       However, when it comes to complex signal $x\[n\]$, we obtain the inner product of it with itself to produce a real value by $$\\left\\langle x\_n, x\_n\\right\\rangle=\\sum\_{n=0}^{N-1} x\[n\] x^\*\[n\]=\\sum\_{n=0}^{N-1}|x\[n\]|^2$$

*   **The orthogonality of basis functions** $\\{ \\cdots, e^{-i 3 x}, e^{-i 2 x}, e^{-i x}, 1, e^{i x}, e^{i 2 x}, e^{i 3 x}, \\cdots \\}$:

$$\\left\\langle e^{i m x}, e^{i n x}\\right\\rangle=\\int\_{-\\pi}^\\pi e^{i m x} e^{-i n x} \\mathrm{dx}=\\int\_{-\\pi}^\\pi e^{i(m-n) x} d x=\\left\\{\\begin{array}{cc}  
0 & m \\neq n \\\\  
2 \\pi & m=n  
\\end{array}\\right.$$

*   **Standard basis** of the complex Fourier series:

       Given the above equation, we have $$\\left\\langle e^{i n x}, e^{i n x}\\right\\rangle=\\int\_{-\\pi}^\\pi e^{i n x} e^{-i n x} \\mathrm{dx}=2\\pi, ||e^{-i n x}||\_c = \\sqrt{2\\pi}$$ 

       We can obtain the standard basis by dividing $\\{ \\cdots, e^{-i 3 x}, e^{-i 2 x}, e^{-i x}, 1, e^{i x}, e^{i 2 x}, e^{i 3 x}, \\cdots \\}$ with $\\sqrt{2\\pi}$.

## 4\. Discrete Fourier Transform

### 4.1 Understanding DFT step by step

**Discrete Fourier Transform (DFT) assumes that the input signal is periodic with period N, which means the input signal is assumed to repeat itself every N samples.** So, suppose we **evenly** sample $N$ points within the period of the function $f(x)$, where $T=2\\pi$, the sample denotes as $\[f\_0, f\_1, ..., f\_{N-1}\]$. $$f(x) = \\sum\_{n=-\\infty}^{\\infty} c\_{k} e^{ikx}$$ 

<p align="center" width="100%">
<img width = "40%" src = "https://user-images.githubusercontent.com/117964124/221496522-c3b01044-9855-4d19-9c08-ea020f057a1e.png">
</p>

#### **(1) Question** 1: how to obtain the value of the second data point $x=\\frac{2\\pi}{N}$?

If we take the second data point $x=\\frac{2\\pi}{N}$ into the function $f(x)$, we have $$f(x) = f\\left(\\frac{2\\pi}{N}\\right)= \\sum\_{n=-\\infty}^{\\infty} c\_{k} e^{k\\frac{2\\pi i}{N}} = \\cdots+c\_{-2} e^{-2 \\frac{2 \\pi i}{N}}+c\_{-1} e^{-1 \\frac{2 \\pi i}{N}}+c\_0 e^{0 \\frac{2 \\pi i}{N}}+c\_1 e^{1 \\frac{2 \\pi i}{N}}+c\_2 e^{2 \\frac{2 \\pi i}{N}}+\\cdots+c\_{N-1} e^{(N-1) \\frac{2 \\pi i}{N}}+c\_N e^{N \\frac{2 \\pi i}{N}}+c\_{N+1} e^{(N+1) \\frac{2 \\pi i}{N}}+\\cdots $$

When $N=8$, means we sample 8 points within the period, $e^{k\\frac{2\\pi i}{8}}$ can be represented as follows in the complex space:

<p align="center" width="100%">
<img width = "40%" src = "https://user-images.githubusercontent.com/117964124/221767984-8acd191d-0a31-46d1-8226-e7ba75cac9e3.png">
</p>

From the above figure, we can tell that when $k = 9$, we get the same complete value as $k=1$. Thus, even though $k$ ranges from negative infinity to positive infinity, the complex value would only vary between $k=0$ to $k=7$. Now, we let $w=e^{\\frac{2\\pi i}{N}}$, then $w^N = w^0 = 1$, we can summarize the equation of $f\\left(\\frac{2\\pi}{N}\\right)$ as 

$$\\begin{align\*}  
f\\left(\\frac{2 \\pi}{N}\\right)=\\cdots+ & c\_{-2} w^{-2}+c\_{-1} w^{-1}+c\_0 w^{0}+c\_1 w^{1}+c\_2 w^{2}+\\cdots+c\_{N-1} w^{N-1}+c\_N w^{N}+c\_{N+1} w^{N+1}+\\cdots \\\\  
\= & \\left(c\_0+c\_N+c\_{-N}+c\_{2 N}+c\_{-2 N}+\\cdots\\right) w^0 \\\\  
& +\\left(c\_1+c\_{N+1}+c\_{2 N+1}+c\_{-N+1}+c\_{-2 N+1} \\cdots\\right) w^1 \\\\  
& +  \\left(c\_2+c\_{N+2}+c\_{2 N+2}+c\_{-N+2}+c\_{-2 N+2} \\cdots\\right) w^2 \\\\  
& \\cdots \\\\  
& + \\left(c\_{N-1}+c\_{2 N-1}+c\_{3 N-1}+c\_{-1}+c\_{-N-1} \\cdots\\right) w^{N-1}  
\\end{align\*}$$

We conclude that: to obtain the value of $f\\left(\\frac{2\\pi}{N}\\right)$, we only need $N$ basis of $w$ and corresponding coefficients of these bases. 

#### (2) **Question 2**: how to obtain the value of the second data point $x=\\frac{4\\pi}{N}$? Can it be represented by $N$ basis of $w$ as well? How about the rest of the points?

By taking $w=e^{\\frac{2\\pi i}{N}}$, the value of the data points can be written as $$f\\left(\\frac{2 \\pi}{N}\\right)=\\sum\_{n=-\\infty}^{\\infty} c\_{n} w^{n}, \\quad f\\left(2\\frac{2 \\pi}{N}\\right)=\\sum\_{n=-\\infty}^{\\infty} c\_{n} w^{2n}, \\quad f\\left(3\\frac{2 \\pi}{N}\\right)=\\sum\_{n=-\\infty}^{\\infty} c\_{n} w^{3n}, ...$$

Thus, we can obtain 

$$\\begin{align\*}  
f\\left(2\\frac{2 \\pi}{N}\\right)=\\left(c\_0+c\_N+c\_{-N}+c\_{2 N}+c\_{-2 N}+\\cdots\\right) w^{2 \\times 0} \\\\  
+\\left(c\_1+c\_{N+1}+c\_{2 N+1}+c\_{-N+1}+c\_{-2 N+1} \\cdots\\right) w^{2 \\times 1} \\\\  
\+  \\left(c\_2+c\_{N+2}+c\_{2 N+2}+c\_{-N+2}+c\_{-2 N+2} \\cdots\\right) w^{2 \\times 2} \\\\  
\\cdots \\\\  
\+ \\left(c\_{N-1}+c\_{2 N-1}+c\_{3 N-1}+c\_{-1}+c\_{-N-1} \\cdots\\right) w^{2 \\times \\left(N-1\\right)}  
\\end{align\*}$$

As $w^N=1$, for any integer $k$ in $w^k$, we can find a value to match it among $\\{w^0, w^1, w^2, ..., w^{N-1}\\}$. For the third data point $x=\\frac{4\\pi}{N}$, although we still have $N=8$ components in the above equation, we can further combine the terms based on $w^8 = w^0, w^{10} = w^2, ..., w^{14} = w^6$. Thus, we only need $\\frac{N}{2} = 4$ basis of $w$ to represent the third data point. Or, if we do not combine the like terms in this equation, we can also represent the third data point by $N=8$ basis of $w$, which is from $\\{w^0, w^2, w^4, ..., w^{14}\\}$.

The rest data points (including those outside the sampling range) can be denoted as $x=m\\frac{2\\pi}{N}$, and we can replace the $w$ in the equation from Question 1 with $w^m$. Hence, we can obtain 

$$\\begin{align\*}  
f\\left(m\\frac{2\\pi}{N}\\right)=\\left(c\_0+c\_N+c\_{-N}+c\_{2 N}+c\_{-2 N}+\\cdots\\right) w^{m \\times 0} \\\\  
+\\left(c\_1+c\_{N+1}+c\_{2 N+1}+c\_{-N+1}+c\_{-2 N+1} \\cdots\\right) w^{m \\times 1} \\\\  
\+  \\left(c\_2+c\_{N+2}+c\_{2 N+2}+c\_{-N+2}+c\_{-2 N+2} \\cdots\\right) w^{m \\times 2} \\\\  
\\cdots \\\\  
\+ \\left(c\_{N-1}+c\_{2 N-1}+c\_{3 N-1}+c\_{-1}+c\_{-N-1} \\cdots\\right) w^{m \\times \\left(N-1\\right)}  
\\end{align\*}$$

where we only need $\\lceil \\frac{N}{m} \\rceil$ basis of $w$ to represent the data point, or we can use $N$ basis of $w$ to represent the value. 

To keep the consistency of the expression of all sampled data points, we can say that for each value, we can use $N$ basis of $w$ to represent it.

#### (3) **Question 3**: Can we use $N$ coefficients to represent $f(x)$?

From the above equation, we know that any data point of the signal can be represented as a linear combination of $N$ terms of basis $w$, ranging from $w^0$ to $w^{N-1}$, thus $f(x)$ can be written as $$f(x)=c\_0+c\_1 e^{i x}+c\_2 e^{i 2 x}+\\cdots+c\_{N-1} e^{i(N-1) x}$$

where $c\_0$ here actually represents the sum of $c\_0+c\_N+c\_{-N}+c\_{2 N}+c\_{-2 N}+...$ from the above equation. Similarly, the remaining coefficients $c\_n$ follow the same rules as $c\_0$, where **each coefficient here is the sum of its corresponding coefficient terms from the above equation**. Thus, by taking $N$ evenly sampled data points into this equation, we have

$$  
\\begin{aligned}  
& f\\left(0 \\frac{2 \\pi}{N}\\right)=c\_0+c\_1+c\_2+\\cdots+c\_{N-1} \\\\  
& f\\left(1 \\frac{2 \\pi}{N}\\right)=c\_0+c\_1 w+c\_2 w^2+\\cdots+c\_{N-1} w^{N-1} \\\\  
& f\\left(2 \\frac{2 \\pi}{N}\\right)=c\_0+c\_1 w^2+c\_2 w^4+\\cdots+c\_{N-1} w^{2(N-1)} \\\\  
& f\\left(3 \\frac{2 \\pi}{N}\\right)=c\_0+c\_1 w^3+c\_2 w^6+\\cdots+c\_{N-1} w^{3(N-1)} \\\\  
&f \\left(N-1) \\frac{2 \\pi}{N}\\right)=c\_0+c\_1 w^{N-1}+c\_2 w^{2(N-1)}+\\cdots+c\_{N-1} w^{(N-1)^2}  
\\end{aligned}  
$$

The equation can be written in the Fourier matrix as 

$$  
\\left\[\\begin{array}{c}  
f\_0 \\\\  
f\_1 \\\\  
f\_2 \\\\  
f\_3 \\\\  
\\vdots \\\\  
f\_{N-1}  
\\end{array}\\right\]=\\left\[\\begin{array}{cccccc}  
1 & 1 & 1 & 1 & \\cdots & 1 \\\\  
1 & w & w^2 & w^3 & \\cdots & w^{N-1} \\\\  
1 & w^2 & w^4 & w^6 & \\cdots & w^{2(N-1)} \\\\  
1 & w^3 & w^6 & w^9 & \\cdots & w^{3(N-1)} \\\\  
\\vdots & \\vdots & \\vdots & \\vdots & \\vdots & \\vdots \\\\  
1 & w^{N-1} & w^{2(N-1)} & w^{3(N-1)} & \\cdots & w^{(N-1)^2}  
\\end{array}\\right\]\\left\[\\begin{array}{c}  
c\_0 \\\\  
c\_1 \\\\  
c\_2 \\\\  
c\_3 \\\\  
\\vdots \\\\  
c\_{N-1}  
\\end{array}\\right\]  
$$

The process we obtain the Fourier complex coefficients $\[c\_0, c\_1, ..., c\_{N-1}\]$ from the given value of data points $\[f\_0, f\_1, ..., f\_{N-1}\]$ is called **discrete Fourier transform (DFT)**, while the inverse process to obtain values of data points based on the Fourier formula is called **inverse discrete Fourier transform (IDFT)**.

#### (4) Question 4: How many samples do we need to get the coefficients of the complex Fourier series?

Example: $f(x) = 1+ e^{ix} + e^{i2x} + e^{i3x} $

*   If we take $N=4$ samples $\[f(0)=4, f\\left(1 \\frac{2 \\pi}{4}\\right)=f\\left(2 \\frac{2 \\pi}{4}\\right)=f\\left(3 \\frac{2 \\pi}{4}\\right)=0\]$, we can perfectly get the four coefficients of the signal by

$$  
\\left\[\\begin{array}{cccc}  
1 & 1 & 1 & 1 \\\\  
1 & w & w^2 & w^3 \\\\  
1 & w^2 & w^4 & w^6 \\\\  
1 & w^3 & w^6 & w^9  
\\end{array}\\right\]^{-1}\\left\[\\begin{array}{c}  
4 \\\\  
0 \\\\  
0 \\\\  
0  
\\end{array}\\right\]=\\left\[\\begin{array}{c}  
1 \\\\  
1 \\\\  
1 \\\\  
1  
\\end{array}\\right\]  
$$

       where $w=e^{\\frac{2 \\pi i}{4}}=i$.

*   If we take $N=3$ samples $\[f(0)=4, f\\left( \\frac{2 \\pi}{3}\\right)= 1, f\\left(2 \\frac{2 \\pi}{3}\\right)= 1\]$, we can only get three coefficients of the signal by

$$  
\\left\[\\begin{array}{ccc}  
1 & 1 & 1 \\\\  
1 & w & w^2 \\\\  
1 & w^2 & w^4  
\\end{array}\\right\]^{-1}\\left\[\\begin{array}{c}  
4 \\\\  
1 \\\\  
1  
\\end{array}\\right\]=\\left\[\\begin{array}{c}  
2 \\\\  
1 \\\\  
1  
\\end{array}\\right\]  
$$

*   If we take $N=6$ samples, we can still perfectly get the four coefficients of the signal as the coefficients of $e^{i4x}$ and $e^{i5x}$ is 0.

$$  
W\_6^{-1}\\left\[\\begin{array}{c}  
4 \\\\  
\\sqrt{3} i \\\\  
1 \\\\  
0 \\\\  
1 \\\\  
\-\\sqrt{3} i  
\\end{array}\\right\]=\\left\[\\begin{array}{c}  
1 \\\\  
1 \\\\  
1 \\\\  
1 \\\\  
0 \\\\  
0  
\\end{array}\\right\]  
$$

*   Given a sample $N=4$ data points, we can get only one result of DFT, however, the function of the signal can vary, for example,

$$  
\\begin{gathered}  
f(x)=1+e^{i x}+e^{i 2 x}+e^{i 3 x} \\\\  
f(x)=1+e^{i x}+e^{i 2 x}+e^{-i x} \\\\  
f(x)=\\frac{1}{2} e^{-i x}+1+e^{i x}+e^{i 2 x}+\\frac{1}{2} e^{i 3 x} \\\\  
f(x)=1+\\frac{1}{3} e^{i x}+\\frac{1}{3} e^{i 5 x}+\\frac{1}{3} e^{i 9 x}+e^{i 2 x}+e^{i 3 x}  
\\end{gathered}  
$$

       The above equations all fit the sample value as $\[f(0)=4, f\\left(1 \\frac{2 \\pi}{4}\\right)=f\\left(2 \\frac{2 \\pi}{4}\\right)=f\\left(3 \\frac{2 \\pi}{4}\\right)=0\]$, it can be observed from the DFT matrix 

$$  
\\left\[\\begin{array}{c}  
f\\left(0 \\frac{2 \\pi}{4}\\right) \\\\  
f\\left(1 \\frac{2 \\pi}{4}\\right) \\\\  
f\\left(2 \\frac{2 \\pi}{4}\\right) \\\\  
f\\left(3 \\frac{2 \\pi}{4}\\right)  
\\end{array}\\right\]=\\left\[\\begin{array}{cccc}  
1 & 1 & 1 & 1 \\\\  
1 & w & w^2 & w^3 \\\\  
1 & w^2 & w^4 & w^6 \\\\  
1 & w^3 & w^6 & w^9  
\\end{array}\\right\]\\left\[\\begin{array}{c}  
\\left(\\cdots+c\_{-4}+c\_0+c\_4+\\cdots\\right) \\\\  
\\left(\\cdots+c\_{-3}+c\_1+c\_5+\\cdots\\right) \\\\  
\\left(\\cdots+c\_{-2}+c\_2+c\_6+\\cdots\\right) \\\\  
\\left(\\cdots+c\_{-1}+c\_3+c\_7+\\cdots\\right)  
\\end{array}\\right\]  
$$

       With limited samples, it is hard to get the precise coefficients of the original signal. However, suppose we can sample infinite data points from the period, then we can more precisely get the Fourier coefficients of the signal with less information loss. As we can observe from the matrix of coefficients, When $N\\rightarrow \\infty$, $c\_N\\rightarrow 0$ as the extremely high frequency is unlikely to contribute to the signal, thus we can get more precise low-frequency components of the signal. 

$$  
\\left\[\\begin{array}{c}  
\\left(\\cdots+c\_{-2 N} + c\_{-N}+ c\_0+c\_N+c\_{2 N}+\\cdots\\right) \\\\  
\\left(\\cdots+\_{-2 N+1}+ c\_{-N+1} + c\_1+c\_{N+1}+c\_{2 N+1}+\\cdots\\right) \\\\  
\\left(\\cdots+c\_{-2 N+2}+c\_{-N+2}+c\_2+c\_{N+2}+c\_{2 N+2}+\\cdots\\right) \\\\  
\\left(\\cdots+c\_{-N-1}+c\_{-1}+c\_{N-1}+c\_{2 N-1}+c\_{3 N-1}+\\cdots\\right)  
\\end{array}\\right\]  
$$

#### (5) Question 5: How to find the coefficients if $w$ is outside $\[w^0, ..., w^{N-1}\]$?

Given an example of a real function $f(x) = 1 + \\cos x + \\cos 2x$, the function can be converted to complex function as $$f(x)=\\frac{1}{2} e^{-i 2 x}+\\frac{1}{2} e^{-i x}+1+\\frac{1}{2} e^{i x}+\\frac{1}{2} e^{i 2 x}$$

As we can see, the real function is a linear combination of $\\cos nx$ with **3** different frequencies, while the complex function contains **5** different complex frequencies. Also, in the complex function, the basis $w$ no longer fits the value from $w^0$ to $w^{N-1}$. However as we can observe from the coefficients in Question 1, $e^{-i x}$ shares the same value as $e^{-i \\left(N-1 \\right) x}$, which is the last term in the equation and the corresponding coefficient is $c\_{N-1}$. Similarly, the coefficient of $e^{-i 2 x}$ is $c\_{N-2}$.

*   In this example, if we sample $N=6$, we get 6 coefficients as

$$  
W\_6^{-1}\\left\[\\begin{array}{c}  
3 \\\\  
1 \\\\  
0 \\\\  
1 \\\\  
0 \\\\  
1  
\\end{array}\\right\]=\\left\[\\begin{array}{c}  
1 \\\\  
0.5 \\\\  
0.5 \\\\  
0 \\\\  
0.5 \\\\  
0.5  
\\end{array}\\right\]  
$$

*   If we sample $N=3$, we get 3 coefficients as

$$  
W\_5^{-1}\\left\[\\begin{array}{c}  
3 \\\\  
0.5 \\\\  
0.5 \\\\  
0.5 \\\\  
0.5  
\\end{array}\\right\]=\\left\[\\begin{array}{c}  
1 \\\\  
0.5 \\\\  
0.5 \\\\  
0.5 \\\\  
0.5  
\\end{array}\\right\]  
$$

*   As we can see when we sample $N=6$ from the signal, there is 1 coefficient as 0, which means only 5 coefficients are needed to get the full complex Fourier series of the signal. Thus, if we sample $N=5$, we can get exactly the coefficients of the signal

$$  
W\_5^{-1}\\left\[\\begin{array}{c}  
3 \\\\  
0.5 \\\\  
0.5 \\\\  
0.5 \\\\  
0.5  
\\end{array}\\right\]=\\left\[\\begin{array}{c}  
1 \\\\  
0.5 \\\\  
0.5 \\\\  
0.5 \\\\  
0.5  
\\end{array}\\right\]  
$$

### 4.2 The DFT matrix

#### 4.2.1 Hermitian matrices

A Hermitian matrix is a square matrix that is equal to its own complex conjugate transpose, which can be denoted as $A^H = \\overline{A} ^ T = A$. The diagonal elements of a Hermitian matrix are necessarily real, and the off-diagonal elements come in complex conjugate pairs. For example, 

$$  
\\bar{A}^T=A=\\left\[\\begin{array}{cc}  
2 & 3+i \\\\  
3-i & 5  
\\end{array}\\right\]  
$$

#### 4.2.2 Unitary matrices

A unitary matrix is a square matrix with perpendicular columns of unit length. As we learned from section [3.3](#33-complex-basis-function), given a set of complex vectors $Q = \[\\mathbf{q\_1}, \\mathbf{q\_2}, \\mathbf{q\_3}, ..., \\mathbf{q\_n}\]$, they are orthonormal if:

$$  
\\overline{\\mathbf{q}}\_j \\mathbf{q}\_k= \\begin{cases}0 & j \\neq k \\\\ 1 & j=k\\end{cases}  
$$

Thus, we have $Q^HQ=I$. Here we say $Q$ is a unitary matrix. 

#### 4.2.3 Properties of DFT matrix

We denote the Fourier matrix as 

$$F\_N = \\left\[\\begin{array}{cccccc}  
1 & 1 & 1 & 1 & \\cdots & 1 \\\\  
1 & w & w^2 & w^3 & \\cdots & w^{N-1} \\\\  
1 & w^2 & w^4 & w^6 & \\cdots & w^{2(N-1)} \\\\  
1 & w^3 & w^6 & w^9 & \\cdots & w^{3(N-1)} \\\\  
\\vdots & \\vdots & \\vdots & \\vdots & \\vdots & \\vdots \\\\  
1 & w^{N-1} & w^{2(N-1)} & w^{3(N-1)} & \\cdots & w^{(N-1)^2}  
\\end{array}\\right\]$$

*   **Proof** that the columns in the Fourier matrix are orthogonal:

       Given $w = e^{i\\left(\\frac{2 \\pi}{N}\\right)}$, the inner product between the columns can be written as $$\\sum\_{n=0}^{N-1} \\omega^{(k-l) n}$$

       If $k=l$, each term equals 1. If $k \\neq l$ the sum is given by the [geometric series formula](https://mathworld.wolfram.com/GeometricSeries.html) as $$\\frac{1-\\omega^{N(k-l)}}{1-\\omega^{k-l}}$$

       As $w^N = 1$, we can conclude that the inner product of the columns of the Fourier matrix (as complex vectors) can be written as 

$$  
\\sum\_{n=0}^{N-1} e^{i\\left(\\frac{2 \\pi}{N}\\right) n k} e^{-i\\left(\\frac{2 \\pi}{N}\\right) n l}=\\left\\{\\begin{array}{c}  
0, k \\neq l \\\\  
N, k=l  
\\end{array}\\right.  
$$

Thus we can obtain the normalized version of the Fourier matrix as $W = \\frac{1}{\\sqrt{N}}F\_N$. Based on the definition of unitary matrix, $W$ is a unitary matrix where the length of the column vectors is 1, and the column vectors are orthogonal. Hence, we have $W^HW= I$. As the transpose of $F\_N^T = F\_N$, we can further obtain that $\\frac{1}{N}F\_N^HF\_N = I$, or $\\frac{1}{N}\\overline F\_N F\_N = I$. Based on the definition of the inverse matrix, we have $F\_N^{-1} F\_N = I = \\frac{1}{N}F\_N^HF\_N$, and we can further obtain $F\_N^{-1}  =\\frac{1}{N}F\_N^H = \\frac{1}{N}\\overline F\_N$. The matrix of $F\_N^{-1}$ is known as DFT matrix, and can be written as:

$$  
\\frac{1}{N}\\left\[\\begin{array}{cccccc}  
1 & 1 & 1 & 1 & \\cdots & 1 \\\\  
1 & \\bar{w} & \\bar{w}^2 & \\bar{w}^3 & \\cdots & \\bar{w}^{N-1} \\\\  
1 & \\bar{w}^2 & \\bar{w}^4 & \\bar{w}^6 & \\cdots & \\bar{w}^{2(N-1)} \\\\  
1 & w^3 & w^6 & w^9 & \\cdots & w^{3(N-1)} \\\\  
\\vdots & \\vdots & \\vdots & \\vdots & \\vdots & \\vdots \\\\  
1 & \\bar{w}^{N-1} & \\bar{w}^{2(N-1)} & \\bar{w}^{3(N-1)} & \\cdots & \\bar{w}^{(N-1)^2}  
\\end{array}\\right\]\\left\[\\begin{array}{c}  
f\_0 \\\\  
f\_1 \\\\  
f\_2 \\\\  
f\_3 \\\\  
\\vdots \\\\  
f\_{N-1}  
\\end{array}\\right\]=\\left\[\\begin{array}{c}  
c\_0 \\\\  
c\_1 \\\\  
c\_2 \\\\  
c\_3 \\\\  
\\vdots \\\\  
c\_{N-1}  
\\end{array}\\right\]  
$$

Some materials write the DFT as: $$X\[k\]=\\sum\_{n=0}^{N-1} x\[n\] e^{-k \\frac{2 \\pi n i}{N}}$$

where $\\overline w = e^{- \\frac{2 \\pi n i}{N}}$, thus each $X\[k\]$ is the inner product of each row in matrix $\\overline F\_N$ with the sample values ranging from $f\_0$ to $f\_{N-1}$. However, as per the equation of obtaining $X\[k\]$, the Fourier coefficients are computed by dividing $X\[k\]$ by $N$.

Also, some materials mark DFT matrix as 

$$  
W=\\frac{1}{\\sqrt{N}}\\left\[\\begin{array}{cccccc}  
1 & 1 & 1 & 1 & \\cdots & 1 \\\\  
1 & \\omega & \\omega^2 & \\omega^3 & \\cdots & \\omega^{N-1} \\\\  
1 & \\omega^2 & \\omega^4 & \\omega^6 & \\cdots & \\omega^{2(N-1)} \\\\  
1 & \\omega^3 & \\omega^6 & \\omega^9 & \\cdots & \\omega^{3(N-1)} \\\\  
\\vdots & \\vdots & \\vdots & \\vdots & \\ddots & \\vdots \\\\  
1 & \\omega^{N-1} & \\omega^{2(N-1)} & \\omega^{3(N-1)} & \\cdots & \\omega^{(N-1)(N-1)}  
\\end{array}\\right\]  
$$

where $\\omega = e^{-\\frac{2\\pi i}{N}}$ is the primitive $N$ th root of unity. 

## 5\. Fast Fourier Transform

A fast Fourier transform (FFT) is an algorithm that computes the DFT of a sequence, or its inverse (IDFT). As we have discussed in [1.4](#14-matrix-vector-multiplication), the matrix and vector multiplication has a property that swapping the corresponding columns in the matrix and rows in the vector would not change the result. The FFT algorithm is utilizing this property to accelerate the computation process. As we introduced in the previous section, the computation of Fourier coefficients by DFT is multiplying the DFT matrix with the vector containing sample values. The number of multiplication operations required is $N \\times N$. 

$$  
\\left\[\\begin{array}{cccccc}  
1 & 1 & 1 & 1 & \\cdots & 1 \\\\  
1 & \\bar{w} & \\bar{w}^2 & \\bar{w}^3 & \\cdots & \\bar{w}^{N-1} \\\\  
1 & \\bar{w}^2 & \\bar{w}^4 & \\bar{w}^6 & \\cdots & \\bar{w}^{2(N-1)} \\\\  
1 & w^3 & w^6 & w^9 & \\cdots & w^{3(N-1)} \\\\  
\\vdots & \\vdots & \\vdots & \\vdots & \\vdots & \\vdots \\\\  
1 & \\bar{w}^{N-1} & \\bar{w}^{2(N-1)} & \\bar{w}^{3(N-1)} & \\cdots & \\bar{w}^{(N-1)^2}  
\\end{array}\\right\]\\left\[\\begin{array}{c}  
f\_0 \\\\  
f\_1 \\\\  
f\_2 \\\\  
f\_3 \\\\  
\\vdots \\\\  
f\_{N-1}  
\\end{array}\\right\]  
$$

Now, we start with a simple example of a $4 \\times 4$ DFT matrix, and re-allocate the columns by grouping them into odd and even columns as $\\tilde{F\_4}$. The matrix can be further decomposed into four segments.

<p align="center" width="100%">
<img width = "40%" src = "https://user-images.githubusercontent.com/117964124/223627839-24e2ec6c-1a5b-474b-b9e7-6f9d8258e2cb.png">
</p>

As four dimensional DFT matrix can be written as 

$$  
\\widetilde{F\_4}=\\left\[\\begin{array}{cc}  
F\_2 & D\_2 F\_2 \\\\  
F\_2 & -D\_2 F\_2  
\\end{array}\\right\]  
$$

We can further extend the decomposition to general DFT matrix as

$$  
\\widetilde{F\_{2 N}}=\\left\[\\begin{array}{cc}  
F\_N & D\_N F\_N \\\\  
F\_N & -D\_N F\_N  
\\end{array}\\right\]  
$$

where $D\_N$ is 

$$  
D\_N=\\left\[\\begin{array}{ccccc}  
1 & & & & \\\\  
& \\bar{w} & & & \\\\  
& & \\bar{w}^2 & & \\\\  
& & & \\ddots & \\\\  
& & & &\\bar{w}^{N-1}  
\\end{array}\\right\]  
$$

*   **Proof** of the decomposition can be given as follows:

       Suppose that $F\_{2N}$ can be decomposed into 4 segments and $\\overline w^{2N}=\\overline w^0 = 1$, we can obtain the even column of the **top left** $F\_N$ as $\[1,\\omega^x, \\omega^{2x}, \\omega^{3x}, ...,  \\omega^{(N-1)x}\]$, following the same column of the **bottom left** segment as $\[\\omega^{Nx}, \\omega^{(N+1)x}, \\omega^{(N+2)x}, ..., \\omega^{(2N-1)x}\]$, where $\\omega=\\overline w$, and $x \\in \\{0, 2, 4, ...,2\\left(N-1\\right)\\}$, $\\omega^x$ is the root unit of the $j$ th column. As $x$ is even number, we have $\\omega^{Nx} = \\omega^{2N} = \\omega^0 = 1$, and $\\omega^{(N+m)x} = \\omega^N \\cdot \\omega^{mx} = \\omega^{mx}$. Thus, we have the top left segment $F\_N$ equals to the bottom left segment.

       Based on the left segments we investigated, we can then obtain the **top right** segment as $\[1,\\omega^{\\left( x+1 \\right)}, \\omega^{2\\left( x+1 \\right)}, \\omega^{3\\left( x+1 \\right)}, ...,  \\omega^{(N-1)\\left( x+1 \\right)}\]$, which is given by $D\_N F\_N$. The **bottom right** segment following the same column is marked as $\[\\omega^{N\\left( x+1 \\right)}, \\omega^{(N+1)\\left( x+1 \\right)}, \\omega^{(N+2)\\left( x+1 \\right)}, ...,  \\omega^{(2N-1)\\left( x+1 \\right)}\]$. As $x$ is even number and $\\omega^N = -1$, we have $\\omega^{(N+m)\\left( x+1 \\right)} = \\omega^{Nx} \\cdot \\omega^{mx} \\cdot \\omega^N \\cdot \\omega^m = -\\omega^{m\\left(x+1\\right)}$, thus we have the bottom right segment marked as $-D\_N F\_N$.

*   **Overview of FFT:**

       Given $\\mathbf{x}=\[f\_0, f\_1, ..., f\_{N-1}\]^T$, we will implement the following steps of FFT to obtain the Fourier coeffiecients:

$$  
F\_N \\mathbf{x}=\\widetilde{F\_N}\\left\[\\begin{array}{c}  
f\_0 \\\\  
f\_2 \\\\  
\\vdots \\\\  
f\_{N-2} \\\\  
f\_1 \\\\  
f\_3 \\\\  
\\vdots \\\\  
f\_{N-1}  
\\end{array}\\right\]=\\left\[\\begin{array}{cc}  
F\_{\\frac{N}{2}} & D\_{\\frac{N}{2}} F\_{\\frac{N}{2}} \\\\  
F\_{\\frac{N}{2}} & -D\_{\\frac{N}{2}} F\_{\\frac{N}{2}}  
\\end{array}\\right\]\\left\[\\begin{array}{c}  
f\_0 \\\\  
f\_2 \\\\  
\\vdots \\\\  
f\_{N-2} \\\\  
f\_1 \\\\  
f\_3 \\\\  
\\vdots \\\\  
f\_{N-1}  
\\end{array}\\right\]=\\left\[\\begin{array}{cc}  
I & D\_{\\frac{N}{2}} \\\\  
I & -D\_{\\frac{N}{2}}  
\\end{array}\\right\]\\left\[\\begin{array}{cc}  
F\_{\\frac{N}{2}} & \\\\  
& F\_{\\frac{N}{2}}  
\\end{array}\\right\]\\left\[\\begin{array}{c}  
f\_0 \\\\  
f\_2 \\\\  
\\vdots \\\\  
f\_{N-2} \\\\  
f\_1 \\\\  
f\_3 \\\\  
\\vdots \\\\  
f\_{N-1}  
\\end{array}\\right\]= \\left\[\\begin{array}{cc}  
I & D\_{\\frac{N}{2}} \\\\  
I & -D\_{\\frac{N}{2}}  
\\end{array}\\right\]\\left\[\\begin{array}{c}  
F\_{\\frac{N}{2}} \\mathbf{x}\_{\\text {even }} \\\\  
F\_{\\frac{N}{2}} \\mathbf{x}\_{\\text {odd }}  
\\end{array}\\right\]  
$$

       where $\\widetilde {F\_N}$ is the Fourier matrix with exchanged odd and even columns. **Usually, for the compuation efficiency,** $2^k$ **is recommended as the sampling length to compute the Fourier coefficients** ([Danielson-Lanczos Lemma](https://mathworld.wolfram.com/Danielson-LanczosLemma.html)). If the number of points $N$ is not a power of two, a transform can be performed on sets of points corresponding to the prime factors of N which is slightly degraded in speed.

$$ F\_N \\mathbf{x} = \\left\[\\begin{array}{cc}  
I & D\_{\\frac{N}{2}} \\\\  
I & -D\_{\\frac{N}{2}}  
\\end{array}\\right\]\\left\[\\begin{array}{c}  
F\_{\\frac{N}{2}} \\mathbf{x}\_{\\text {even }} \\\\  
F\_{\\frac{N}{2}} \\mathbf{x}\_{\\text {odd }}  
\\end{array}\\right\] $$

       For the computation of $F\_N \\mathbf{x}$, we need $N \\times N = N^2$ multiplication operations. Thus, for the computation of $F\_{\\frac{N}{2}}\\mathbf{x}\_\\mathrm{even/odd}$, we require $\\frac{N}{2} \\times \\frac{N}{2} =  \\frac{N^2}{4}$ operations each. Also, as $D\_{\\frac{N}{2}}$ is diagonal matrix, the number of multiplication operations between diagonal matrix and vector is $\\frac{N}{2}$ each here. Hence, the total multiplication operations we need for the FFT based method is $2 \\times \\left( \\frac{N}{2} \\right)^2 + N$ as compared to the original computation which requires $N^2$. The use of FFT becomes more beneficial as N increases. Think that we can further break down $F\_{\\frac{N}{2}}$ into $F\_{\\frac{N}{4}}$ to compute $F\_{\\frac{N}{2}} \\mathbf{x}\_{\\mathrm{even/odd}}$ by repeating the steps above, the total computation complexity decreases from $O(N^2)$ to $O(NlogN)$. 

*   **Implementation of FFT (recursion example):**

<p align="center" width="100%">
<img width = "70%" src = "https://user-images.githubusercontent.com/117964124/224487371-33ddeb2b-eb7c-42a2-9fff-faaca71032c7.png">
</p>

## 6\. The Continuous-time Fourier Transform

<p align="center" width="100%">
<img width = "40%" src = "https://user-images.githubusercontent.com/117964124/224715672-40c715ab-0cc4-4af6-8020-5ccb5ecaa8f3.png">
</p>

Given a signal $x(t)$ with a finite duration, which means $x(t) = 0$ for $|t| > T\_1$. Based on the aperiodic signal, we can construct an infinite periodic signal $\\widetilde{x}(t)$. From the definition of Fourier series representing the periodic signals, we have 

$$  
\\widetilde{x}(t)=\\sum\_{n=-\\infty}^{+\\infty} c\_n e^{i n \\omega\_0 t}  
$$

and 

$$  
c\_n=\\frac{1}{T} \\int\_{-T / 2}^{T / 2} \\widetilde{x}(t) e^{-i n \\omega\_0 t} d t  
$$

where $\\omega\_0 = \\frac{2\\pi}{T}$. Since $\\widetilde{x}(t) = x(t)$ for $|t| \< T\_1$ and $x(t) = 0$ for $|t| > T\_1$, we have 

$$  
c\_n=\\frac{1}{T} \\int\_{-T / 2}^{T / 2}x(t) e^{-i n \\omega\_0 t} d t = \\frac{1}{T} \\int\_{-\\infty}^{\\infty} x(t) e^{-i n \\omega\_0 t} d t  
$$

To move from periodic functions $\\widetilde{x}(t)$ (with period T) to aperiodic functions $x(t)$ we simply let the period get very large ($T \\rightarrow \\infty$), the behavior as $T$ increases can be found [here](https://lpsa.swarthmore.edu/Fourier/Series/FTFS.html). Thus, as $T \\rightarrow \\infty$, $\\omega\_0 = \\frac{2\\pi}{T} \\rightarrow 0$, which means $n \\omega\_0$ can take on any value, so we can define a new variable $\\omega = n \\omega\_0$. By defining $X(\\omega) = T c\_n$, we have the **Fourier transform** of $x(t)$ as 

$$  
X(\\omega)=\\int\_{-\\infty}^{\\infty} x(t) e^{-i \\omega t} d t  
$$

Note that the coefficients $c\_n = \\frac{1}{T}X(n\\omega\_0)$, so $\\widetilde{x}(t)$ can be represented as 

$$  
\\widetilde{x}(t)=\\sum\_{n = -\\infty}^{+\\infty} \\frac{1}{T} X\\left(n \\omega\_0\\right) e^{i n \\omega\_0 t}=\\frac{1}{2 \\pi} \\sum\_{\\omega=-\\infty}^{+\\infty} X\\left(\\omega\\right) e^{i \\omega t} \\omega\_0  
$$

As $T \\rightarrow \\infty$, $\\widetilde{x}(t) \\rightarrow x(t)$ $\\omega\_0$ is getting small and can be replaced by $d\\omega$, we can get the **inverse Fourier transform** as

$$  
x(t)=\\frac{1}{2 \\pi} \\sum\_{n=-\\infty}^{+\\infty} X(\\omega) e^{i \\omega t} d \\omega=\\frac{1}{2 \\pi} \\int\_{-\\infty}^{+\\infty} X(\\omega) e^{i \\omega t} d \\omega  
$$

## 7\. Reference

In addition to the links provided in the article, here are some additional references.

(1) Yao, J. (n.d.). Chapter 4: Amplitude Modulation (AM). University of Ottawa. Retrieved from [https://www.site.uottawa.ca/~jpyao/courses/ELG3120_files/ch4.pdf](https://www.site.uottawa.ca/~jpyao/courses/ELG3120_files/ch4.pdf)

(2) Chang Xiao. (n.d.). \[YouTube Channel\]. YouTube. Retrieved from https://www.youtube.com/@changxiao2687
