# Lecture 1.3: What Makes Quantum Hard?

## From seating arrangements to quantum spins

Last lecture, you wrestled with the wedding seating problem. You discovered that even with just 30 guests, brute-force search was impossible—there were simply too many arrangements to check.

Today we'll see that this problem was actually a physics problem in disguise. And understanding why it was hard will lead us directly to the core idea of quantum computing. <!--  --> \### The Explosion of Configurations

Recall the wedding problem: you had $N$ guests and needed to find the seating arrangement that minimized drama. The number of possible arrangements was: $$
N! = N \times (N-1) \times (N-2) \times \cdots \times 1
$$

For 30 guests, that's about $10^{32}$ possibilities—far more than any computer could ever search.

Now let's consider a simpler counting problem. Imagine a row of spins, each pointing either up ($\uparrow$) or down ($\downarrow$):

$$
\uparrow \quad \downarrow \quad \uparrow \quad \uparrow \quad \downarrow \quad \downarrow \quad \uparrow \quad \downarrow \quad \uparrow \quad \downarrow
$$

We can encode this as a list of numbers: $s_i = +1$ for up, $s_i = -1$ for down: $$
s = [+1, -1, +1, +1, -1, -1, +1, -1, +1, -1]
$$

How many such configurations exist for $N$ spins?

Each spin has 2 choices, and the choices are independent: $$
\underbrace{2 \times 2 \times 2 \times \cdots \times 2}_{N \text{ times}} = 2^N
$$

This is exponential growth:

| $N$ | $2^N$       | Context                     |
|-----|-------------|-----------------------------|
| 10  | 1,024       | Easy                        |
| 20  | \~1 million | Still manageable            |
| 30  | \~1 billion | Getting hard                |
| 50  | \~$10^{15}$ | Exceeds all computers       |
| 300 | \~$10^{90}$ | More than atoms in universe |

The wedding problem ($N!$) is even worse than $2^N$, but both are intractable for large $N$. No classical algorithm can search through all possibilities.

This is the first key insight:

> **The number of configurations grows exponentially with the number of particles.**

In the wedding problem, you had a "drama matrix" $D_{ij}$ that told you how much two guests disliked sitting together. You minimized the total drama.

Physics has exactly the same structure—but we call the cost function energy, and we call the drama matrix the **Hamiltonian**.

#### Why Energy?

Nature has a deep tendency to minimize energy:

- Hot coffee cools down (energy flows to the environment) - Balls roll downhill (gravitational potential energy decreases)

- Crystals form from liquid (atoms find low-energy arrangements)

- Magnets align with external fields (magnetic energy decreases)

When a system reaches its lowest possible energy, we call that the **ground state**.

Finding the ground state of a physical system is exactly like finding the optimal seating arrangement: you're searching for the configuration that minimizes a cost function.


#### Spins in a Magnetic Field

Let's start with the simplest case: a single spin in an external magnetic field $B$.

A spin is like a tiny compass needle. It wants to align with the magnetic field, because that's the lowest-energy configuration.

We write the energy as: $$
H = -B \, s
$$

where $s = +1$ (up) or $s = -1$ (down).

Let's check the signs. If $B > 0$ (field pointing up): - Spin up ($s = +1$): $H = -B(+1) = -B$ (negative energy, **low**) - Spin down ($s = -1$): $H = -B(-1) = +B$ (positive energy, **high**)

The spin pointing *with* the field has lower energy.

For $N$ spins, each feeling the same field $B$: $$
H = -B \sum_{i=1}^{N} s_i
$$

The ground state is obvious: all spins point up (if $B > 0$), giving energy $H = -NB$.

This is too easy. The interesting physics comes when spins interact with *each other*.

#### Spin-Spin Interactions: The Ising Model

Real materials aren't just spins in a field—neighboring spins also interact with each other. An electron's spin on one atom can influence the spin on a neighboring atom.

We model this with the **Ising model**, one of the most important models in physics. For a 1D chain with nearest-neighbor interactions: $$
H = -J \sum_{i=1}^{N-1} s_i s_{i+1} - B \sum_{i=1}^{N} s_i
$$

Let's unpack each term.  We still have the interaction of each spin with the external B-field.  But now we also have an interaction term for interactions between spins:
$$
-J \sum_{i=1}^{N-1} s_i s_{i+1}
$$

This sums over adjacent pairs: $(s_1, s_2)$, $(s_2, s_3)$, ..., $(s_{N-1}, s_N)$.

