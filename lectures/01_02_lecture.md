# Lecture 2: What Are Classical Computers Bad At?

**Goals of today**:
1. What are classical computers good at?
2. What are classical computers bad at?
%3. Why is quantum hard?


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


Here's a draft section to add. This would fit well right after you discuss matrix-vector multiplication and before you mention GPUs being good at this:

---

## How Fast Is Your Computer?

Before we can understand what problems are hard, we need to understand what computers actually *do*—and how fast they do it.

### The CPU: One Thing at a Time

At its core, a CPU (Central Processing Unit) is surprisingly simple. It has:

- **Registers**: small memory slots that hold numbers (e.g., 32-bit or 64-bit values)
- **An ALU** (Arithmetic Logic Unit): the circuit that actually adds, multiplies, etc.

A single operation looks like this:

$$
R_0, R_1 \xrightarrow{\text{ALU}} R_2
$$

Take two numbers from registers, do one arithmetic operation, store the result.

How fast can this happen? A modern CPU runs at roughly 3 GHz—that's $3 \times 10^9$ clock cycles per second. If we assume (optimistically) one operation per cycle, that gives us:

$$
\text{CPU} \sim 3 \times 10^9 \text{ operations/second}
$$

That sounds fast. But for large simulations, it's not nearly enough.

### The GPU: Many Things at Once

A GPU (Graphics Processing Unit) takes a different approach. Instead of one very fast ALU, it has *thousands* of smaller ALUs running in parallel.

A modern GPU like the NVIDIA RTX 4090 has:

$$
\sim 16{,}384 \text{ cores}
$$

Each core can do roughly $3 \times 10^9$ operations per second. Running in parallel:

$$
3 \times 10^9 \times 16{,}384 \approx 50 \times 10^{12} \text{ operations/second}
$$

This is measured in **FLOPS** (Floating Point Operations Per Second):

$$
\text{GPU} \sim 30\text{–}80 \text{ TFLOPS} \quad (10^{12} \text{ FLOPS})
$$

That's roughly 10,000 times faster than a single CPU core—but only for problems that can be split into many independent calculations.

### Why This Matters

Remember our classical simulation:

- Update $N$ particles
- Each update is the same operation with different data
- Perfect for parallelization

This is why GPUs dominate:
- Physics simulations
- Movie CGI and video games
- Training neural networks (matrix multiplications)

Classical computers aren't just "fast." They're fast *at specific things*: problems that decompose into many independent, repeated operations.


Here's a draft section that could follow the CPU/GPU discussion:

---

## Example: How Does an LLM Work?

You've probably used ChatGPT or Claude. These are **Large Language Models** (LLMs). Let's peek under the hood and estimate how much computation goes into generating a single word.

### Step 1: Words Become Vectors

When you type a prompt like:

> "How long should I cook raw chicken in the air fryer?"

The model doesn't see words—it sees numbers. Each word (or "token") is converted into a vector through an **encoding** process.

A typical embedding dimension is around $N \sim 10{,}000$. So the word "fish" becomes a vector:

$$
\vec{x}_{\text{fish}} = \begin{bmatrix} x_1 \\ x_2 \\ \vdots \\ x_{10,000} \end{bmatrix}
$$

These 10,000 numbers encode the "meaning" of the word in a high-dimensional space—words with similar meanings end up as similar vectors.

### Step 2: Attention—Which Words Matter?

Here's where it gets interesting. The model needs to understand relationships between words.

In "How long do fish live?", the answer depends heavily on "fish." But in "How long do stars live?", the same question has a completely different answer.

The **attention mechanism** computes weights between every pair of words:

> How much should word $i$ "pay attention to" word $j$?

This requires comparing every word to every other word—an $N \times N$ operation for each layer.

### Step 3: Many Layers

The model doesn't just do this once. A modern LLM has roughly **80 layers**, each performing:

- Attention calculations
- Matrix multiplications
- Nonlinear transformations

Each layer refines the representation, building up from simple word meanings to complex reasoning.

### Step 4: Predict the Next Word

The final output is a vector of probabilities over the entire vocabulary—typically $\sim 80{,}000$ possible words:

$$
\vec{p} = \begin{bmatrix} P(\text{"the"}) \\ P(\text{"about"}) \\ P(\text{"fish"}) \\ \vdots \end{bmatrix}
$$

The model samples from this distribution to pick the next word, then repeats the whole process.

### Counting Operations

Let's estimate the computational cost for generating **one token**:

| Step | Operations |
|------|------------|
| Matrix multiply per layer | $(10{,}000)^2 = 10^8$ |
| Number of layers | $\sim 80$ |
| **Total** | $\sim 8 \times 10^9$ |

With a GPU running at $\sim 30$ TFLOPS ($30 \times 10^{12}$ ops/sec):

$$
\text{Time per token} \sim \frac{8 \times 10^9}{30 \times 10^{12}} \sim 0.3 \text{ ms}
$$

In practice, with memory bottlenecks and overhead:

$$
\boxed{\sim 1 \text{ ms per word}}
$$

That's why ChatGPT can type at roughly the speed you read—it's generating hundreds of words per second, each requiring billions of operations.

### The Takeaway

An LLM is essentially:
- Massive matrix multiplications
- Repeated 80 times per token
- Perfectly suited for GPUs

This is classical computing at its finest: huge linear algebra problems, embarrassingly parallel, running on hardware designed exactly for this task.


The question we'll tackle next: **Are there problems where this strategy fails?**

Here's a draft section that transitions to "what classical computers are bad at":

---

## Why Do We Need a Quantum Computer?

We've seen that classical computers—especially GPUs—are extraordinarily powerful. Matrix multiplications, particle simulations, neural networks: all conquered by brute-force parallelism.

But are there problems where this strategy fails completely?

### The Traveling Salesman Problem

Imagine you're a salesman who needs to visit 30 cities across the country. You want to find the shortest route that visits every city exactly once and returns home.

```{image} ./01_02_lecture_files/traveling_salesman.svg
:alt: Traveling Salesman Problem
:width: 500px
```
(I know this image is horrible - someone want to make a better SVG code???)

This is the famous **Traveling Salesman Problem** (TSP).

How hard could it be? Let's count the possibilities.

For $N$ cities, the number of possible routes is:
$$
N! \quad \text{(}N \text{ factorial)}
$$

Let's make a table. Assume a CPU checking $10^9$ routes per second:

| $N$ cities | Possible routes | Time to check all (CPU) |
|------------|-----------------|-------------------------|
| 10 | $10! \sim 4 \times 10^6$ | ~4 ms |
| 20 | $20! \sim 2 \times 10^{18}$ | ~80 years |
| 50 | $50! \sim 3 \times 10^{64}$ | ~$10^{48}$ years |

Wait—$10^{48}$ years? How long is that?

### Cosmic Perspective

| Event | Time |
|-------|------|
| Big Bang | 0 |
| **Now** | $1.4 \times 10^{10}$ years |
| Sun engulfs Earth | $\sim 5 \times 10^{9}$ years from now |
| Last stars burn out | $\sim 10^{14}$ years |
| Heat death of universe | $\sim 10^{100}$ years |

For 50 cities, the brute-force calculation would take $10^{48}$ years—longer than all stars will exist, but still far short of heat death.

By Stirling's approximation:
$$
N! \approx \sqrt{2\pi N} \left(\frac{N}{e}\right)^N \sim \left(\frac{N}{e}\right)^N
$$

This is faster than exponential. It's not $2^N$—it's closer to $N^N$.

### The Problem

The traveling salesman problem isn't exotic. Problems with this structure appear everywhere:

- **Logistics**: Delivery routes, airline scheduling
- **Biology**: Protein folding, gene sequencing
- **Finance**: Portfolio optimization
- **Manufacturing**: Circuit board drilling, job scheduling

These are problems where:
- The number of possibilities grows as $N!$ or $2^N$
- There's no shortcut—you can't decompose it into independent parallel tasks
- Classical computers hit a wall, no matter how fast

This is what classical computers are bad at: problems where the search space grows exponentially (or faster) and the structure doesn't allow for parallelization.


Here's the revised section with the new framing:

---

## Simulating Quantum Matter: The Spin Problem

So far we've looked at the traveling salesman—a classical optimization problem. Let's now consider something that feels more inherently quantum: **simulating actual quantum materials**.

