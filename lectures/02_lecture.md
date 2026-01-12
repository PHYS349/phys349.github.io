# Lecture 2: What Classical Computers Are Bad At — and Why Quantum Is Hard

**Goals of today**:
1. What are classical computers good at?
2. What are classical computers bad at?
3. Why is quantum hard?


Before we talk about why quantum systems are difficult to simulate, we should be honest about something:

> **Classical computers are incredibly good at many problems.**

The success of modern science, engineering, and AI is not accidental. It is the result of an almost perfect match between classical computers and the structure of many physical and mathematical problems.  Let’s start there.
## What are classical computers good at?

### 1. Simulating classical dynamics

%![[Pasted image 20260108111749.png|300]]

Suppose you want to simulate a classical physical system:  particles moving under forces. For example billions of stars orbiting around a galaxy.  What does a classical simulation actually do?

Let's say you want to simulate
- $N$ particles
- $T$ time steps

At each time step:
1. You store the positions and velocities of all particles
$$\vec{x} = \begin{bmatrix} x_0(t) \\ x_1(t) \\ x_2(t) \\ \vdots \end{bmatrix}, \quad \vec{v} = \begin{bmatrix} v_0(t) \\ v_1 \\ v_2 \\ \vdots \end{bmatrix}$$
2. You compute the forces
3. You update positions and velocities  
4. You repeat

If the particles are not interacting with each other - for example they are all attracted to some central mass - then the total computational cost scales roughly like:  
$$  
\text{Cost} \sim N \times T  
$$
However, if every particle also interacts with every other particle at every time step, then it can in general can be represented by:
$$
\underbrace{
\begin{bmatrix}
\cdot & \cdot & \cdots & \cdot \\
\cdot & \cdot & \cdots & \cdot \\
\vdots & \vdots & \ddots & \vdots \\
\cdot & \cdot & \cdots & \cdot
\end{bmatrix}
}_{N \times N}
\begin{bmatrix}
x_0 \\
\vdots \\
v_0 \\
\vdots
\end{bmatrix}
\xrightarrow{\text{interacting}} N^2 \text{ computations}
$$
For $T$ time steps, this will take $N^2 \times T$ computations.   This is still polynomial scaling.  

If you double the number of particles, the computation becomes 4x harder — but not catastrophically harder.

At its core, modern computing is built on linear algebra: the repeated multiplication of matrices and vectors. And classical computers are astonishingly good at this.

The core operation of modern computation is:  
$$
\text{matrix} \times \text{vector}  
$$
If you have:
- an $N \times N$ matrix
- multiplied by an (N)-dimensional vector
The cost scales as:  
$$O(N^2)   $$
Again: polynomial scaling.

We have now entered the age of AI.  Machine learning is almost nothing but repeated matrix–vector multiplications.

%![[Pasted image 20260108124117.png|300]]

A modern neural network:
- represents information as vectors
- repeatedly applies linear transformations
- followed by nonlinear activation functions

Large language models work this way:

- a token is represented as a high-dimensional vector
- that vector is multiplied by large matrices
- the result is fed forward through many layers

The vectors can be huge — tens or hundreds of thousands of dimensions — but the operations are still matrix–vector products.

These are exactly the operations GPUs were built to accelerate. Classical hardware is doing what it does best.  

This is why classical simulations of our climate or galaxies—and even CGI in movies and games—can be so accurate. GPUs are designed for exactly this kind of workload: many simple calculations repeated over and over on huge amounts of data.

A modern GPU can have more than 10,000 cores, each performing a small operation every clock cycle. In a particle simulation, each particle update is essentially the same computation, just with different input values. This pattern is called data parallelism.

For example, an NVIDIA GeForce RTX 4090 has 16,384 CUDA cores and peak throughput on the order of $80$ TFLOPS (roughly $80 \times 10^{12}$ floating-point operations per second), which is why it can update and render massive systems so efficiently.

Classical physics and linear algebra maps _beautifully_ onto classical hardware.


## What are classical computers bad at?

Let’s start with a concrete question. **Suppose you want to compute the lowest-energy configuration of a molecule consisting of nuclei and electrons.**