The coupling constant $J$ describes the interaction strength. The product $s_i s_{i+1}$ is: - $+1$ if spins are aligned ($\uparrow\uparrow$ or $\downarrow\downarrow$) - $-1$ if spins are anti-aligned ($\uparrow\downarrow$ or $\downarrow\uparrow$)

What about the sign of $J$?

**If** $J > 0$ (ferromagnetic): - Aligned spins: $H = -J(+1) = -J$ (low energy ✓) - Anti-aligned spins: $H = -J(-1) = +J$ (high energy) - Spins **want to align**

**If** $J < 0$ (antiferromagnetic): - Aligned spins: $H = -J(+1) = +|J|$ (high energy) - Anti-aligned spins: $H = -J(-1) = -|J|$ (low energy ✓) - Spins **want to anti-align**

This is exactly the drama matrix from the wedding problem! Positive $J$ means the spins "get along" (want to be the same); negative $J$ means they "fight" (want to be different).

For a 1D chain, we can visualize the coupling:

$$
\uparrow \quad \downarrow \quad \uparrow \quad \downarrow \quad \uparrow   \\
\underset{J_{01}}{\smile} \,\,\,\, \underset{J_{12}}{\smile} \,\,\,\, \underset{J_{23}}{\smile} \,\, \underset{J_{34}}{\smile}
$$

If we wanted to write this as a matrix (like the drama matrix), it would have a simple structure—only the entries just off the diagonal are nonzero:

$$
J_{\text{matrix}} = \begin{pmatrix}
0 & J & 0 & 0 & 0 \\
J & 0 & J & 0 & 0 \\
0 & J & 0 & J & 0 \\
0 & 0 & J & 0 & J \\
0 & 0 & 0 & J & 0
\end{pmatrix}
$$

Compare this to the drama matrix from the wedding problem—same structure, different physical interpretation.

#### The Competition

The interesting physics happens when the two terms compete.

Consider an antiferromagnet ($J < 0$) in an external field ($B > 0$): - The interaction term wants neighboring spins to anti-align: $\uparrow\downarrow\uparrow\downarrow\uparrow\downarrow$ - The field term wants all spins to point up: $\uparrow\uparrow\uparrow\uparrow\uparrow\uparrow$

Which wins? It depends on the relative strength of $|J|$ versus $B$.

-   If $|J| \gg B$: interactions dominate → alternating pattern
-   If $B \gg |J|$: field dominates → all spins up
-   If $|J| \approx B$: **frustration**—the system can't satisfy both constraints

The crossover between these regimes is called a **phase transition**. You'll explore this in the homework.


## Nature's strategy: Simulated Annealing

You now have a well-defined optimization problem:

> Given $J$ and $B$, find the spin configuration $s = [s_1, s_2, \ldots, s_N]$ that minimizes $H(s)$.

For small $N$, you can try all $2^N$ configurations. But for $N = 50$, that's $10^{15}$ configurations—impossible.

Is there a smarter approach?

Think about how nature actually solves this problem. When you cool a material slowly, the atoms don't instantly jump to the ground state. Instead:

1.  At high temperature, atoms jiggle around randomly
2.  As temperature drops, atoms start settling into lower-energy arrangements
3.  At very low temperature, the system freezes into (hopefully) the ground state

This process is called **annealing**, and we can simulate it on a computer.

Here is the simulated annealing algorithm.

**Step 1: Start hot**

Initialize with a random spin configuration: $$
s = [+1, -1, +1, +1, -1, -1, +1, -1, +1, -1]
$$

At high temperature, any configuration is acceptable.

**Step 2: Propose a change**

Here we randomly change the system.  This could be swapping two neighboring spins.  It could be by randomly flipping any single spin.  This is supposed to model the randomness intrinsic to hot systems - they are constantly changing.  We make the change.  Then we calculate the energy change: 
$$
\Delta E = E_{\text{new}} - E_{\text{old}}
$$

**Step 3: Accept or reject**

Here’s the standard Metropolis acceptance rule:

Let \(\Delta E = E_{\text{new}} - E_{\text{old}}\).

- If \(\Delta E \le 0\) (energy decreases or stays the same): **always accept** the move  
- If \(\Delta E > 0\) (energy increases): **accept with probability**  
  \[
  p = e^{-\Delta E / T}
  \]

In practice: draw \(u \sim \text{Uniform}(0,1)\). Accept the move if \(u < e^{-\Delta E/T}\).

At high temperature \(T\), \(e^{-\Delta E/T}\) is closer to 1, so the algorithm often accepts uphill moves and explores widely.

