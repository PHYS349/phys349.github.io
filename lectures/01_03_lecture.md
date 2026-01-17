
# Lecture 3: Quantum Computer, the Configuration Interferer

## Why Was the Wedding Problem Hard?

Let's step back and understand what made the wedding seating problem so difficult.

### The Scaling

For $N$ guests, the number of possible seating arrangements is:
$$
N! = N \times (N-1) \times (N-2) \times \cdots \times 1
$$

Using Stirling's approximation:
$$
N! \approx \sqrt{2\pi N} \left(\frac{N}{e}\right)^N \sim N^N
$$

This is **faster than exponential**. Compare:
- Exponential: $2^N$
- Factorial: $N! \sim N^N$

For large $N$:
$$
N^N \gg 2^N
$$

So factorial scaling is even worse than the $2^N$ we discussed for spins. Both are intractable for large $N$, but $N!$ explodes even faster.

### A Brief Word on Complexity

Computer scientists classify problems by how their difficulty scales:

| Class | Scaling | Example | Tractable? |
|-------|---------|---------|------------|
| **Polynomial** | $N$, $N^2$, $N^3$, ... | Sorting a list | ✓ Yes |
| **Exponential** | $2^N$, $e^N$ | Spin ground states | ✗ No |
| **Factorial** | $N!$, $N^N$ | Traveling salesman, wedding | ✗ Very no |

There's a famous class of problems called **NP** (nondeterministic polynomial). These are problems where:
- **Finding** a solution is hard
- **Verifying** a solution is easy

The wedding problem is like this: if someone hands you a seating chart, you can quickly compute the drama score and check if it's good. But *finding* the optimal chart requires searching through $N!$ possibilities.

Whether there's a fast algorithm for NP problems is one of the great open questions in mathematics (the P vs NP problem). For now, we assume there isn't—and that means brute force is often our only option.

---

## Why Is Quantum Hard?

We've seen that some classical problems (wedding seating, traveling salesman) are hard because the search space grows as $N!$ or $2^N$.

But here's a deeper question: **why is simulating quantum systems hard?**

After all, nature does it effortlessly. Every atom, every molecule, every piece of matter is a quantum system that evolves according to the Schrödinger equation. Nature isn't "searching"—it just *is*.

To understand why classical computers struggle with quantum simulation, let's return to the spin problem—but now treat it properly as a quantum system.

---

## Your First Quantum Simulation: The Ising Model

The wedding problem was actually a disguised version of one of the most important models in quantum physics: the **Ising model**.

### The Setup

Imagine a row of spins, each of which can point up ($\uparrow$) or down ($\downarrow$):

$$
\uparrow \quad \downarrow \quad \uparrow \quad \uparrow \quad \downarrow \quad \downarrow \quad \uparrow \quad \downarrow \quad \uparrow \quad \downarrow
$$

We encode these as numbers: $s_i = +1$ for up, $s_i = -1$ for down.

$$
s = [1, -1, 1, 1, -1, -1, 1, -1, 1, -1]
$$

### The Cost Function Is the Energy

In physics, the "cost function" is the **energy**, described by the **Hamiltonian**:

$$
H(s) = \sum_{i<j} J_{ij} \, s_i s_j + \sum_i B_i \, s_i
$$

Let's unpack this:

**First term — Interactions:**
$$
\sum_{i<j} J_{ij} \, s_i s_j
$$

- $J_{ij}$ is a matrix describing how strongly spin $i$ interacts with spin $j$
- If $J_{ij} > 0$: spins want to **anti-align** (↑↓ is lower energy than ↑↑)
- If $J_{ij} < 0$: spins want to **align** (↑↑ is lower energy than ↑↓)
- This is exactly the "drama matrix" from the wedding problem!

**Second term — External field:**
$$
\sum_i B_i \, s_i
$$

- $B_i$ is a vector describing the external magnetic field at each site
- If $B_i > 0$: spin $i$ wants to point **up** (lower energy when $s_i = +1$)
- If $B_i < 0$: spin $i$ wants to point **down**

### The Problem

**Finding the ground state** means choosing the configuration $s = [s_1, s_2, \ldots, s_N]$ that minimizes $H(s)$.

