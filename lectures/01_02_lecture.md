# Lecture 2: What Classical Computers Are Bad At?

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

N = 30  # Number of guests

# Generate the upper triangle with -1 to 1
upper = np.triu(np.random.uniform(-1, 1, (N, N)), k=1)

# Make it symmetric (Matrix + Transpose)
# The diagonal remains 0 because k=1 in the step above
drama_matrix = upper + upper.T

print(drama_matrix)
```


## Link to the 30 x 30 drama matrix
Here is the link to the 30 x 30 drama matrix.  It loads with gibberish string. Change name to ``drama_matrix.csv".  

{download}`Download Drama Matrix CSV <./01_02_lecture_files/drama_matrix.csv>`

Import with 
```python
# Load your drama matrix (or generate for testing)
drama_matrix = np.loadtxt('drama_matrix.csv', delimiter=',', dtype=float)
```

%[Download Drama Matrix CSV](./01_02_lecture_files/drama_matrix.csv)     # need to add to YAML if do this 