At low temperature \(T\), \(e^{-\Delta E/T}\) becomes very small for \(\Delta E>0\), so the algorithm rarely accepts uphill moves and tends to get stuck in low-energy states.

**Step 4: Cool down and repeat**

Gradually lower the temperature while repeating steps 2–3. The system will (hopefully) settle into a low-energy configuration.



```         
Algorithm: Simulated Annealing (Metropolis)
───────────────────────────────────────────

Initialize random configuration s

Set initial temperature T = T_high

While T > T_low:
a. Pick random spin i
b. Compute ΔE if we flip spin i
c. If ΔE ≤ 0: flip the spin
Else:
Draw u ~ Uniform(0,1)
If u < exp(-ΔE/T): flip the spin
d. Lower T slightly (e.g., T → 0.99 × T)

Return final configuration
```

This is a heuristic—it doesn't guarantee the global minimum, but it often finds good solutions. It's inspired directly by how nature finds ground states.  Simulated annealing is clever, but it's still classical. It explores configurations one at a time, making local changes and hoping to find the global minimum. For some problems, this works well. For others, the energy landscape has many local minima, and the algorithm gets stuck.

Is there a fundamentally different approach? What if, instead of exploring configurations one at a time, we could explore all configurations simultaneously?  This is exactly what quantum mechanics allows.

------------------------------------------------------------------------

## Nature Can Explore All Configurations at the Same Time

Here is the key idea that separates quantum from classical:

> **In quantum mechanics, a system doesn't have to be in one configuration—it can be in a superposition of all configurations simultaneously.**

This isn't just uncertainty about which configuration the system is in. The system genuinely *is* in all configurations at once, with each configuration carrying a complex amplitude that determines how it contributes to the whole.

Let's build up to this idea step by step.

### From Classical Position to Quantum Wavefunction

Think about a single particle moving in space.

**Classical picture:** At any moment, the particle has a definite position $x(t)$. We might not know exactly where it is, but it's definitely *somewhere*.

**Quantum picture:** The particle is described by a **wavefunction** $\psi(x)$—a complex number assigned to every possible position $x$.

```         
Classical:     •  ← particle is HERE
               x

Quantum:      ~~~•~~~  ← amplitude spread over positions
              ψ(x)
```

The wavefunction tells us: if we were to measure the particle's position, how likely are we to find it at each location? But before measurement, the particle doesn't have a definite position—it exists as a wave spread across all possibilities.

Let's give this a name: each possible position $x$ is a **configuration**. The wavefunction assigns a complex amplitude to every configuration.

### Complex Amplitudes and Interference

What is a "complex amplitude"? It's a complex number $c =a + i b =  |c| e^{i \phi}$, which has two parts: 
- **Magnitude** $|c| = \sqrt{a^2 + b^2}$: how "much" of that configuration 
- **Phase** $\phi$: the "angle" of the complex number (from $0$ to $2\pi$)

You can visualize a complex amplitude as a little arrow (or clock hand): the length is the magnitude, and the direction is the phase.

```         
Complex plane:

        Im
        ↑
        |   ↗ c = a + bi
        |  /
        | / |c| = magnitude
        |/θ        
    ----+------ Re
        |    θ = phase
```

**Why does phase matter?** Because of interference. When two amplitudes combine:

```         
CONSTRUCTIVE (same phase):       DESTRUCTIVE (opposite phase):

    ↗           ↗                    ↗           ↙
     \         /                      \         /
      \       /                        \       /
       ↘     ↙                          ↘     ↙
         ↓                                •
    BIG amplitude                    ZERO amplitude
    (high probability)               (low probability)
```
(Sorry these figures are awful. These are all new lectures. If you'd like to help with svg figures please see these [instructions](../resources/figure_instruction.md). )



This is the heart of quantum mechanics: amplitudes with the same phase add up (constructive interference), while amplitudes with opposite phases cancel (destructive interference).

We'll explore complex numbers more next week. For now, just remember: each configuration gets a magnitude *and* a phase, and the phase determines how configurations interfere.

### Simplifying: Two Positions

Continuous position space is complicated. Let's simplify to the extreme: imagine a particle that can only be in two positions, Left or Right.

**Classical:** The particle is either at L or at R. Even if we're uncertain, it's definitely one or the other.

**Quantum:** The particle has a complex amplitude for each position: $$
|\psi\rangle = c_L |L\rangle + c_R |R\rangle
$$

where: 
- $c_L$ is the amplitude to be on the left 
- $c_R$ is the amplitude to be on the right 
- $|L\rangle$ and $|R\rangle$ are the two configurations

This is called a **superposition**. The particle isn't at L *or* R—it's in both configurations simultaneously, with amplitudes $c_L$ and $c_R$.

There's one constraint: the total probability must equal 1. Since probability is the magnitude squared: $$
|c_L|^2 + |c_R|^2 = 1
$$

This is called **normalization**. It guarantees that if we measure the position, we'll definitely find the particle somewhere.

### A Physical Two-Configuration System: Spin

There's a real physical system that behaves exactly this way: the **spin** of an electron.

When you measure an electron's spin along any axis (say, the $z$-axis), you only ever get one of two results: "up" ($\uparrow$) or "down" ($\downarrow$). There's no in-between.

But before measurement, the spin can be in a superposition: $$
|\psi\rangle = c_\uparrow |\uparrow\rangle + c_\downarrow |\downarrow\rangle
$$

This is mathematically identical to the two-position system: - Two configurations: $|\uparrow\rangle$ and $|\downarrow\rangle$ - Two complex amplitudes: $c_\uparrow$ and $c_\downarrow$ - Normalization: $|c_\uparrow|^2 + |c_\downarrow|^2 = 1$

We can represent the quantum state as a vector: $$
|\psi\rangle = \begin{pmatrix} c_\uparrow \\ c_\downarrow \end{pmatrix}
$$

This is called the **state vector**. For a two-configuration system, it's a vector in $\mathbb{C}^2$ (two-dimensional complex space).

### Time Evolution: Matrices

How does a quantum state change in time?

Recall classical mechanics: position updates via velocity. 
$$
x(t + \Delta t) = x(t) + v \cdot \Delta t
$$

In quantum mechanics, the state vector updates via matrix multiplication:
 
$$
\begin{pmatrix} c_\uparrow(t+\Delta t) \\ c_\downarrow(t+\Delta t) \end{pmatrix}
=\begin{pmatrix} U_{00} & U_{01} \\ U_{10} & U_{11} \end{pmatrix}
\begin{pmatrix} c_\uparrow(t) \\ c_\downarrow(t) \end{pmatrix}
$$

The matrix $U$ is called a **unitary matrix**. "Unitary" means it preserves normalization—if $|c_\uparrow|^2 + |c_\downarrow|^2 = 1$ before evolution, it's still 1 afterward. (You'll prove this in the homework.)

**Computational cost:** For a 2×2 matrix times a 2-vector, we need about $2^2 = 4$ multiplications per time step. Easy!

### Two Spins: Where It Gets Interesting

Now let's add a second spin.

**Classical:** With two spins, there are four possible configurations: $$
\uparrow\uparrow, \quad \uparrow\downarrow, \quad \downarrow\uparrow, \quad \downarrow\downarrow
$$

The system is in exactly one of these at any moment.

**Quantum:** The system can be in a superposition of *all four* configurations: $$
|\psi\rangle = c_{\uparrow\uparrow}|\uparrow\uparrow\rangle + c_{\uparrow\downarrow}|\uparrow\downarrow\rangle + c_{\downarrow\uparrow}|\downarrow\uparrow\rangle + c_{\downarrow\downarrow}|\downarrow\downarrow\rangle
$$

Each configuration gets its own complex amplitude—its own magnitude and phase.

**A note on notation:** Here the subscripts like $\uparrow\uparrow$ are labels for configurations. This is different from the classical Ising model where we used $s_i = \pm 1$ as numerical values. In the quantum case, $c_{\uparrow\uparrow}$ is a complex number (the amplitude), while $|\uparrow\uparrow \rangle$ is a basis state (the configuration).

The state vector now has 4 components: $$
|\psi\rangle = \begin{pmatrix} c_{\uparrow\uparrow} \\ c_{\uparrow\downarrow} \\ c_{\downarrow\uparrow} \\ c_{\downarrow\downarrow} \end{pmatrix}
$$

And time evolution requires a **4×4 matrix**: $$
|\psi(t+\Delta t)\rangle = U \, |\psi(t)\rangle
$$

where $U$ is now a $4 \times 4$ unitary matrix.

**Computational cost:** $4 \times 4 = 16$ operations per time step.

### A Glimpse of Entanglement

Here's something remarkable. Consider the two-spin state: $$
|\psi\rangle = \frac{1}{\sqrt{2}}|\uparrow\uparrow\rangle + \frac{1}{\sqrt{2}}|\downarrow\downarrow\rangle
$$