This is exactly what you did in the wedding homework—except now you know you were solving a physics problem.

But wait. This is still classical. Each spin is definitely up or definitely down. We're just searching through $2^N$ classical configurations.

**Where does quantum mechanics enter?**


**Classical mechanics: one configuration evolves in time.
Quantum mechanics: a wave over all configurations evolves in time.**

**A classical system is in one configuration at a time. A quantum system is a wave spread across all configurations simultaneously—each configuration carries an amplitude and a phase, and these waves can interfere constructively or destructively.**


Here's a draft for that section:

---

## From Classical to Quantum

### The Leap

For a single particle, the transition from classical to quantum mechanics looked like this:

| Classical | Quantum |
|-----------|---------|
| Newton: $F = ma$ | Schrödinger equation |
| Single trajectory $x(t)$ | Wavefunction $\psi(x,t)$ |
| Particle is at one position | Amplitude spread over all positions |

In classical mechanics, a particle has a definite position at each moment. In quantum mechanics, the particle is described by a **wave over all possible positions**. Each position $x$ gets a complex amplitude $\psi(x)$, and these amplitudes can interfere—just like in the double-slit experiment, where the wavefunction goes through *both* slits and produces constructive and destructive interference on the screen.

Let's give a name to this: each position is a **configuration**, and the wavefunction assigns a complex amplitude to every configuration.

### Complex Amplitudes

A complex amplitude has two parts:
- **Magnitude** (how much)
- **Phase** (what angle)

We'll represent this as a circle: the radius is the amplitude, and a line indicates the phase (which wraps around from $0$ to $2\pi$).

<img src="./01_02_lecture_files/complex_amplitude.svg" alt="Complex amplitude" width="150"/>

We'll explore complex numbers more next week. For now, just think: each configuration gets a little "clock hand" with a length and an angle.

---

### Simplifying: Two Positions

Let's make things as simple as possible. Imagine a particle that can only be in two positions: **Left** or **Right**.

**Classical picture:**
The particle is either at L or at R. Even if we're uncertain, it's still definitely one or the other—we just don't know which.

**Quantum picture:**
The particle has a complex amplitude for each position:

$$
|\psi\rangle = c_L |L\rangle + c_R |R\rangle
$$

where:
- $c_L = \psi(x_L)$ is the amplitude to be on the left
- $c_R = \psi(x_R)$ is the amplitude to be on the right

This is a **two-configuration system** (or "two-state system"). The particle isn't at L *or* R—it's in a superposition of both.

---

### A Physical Two-Configuration System: Spin

There's a real physical system that behaves exactly this way: the **spin** of an electron.

A spin-1/2 particle can only be measured as "up" or "down" along any axis. These are the two configurations:

$$
|\psi\rangle = c_\uparrow |\uparrow\rangle + c_\downarrow |\downarrow\rangle
$$

This is the same mathematics as the two-position system:
- Two configurations: $|\uparrow\rangle$ and $|\downarrow\rangle$
- Two complex amplitudes: $c_\uparrow$ and $c_\downarrow$

We can work with either picture—positions or spins. The math is identical.

---

### Time Evolution

How does a quantum state change in time?

Recall from classical mechanics, we updated position using velocity:
$$
x(t + \Delta t) = x(t) + v \cdot \Delta t
$$

In quantum mechanics, we update the **amplitudes**. For a two-state system, this is a matrix equation:

$$
\begin{pmatrix}
c_\uparrow(t+\Delta t) \\
c_\downarrow(t+\Delta t)
\end{pmatrix}
=
\begin{pmatrix}
U_{00} & U_{01} \\
U_{10} & U_{11}
\end{pmatrix}
\begin{pmatrix}
c_\uparrow(t) \\
c_\downarrow(t)
\end{pmatrix}
$$

The matrix $U$ is called a **unitary** matrix. What does unitary mean?

> **Unitary:** A matrix that conserves total probability (or equivalently, conserves energy in a closed system).

Mathematically: $U^\dagger U = I$, which guarantees that if $|c_\uparrow|^2 + |c_\downarrow|^2 = 1$ before the evolution, it's still $1$ afterward.