This is not an abstract problem.  Nature solves is constantly solving it. If molecules are excited, they emit light, loosing energy until it relaxes to the ground state. 

That ground state determines chemical and material properties.  So the question is not _whether_ a solution exists.   The question is whether a classical computer can find it.

First a question for you...

> [!iClicker]
> # Complexity Threshold
> We want to simulate the positions of the nuclei and electrons of a molecule in the lowest energy configuration?  **At what number of atoms do you think classical computation breaks down?**
>
> - $2$
> - $10$
> - $100$
> - $10^4$
> - $10^8$
> - $10^{12}$
> - $10^{15}$
> - $10^{18}$
> - $10^{20}$

The answer is about 10.  Even with as few as ten nuclei, exact calculations become impossible and approximations are unavoidable. Welcome to the field of **computational chemistry**. The central question becomes: _what are the most accurate approximations we can make?_

At first glance, it seems like classical computers _should_ be able to do this.  After all:
- We simulate trillions of trajectories in weather models 
- We simulate the motion of stars in a galaxy
- For goodness sake, we have semi-accurate simulations of black holes!

So why not molecules? Why not just 
- try many configurations
- compute their energies
- pick the lowest one?

This sounds reasonable.  Let’s examine why it fails.

>[!Question]
>> **If classical computers can simulate trillions of trajectories, why can’t they find the ground state of a moderately large molecule?**


## Why is quantum hard? 

The difficulty is that quantum systems do not have a single configuration.  They exist in **superposition**.  Classical simulations follow definite trajectories - each particle has a definite position at any given time.  

Nature however is not so simple.  **Nature explores all possible configurations simultaneously!**  

To understand this let's go back to the beginning of quantum mechanics and see what quantum theory forced us to do.
### Quantum superposition
![[Pasted image 20260108112253.png|300]]
Recall the Schrödinger picture for a single particle.  A quantum particle does not occupy one position at a time.  It occupies many positions simultaneously, each with:
- an amplitude
- a phase
We will use a slightly different language for this to prepare ourselves for what is coming.  We could say that every position is a different possible configuration for the particle.   Each of these positions has an amplitude and phase associated with it, as encoded in the wavefunction:  
$$\psi(x,t)  $$
At every position $x$,  there is a complex number whose magnitude and phase evolve in time, as governed by a wave equation, in this case the Schrodinger equation.



Complex numbers play an important role in nature, which we’ll learn about soon. For now, we’ll represent a complex number as an arrow in the complex plane:

%![[Pasted image 20260108162726.png|200]]

The angle of the arrow is the phase, and the radius is the amplitude.

Now imagine we discretize space into a set of points $x_0, x_1, \ldots, x_{N-1}$. Instead of drawing a continuous wavefunction, we draw a little phasor (unit-circle arrow) at each position:

%![[Pasted image 20260108163329.png|400]]

In our language, each position is a “configuration,” and it carries a complex number
$$
c_i = \psi(x_i).
$$

We can bundle all of these complex amplitudes into a single state vector:
$$
\vec{\psi} =
\begin{pmatrix}
c_0 \\
c_1 \\
\vdots \\
c_{N-1}
\end{pmatrix}.
$$

The interpretation is:

- $|c_i|^2$ is the probability of finding the particle at position $x_i$ if we measure position (more about measurement later).
- To guarantee the particle is somewhere, the probabilities must add to 1:
$$
\sum_i |c_i|^2 = 1.
$$

### Time evolution is linear

The Schrödinger equation tells us how the state changes in time. The key structural point is that quantum evolution is **linear**: the new vector is obtained by applying a linear operator (a matrix) to the old vector.

So we can write, for a small time step $\Delta t$,
$$
\underbrace{
\begin{pmatrix}
c_0(t+\Delta t) \\
c_1(t+\Delta t) \\
\vdots \\
c_{N-1}(t+\Delta t)
\end{pmatrix}
}_{\psi(t+\Delta t)}
=
\underbrace{
\begin{pmatrix}
H_{00} & H_{01} & \cdots & H_{0,N-1} \\
H_{10} & H_{11} & \cdots & H_{1,N-1} \\
\vdots & \vdots & \ddots & \vdots \\
H_{N-1,0} & H_{N-1,1} & \cdots & H_{N-1,N-1}
\end{pmatrix}
}_{N\times N}
\underbrace{
\begin{pmatrix}
c_0(t) \\
c_1(t) \\
\vdots \\
c_{N-1}(t)
\end{pmatrix}
}_{\psi(t)}.
$$