Can we write this as "spin 1 in some state" times "spin 2 in some state"?

Try it: if spin 1 is $(a|\uparrow\rangle + b|\downarrow\rangle)$ and spin 2 is $(c|\uparrow\rangle + d|\downarrow\rangle)$, the combined state would be: $$
ac|\uparrow\uparrow\rangle + ad|\uparrow\downarrow\rangle + bc|\downarrow\uparrow\rangle + bd|\downarrow\downarrow\rangle
$$

For this to equal $\frac{1}{\sqrt{2}}|\uparrow\uparrow\rangle + \frac{1}{\sqrt{2}}|\downarrow\downarrow\rangle$, we'd need $ac = bd = \frac{1}{\sqrt{2}}$ and $ad = bc = 0$. But if $ad = 0$, then either $a=0$ or $d=0$, which would make $ac = 0$ or $bd = 0$. Contradiction!

This state **cannot** be written as a product. The two spins are **entangled**—their fates are correlated in a way that has no classical analog. We'll explore entanglement much more in future lectures.

### N Spins: The Exponential Wall

Now let's generalize. For $N$ spins:

| $N$ spins | Configurations | State vector size | Matrix size         |
|-----------|----------------|-------------------|---------------------|
| 1         | 2              | 2                 | 2 × 2               |
| 2         | 4              | 4                 | 4 × 4               |
| 3         | 8              | 8                 | 8 × 8               |
| 10        | 1,024          | 1,024             | 1,024 × 1,024       |
| 20        | \~1 million    | \~1 million       | \~$10^{12}$ entries |
| $N$       | $2^N$          | $2^N$             | $2^N \times 2^N$    |

The general quantum state is a superposition over all $2^N$ configurations: $$
|\psi\rangle = \sum_{\text{all } 2^N \text{ configs}} c_{\text{config}} \,|\text{config}\rangle
$$

Or in vector form: $$
|\psi\rangle = \begin{pmatrix} c_{00\cdots0} \\ c_{00\cdots1} \\ \vdots \\ c_{11\cdots1} \end{pmatrix} \leftarrow 2^N \text{ amplitudes}
$$

Time evolution: $$
|\psi(t+\Delta t)\rangle = \underbrace{U}_{2^N \times 2^N} \, |\psi(t)\rangle
$$

### The Cost of Simulating Quantum Mechanics

The computational cost to simulate one time step: $$
\text{Cost} \sim (2^N)^2 = 2^{2N}
$$

For $T$ time steps: $$
\text{Total cost} \sim 2^{2N} \times T
$$

**This is why quantum mechanics is hard to simulate.**

For 50 spins, you need a matrix with $(2^{50})^2 \approx 10^{30}$ entries. No computer on Earth—no computer that could ever exist—can store that.

And yet nature does this effortlessly. Every atom, every molecule, every piece of matter is constantly "computing" quantum evolution with exponentially many configurations.

------------------------------------------------------------------------

## Measurement: The Collapse

We've seen that quantum states can be superpositions of exponentially many configurations. But here's the catch:

> **When you measure a quantum system, you get one classical outcome.**

Consider a single spin in superposition: $$
|\psi\rangle = \frac{1}{\sqrt{2}}|\uparrow\rangle + \frac{1}{\sqrt{2}}|\downarrow\rangle
$$

If we measure the spin—say, by sending it through a **Stern-Gerlach apparatus** (a device that uses a magnetic field gradient to spatially separate spin-up from spin-down)—we don't get "both." We get either $\uparrow$ or $\downarrow$.

The probabilities are given by the **Born rule**: $$
P(\uparrow) = |c_\uparrow|^2, \qquad P(\downarrow) = |c_\downarrow|^2
$$

In this case, $P(\uparrow) = P(\downarrow) = 1/2$. It's a coin flip.

After measurement, the state collapses to the measured outcome: - If we measure $\uparrow$, the state becomes $|\uparrow\rangle$ - If we measure $\downarrow$, the state becomes $|\downarrow\rangle$

The superposition is destroyed. This is one of the deep mysteries of quantum mechanics—but for now, we take it as a rule.

### The Fundamental Tension

This creates a fundamental tension for quantum computing:

-   **Before measurement:** The state has $2^N$ amplitudes, all evolving and interfering
-   **After measurement:** We get just $N$ classical bits (one per spin)

We put in $N$ bits, we get out $N$ bits. Where did the exponential complexity go?