The cost of this update? For a $2 \times 2$ matrix: about $2^2 = 4$ operations per time step.

---

### Measurement: The Collapse

What happens if we actually measure the spin—say, by sending it through a Stern-Gerlach apparatus?

**During measurement, you get one of the outcomes.**

- If you measure "up," the state collapses to $|\uparrow\rangle$
- If you measure "down," the state collapses to $|\downarrow\rangle$

The probabilities are:
$$
P(\uparrow) = |c_\uparrow|^2, \qquad P(\downarrow) = |c_\downarrow|^2
$$

Before measurement: a superposition of both.
After measurement: definitely one or the other.

This is one of the deep mysteries of quantum mechanics—but for now, it's a rule we'll use.

---

## Two Spins: The Explosion Begins

Now let's add a second spin.





Here's the continuation:

---

## Two Spins: Where It Gets Interesting

### Classical: Four Possibilities

With two spins, classically we have four possible configurations:

$$
\uparrow\uparrow, \quad \uparrow\downarrow, \quad \downarrow\uparrow, \quad \downarrow\downarrow
$$

The system is in exactly one of these at any moment.

### Quantum: A Superposition of All Four

In quantum mechanics, the system can be in a superposition of *all* configurations simultaneously:

$$
|\psi\rangle = c_{11}|\uparrow\uparrow\rangle + c_{10}|\uparrow\downarrow\rangle + c_{01}|\downarrow\uparrow\rangle + c_{00}|\downarrow\downarrow\rangle
$$

Each configuration gets its own complex amplitude—its own magnitude and phase:

<img src="./01_02_lecture_files/two_spin_configs.svg" alt="Two spin configurations" width="300"/>

The state vector now has **4 components**:

$$
|\psi\rangle = \begin{pmatrix} c_{11} \\ c_{10} \\ c_{01} \\ c_{00} \end{pmatrix}
$$

And time evolution requires a **4 × 4 matrix**:

$$
\begin{pmatrix}
c_{11}(t+\Delta t) \\
c_{10}(t+\Delta t) \\
c_{01}(t+\Delta t) \\
c_{00}(t+\Delta t)
\end{pmatrix}
=
\begin{pmatrix}
U_{00} & U_{01} & U_{02} & U_{03} \\
U_{10} & U_{11} & U_{12} & U_{13} \\
U_{20} & U_{21} & U_{22} & U_{23} \\
U_{30} & U_{31} & U_{32} & U_{33}
\end{pmatrix}
\begin{pmatrix}
c_{11}(t) \\
c_{10}(t) \\
c_{01}(t) \\
c_{00}(t)
\end{pmatrix}
$$

Cost per time step: $4 \times 4 = 16$ operations.

---

## N Spins: The Exponential Wall

Now let's generalize. For $N$ spins:

| $N$ spins | Configurations | State vector size | Matrix size |
|-----------|----------------|-------------------|-------------|
| 1 | 2 | 2 | 2 × 2 |
| 2 | 4 | 4 | 4 × 4 |
| 3 | 8 | 8 | 8 × 8 |
| 10 | 1,024 | 1,024 | 1,024 × 1,024 |
| $N$ | $2^N$ | $2^N$ | $2^N \times 2^N$ |

The general quantum state is:

$$
|\psi\rangle = \sum_{\text{all } 2^N \text{ configs}} c_{\text{config}} \,|\text{config}\rangle
$$

Or in vector form:

$$
|\psi\rangle = \begin{pmatrix} c_{00\cdots0} \\ c_{00\cdots1} \\ \vdots \\ c_{11\cdots1} \end{pmatrix} \leftarrow 2^N \text{ amplitudes}
$$

Time evolution:

$$
|\psi(t+\Delta t)\rangle = \underbrace{U}_{2^N \times 2^N} \, |\psi(t)\rangle
$$

### The Cost of Simulating Quantum Mechanics

$$
\text{Cost} \propto (2^N \times 2^N) \times T = 2^{2N} \times T
$$

where $T$ is the number of time steps.

**That's it. That's why quantum mechanics is hard to simulate.**

For 50 spins, you need a matrix with $(2^{50})^2 \approx 10^{30}$ entries. No computer on Earth can store that.