If this matrix is dense, that update takes about $O(N^2)$ operations per time step.

We are still okay!!! This is still **polynomial** scaling. GPUs got this.

And we can take this all the way down to the simplest case, where the particle can only be in **two** locations…

### Two-position system

Suppose the particle can only be in two positions: Left or Right. We draw a unit circle at each site to represent phase and amplitude.

![[Pasted image 20260108130320.png]]

In Dirac (bra–ket) notation, the state is:
$$
|\Psi\rangle = c_0|L\rangle + c_1|R\rangle.
$$

Normalization becomes:
$$
|c_0|^2 + |c_1|^2 = 1.
$$

### Time evolution is linear

A key feature of quantum mechanics is linearity: time evolution acts linearly on the state vector. Over a short time step $\Delta t$, we can write the update as a $2\times 2$ matrix acting on the coefficient vector:

$$
\underbrace{
\begin{pmatrix}
c_0(t+\Delta t) \\
c_1(t+\Delta t)
\end{pmatrix}
}_{\psi(t+\Delta t)}
=
\underbrace{
\begin{pmatrix}
H_{00} & H_{01} \\
H_{10} & H_{11}
\end{pmatrix}
}_{2\times 2}
\underbrace{
\begin{pmatrix}
c_0(t) \\
c_1(t)
\end{pmatrix}
}_{\psi(t)}.
$$

If we multiply it out, this is just:
$$
\begin{pmatrix}
c_0(t+\Delta t) \\
c_1(t+\Delta t)
\end{pmatrix}
=
\begin{pmatrix}
H_{00}c_0(t) + H_{01}c_1(t) \\
H_{10}c_0(t) + H_{11}c_1(t)
\end{pmatrix}.
$$

(We will later connect this matrix more precisely to the Hamiltonian and the Schrödinger equation.)

---

### A physical two-configuration system: spin

There is a real physical system with two configurations: the spin of an electron. An electron has spin $S=1/2$, and the spin along a chosen axis (usually $z$) can be either up or down.

The two basis states are:
- $|\uparrow\rangle$
- $|\downarrow\rangle$

We know this experimentally because if we send a spin-$1/2$ particle through a Stern–Gerlach apparatus, it separates into only two beams.

%![[Pasted image 20260108130345.png]]

A general state is:
$$
|\Psi\rangle = c_0|\uparrow\rangle + c_1|\downarrow\rangle,
$$
with normalization:
$$
|c_0|^2 + |c_1|^2 = 1.
$$

If $|\uparrow\rangle$ and $|\downarrow\rangle$ are energy eigenstates with energies $E_\uparrow$ and $E_\downarrow$, then the time evolution is:
$$
|\Psi(t)\rangle =
c_0\,e^{-iE_\uparrow t/\hbar}|\uparrow\rangle +
c_1\,e^{-iE_\downarrow t/\hbar}|\downarrow\rangle.
$$

This doesn’t look so bad: it’s just two complex numbers. Are you ready for things to get crazy?


### The exponential wall

> **What if we have two spins?**

Each spin can be up or down, so the system has four configurations:
$$
|\uparrow\uparrow\rangle,\;
|\uparrow\downarrow\rangle,\;
|\downarrow\uparrow\rangle,\;
|\downarrow\downarrow\rangle.
$$

Each configuration has an associated complex amplitude (magnitude + phase). The most general state is:
$$
|\Psi\rangle =
c_{00}|\uparrow\uparrow\rangle +
c_{01}|\uparrow\downarrow\rangle +
c_{10}|\downarrow\uparrow\rangle +
c_{11}|\downarrow\downarrow\rangle,
$$
with normalization
$$
|c_{00}|^2 + |c_{01}|^2 + |c_{10}|^2 + |c_{11}|^2 = 1.
$$

Already, the number of complex amplitudes has doubled—from $2$ to $4$.