This isn't an abstract puzzle. It's a problem that directly maps to real physics: How do the magnetic moments in a material align? What is the lowest energy state of a chunk of iron, or a high-temperature superconductor, or a quantum magnet?

### Why Do We Care About the Lowest Energy?

Here's a deep fact about nature: **systems tend toward their lowest energy state**.

Heat up a chicken, and what happens? It eventually cools off. The thermal energy dissipates into the environment until the chicken equilibrates with room temperature.

This happens everywhere, constantly:
- A hot cup of coffee cools down
- A vibrating tuning fork goes silent
- An excited atom emits a photon and relaxes

Nature is always "computing" the answer to the question: *What is the lowest energy configuration?*

When you cool a material down, the atoms and electrons rearrange themselves, settle into lower and lower energy states, and eventually approach the **ground state**—the configuration with the absolute minimum energy.

Nature does this effortlessly. For a classical computer trying to predict the answer? It can be impossibly hard.

### Spins: Tiny Bar Magnets

Many particles have a property called **spin**. For our purposes, think of each particle as a tiny bar magnet that can point either **up** (↑) or **down** (↓).

$$
\text{Spin states:} \quad |\uparrow\rangle \quad \text{or} \quad |\downarrow\rangle
$$

Now imagine a line of these tiny magnets:

$$
\uparrow \quad \downarrow \quad \uparrow \quad \downarrow \quad \uparrow \quad \downarrow \quad \uparrow \quad \downarrow
$$

### The Interaction: Neighbors Want to Anti-Align

If you've played with bar magnets, you know: opposite poles attract. Two neighboring magnets "want" to point in opposite directions:

$$
\uparrow \downarrow \quad \text{(happy—low energy)}
$$
$$
\uparrow \uparrow \quad \text{(unhappy—high energy)}
$$

If this were the only rule, the ground state would be easy to find:

$$
\uparrow \downarrow \uparrow \downarrow \uparrow \downarrow \uparrow \downarrow \quad \text{(alternating)}
$$

Simple! Polynomial complexity—just alternate.

### The Complication: External Magnetic Field

Now turn on an external magnetic field $\vec{B}$ pointing up. This field wants all spins to align with it:

$$
\uparrow \uparrow \uparrow \uparrow \uparrow \uparrow \uparrow \uparrow \quad \text{(all aligned with } B \text{)}
$$

But wait—now we have **competing demands**:

- **Neighbors** want to anti-align: ↑↓↑↓↑↓
- **External field** wants all aligned: ↑↑↑↑↑↑

What configuration actually minimizes the total energy?

### Why This Is Hard

Unlike the alternating pattern, there's no obvious answer. The ground state depends on:
- The strength of the neighbor-neighbor interaction
- The strength of the external field
- The geometry (1D chain? 2D grid? 3D lattice?)

To find the true minimum, we might need to check all possible configurations.

How many configurations are there?

Each spin has **2 choices**: ↑ or ↓

| $N$ spins | Configurations |
|-----------|----------------|
| 1 | $2$ |
| 2 | $2 \times 2 = 4$ |
| 3 | $2 \times 2 \times 2 = 8$ |
| $N$ | $2^N$ |

For $N$ spins:
$$
\text{Configurations} = \underbrace{2 \times 2 \times 2 \times \cdots \times 2}_{N \text{ times}} = 2^N
$$

Let's put numbers to this:

| $N$ spins | Configurations | Memory to store all |
|-----------|----------------|---------------------|
| 10 | $2^{10} \sim 10^3$ | ~16 KB |
| 30 | $2^{30} \sim 10^9$ | ~16 GB |
| 50 | $2^{50} \sim 10^{15}$ | ~16,000 TB |
| 100 | $2^{100} \sim 10^{30}$ | More than atoms on Earth |

### Polynomial vs. Exponential

This is the key distinction:

| Complexity | Scaling | Example | Tractable? |
|------------|---------|---------|------------|
| **Polynomial** | $N^2$, $N^3$, etc. | Non-interacting spins | ✓ Yes |
| **Exponential** | $2^N$, $N!$, etc. | Interacting spins | ✗ No |