---

## The Key Insight: Waves Over Configuration Space

Let's step back and see what quantum mechanics really is.

**Quantum mechanics is a wave equation over configuration space.**

Think about the double-slit experiment: a particle goes through *both* slits simultaneously, and the two paths interfere when they meet again. Depending on the relative phase of the two paths, we get:
- **Constructive interference:** amplitudes add, probability increases
- **Destructive interference:** amplitudes cancel, probability decreases

The same thing happens with spins—but now the "configurations" aren't positions, they're spin states. The wavefunction spreads over all $2^N$ configurations, and these configurations can interfere with each other.

Imagine a grid where each dot represents one configuration:

<img src="./01_02_lecture_files/config_space.svg" alt="Configuration space" width="350"/>

Each dot has a complex amplitude (magnitude and phase). Time evolution mixes them all together—every configuration can influence every other configuration through the $2^N \times 2^N$ matrix.

---

## Measurement: The Catch

Here's the frustrating part.

After all that exponential complexity—$2^N$ amplitudes evolving and interfering—when you actually **measure** the system, you get just **one** classical outcome:

$$
\text{Measurement} \rightarrow |001011010100\rangle
$$

Just a string of 0s and 1s. One configuration out of $2^N$.

This is the fundamental tension of quantum computing:
- **The state** has $2^N$ complexity
- **Preparation and measurement** are still classical—you can only put in and read out $N$ bits

The art of quantum algorithms is engineering the interference so that the *right* answer has high probability when you measure.

---

## So What Is a Quantum Computer?

A quantum computer is a machine that:

1. **Prepares** an initial state (usually all spins in one configuration, like $|00\cdots0\rangle$)

2. **Evolves** the state through a sequence of operations (quantum gates), allowing the wavefunction to spread across configurations

3. **Exploits interference:** If amplitude flows from one configuration to another by two different paths, and those paths come back together, they can interfere:
   - Same phase → constructive interference → higher probability
   - Opposite phase → destructive interference → probability cancels

4. **Measures** at the end, collapsing to one classical outcome

The goal: design the gates so that *wrong* answers interfere destructively and *right* answers interfere constructively.

Nature does this evolution effortlessly for any physical system. The question is: can we control it precisely enough to compute something useful?







Here's the continuation:

---

## But Wait... Are All $2^N$ States Important?

You might be thinking: okay, there are $2^N$ basis states, but surely most of them don't matter?

That's actually a great insight.

For example:
- Some spin configurations might have extremely high energy—you can probably ignore them
- Two particles far apart might never interact—maybe you can assume they're never in a coherent superposition
- Maybe only a small "corner" of the $2^N$-dimensional space is physically relevant

This is the foundation of **computational quantum physics**: choosing the right basis and figuring out which states you can safely ignore.

The entire field is built on questions like:
- What basis should we use?
- Which high-energy states can we throw away?
- Can we approximate entanglement as local?
- Can we treat some degrees of freedom classically?

### The Fundamental Problem

Here's the catch: **how do you know your approximation is accurate if you can't solve the problem exactly to check?**

This is genuinely difficult. The only way to validate an approximation is to compare it to experiment—to nature itself.

In practice, simulations of quantum systems often try to make things as classical as possible. For example, you might treat each atom in a large molecule as a classical particle obeying $F = ma$. This is a terrible approximation in most cases—atoms are quantum objects!—but it tells you *something*, and sometimes that's the best we can do.

This is the state of the art right now: find the best approximations, push them as far as possible, and hope they capture the essential physics.

---

## The Insight from the 1980s

Now we arrive at the idea that launched quantum computing.

Take another look at a protein:

<img src="./01_02_lecture_files/protein.png" alt="Protein structure" width="400"/>

With your new appreciation for $2^N$ scaling, think about what nature is doing here.

This molecule has thousands of atoms, each with electrons in superposition, all entangled with each other. The number of quantum configurations is astronomical—a classical computer the size of the observable universe could not simulate this system before the heat death of the universe.

It really is absurd.

**And yet nature does it effortlessly.** Every molecule, every protein, every piece of matter around you is "computing" its ground state constantly, just by existing.