Now let’s do three spins.

Each spin is up/down, so there are $2^3 = 8$ configurations:
$$
|\uparrow\uparrow\uparrow\rangle,\;
|\uparrow\uparrow\downarrow\rangle,\;
|\uparrow\downarrow\uparrow\rangle,\;
|\uparrow\downarrow\downarrow\rangle,\;
|\downarrow\uparrow\uparrow\rangle,\;
|\downarrow\uparrow\downarrow\rangle,\;
|\downarrow\downarrow\uparrow\rangle,\;
|\downarrow\downarrow\downarrow\rangle.
$$

The general state is:
$$
|\Psi\rangle =
c_{000}|\uparrow\uparrow\uparrow\rangle +
c_{001}|\uparrow\uparrow\downarrow\rangle +
c_{010}|\uparrow\downarrow\uparrow\rangle +
c_{011}|\uparrow\downarrow\downarrow\rangle +
c_{100}|\downarrow\uparrow\uparrow\rangle +
c_{101}|\downarrow\uparrow\downarrow\rangle +
c_{110}|\downarrow\downarrow\uparrow\rangle +
c_{111}|\downarrow\downarrow\downarrow\rangle,
$$
with normalization
$$
\sum_{b_1,b_2,b_3\in\{0,1\}} |c_{b_1b_2b_3}|^2 = 1.
$$

### Question

> **If one spin requires 2 amplitudes, how many amplitudes are required for:**
>
> - 2 spins?
> - 10 spins?
> - 100 spins?

### The scaling problem

This is the heart of the problem. For $N$ two-level quantum systems,
$$
\text{number of complex amplitudes} = 2^N.
$$

Let’s see how fast this explodes:

- $N=10$: $2^{10} = 1024$ amplitudes. Totally fine. (Kilobytes.)
- $N=20$: $2^{20} \approx 10^6$ amplitudes. Still fine. (Megabytes.)
- $N=40$: $2^{40} \approx 10^{12}$ amplitudes. Uh oh. (Terabytes.)
- $N=100$: $2^{100} \approx 10^{30}$ amplitudes. Absolutely not.

No classical computer could ever even store that state—forget trying to time-evolve it. This is not a hardware problem. It’s a scaling problem.

### Why classical simulation fails

Classical computers are good at:

- tracking one configuration
- evolving it forward in time
- repeating this many times

Quantum systems require:

- tracking **many configurations at once**
- including their **relative phases**
- which produce **interference**

This is why brute-force search fails, trajectory sampling fails, and “just use a GPU” fails.

Nature is not sampling configurations.
It is evolving the **entire superposition at once**.

> **Quantum mechanics is hard to simulate classically because classical computers were never designed to store or manipulate exponentially large state spaces.**

This is not a bug. It is the central insight that leads to quantum computing.

---

### The next question

If classical computers fail because they cannot represent quantum states…
and nature evolves quantum states effortlessly…

> **Can we use quantum systems themselves as computers?**

This is essentially Feynman’s famous motivation for quantum computing: if nature is quantum, then simulating nature should be done with a quantum machine.

---

### A moment of respect for nature

With your new appreciation for the complexity of quantum systems, let’s admire what nature is quietly doing all around us.

On the left is a caffeine molecule—this is hard, but with supercomputers and many approximations we can often get decent results.

What about the protein on the right?

![[Pasted image 20260108132754.png]]

At this point you might ask: do we really need to keep track of *all* those amplitudes? What if only a polynomial number are important, and the rest are basically zero?

That’s a great question. But then you have a new problem: **how do you know which ones are important?**

One approach is physics intuition and approximation:
- maybe far-apart atoms don’t strongly affect each other,
- maybe only certain orbitals matter,
- maybe correlations are “local.”

That’s the art (and pain) of computational chemistry. It gets us far—but it also raises a brutal question:

How do we know the approximation is accurate, if the exact calculation is already impossible?





%For later: 

%![[Pasted image 20260108145808.png]]
%Are all of the bases necessary?  Sometimes - 




---


## Homework

### Permutations and the “seating explosion”

You have $N$ people and $N$ seats in a row.