The answer: the $2^N$ amplitudes don't disappear—they **interfere**. During the computation, amplitudes flow between configurations. When they meet, they can add (constructive interference) or cancel (destructive interference). By the time we measure, the interference has concentrated probability onto a small number of outcomes.

The exponential complexity was used to orchestrate interference—it shaped *which* outcomes are likely.

### A Common Misconception

It's tempting to think: "A quantum computer tries all $2^N$ answers simultaneously and just picks the best one."

**This is wrong.**

Measurement doesn't return the best answer—it returns a *random* answer, weighted by the squared amplitudes. If you just put a quantum computer in superposition and measured, you'd get a random configuration. That's no better than guessing!

The hard part is designing the quantum operations so that the *right* answer has high probability. This requires carefully engineering interference—making wrong answers cancel and right answers reinforce.

------------------------------------------------------------------------

## What Is a Quantum Computer?

We now have all the pieces. A quantum computer is:

### 1. Prepare an Initial State

Start with all qubits in a known configuration, typically all zeros: $$
|00\cdots0\rangle
$$

This is easy—it's just a classical state.

### 2. Evolve Through Quantum Gates

Apply a sequence of operations (called **quantum gates**) that cause the wavefunction to spread across configurations.

Each gate is a unitary matrix. Simple gates act on one or two qubits at a time, but their combined effect creates superpositions over all $2^N$ configurations.

### 3. Exploit Interference

Here's the key insight. If amplitude flows from configuration A to configuration B by two different paths, and those paths come back together, the amplitudes **interfere**:

-   **Same phase → constructive interference:** amplitudes add, probability increases
-   **Opposite phase → destructive interference:** amplitudes cancel, probability decreases