This led physicists in the 1980s—most famously Richard Feynman—to a radical idea:

> **What if we used quantum systems—nature itself—to simulate quantum physics?**

At first this sounds almost silly. What if we used a molecule to simulate a molecule? Just... make the molecule and measure its properties?

Well, yes—but here's the key insight that makes it useful:

### The Difference: Individual Control

A natural molecule is quantum, but we have no control over it. We can't:
- Prepare it in an arbitrary initial state
- Watch it evolve step by step
- Measure individual atoms
- Slow down time to see what's happening

A **quantum computer** is a quantum system where we *do* have all of this:

| Natural quantum system | Quantum computer |
|------------------------|------------------|
| Nanometer length scales | Micron-scale (visible!) |
| Femtosecond timescales | Microsecond timescales (measurable!) |
| No individual control | Control each qubit |
| Can only measure bulk properties | Measure each qubit individually |
| Fixed by nature | Programmable |

We're taking quantum mechanics and scaling it up to length scales and time scales where we have **full control and full knowledge**.

---

## That's a Quantum Computer

A quantum computer is:

1. A quantum system with $2^N$ configurations
2. That we can **prepare** in a known initial state
3. That we can **evolve** with controlled operations (gates)
4. Where configurations can **interfere** with each other
5. That we can **measure** at the end, one qubit at a time

It's nature's exponential complexity, harnessed and controlled.

The question now becomes: what can we actually *do* with it? And can we build one that works?

Here are 10 possible homework problems for Lecture 3:


# Homework

### Ising Energy by Hand
Consider 4 spins in a line with the Hamiltonian:
$$
H(s) = J \sum_{i=1}^{3} s_i s_{i+1} + B \sum_{i=1}^{4} s_i
$$
with $J = 1$ and $B = 0.5$.

- (a) Calculate the energy for $s = [+1, +1, +1, +1]$.
- (b) Calculate the energy for $s = [+1, -1, +1, -1]$.
- (c) Find the ground state configuration by checking all 16 possibilities (you may use Python or do by hand).
- (d) How does the ground state change if $B = 2$? Explain physically why.



###  Normalization
A single spin is in the state:
$$
|\psi\rangle = c_\uparrow |\uparrow\rangle + c_\downarrow |\downarrow\rangle
$$
with $c_\uparrow = \frac{1}{2}$ and $c_\downarrow = \frac{\sqrt{3}}{2}$.

- (a) Verify that this state is normalized: $|c_\uparrow|^2 + |c_\downarrow|^2 = 1$.
- (b) What is the probability of measuring spin up? Spin down?
- (c) If you measured 1000 spins all prepared in this state, approximately how many would you expect to find spin up?

---

### Two-Spin States
Write out the full quantum state for two spins in the following situations:

- (a) Both spins are definitely up.
- (b) The first spin is up, the second is in an equal superposition of up and down.
- (c) Both spins are in equal superpositions of up and down, but independently (not entangled).

For (c), expand your answer to show all four terms with their coefficients.

---

### Matrix Size Scaling
Time evolution of $N$ qubits requires multiplying a $2^N \times 2^N$ matrix by a $2^N$ vector.

- (a) How many multiplication operations does one matrix-vector multiplication require (to leading order)?
- (b) A GPU can perform $10^{13}$ operations per second. How long would one time step take for $N = 30$? For $N = 50$?
- (c) Compare your answer for $N = 50$ to the age of the universe ($\sim 4 \times 10^{17}$ seconds).

---

### Measurement Collapse (Conceptual)
A two-spin system is in the state:
$$
|\psi\rangle = \frac{1}{2}|\uparrow\uparrow\rangle + \frac{1}{2}|\uparrow\downarrow\rangle + \frac{1}{2}|\downarrow\uparrow\rangle + \frac{1}{2}|\downarrow\downarrow\rangle
$$

- (a) Verify this state is normalized.
- (b) What is the probability of measuring both spins up?
- (c) You measure the first spin and get $\uparrow$. What are the possible states of the system after this measurement? What are their probabilities?

---

###  Classical Uncertainty vs Quantum Superposition
Explain the difference between:
- (A) "The spin is either up or down, but I don't know which."
- (B) "The spin is in a superposition of up and down."