**(a)** How many distinct seatings are possible? (Give an exact expression.)

**(b)** Evaluate the number for $N=5,\,10,\,20$.

**(c)** Use Stirling’s approximation
$$
N! \approx \sqrt{2\pi N}\left(\frac{N}{e}\right)^N
$$
to estimate $\log_{10}(N!)$ for $N=50$. (You can keep only the dominant terms.)

**(d)** Based on (c), is $N!$ polynomial, exponential, or something else? Explain in one sentence.

---

###  “Which scaling is it?” (classification)

Put these in order from *slowest growth* to *fastest growth* for large $N$:

1. $N^2$
2. $3N + 7$
3. $N^5$
4. $2^N$
5. $10^N$
6. $N!$
7. $N^N$
8. $\log(N)$
9. $N \log(N)$ 


---

### Linear algebra costs (matrix/vector scaling)

Assume dense objects (no special structure), and count the number of multiply-add operations to leading order.

**(a)** Dot product of two length-$N$ vectors: $\vec{a}\cdot\vec{b}$.

**(b)** Multiply an $N\times N$ matrix by a length-$N$ vector.

**(c)** Multiply two $N\times N$ matrices.

For each: give big-O scaling and a one-line explanation.


---

###  Configurations of spins (classical counting)

A spin-$1/2$ has two possible measurement outcomes along $z$: $|\uparrow\rangle$ or $|\downarrow\rangle$.

**(a)** List all basis configurations for 4 spins along $z$. (There should be $2^4$ of them.)

**(b)** In general, how many basis configurations exist for $N$ spins? (Give an expression.)

**(c)** If you store one complex amplitude per configuration using 16 bytes (8 bytes real + 8 bytes imaginary), estimate the memory required to store a general state for:
- $N=30$
- $N=40$
- $N=50$

---
###  “How many spins could you model?” (back-of-the-envelope)

Assume you have a computer with 16 GB of usable RAM.

**(a)** Using the same 16-bytes-per-amplitude assumption, what is the largest $N$ such that you can store all $2^N$ amplitudes in memory?

**(b)** Why is “storing the state” only the first problem? Give one additional reason simulation can be harder than memory alone.



---
### Homework: Wedding Planning

The goal of this homework is to show that “hard” problems are not unique to quantum mechanics. Even in the classical world, there are problems where the number of possibilities explodes so fast that “just try them all” becomes impossible.

In this problem, you are a wedding planner trying to seat guests to minimize drama.

Imagine you have a list of guests and a set of tables. You want to create a seating chart where everyone is happy. You might have a a couple different preferences and constraints:

1. **Preferences:** Aunt May really wants to sit near the window. (This is a local bias).
2. **Interactions:** If you put Uncle John and Cousin Bob next to each other, they will fight. (This is a pair-wise penalty).

%![[Pasted image 20260108145758.png]]

In this problem you will use Python to find the optimum, the solution solution that minimizes drama.  How many guests do you think you can do on your laptop or desktop?   

*Please see the GitHub page Python Resources for help getting started with Python if you are new to it.  LLM's are great for learning coding - just make sure you know what it is doing - for example don't just put this whole problem in and ask for a solution - but it is fine if you ask "How do I make a random matrix in Python ?"  **

*For Homework submission, upload your python code converted to a PDF with comments.
*

#### Part A: Setup
To make our lives easy, we will seat guests all in a single line with positions i =  0 to N-1.   We will also only assume there are "interaction" preferences -  guest 1 does not want to sit next to guest 10, but not guest 1 wants to sit at seat 10.  

You are seating $N$ guests in a row. Label the guests by integers:
$$
0,1,2,\ldots,N-1.
$$
Let's start with $N=5$ guests, 
$$
N=5,\quad \text{guests } \{0,1,2,3,4\}.
$$
Each pair of guests $(i,j)$ has a “drama score” $D_{ij}$:

- $D_{ij} > 0$ means guest $i$ dislikes sitting next to guest $j$
- $D_{ij} < 0$ means guest $i$ likes sitting next to guest $j$
- $D_{ij} = 0$ means neutral

Here is your first drama matrix: 