```         
Path 1:  A ──────→ B   (phase φ₁)
                   ↘
                    ⊕ → Interference!
                   ↗
Path 2:  A ──────→ B   (phase φ₂)

If φ₁ = φ₂:  amplitudes ADD      → high probability
If φ₁ = φ₂ + π: amplitudes CANCEL → low probability
```
(Sorry these figures are awful. These are all new lectures. If you'd like to help with svg figures please see these [instructions](../resources/figure_instruction.md). )


This is exactly like the double-slit experiment, but now it's happening in the abstract space of all $2^N$ configurations.

### 4. Measure at the End

Finally, measure all the qubits. The superposition collapses to a single classical outcome: $$
|001011010\cdots\rangle
$$

Just a string of 0s and 1s.

### The Goal

The art of quantum algorithms is designing the gates so that: 
- **Wrong answers** interfere **destructively** → low probability 
-  **Right answers** interfere **constructively** → high probability

When you measure, you're likely to get the right answer.

This is not magic. It's engineering interference patterns in a $2^N$-dimensional space.

------------------------------------------------------------------------

## Summary: Classical vs. Quantum

|   | Classical | Quantum |
|------------------|------------------------------|------------------------|
| **State** | One configuration | Superposition of all $2^N$ configurations |
| **Stored information** | $N$ bits | $2^N$ complex amplitudes |
| **Evolution** | Update one configuration | Matrix multiply all amplitudes |
| **Exploration** | One path at a time | All paths simultaneously |
| **Output** | Deterministic | Probabilistic (via interference) |

The exponential state space is both the source of quantum power and the reason quantum systems are hard to simulate classically.





------------------------------------------------------------------------

# Homework

## Problem 1: Ising Energy by Hand

Consider 4 spins in a line with the Hamiltonian: $$
H = -J \sum_{i=1}^{3} s_i s_{i+1} - B \sum_{i=1}^{4} s_i
$$

with $J = 1$ (ferromagnetic) and $B = 0.5$.

**(a)** Calculate the energy for $s = [+1, +1, +1, +1]$.

**(b)** Calculate the energy for $s = [+1, -1, +1, -1]$.

**(c)** Find the ground state(s) by checking all 16 configurations. You may use Python or do by hand. If multiple configurations have the same lowest energy, list all of them.

**(d)** What is the ground state if $B = 0$? (Hint: there may be more than one!) Explain physically why there is degeneracy.

**(e)** What is the ground state if $B = 10$? Why is it unique now?

## Problem 2: Phase Transition in the Antiferromagnet

Now consider an **antiferromagnetic** chain with $N = 10$ spins: $$
H = -J \sum_{i=1}^{N-1} s_i s_{i+1} - B \sum_{i=1}^{N} s_i
$$

with $J = -1$ (antiferromagnetic, so neighboring spins want to anti-align).

**(a)** What is the ground state when $B = 0$? What is its energy? (Note: there may be two degenerate ground states—list both if so.)

**(b)** What is the ground state when $B = 100$? What is its energy?

**(c)** Write a Python program to find the ground state (by brute force) for values of $B$ from 0 to 5 in steps of 0.25. For each $B$, record: - The ground state configuration(s) (if there are ties, pick one) - The ground state energy - The "magnetization" $m = \frac{1}{N}\sum_i s_i$ (average spin)

**(d)** Plot the magnetization $m$ versus $B$. At what value of $B$ does the ground state change from the alternating pattern to the fully aligned state? This is the **critical field** $B_c$ of the phase transition.

**(e)** Explain in 2-3 sentences why the transition happens at this particular value of $B$.

**(f)** **(Optional)** Derive the critical field analytically. Compare the energy of the alternating state $E_{\text{alt}}$ to the energy of the all-up state $E_{\text{up}}$ and find the value of $B$ where they cross.

## Problem 3: Simulated Annealing

Implement simulated annealing to find the ground state of the antiferromagnetic chain from Problem 2.

**(a)** Write a Python function `simulated_annealing(J, B, N, T_init, T_final, steps)` that: - Starts with a random spin configuration - Proposes single spin flips - Accepts/rejects according to the Metropolis criterion: accept if $\Delta E < 0$, otherwise accept with probability $e^{-\Delta E/T}$ - Gradually cools from `T_init` to `T_final` over `steps` iterations

You can use either linear cooling (`T = T_init - (T_init - T_final) * step / steps`) or exponential cooling (`T = T_init * (T_final / T_init)^(step / steps)`). Exponential cooling is often more effective.

**(b)** Run your algorithm for $N = 10$, $J = -1$, $B = 1$, with $T_{\text{init}} = 10$, $T_{\text{final}} = 0.01$, and 10,000 steps. Plot the energy versus iteration number.

**(c)** Does your algorithm find the true ground state (which you know from Problem 2c)? Run it 10 times and report how often it succeeds. Consider it a "success" if the final energy matches the brute-force ground state energy to within 0.01.

**(d)** Now try $N = 30$, $J = -1$, $B = 1$. You can no longer verify by brute force. Run simulated annealing 10 times and report the lowest energy found. How consistent are the results?

*Hint: For* $N = 30$ antiferromagnetic with $B = 1$, the ground state energy should be around $E \approx -39$. If you're getting energies much higher than this, try increasing the number of steps or adjusting your cooling schedule.

**Starter code:**

``` python
import numpy as np
import matplotlib.pyplot as plt

def compute_energy(s, J, B):
    """
    Compute Ising energy for configuration s.
    s: array of +1/-1 values
    J: nearest-neighbor coupling
    B: external field
    """
    interaction = -J * np.sum(s[:-1] * s[1:])
    field = -B * np.sum(s)
    return interaction + field

def simulated_annealing(J, B, N, T_init, T_final, steps):
    """
    Find low-energy spin configuration using simulated annealing.
    Returns: final configuration, final energy, energy history
    """
    # Initialize random configuration
    s = np.random.choice([-1, 1], size=N)
    E = compute_energy(s, J, B)
    
    energy_history = [E]
    
    for step in range(steps):
        # Exponential cooling schedule
        T = T_init * (T_final / T_init) ** (step / steps)
        
        # Your code here:
        # 1. Pick a random spin index i
        # 2. Compute energy change ΔE if we flip spin i
        #    (Hint: you don't need to recompute the full energy—
        #     only the terms involving spin i change)
        # 3. Accept or reject according to Metropolis criterion
        # 4. If accepted, flip the spin and update E
        
        energy_history.append(E)
    
    return s, E, energy_history

# Test your implementation
s_final, E_final, history = simulated_annealing(J=-1, B=1, N=10, 
                                                  T_init=10, T_final=0.01, 
                                                  steps=10000)
print(f"Final energy: {E_final}")
print(f"Final configuration: {s_final}")

plt.plot(history)
plt.xlabel('Step')
plt.ylabel('Energy')
plt.title('Simulated Annealing')
plt.show()
```

% ## Problem 4: Quantum State Vectors

% **(a)** A single spin is in the state: $$
% |\psi\rangle = \frac{1}{2}|\uparrow\rangle + \frac{\sqrt{3}}{2}|\downarrow\rangle
% $$ - Verify this state is normalized. - What is the probability of measuring spin up? Spin down? - If you measured 1000 spins prepared in this state, how many would you expect to find spin up?

% **(b)** Write out the state vector for two spins where: - The first spin is $|\uparrow\rangle$ - The second spin is in equal superposition: $\frac{1}{\sqrt{2}}(|\uparrow\rangle + |\downarrow\rangle)$

% The combined state of two independent systems is their **tensor product**: $|\psi_1\rangle \otimes |\psi_2\rangle$. This means you multiply every amplitude of the first system by every amplitude of the second.

% Express your answer as a 4-component vector in the basis $\{|\uparrow\uparrow\rangle, |\uparrow\downarrow\rangle, |\downarrow\uparrow\rangle, |\downarrow\downarrow\rangle\}$.

% **(c)** How many complex amplitudes are needed to describe: - 5 spins? - 10 spins? - 50 spins?

% **(d)** If each complex amplitude requires 16 bytes to store (8 bytes for the real part, 8 for imaginary), how much memory is needed to store the state vector for 30 spins? Compare to your computer's RAM.

% ## Problem 5: Why Unitary?

% Time evolution in quantum mechanics must preserve probability. Let's prove that this requires the evolution matrix to be unitary.

% A single spin has state: $$
% |\psi\rangle = \begin{pmatrix} c_0 \\ c_1 \end{pmatrix}
% $$

% The normalization condition is $|c_0|^2 + |c_1|^2 = 1$.

% After time evolution by matrix $U$: $$
% \begin{pmatrix} c_0' \\ c_1' \end{pmatrix} = \begin{pmatrix} U_{00} & U_{01} \\ U_{10} & U_{11} \end{pmatrix} \begin{pmatrix} c_0 \\ c_1 \end{pmatrix}
% $$

% **(a)** Write out $c_0'$ and $c_1'$ in terms of the matrix elements $U_{ij}$ and the original amplitudes.

% **(b)** For probability to be conserved (i.e., $|c_0'|^2 + |c_1'|^2 = 1$) for *any* normalized initial state, what conditions must the columns of $U$ satisfy?

% *Hint: Try specific initial states like* $|\psi\rangle = (1, 0)^T$ and $|\psi\rangle = (0, 1)^T$ first.

% **(c)** Show that the condition from (b) can be written as $U^\dagger U = I$, where $U^\dagger$ is the conjugate transpose (transpose, then complex conjugate each entry). A matrix satisfying this is called **unitary**.

% **(d)** Verify that the following matrix is unitary: $$
% H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}
% $$