For polynomial scaling, doubling $N$ makes the problem harder by a constant factor (4×, 8×, etc.).

For exponential scaling, **adding one spin doubles the problem size**.

$$
N \to N+1 \implies 2^N \to 2^{N+1} = 2 \times 2^N
$$

This is why interacting quantum systems are fundamentally hard to simulate classically.

### Nature as a Computer
Is there a fundamentally different way to compute?

Could we build a machine that explores many possibilities simultaneously—not by having more processors, but by exploiting the physics of superposition itself?

This is the promise of quantum computing.



Here's the remarkable thing: while we struggle to simulate 50 spins on a classical computer, _nature does it effortlessly_.

Every piece of iron, every magnetic material, every superconductor contains $10^{23}$ interacting quantum particles—and nature "computes" the ground state every time the material cools down.

Nature is a simulator doing calculations we could never do.

This raises a tantalizing question: *Could we harness the quantumness of nature itself as our computer?*

We'll explore that idea in the next lecture.




















<!-- 




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

 -->


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
# Homework: Wedding Planning
**This is the second part of the first homework.  It is due Wed at 11:59PM with Part I in Gradescope.**

The goal of this homework is to show that “hard” problems are not unique to quantum mechanics. Even in the classical world, there are problems where the number of possibilities explodes so fast that “just try them all” becomes impossible.

In this problem, you are a wedding planner trying to seat guests to minimize drama.

Imagine you have a list of guests and a set of tables. You want to create a seating chart where everyone is happy. You might have a a couple different preferences and constraints:

1. **Preferences:** Aunt May really wants to sit near the window. (This is a local bias).
2. **Interactions:** If you put Uncle John and Cousin Bob next to each other, they will fight. (This is a pair-wise penalty).

%![[Pasted image 20260108145758.png]]

In this problem you will use Python to find the optimum, the solution solution that minimizes drama.  How many guests do you think you can do on your laptop or desktop?   

*Please see the GitHub page Python Resources for help getting started with Python if you are new to it.  LLM's are great for learning coding - just make sure you know what it is doing - for example don't just put this whole problem in and ask for a solution - but it is fine if you ask "How do I make a random matrix in Python ?"  **

**For Homework submission, upload your python code converted to a PDF with comments.**

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
 s = [1,4,2,0,3]
```

If you get $E = -1$ you are ready to go to the next section. (check this and let me know if this is correct - Jon)


#### Part B: Brute-force search
Now for the $N=5$ example use brute force searching to find the minimum drama solution. 
There is one seating vector $s$ that achieves it.

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

#### Part D:  The Hero Problem, 30 Guests
This part is a competition.  Whoever gets the lowest drama score gets 5 extra points on the first exam.  Please bring your $s$ and lowest drama with you to class on Thursday 1/22 .  I will have a python file with the drama matrix on GitHub for you to download. 

- How many possible solutions are there?  Brute force becomes impossible quickly.  

- What strategies can you come up with besides using raw compute power?  Feel free to ask LLM's for help.  Use any Python library you want.  This is an optimization problem, but gradient descent optimization algorithms will not help because your input is discrete.  


#### Link to the 30 x 30 drama matrix
Here is the link to the 30 x 30 drama matrix.  It loads with gibberish string. Change name to ``drama_matrix.csv".  

{download}`Download Drama Matrix CSV <./01_02_lecture_files/drama_matrix.csv>`

Import with 
```python
# Load your drama matrix (or generate for testing)
drama_matrix = np.loadtxt('drama_matrix.csv', delimiter=',', dtype=float)
```

%[Download Drama Matrix CSV](./01_02_lecture_files/drama_matrix.csv)     # need to add to YAML if do this 



#### Hints:
If you want to make your own drama matrix for testing, use this:
```python
import numpy as np

# Set the seed so every student gets the EXACT same numbers
np.random.seed(42)

N = 10  # Number of guests

# Generate the upper triangle with -1 to 1
upper = np.triu(np.random.uniform(-1, 1, (N, N)), k=1)

# Make it symmetric (Matrix + Transpose)
# The diagonal remains 0 because k=1 in the step above
drama_matrix = upper + upper.T

print(drama_matrix)
```