| $D_{ij}$ |   0 |   1 |   2 |   3 |   4 |
| -------: | --: | --: | --: | --: | --: |
|    **0** |   0 |   3 |  -1 |   2 |   0 |
|    **1** |   3 |   0 |   4 |  -2 |   1 |
|    **2** |  -1 |   4 |   0 |   5 |  -3 |
|    **3** |   2 |  -2 |   5 |   0 |   2 |
|    **4** |   0 |   1 |  -3 |   2 |   0 |
In this matrix, we assumed $D_{ij}=D_{ji}$ and  $D_{ii}=0$.

In Python make the matrix:

```python
import numpy as np

# Define the matrix manually
drama_matrix = np.array([
    [ 0,  3, -1,  2,  0],
    [ 3,  0,  4, -2,  1],
    [-1,  4,  0,  5, -3],
    [ 2, -2,  5,  0,  2],
    [ 0,  1, -3,  2,  0]
])

print(drama_matrix)
```


A seating arrangement is a permutation of the guests:
$$
s = [s_0, s_1, \ldots, s_{N-1}],
$$
where each guest appears exactly once.

Example seating for $N=5$:
$$
s=[0,1,2,3,4].
$$
Only adjacent neighbors matter:
$$
(s_0,s_1),\ (s_1,s_2),\ \ldots,\ (s_{N-2},s_{N-1}).
$$
Define the total drama energy:
$$
E(s)=\sum_{k=0}^{N-2} D_{s_k,s_{k+1}}.
$$

Your goal: Use Python to find a seating arrangement with the smallest energy (least drama).

Write a Python function that takes:
- a seating list `s`
- a drama matrix `drama_matrix`

and returns the scalar energy $E(s)$.  

Check your function on at least two different settings for $N=5$.

Check the drama score for
```python
 s = [1,4,2,0,3,5]
```

If you get $E = XXX$ you are ready to go to the next section.


#### Part B: Brute-force search
Now for the $N=5$ example use brute force searching to find the minimum drama solution. 
There is one seat $s$ that achieves it.

How many solutions did it have to search?

#### Part C:  10 Guests
Do the same for 10 guests.  
```pyton
drama_matrix = np.array([ 
	[ 0, 4, -1, 2, -5, 3, 1, -2, 0, 5], 
	[ 4, 0, 3, -2, 1, -4, 5, 0, -3, 2],
	[-1, 3, 0, 5, -2, 4, -3, 1, 2, -4], 
	[ 2, -2, 5, 0, 3, -1, 4, -5, 1, -3],
	[-5, 1, -2, 3, 0, 2, -4, 5, -1, 0],
	[ 3, -4, 4, -1, 2, 0, -2, 3, 5, -5],
	[ 1, 5, -3, 4, -4, -2, 0, 1, -5, 3],
	[-2, 0, 1, -5, 5, 3, 1, 0, 4, -1], 
	[ 0, -3, 2, 1, -1, 5, -5, 4, 0, 2], 
	[ 5, 2, -4, -3, 0, -5, 3, -1, 2, 0] ])
```

- Find the minimum solution $s$.

- How many did it have to search?

#### Part D:  The Hero Problem, 100 Guests
This part is a competition.  Whoever gets the lowest drama score gets 5 extra points on the first exam.  Please bring your $s$ with you to class on Tuesday XXX.  I will have a python file with the drama matrix on GitHub for you to download. 

- How many possible solutions are there?  Brute force becomes impossible quickly.  

- What strategies can you come up with besides using raw compute power?  Feel free to ask LLM's for help.  Use any Python library you want.  This is an optimization problem, but gradient descent optimization algorithms will not help because your input is discrete.  Maybe look into simulated annealing as a place to start?     

#### Hints:
If you want to make your own drama matrix for testing, use this:
```python
import numpy as np

# Set the seed so every student gets the EXACT same numbers
np.random.seed(42)

N = 10 # Number of guests

# Generate the upper triangle with random integers (-5 to 5)
upper = np.triu(np.random.randint(-5, 6, (N, N)), k=1)

# Make it symmetric (Matrix + Transpose)
# The diagonal remains 0 because k=1 in the step above
drama_matrix = upper + upper.T

print(drama_matrix)
```