% This is called the **Hadamard gate**—one of the most important gates in quantum computing.

% ## Problem 6: The Exponential Wall (Short Essay)

% In approximately half a page, answer the following:

% **(a)** Why can't classical computers efficiently simulate large quantum systems? Be specific about what scales exponentially.

% **(b)** Nature simulates quantum systems effortlessly—every molecule is constantly evolving quantum mechanically. Why can't we just "use nature" to solve computational problems? What's the key property that a quantum computer has that a random molecule doesn't?

% **(c)** A quantum computer with 50 qubits has a state vector with $2^{50} \approx 10^{15}$ amplitudes. But when we measure, we only get 50 bits of output. Explain how this is useful for computation. Your answer should include the word "interference."


% \## Problem 7: The Wedding Problem Revisited (Optional Challenge)

% Apply simulated annealing to the 30-guest wedding problem from last week.

% **(a)** Adapt your code to work with permutations instead of spin flips. A natural "move" is to swap two randomly chosen guests.

% **(b)** Compare your best result from simulated annealing to your best result from last week. Did you do better?

% **(c)** In 2-3 sentences: why might the wedding problem (\$N!\$ configurations) be harder for simulated annealing than the spin problem (\$2\^N\$ configurations)?

% *Hint: Think about what happens when you swap two guests vs. flip one spin. How much does the energy typically change? How "rough" is the energy landscape?*






**(e)** What is the ground state for $B=−0.5$? Compare to $B=+0.5$. Explain why only the sign matters here.
**(e)** What is the ground state if $B = 10$? Why is it unique now?

## Problem 2: Phase Transition in the Antiferromagnet

Now consider an **antiferromagnetic** chain with $N = 10$ spins: $$
H = -J \sum_{i=1}^{N} s_i s_{i+1} - B \sum_{i=1}^{N} s_i
H = -J \sum_{i=1}^{N-1} s_i s_{i+1} - B \sum_{i=1}^{N} s_i
$$

with $J = -1$ (antiferromagnetic, so neighboring spins want to anti-align) and $s_{N+1}=s_{1}$.
with $J = -1$ (antiferromagnetic, so neighboring spins want to anti-align).