In your answer (~1 paragraph):
- Describe what experiment could distinguish these two cases.
- Use the word "interference" in your explanation.

---

###  Feynman's Insight (Short Essay)
In ~0.5 pages, answer the following:

Richard Feynman proposed using quantum systems to simulate quantum physics. 

- (a) Why can't classical computers efficiently simulate large quantum systems?
- (b) What is the key difference between a natural quantum system (like a protein) and a quantum computer?
- (c) Why is "individual control" important for a quantum computer to be useful?



I like both of these! They're hands-on and build real intuition.

Here's how I'd structure them:

---

### Problem: Probability Conservation and Unitarity

A single spin has the state:
$$
|\psi\rangle = \begin{pmatrix} c_0 \\ c_1 \end{pmatrix}
$$

The total probability must equal 1:
$$
|c_0|^2 + |c_1|^2 = 1
$$

Time evolution is described by a 2×2 matrix $U$:
$$
\begin{pmatrix} c_0' \\ c_1' \end{pmatrix} = \begin{pmatrix} U_{00} & U_{01} \\ U_{10} & U_{11} \end{pmatrix} \begin{pmatrix} c_0 \\ c_1 \end{pmatrix}
$$

**(a)** Write out $|c_0'|^2 + |c_1'|^2$ in terms of the matrix elements $U_{ij}$ and the original amplitudes $c_0, c_1$.

**(b)** For probability to be conserved for *any* initial state, what condition must the matrix $U$ satisfy? 

*Hint: Consider what happens if you start in $|c_0|^2 = 1$ vs $|c_1|^2 = 1$ vs an equal superposition.*

**(c)** Show that this condition can be written as $U^\dagger U = I$, where $U^\dagger$ is the conjugate transpose. A matrix satisfying this is called **unitary**.

**(d)** Verify that the following matrix is unitary:
$$
U = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}
$$

---

### Problem: Rabi Oscillations — Your First Quantum Simulation

A spin in a magnetic field oscillates between up and down. Let's simulate this.

The Hamiltonian is:
$$
H = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}
$$

This says: the spin can "hop" between up and down. The time evolution operator for a small time step $\Delta t$ is:
$$
U(\Delta t) = e^{-iH\Delta t}
$$

For this particular $H$, the matrix exponential works out to:
$$
U(\Delta t) = \begin{pmatrix} \cos(\Delta t) & -i\sin(\Delta t) \\ -i\sin(\Delta t) & \cos(\Delta t) \end{pmatrix}
$$

**(a) Setup:** Start with the spin in the "up" state:
$$
|\psi(0)\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}
$$

**(b) Evolve:** Write a Python program that:
1. Starts with $|\psi\rangle = [1, 0]$
2. Repeatedly applies $U(\Delta t)$ for small $\Delta t$ (try $\Delta t = 0.05$)
3. At each step, records $P_\uparrow = |c_0|^2$ and $P_\downarrow = |c_1|^2$
4. Runs for $T = 200$ time steps

**(c) Plot:** Plot $P_\uparrow(t)$ and $P_\downarrow(t)$ vs time on the same graph.

**(d) Interpret:** 
- What do you observe? 
- What is the period of oscillation?
- Verify that $P_\uparrow + P_\downarrow = 1$ at all times.

**(e) Conceptual:** This oscillation is called **Rabi oscillation**. In your own words, explain what is physically happening to the spin. Why doesn't it just stay "up"?

---

**Starter code for (b):**

```python
import numpy as np
import matplotlib.pyplot as plt

# Time step
dt = 0.05

# Time evolution matrix
U = np.array([
    [np.cos(dt), -1j*np.sin(dt)],
    [-1j*np.sin(dt), np.cos(dt)]
])

# Initial state: spin up
psi = np.array([1.0 + 0j, 0.0 + 0j])

# Storage for probabilities
P_up = []
P_down = []

# Time evolution loop
for step in range(200):
    # Record probabilities
    P_up.append(np.abs(psi[0])**2)
    P_down.append(np.abs(psi[1])**2)
    
    # Evolve state
    psi = U @ psi

# Plot results
# ... your code here ...
```
