# Lecture 3.7: Grover's Search Algorithm

## The Power of Quantum Search

Deutsch-Jozsa showed that quantum computers can solve *some* problems exponentially faster than classical computers — but those problems were artificial.

**Grover's algorithm** (1996) changed everything. It provides a quantum speedup for the most general problem imaginable: **searching an unstructured database**.

The speedup is "only" quadratic ($O(\sqrt{N})$ instead of $O(N)$), not exponential. But it applies to an enormous class of problems:
- Database search
- Satisfiability (SAT)
- Optimization
- Cryptographic attacks
- Any problem that can be phrased as "find an $x$ such that $f(x) = 1$"

This lecture develops Grover's algorithm from scratch, with full mathematical details and geometric intuition.

---

## The Unstructured Search Problem

### Setup

You have a search space of $N = 2^n$ items, labeled $0, 1, 2, \ldots, N-1$.

There's a function $f: \{0, 1, \ldots, N-1\} \to \{0, 1\}$ that marks "solutions":
$$
f(x) = \begin{cases} 1 & \text{if } x \text{ is a solution} \\ 0 & \text{otherwise} \end{cases}
$$

**Goal:** Find an $x$ such that $f(x) = 1$.

### The "Unstructured" Part

**Unstructured** means the function $f$ has no exploitable pattern. The items are not sorted, there's no index, no hints about where solutions might be.

Think of it as searching for a needle in a haystack, where each piece of hay looks exactly like every other piece until you check it.

### Classical Complexity

Classically, the only option is to check items one by one:
1. Pick an item $x$
2. Evaluate $f(x)$
3. If $f(x) = 1$, done!
4. Otherwise, repeat with a different $x$

**Expected queries:** If there's 1 solution among $N$ items, you need $O(N)$ queries on average (about $N/2$).

**Worst case:** You might need to check all $N$ items.

### Quantum Complexity: Grover's Result

Grover's algorithm finds a solution with high probability using only:
$$
O(\sqrt{N}) \text{ queries}
$$

For $N = 1,000,000$ items:
- Classical: ~500,000 queries on average
- Quantum: ~1,000 queries

A **quadratic speedup**!

```{admonition} Grover's Algorithm: The Headline
:class: important

**Problem:** Find $x$ such that $f(x) = 1$ (unstructured search)

**Classical:** $O(N)$ queries

**Quantum:** $O(\sqrt{N})$ queries

This is **provably optimal** — no quantum algorithm can do better for unstructured search.
```

---

## The Algorithm Overview

Grover's algorithm uses the now-familiar recipe:
1. **Superposition:** Start with uniform superposition over all items
2. **Oracle:** Mark solutions with a phase flip
3. **Amplification:** Increase the amplitude of marked items
4. **Repeat:** Iterate steps 2-3 about $\sqrt{N}$ times
5. **Measure:** Get a solution with high probability

The key innovation is step 3: **amplitude amplification**. Instead of a single interference step (like Deutsch-Jozsa), Grover iterates to gradually boost the solution amplitude.

### The Circuit

```{figure} ./03_07_lecture_files/grover_circuit.svg
:width: 550px
:name: grover-circuit

Grover's algorithm: Initialize, then repeat (Oracle + Diffusion) about $\sqrt{N}$ times.
```

---

## The Grover Oracle

### What It Does

The Grover oracle marks solutions by flipping their phase:
$$
O_f|x\rangle = \begin{cases} -|x\rangle & \text{if } f(x) = 1 \\ |x\rangle & \text{if } f(x) = 0 \end{cases}
$$

Compactly:
$$
O_f|x\rangle = (-1)^{f(x)}|x\rangle
$$

This is exactly the phase kickback trick from Deutsch-Jozsa!

### Implementation

Use the standard oracle $U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$ with ancilla in $|-\rangle$:

$$
U_f|x\rangle|-\rangle = (-1)^{f(x)}|x\rangle|-\rangle
$$

The ancilla stays in $|-\rangle$, so we can ignore it and write:
$$
O_f|x\rangle = (-1)^{f(x)}|x\rangle
$$

### Effect on Superposition

Starting from uniform superposition:
$$
|s\rangle = \frac{1}{\sqrt{N}}\sum_{x=0}^{N-1}|x\rangle
$$

After the oracle:
$$
O_f|s\rangle = \frac{1}{\sqrt{N}}\sum_{x=0}^{N-1}(-1)^{f(x)}|x\rangle
$$

Solutions get a minus sign; non-solutions are unchanged.

**The problem:** This minus sign is a phase — it's not directly observable! Measuring now would give a random $x$, just like before.

We need a way to convert this phase difference into an amplitude difference.

---

## The Diffusion Operator

### The Key Idea: Inversion About the Mean

The diffusion operator "inverts amplitudes about their mean."

If the amplitudes are $a_0, a_1, \ldots, a_{N-1}$ with mean $\bar{a} = \frac{1}{N}\sum_x a_x$, then diffusion transforms:
$$
a_x \to 2\bar{a} - a_x
$$

### Why This Helps

After the oracle, solutions have amplitude $-\frac{1}{\sqrt{N}}$ (negative) and non-solutions have amplitude $+\frac{1}{\sqrt{N}}$ (positive).

The mean is slightly positive (most items are non-solutions).

Inverting about this positive mean:
- Non-solutions (positive, close to mean) stay positive but decrease slightly
- Solutions (negative, far below mean) become positive and **increase significantly**

Each iteration boosts the solution amplitude!

### Mathematical Definition

The diffusion operator is:
$$
D = 2|s\rangle\langle s| - I
$$

where $|s\rangle = \frac{1}{\sqrt{N}}\sum_x |x\rangle$ is the uniform superposition.

**Action on a state $|\psi\rangle = \sum_x a_x |x\rangle$:**

$$
D|\psi\rangle = 2|s\rangle\langle s|\psi\rangle - |\psi\rangle = 2|s\rangle \cdot \bar{a}\sqrt{N} - \sum_x a_x|x\rangle
$$

where $\bar{a} = \frac{1}{\sqrt{N}}\langle s|\psi\rangle = \frac{1}{N}\sum_x a_x$.

So:
$$
D|\psi\rangle = \sum_x (2\bar{a} - a_x)|x\rangle
$$

This is exactly "inversion about the mean"!

### Worked Example: N = 4, One Solution

Let's trace through Grover's algorithm completely for $N = 4$ items with one solution at $x = 3$.

**Initial state (after Hadamard):**
$$
|s\rangle = \frac{1}{2}(|00\rangle + |01\rangle + |10\rangle + |11\rangle)
$$

Amplitudes: $a_0 = a_1 = a_2 = a_3 = \frac{1}{2}$

**After Oracle (marks $x = 3$):**

The oracle flips the sign of $|11\rangle$:
$$
O_f|s\rangle = \frac{1}{2}(|00\rangle + |01\rangle + |10\rangle - |11\rangle)
$$

Amplitudes: $a_0 = a_1 = a_2 = \frac{1}{2}$, $a_3 = -\frac{1}{2}$

**Compute the mean:**
$$
\bar{a} = \frac{1}{4}\left(\frac{1}{2} + \frac{1}{2} + \frac{1}{2} - \frac{1}{2}\right) = \frac{1}{4}
$$

**After Diffusion (inversion about mean):**

For non-solutions ($x = 0, 1, 2$):
$$
a'_x = 2\bar{a} - a_x = 2 \cdot \frac{1}{4} - \frac{1}{2} = 0
$$

For the solution ($x = 3$):
$$
a'_3 = 2\bar{a} - a_3 = 2 \cdot \frac{1}{4} - \left(-\frac{1}{2}\right) = \frac{1}{2} + \frac{1}{2} = 1
$$

**Final state after 1 iteration:**
$$
|11\rangle
$$

**Result:** After just ONE Grover iteration, we have 100% probability of measuring the solution!

```{figure} ./03_07_lecture_files/grover_amplitudes_n4.svg
:width: 500px
:name: grover-amplitudes-n4

Amplitude evolution for $N=4$: (a) Initial uniform, (b) After oracle, (c) After diffusion.
```

```{admonition} Why N=4 is Special
:class: note

For $N = 4$ with $M = 1$, the optimal number of iterations is exactly 1, and the success probability is exactly 100%. 

This is because $\theta = \arcsin(1/2) = \pi/6$, and one iteration rotates by $2\theta = \pi/3$, bringing the state from angle $\pi/6$ to angle $\pi/6 + \pi/3 = \pi/2$ — exactly aligned with $|w\rangle$!

For larger $N$, we need more iterations and the success probability is close to (but not exactly) 100%.
```

### Circuit Implementation

The diffusion operator can be written as:
$$
D = H^{\otimes n} (2|0\rangle\langle 0| - I) H^{\otimes n}
$$

**Circuit:**
1. Apply $H^{\otimes n}$
2. Apply a "conditional phase flip" that puts $-1$ on all states except $|0\rangle^{\otimes n}$
3. Apply $H^{\otimes n}$

The middle step can be implemented as:
- Apply X to all qubits
- Apply a multi-controlled Z gate (phase flip if all qubits are 1)
- Apply X to all qubits

```{figure} ./03_07_lecture_files/diffusion_circuit.svg
:width: 450px
:name: diffusion-circuit

Diffusion operator circuit: H, conditional phase, H.
```

```{admonition} The Grover Iteration
:class: tip

One **Grover iteration** consists of:
$$
G = D \cdot O_f
$$

That's: Oracle (mark solutions) → Diffusion (amplify marked items)

Repeat $G$ about $\frac{\pi}{4}\sqrt{N}$ times to maximize success probability.
```

---

## Geometric Interpretation

The most elegant way to understand Grover's algorithm is geometrically.

### The Two-Dimensional Subspace

Define two states:
- $|w\rangle$ = uniform superposition over **solutions** (winners)
- $|w^\perp\rangle$ = uniform superposition over **non-solutions**

If there are $M$ solutions out of $N$ items:
$$
|w\rangle = \frac{1}{\sqrt{M}}\sum_{x: f(x)=1}|x\rangle, \quad |w^\perp\rangle = \frac{1}{\sqrt{N-M}}\sum_{x: f(x)=0}|x\rangle
$$

These are orthonormal: $\langle w | w^\perp \rangle = 0$.

### The Initial State

The uniform superposition can be written as:
$$
|s\rangle = \sin\theta |w\rangle + \cos\theta |w^\perp\rangle
$$

where $\sin\theta = \sqrt{M/N}$.

For small $M/N$, we have $\theta \approx \sqrt{M/N}$.

### The Oracle as Reflection

The oracle $O_f$ reflects about $|w^\perp\rangle$:
- $|w\rangle \to -|w\rangle$ (solutions get flipped)
- $|w^\perp\rangle \to |w^\perp\rangle$ (non-solutions unchanged)

Geometrically, this is a reflection across the $|w^\perp\rangle$ axis.

### The Diffusion as Reflection

The diffusion operator $D$ reflects about $|s\rangle$:
$$
D = 2|s\rangle\langle s| - I
$$

This is a reflection across the initial state $|s\rangle$.

### The Grover Iteration as Rotation

The composition of two reflections is a **rotation**!

$G = D \cdot O_f$ rotates the state by angle $2\theta$ toward $|w\rangle$.

```{figure} ./03_07_lecture_files/grover_geometry.svg
:width: 450px
:name: grover-geometry

Geometric view: Each Grover iteration rotates the state by $2\theta$ toward the solution space.
```

**Starting point:** $|s\rangle$ makes angle $\theta$ with $|w^\perp\rangle$.

**After 1 iteration:** Angle is $3\theta$.

**After 2 iterations:** Angle is $5\theta$.

**After $k$ iterations:** Angle is $(2k+1)\theta$.

### Optimal Number of Iterations

We want the state to reach $|w\rangle$, which is at angle $\pi/2$ from $|w^\perp\rangle$.

Set $(2k+1)\theta = \pi/2$:
$$
k = \frac{\pi/2 - \theta}{2\theta} \approx \frac{\pi}{4\theta} - \frac{1}{2}
$$

For $\theta \approx \sqrt{M/N}$:
$$
k \approx \frac{\pi}{4}\sqrt{\frac{N}{M}}
$$

With $M = 1$ solution:
$$
k \approx \frac{\pi}{4}\sqrt{N}
$$

```{admonition} The Magic Number
:class: important

For 1 solution among $N$ items, the optimal number of Grover iterations is:
$$
k_{\text{opt}} = \left\lfloor \frac{\pi}{4}\sqrt{N} \right\rfloor
$$

Examples:
- $N = 100$: about 8 iterations
- $N = 10,000$: about 79 iterations
- $N = 1,000,000$: about 785 iterations
```

### The Overshooting Problem

What happens if we iterate too many times?

The state keeps rotating past $|w\rangle$! After $k_{\text{opt}}$ iterations, additional iterations **decrease** the success probability.

This is unlike classical search, where more queries always help.

```{figure} ./03_07_lecture_files/grover_overshoot.svg
:width: 400px
:name: grover-overshoot

Too many iterations: the state rotates past the solution.
```

### Success Probability

After $k$ iterations, the state is:
$$
|\psi_k\rangle = \sin((2k+1)\theta)|w\rangle + \cos((2k+1)\theta)|w^\perp\rangle
$$

Success probability:
$$
P_{\text{success}} = |\langle w | \psi_k \rangle|^2 = \sin^2((2k+1)\theta)
$$

At $k = k_{\text{opt}}$, this is very close to 1 (not exactly 1 because $\pi/4\sqrt{N}$ may not be an integer).

---

## The Grover Operator as a Unitary

### The Grover Iterate

The combination of oracle and diffusion is a single unitary operator:
$$
G = D \cdot O_f
$$

Running Grover for $k$ iterations is simply applying $G^k$:
$$
|\psi_k\rangle = G^k |s\rangle
$$

This perspective is powerful because:
1. $G$ has a simple eigenstructure (we'll see this shortly)
2. We can use **quantum phase estimation** on $G$ to extract information
3. It connects Grover to other quantum algorithms

### Eigenvalues of G

In the 2D subspace spanned by $\{|w\rangle, |w^\perp\rangle\}$, the Grover operator acts as a rotation by $2\theta$:
$$
G = \begin{pmatrix} \cos 2\theta & -\sin 2\theta \\ \sin 2\theta & \cos 2\theta \end{pmatrix}
$$

The eigenvalues are $e^{\pm 2i\theta}$ with eigenvectors:
$$
|g_\pm\rangle = \frac{1}{\sqrt{2}}(|w\rangle \mp i|w^\perp\rangle)
$$

**Why this matters:** The eigenvalues encode $\theta$, which encodes the number of solutions $M = N\sin^2\theta$. This is the key to **quantum counting**.

---

## Amplitude Amplification: The General Framework

Grover's algorithm is actually a special case of a more general technique called **amplitude amplification** (Brassard, Høyer, Mosca, Tapp, 2000).

### The General Setting

Suppose you have:
- A quantum algorithm $\mathcal{A}$ that prepares some state $|\psi\rangle = \mathcal{A}|0\rangle$
- A way to recognize "good" states (an oracle that marks solutions)

The initial state has some overlap with the solution space:
$$
a = |\langle w | \psi \rangle|^2
$$

In standard Grover, $\mathcal{A} = H^{\otimes n}$ and $a = M/N$.

### The Amplification Theorem

```{admonition} Amplitude Amplification Theorem
:class: important

Given:
- Algorithm $\mathcal{A}$ that produces state with overlap $a$ with solutions
- Oracle $O_f$ that marks solutions

Then: $O(\frac{1}{\sqrt{a}})$ applications of $\mathcal{A}$, $\mathcal{A}^{-1}$, and $O_f$ suffice to find a solution with high probability.
```

**The key insight:** If you have *any* method to generate states with overlap $a$ with solutions (even a classical randomized algorithm!), amplitude amplification boosts the success probability using only $O(1/\sqrt{a})$ iterations instead of $O(1/a)$ repetitions.

### Example: Boosting a Weak Classical Algorithm

Suppose a classical algorithm finds a solution with probability $p = 0.01$ (1%).

**Classical approach:** Run 100 times, expect ~1 success. Complexity: $O(1/p) = O(100)$.

**Quantum amplification:** 
1. Implement the classical algorithm as a quantum circuit $\mathcal{A}$
2. Apply amplitude amplification
3. Complexity: $O(1/\sqrt{p}) = O(10)$ — a 10× speedup!

### The Generalized Grover Iterate

For amplitude amplification, the iterate becomes:
$$
G = \mathcal{A} S_0 \mathcal{A}^{-1} O_f
$$

where $S_0 = 2|0\rangle\langle 0| - I$ flips the phase of the initial state $|0\rangle$.

Standard Grover is the special case $\mathcal{A} = H^{\otimes n}$, where $\mathcal{A}^{-1} = \mathcal{A}$ and $\mathcal{A} S_0 \mathcal{A}^{-1} = D$.

---

## Quantum Counting

What if we don't know the number of solutions $M$? **Quantum counting** solves this.

### The Idea

The Grover iterate $G$ has eigenvalues $e^{\pm 2i\theta}$ where $\sin\theta = \sqrt{M/N}$.

If we can estimate $\theta$, we can compute $M$!

### The Algorithm

1. Prepare the state $|s\rangle = H^{\otimes n}|0\rangle^{\otimes n}$
2. Apply **quantum phase estimation** (QPE) to estimate the eigenvalue of $G$
3. Extract $\theta$ from the estimated phase
4. Compute $M = N \sin^2\theta$

### Quantum Phase Estimation (Preview)

QPE is a powerful subroutine that estimates eigenvalues of unitary operators. Given:
- A unitary $U$ with eigenvalue $e^{2\pi i \phi}$
- An eigenstate $|u\rangle$ of $U$

QPE outputs an estimate of $\phi$ using $O(1/\epsilon)$ applications of controlled-$U$ for precision $\epsilon$.

We'll study QPE in detail in the next lecture (Shor's algorithm), where it's the key ingredient.

### Quantum Counting Complexity

**Query complexity:** $O(\sqrt{N})$ — same as Grover!

**What we gain:** Knowledge of $M$, which tells us:
- The optimal number of Grover iterations
- Whether any solutions exist ($M = 0$?)
- The fraction of solutions (useful for sampling)

```{admonition} Quantum Counting Summary
:class: tip

Quantum counting uses QPE on the Grover operator to estimate the number of solutions $M$.

- **Input:** Oracle $O_f$, search space size $N$
- **Output:** Estimate of $M = |\{x : f(x) = 1\}|$
- **Complexity:** $O(\sqrt{N}/\epsilon)$ for multiplicative error $\epsilon$

This is a quadratic speedup over classical counting, which requires $O(N)$ evaluations.
```

---

## The Algorithm Step by Step

Let's put it all together.

### Input
- Oracle $O_f$ that marks solutions
- Number of items $N = 2^n$
- (Optionally) estimate of number of solutions $M$

### Algorithm

1. **Initialize:** Prepare $|0\rangle^{\otimes n}$

2. **Create superposition:** Apply $H^{\otimes n}$ to get $|s\rangle = \frac{1}{\sqrt{N}}\sum_x |x\rangle$

3. **Grover iterations:** Repeat $k = \lfloor \frac{\pi}{4}\sqrt{N/M} \rfloor$ times:
   - Apply oracle $O_f$
   - Apply diffusion $D = H^{\otimes n}(2|0\rangle\langle 0| - I)H^{\otimes n}$

4. **Measure:** Measure all qubits in computational basis

5. **Verify:** Check if the result $x$ satisfies $f(x) = 1$

### Output
- A solution $x$ with $f(x) = 1$, with probability $\geq 1 - O(M/N)$

### Complexity
- **Query complexity:** $O(\sqrt{N/M})$ oracle calls
- **Gate complexity:** $O(\sqrt{N/M} \cdot n)$ gates (each iteration uses $O(n)$ gates)

---

## Why Quadratic, Not Exponential?

A natural question: Deutsch-Jozsa achieves exponential speedup. Why is Grover "only" quadratic?

### The Fundamental Limit

**Theorem (Bennett, Bernstein, Brassard, Vazirani 1997):** Any quantum algorithm for unstructured search requires $\Omega(\sqrt{N})$ queries.

Grover's algorithm is **optimal**! You cannot do better with quantum mechanics for this problem.

### The BBBV Proof Idea

The proof uses a clever "hybrid argument." Here's the intuition:

**Setup:** Consider $N+1$ different oracles:
- $O_0$: no solutions (all items return 0)
- $O_i$ for $i = 1, \ldots, N$: item $i$ is the unique solution

**Key observation:** After $k$ queries, the state depends on which oracle we're using. Call this state $|\psi_i^{(k)}\rangle$ for oracle $O_i$.

**The constraint:** Adjacent oracles $O_i$ and $O_{i+1}$ differ only in how they treat items $i$ and $i+1$. Each query can change the state by at most a bounded amount:
$$
\| |\psi_i^{(k)}\rangle - |\psi_{i+1}^{(k)}\rangle \| \leq O(k/\sqrt{N})
$$

**The requirement:** To distinguish $O_0$ from some $O_i$, the states must be noticeably different. By the triangle inequality:
$$
\| |\psi_0^{(k)}\rangle - |\psi_i^{(k)}\rangle \| \leq \sum_{j=0}^{i-1} \| |\psi_j^{(k)}\rangle - |\psi_{j+1}^{(k)}\rangle \| \leq O(ik/\sqrt{N})
$$

**The conclusion:** For the algorithm to succeed with constant probability on a random oracle, we need $\| |\psi_0^{(k)}\rangle - |\psi_i^{(k)}\rangle \| = \Omega(1)$ for most $i$. This requires $k = \Omega(\sqrt{N})$.

### Intuition: Information Accumulates Slowly

Another way to think about it:

Each oracle query provides limited information:
- Classically: you learn $f(x)$ for one $x$ — that's $O(1)$ bit
- Quantumly: you learn about phases on all $|x\rangle$ simultaneously, but measurement collapses most of this

The quadratic speedup comes from:
- **Quantum parallelism:** query all $x$ at once
- **Amplitude amplification:** boost solution amplitude by $O(1/\sqrt{N})$ per iteration
- After $\sqrt{N}$ iterations, amplitude reaches $O(1)$

But you can't do better because:
- Each query changes the state by a bounded amount (unitary evolution is continuous)
- You need the solution amplitude to go from $1/\sqrt{N}$ to $O(1)$
- This requires $\Omega(\sqrt{N})$ steps

```{admonition} The Optimality of Grover
:class: important

Grover's $O(\sqrt{N})$ query complexity is **provably optimal** for unstructured search.

This is one of the few cases where we have tight upper and lower bounds:
- Upper bound: Grover achieves $O(\sqrt{N})$
- Lower bound: BBBV proves $\Omega(\sqrt{N})$ is necessary

No future quantum algorithm can do better for this problem!
```

### Quantum Walks Perspective

There's an elegant alternative view of Grover's algorithm using **quantum walks**.

A **quantum walk** is the quantum analog of a random walk. Instead of probabilistically moving between states, a quantum walker moves in superposition, with amplitudes that can interfere.

**Grover as a quantum walk:** The algorithm can be viewed as a quantum walk on the complete graph $K_N$:
- Vertices are the $N$ items
- The "marked vertex" is the solution
- The walk dynamics are designed so the walker accumulates amplitude at the marked vertex

**Why this matters:**
1. Quantum walks generalize to **spatial search** on graphs
2. On some graphs, quantum walks achieve speedups beyond Grover
3. Quantum walks are a general algorithmic primitive (like classical random walks)

**Example:** Searching a 2D grid of $N$ items:
- Classical random walk: $O(N \log N)$ steps
- Quantum walk: $O(\sqrt{N \log N})$ steps

The quantum walk framework unifies many quantum algorithms and provides tools for designing new ones.

### Comparison with Structured Problems

| Problem | Classical | Quantum | Speedup |
|---------|-----------|---------|---------|
| Unstructured search | $O(N)$ | $O(\sqrt{N})$ | Quadratic |
| Deutsch-Jozsa (promise) | $O(2^n)$ | $O(1)$ | Exponential |
| Simon (hidden period) | $O(2^{n/2})$ | $O(n)$ | Exponential |
| Factoring | $O(e^{n^{1/3}})$ | $O(n^2)$ | Super-polynomial |

Exponential speedups require **structure** in the problem. For unstructured search, quadratic is the best possible.

---

## Multiple Solutions

What if there are $M > 1$ solutions?

### The Algorithm Still Works!

The geometric picture extends naturally:
- $|w\rangle$ is now the uniform superposition over all $M$ solutions
- Initial angle: $\sin\theta = \sqrt{M/N}$
- Optimal iterations: $k \approx \frac{\pi}{4}\sqrt{N/M}$

**Fewer iterations needed!** More solutions means faster convergence.

### The Catch: You Need to Know M

The optimal number of iterations depends on $M$. If you don't know $M$:

**Option 1: Guess and check**
- Try $k = 1, 2, 4, 8, \ldots$ iterations
- After each, measure and check if $f(x) = 1$
- Expected total queries: $O(\sqrt{N/M})$ (only constant factor overhead)

**Option 2: Quantum counting**
- Use a variant of Grover + quantum phase estimation
- Estimate $M$ with $O(\sqrt{N})$ queries
- Then run Grover with the right $k$

### Finding All Solutions

To find all $M$ solutions:
- Run Grover $O(M)$ times
- Each run finds one solution (possibly a repeat)
- After $O(M \log M)$ runs, you've found all with high probability

---

## Applications

### Database Search

The original motivation: search an unsorted database of $N$ items.

**Classical:** $O(N)$ lookups.
**Quantum:** $O(\sqrt{N})$ lookups.

But be careful! In practice, the "oracle" (database lookup) may dominate the cost. The speedup is in *query complexity*, not necessarily wall-clock time.

### SAT and Constraint Satisfaction

Given a Boolean formula $\phi(x_1, \ldots, x_n)$, find an assignment that satisfies it.

**Search space:** $N = 2^n$ possible assignments.

**Oracle:** $f(x) = 1$ if $\phi(x)$ is true.

**Grover:** Find a satisfying assignment in $O(\sqrt{2^n}) = O(2^{n/2})$ evaluations.

This doesn't solve SAT in polynomial time (it's still exponential in $n$), but it's a quadratic improvement over brute force.

### Cryptographic Attacks

**Symmetric key search:** Given ciphertext $c$, find key $k$ such that $\text{Encrypt}(k, m) = c$.

With $n$-bit keys:
- Classical: $O(2^n)$ encryptions
- Quantum Grover: $O(2^{n/2})$ encryptions

**Implication:** 128-bit keys offer only 64 bits of security against quantum attacks.

**Mitigation:** Use 256-bit keys (128 bits of quantum security).

### Optimization

Find $x$ that minimizes $f(x)$.

**Approach:** Binary search on the minimum value.
- Is there an $x$ with $f(x) < v$?
- Use Grover to search for such an $x$
- Adjust $v$ based on result

**Complexity:** $O(\sqrt{N} \log(\text{range}))$ queries.

### Amplitude Estimation

A generalization of Grover that estimates the fraction of solutions:

Given oracle for $f$, estimate $p = M/N = \Pr_x[f(x) = 1]$.

**Classical:** $O(1/\epsilon^2)$ samples for precision $\epsilon$.
**Quantum:** $O(1/\epsilon)$ queries — quadratic speedup!

This is useful for Monte Carlo estimation, option pricing, and risk analysis.

### Fixed-Point Amplitude Amplification

Standard Grover has a problem: if you don't know $M$, you might **overshoot** and decrease success probability.

**Fixed-point amplitude amplification** (Yoder, Low, Chuang 2014) solves this:

The key idea is to modify the reflection angles in each iteration so that:
1. Success probability increases **monotonically**
2. Running more iterations never hurts
3. The algorithm converges to a fixed point (hence the name)

**The modification:** Instead of reflecting by exactly $\pi$ in the oracle and diffusion, use a sequence of carefully chosen angles that converge to the solution without overshooting.

**Trade-off:** Fixed-point Grover uses about 3× more queries than standard Grover, but it's more robust when $M$ is unknown.

```{admonition} Fixed-Point Grover
:class: tip

Standard Grover can overshoot if you run too many iterations.

Fixed-point amplitude amplification modifies the algorithm so that success probability increases monotonically — running more iterations always helps (up to the limit).

Cost: ~3× more queries, but no need to know $M$ precisely.
```

### Grover Adaptive Search (Optimization)

For optimization problems, we want to find $x$ that **minimizes** $f(x)$, not just satisfies a condition.

**Grover Adaptive Search:**

1. Set threshold $T = $ some initial estimate
2. Define oracle: $g(x) = 1$ if $f(x) < T$, else $0$
3. Use Grover to find $x$ with $f(x) < T$
4. Update $T \leftarrow f(x)$
5. Repeat until no improvement found

**Analysis:** Each phase finds a better solution. The number of phases is at most $O(\log(\text{range}))$. Each Grover search takes $O(\sqrt{N/M_T})$ where $M_T$ is the number of items better than threshold $T$.

**Total complexity:** $O(\sqrt{N})$ on average (by amortized analysis).

**Applications:**
- Combinatorial optimization
- Satisfiability (finding best assignment)
- Machine learning (hyperparameter search)

---

## Lab: Implementing Grover's Algorithm

### Setup

```{code-block} python
import numpy as np
from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister
from qiskit.quantum_info import Statevector
from qiskit_aer import AerSimulator

simulator = AerSimulator()
```

### Creating the Oracle

```{code-block} python
def create_oracle(n, solutions):
    """
    Create a Grover oracle that marks the given solutions.
    
    The oracle flips the phase of marked states: |x⟩ → -|x⟩ if f(x) = 1.
    
    Implementation: For each solution s, we:
    1. Apply X to qubits where s has a 0 (so |s⟩ becomes |11...1⟩)
    2. Apply multi-controlled Z (flips phase of |11...1⟩)
    3. Apply X again to restore the qubits
    
    Note: This implementation marks each solution separately, so if multiple
    solutions share structure, a more efficient circuit might be possible.
    
    Oracle complexity: O(n) gates per solution using O(n) ancillas,
    or O(n²) gates per solution with no ancillas.
    """
    oracle = QuantumCircuit(n, name='Oracle')
    
    for sol in solutions:
        # Step 1: Flip bits that are 0 in the solution
        # This transforms |sol⟩ to |11...1⟩
        for i in range(n):
            if not (sol >> i) & 1:  # If bit i of sol is 0
                oracle.x(i)
        
        # Step 2: Multi-controlled Z gate
        # Flips phase only when all qubits are |1⟩
        if n == 1:
            oracle.z(0)
        elif n == 2:
            oracle.cz(0, 1)
        else:
            # MCZ = H on target, then MCX (Toffoli generalization), then H
            # This flips phase of |11...1⟩
            oracle.h(n-1)
            oracle.mcx(list(range(n-1)), n-1)  # Multi-controlled X
            oracle.h(n-1)
        
        # Step 3: Undo the bit flips
        for i in range(n):
            if not (sol >> i) & 1:
                oracle.x(i)
    
    return oracle.to_gate()
```

```{admonition} Oracle Complexity
:class: warning

The oracle construction above is simple but not optimal:

**Gate complexity:** A multi-controlled Z on $n$ qubits requires:
- $O(n)$ Toffoli gates with $O(n)$ ancilla qubits, OR
- $O(n^2)$ Toffoli gates with no ancillas

**Practical implication:** For large $n$, the oracle can dominate the algorithm cost. Grover's $O(\sqrt{N})$ query speedup only helps when each query (oracle call) is not too expensive.

For specific problems (like SAT), custom oracle constructions can be much more efficient than the generic approach shown here.
```

### Creating the Diffusion Operator

```{code-block} python
def create_diffusion(n):
    """
    Create the Grover diffusion operator.
    D = H^n (2|0><0| - I) H^n
    """
    diffusion = QuantumCircuit(n, name='Diffusion')
    
    # Apply H to all qubits
    diffusion.h(range(n))
    
    # Apply X to all qubits (to flip for |0> detection)
    diffusion.x(range(n))
    
    # Multi-controlled Z (phase flip |11...1>)
    diffusion.h(n-1)
    diffusion.mcx(list(range(n-1)), n-1)
    diffusion.h(n-1)
    
    # Apply X to all qubits
    diffusion.x(range(n))
    
    # Apply H to all qubits
    diffusion.h(range(n))
    
    return diffusion.to_gate()
```

### The Full Grover Circuit

```{code-block} python
def grover_circuit(n, solutions, iterations=None):
    """
    Create the full Grover circuit.
    n: number of qubits
    solutions: list of marked items
    iterations: number of Grover iterations (default: optimal)
    """
    N = 2**n
    M = len(solutions)
    
    if iterations is None:
        # Optimal number of iterations
        iterations = int(np.round(np.pi/4 * np.sqrt(N/M)))
    
    qc = QuantumCircuit(n, n)
    
    # Initialize to uniform superposition
    qc.h(range(n))
    qc.barrier()
    
    # Create oracle and diffusion gates
    oracle = create_oracle(n, solutions)
    diffusion = create_diffusion(n)
    
    # Apply Grover iterations
    for _ in range(iterations):
        qc.append(oracle, range(n))
        qc.append(diffusion, range(n))
        qc.barrier()
    
    # Measure
    qc.measure(range(n), range(n))
    
    return qc, iterations
```

### Testing Grover's Algorithm

```{code-block} python
def test_grover():
    """Test Grover's algorithm with various configurations"""
    print("Grover's Algorithm Test")
    print("="*60)
    
    test_cases = [
        (3, [5], "1 solution in 8 items"),
        (4, [7], "1 solution in 16 items"),
        (4, [3, 7], "2 solutions in 16 items"),
        (5, [13], "1 solution in 32 items"),
    ]
    
    for n, solutions, description in test_cases:
        N = 2**n
        qc, iterations = grover_circuit(n, solutions)
        
        result = simulator.run(qc, shots=1000).result()
        counts = result.get_counts()
        
        # Check success rate
        success = sum(counts.get(format(s, f'0{n}b'), 0) for s in solutions)
        total = sum(counts.values())
        
        print(f"\n{description}")
        print(f"  N = {N}, solutions = {solutions}")
        print(f"  Iterations: {iterations}")
        print(f"  Success rate: {100*success/total:.1f}%")
        print(f"  Top results: {dict(sorted(counts.items(), key=lambda x: -x[1])[:3])}")

test_grover()
```

### Visualizing the Amplitude Evolution

```{code-block} python
def visualize_amplitude_evolution(n, solution, max_iterations=None):
    """Visualize how amplitudes evolve during Grover iterations"""
    import matplotlib.pyplot as plt
    
    N = 2**n
    if max_iterations is None:
        max_iterations = int(np.ceil(np.pi/4 * np.sqrt(N))) + 2
    
    solution_amps = []
    other_amps = []
    
    for k in range(max_iterations + 1):
        qc = QuantumCircuit(n)
        qc.h(range(n))
        
        oracle = create_oracle(n, [solution])
        diffusion = create_diffusion(n)
        
        for _ in range(k):
            qc.append(oracle, range(n))
            qc.append(diffusion, range(n))
        
        sv = Statevector(qc)
        amplitudes = np.abs(sv.data)**2
        
        solution_amps.append(amplitudes[solution])
        # Average of non-solution amplitudes
        other_amps.append((1 - amplitudes[solution]) / (N - 1))
    
    plt.figure(figsize=(10, 5))
    plt.plot(range(max_iterations + 1), solution_amps, 'b-o', label='Solution')
    plt.plot(range(max_iterations + 1), other_amps, 'r-o', label='Non-solution (avg)')
    plt.xlabel('Iterations')
    plt.ylabel('Probability')
    plt.title(f'Grover Amplitude Evolution (N={N}, solution={solution})')
    plt.legend()
    plt.grid(True)
    plt.axhline(y=1/N, color='gray', linestyle='--', label='Initial')
    optimal_k = int(np.round(np.pi/4 * np.sqrt(N)))
    plt.axvline(x=optimal_k, color='green', linestyle='--', label=f'Optimal k={optimal_k}')
    plt.savefig('grover_evolution.png', dpi=150, bbox_inches='tight')
    plt.show()
    
    print(f"Optimal iterations: {optimal_k}")
    print(f"Solution probability at optimal: {solution_amps[optimal_k]:.4f}")

# Uncomment to run:
# visualize_amplitude_evolution(4, 7)
```

### Amplitude Bar Chart (N=4 Example)

```{code-block} python
def visualize_n4_steps():
    """Visualize each step of Grover for N=4, solution=3"""
    import matplotlib.pyplot as plt
    from qiskit.quantum_info import Statevector
    
    n = 2
    solution = 3  # |11⟩
    N = 4
    
    fig, axes = plt.subplots(1, 3, figsize=(12, 4))
    labels = ['|00⟩', '|01⟩', '|10⟩', '|11⟩']
    colors = ['steelblue', 'steelblue', 'steelblue', 'orangered']
    
    # Step 1: After Hadamard (uniform superposition)
    qc = QuantumCircuit(n)
    qc.h(range(n))
    sv = Statevector(qc)
    amps = sv.data.real  # Real part (all positive initially)
    
    axes[0].bar(labels, amps, color=colors)
    axes[0].axhline(y=0, color='black', linewidth=0.5)
    axes[0].set_ylim(-0.6, 1.1)
    axes[0].set_ylabel('Amplitude')
    axes[0].set_title('(a) Initial: After H⊗²')
    
    # Step 2: After Oracle (flip |11⟩)
    oracle = create_oracle(n, [solution])
    qc.append(oracle, range(n))
    sv = Statevector(qc)
    amps = sv.data.real
    
    axes[1].bar(labels, amps, color=colors)
    axes[1].axhline(y=0, color='black', linewidth=0.5)
    axes[1].axhline(y=np.mean(amps), color='green', linestyle='--', label=f'Mean = {np.mean(amps):.2f}')
    axes[1].set_ylim(-0.6, 1.1)
    axes[1].set_title('(b) After Oracle')
    axes[1].legend()
    
    # Step 3: After Diffusion (inversion about mean)
    diffusion = create_diffusion(n)
    qc.append(diffusion, range(n))
    sv = Statevector(qc)
    amps = sv.data.real
    
    axes[2].bar(labels, amps, color=colors)
    axes[2].axhline(y=0, color='black', linewidth=0.5)
    axes[2].set_ylim(-0.6, 1.1)
    axes[2].set_title('(c) After Diffusion')
    
    plt.suptitle('Grover Algorithm: N=4, Solution=|11⟩', fontsize=14)
    plt.tight_layout()
    plt.savefig('grover_n4_steps.png', dpi=150, bbox_inches='tight')
    plt.show()

# Uncomment to run:
# visualize_n4_steps()
```

### The Geometric Picture

```{code-block} python
def visualize_grover_geometry(n, solution, iterations=None):
    """Visualize Grover's algorithm geometrically"""
    import matplotlib.pyplot as plt
    
    N = 2**n
    M = 1  # One solution
    
    theta = np.arcsin(np.sqrt(M/N))
    
    if iterations is None:
        iterations = int(np.round(np.pi/4 * np.sqrt(N/M)))
    
    fig, ax = plt.subplots(figsize=(8, 8))
    
    # Draw axes
    ax.arrow(0, 0, 1.2, 0, head_width=0.03, head_length=0.03, fc='black', ec='black')
    ax.arrow(0, 0, 0, 1.2, head_width=0.03, head_length=0.03, fc='black', ec='black')
    ax.text(1.25, 0, r'$|w^\perp\rangle$', fontsize=12)
    ax.text(0, 1.25, r'$|w\rangle$', fontsize=12)
    
    # Draw initial state
    ax.arrow(0, 0, np.cos(theta), np.sin(theta), head_width=0.03, 
             head_length=0.03, fc='blue', ec='blue', linewidth=2)
    ax.text(np.cos(theta)+0.05, np.sin(theta)+0.05, r'$|s\rangle$', fontsize=12, color='blue')
    
    # Draw states after each iteration
    colors = plt.cm.viridis(np.linspace(0, 1, iterations+1))
    
    for k in range(iterations + 1):
        angle = (2*k + 1) * theta
        x, y = np.cos(angle), np.sin(angle)
        if k > 0:
            ax.plot([0, x], [0, y], '-', color=colors[k], linewidth=1.5)
            ax.plot(x, y, 'o', color=colors[k], markersize=8)
            if k == iterations:
                ax.text(x+0.05, y+0.05, f'After {k} iter', fontsize=10)
    
    # Draw unit circle
    circle = plt.Circle((0, 0), 1, fill=False, color='gray', linestyle='--')
    ax.add_patch(circle)
    
    ax.set_xlim(-0.3, 1.4)
    ax.set_ylim(-0.3, 1.4)
    ax.set_aspect('equal')
    ax.grid(True, alpha=0.3)
    ax.set_title(f'Grover Geometry: N={N}, θ={theta:.3f} rad, {iterations} iterations')
    
    plt.savefig('grover_geometry.png', dpi=150, bbox_inches='tight')
    plt.show()

# Uncomment to run:
# visualize_grover_geometry(4, 7, iterations=3)
```

---

## Common Pitfalls and Practical Considerations

### 1. Wrong Number of Iterations

Too few iterations: low success probability.
Too many iterations: state rotates past the solution, low success probability.

**Solution:** If you don't know $M$, use the "guess and double" strategy.

### 2. The Oracle Dominates Complexity

Grover gives a quadratic speedup in **queries**, but if each query (oracle evaluation) is expensive, the total cost may still be high.

**Example:** Searching a physical database where each lookup requires disk I/O. The oracle cost dominates, and Grover provides little practical benefit.

### 3. No Solutions

If $M = 0$ (no solutions), Grover rotates the state but never reaches a solution. After measurement, you'll get a random non-solution.

**Solution:** Run the algorithm and verify $f(x) = 1$. If verification fails, there may be no solutions (or you need more iterations).

### 4. Classical Preprocessing May Be Better

For some problems, classical preprocessing (sorting, indexing, hashing) reduces the search space more than Grover's $\sqrt{N}$ speedup.

**Example:** Finding a name in a phone book.
- Classical brute force: $O(N)$
- Classical with sorting: $O(\log N)$ (binary search)
- Quantum Grover: $O(\sqrt{N})$

Binary search beats Grover! Use the right tool for the problem.

### 5. Fault Tolerance Requirements

Grover requires $O(\sqrt{N})$ iterations, each with $O(n)$ gates. For large $N$, this is many gates, requiring error correction.

**Current NISQ devices:** Limited to small $N$ (maybe $N \approx 1000$).

**Fault-tolerant era:** Grover becomes practical for larger search spaces.

---

## Grover's Algorithm and NP Problems

### The Temptation

NP-complete problems (like SAT) have $N = 2^n$ possible solutions. Grover searches in $O(\sqrt{N}) = O(2^{n/2})$ time.

**Tempting thought:** "Grover gives exponential speedup for NP problems!"

### The Reality

$O(2^{n/2})$ is still exponential in $n$. It's a **quadratic speedup**, not a polynomial-time algorithm.

| Problem Size | Classical Brute Force | Quantum Grover |
|--------------|----------------------|----------------|
| $n = 20$ | $2^{20} \approx 10^6$ | $2^{10} \appro 10^3$ |
| $n = 40$ | $2^{40} \approx 10^{12}$ | $2^{20} \approx 10^6$ |
| $n = 100$ | $2^{100} \approx 10^{30}$ | $2^{50} \approx 10^{15}$ |
| $n = 256$ | $2^{256}$ (astronomical) | $2^{128}$ (still huge!) |

Grover doesn't make NP problems tractable. It makes them **somewhat less intractable**.

### The BQP vs NP Question

**BQP** = problems solvable by quantum computers in polynomial time.

Current belief: $\text{BQP} \not\supseteq \text{NP}$ (quantum computers probably can't solve all NP problems efficiently).

Grover provides evidence for this: even the best possible quantum search is only quadratically faster.

---

## Summary

1. **The unstructured search problem:** Find $x$ with $f(x) = 1$ among $N$ items.

2. **Complexity:**
   - Classical: $O(N)$ queries
   - Quantum (Grover): $O(\sqrt{N})$ queries
   - This is **provably optimal** (BBBV theorem)

3. **The algorithm:**
   - Initialize: uniform superposition $|s\rangle$
   - Repeat $k \approx \frac{\pi}{4}\sqrt{N/M}$ times: Oracle → Diffusion
   - Measure

4. **The Grover iterate:** $G = D \cdot O_f$ is a rotation by $2\theta$ in the 2D subspace
   - Eigenvalues: $e^{\pm 2i\theta}$ where $\sin\theta = \sqrt{M/N}$
   - This eigenstructure enables quantum counting via phase estimation

5. **Geometric picture:**
   - Oracle = reflection about $|w^\perp\rangle$
   - Diffusion = reflection about $|s\rangle$
   - Two reflections = rotation by $2\theta$

6. **Amplitude amplification** generalizes Grover:
   - Start with any state having overlap $a$ with solutions
   - Amplify to high success probability in $O(1/\sqrt{a})$ iterations
   - Useful for boosting weak classical algorithms

7. **Quantum counting:** Use phase estimation on $G$ to estimate $M$ in $O(\sqrt{N})$ queries

8. **Variants:**
   - **Fixed-point Grover:** Never overshoots, converges monotonically
   - **Grover adaptive search:** Finds minimum in $O(\sqrt{N})$ queries

9. **Applications:**
   - Database search, SAT, constraint satisfaction
   - Cryptographic attacks (halves effective key length)
   - Optimization via adaptive search
   - Monte Carlo via amplitude estimation

10. **Limitations:**
    - Quadratic speedup only (not exponential)
    - Oracle complexity matters
    - Still exponential for NP problems
    - Requires $\Omega(\sqrt{N})$ coherent operations

11. **Quantum walks:** Grover can be viewed as a quantum walk on the complete graph; this perspective generalizes to spatial search on other graphs.

---

## Homework

### Problem 1: Grover by Hand (Small Example)

Consider $N = 4$ items with one solution at $x = 3$.

**(a)** Write the initial state $|s\rangle$ as a vector in the computational basis.

**(b)** Write the oracle matrix $O_f$.

**(c)** Write the diffusion matrix $D$.

**(d)** Compute $O_f|s\rangle$.

**(e)** Compute $D \cdot O_f|s\rangle$.

**(f)** What is the probability of measuring $|11\rangle$ (the solution)?

**(g)** How does this compare to the initial probability $1/4$?

---

### Problem 2: Optimal Iterations

**(a)** For $N = 256$ with $M = 1$ solution, calculate the optimal number of iterations.

**(b)** What is the success probability at the optimal iteration count?

**(c)** What happens if you use exactly double the optimal iterations?

**(d)** What happens if you use exactly half the optimal iterations?

---

### Problem 3: Multiple Solutions

**(a)** For $N = 1024$ with $M = 4$ solutions, calculate the optimal number of iterations.

**(b)** Compare to the case $M = 1$. Why are fewer iterations needed?

**(c)** If you don't know $M$ but know it's between 1 and 10, describe a strategy to find a solution.

---

### Problem 4: Geometric Derivation

**(a)** Prove that the oracle $O_f$ is a reflection about $|w^\perp\rangle$ in the $\{|w\rangle, |w^\perp\rangle\}$ subspace.

**(b)** Prove that the diffusion operator $D$ is a reflection about $|s\rangle$.

**(c)** Use the fact that two reflections compose to a rotation to derive the rotation angle $2\theta$.

---

### Problem 5: Diffusion Operator

**(a)** Verify that $D = 2|s\rangle\langle s| - I$ acts as "inversion about the mean."

**(b)** Show that $D = H^{\otimes n}(2|0\rangle\langle 0| - I)H^{\otimes n}$.

**(c)** The operator $2|0\rangle\langle 0| - I$ flips the phase of all states except $|0\rangle$. Design a circuit for this using X gates and a multi-controlled Z.

---

### Problem 6: Query Complexity Lower Bound

The BBBV theorem says Grover is optimal. Here's intuition for why:

**(a)** In one oracle query, how much can the state change? (Hint: the oracle is a unitary that acts as identity on non-solutions.)

**(b)** The solution amplitude starts at $1/\sqrt{N}$ and needs to reach $O(1)$. If each query can change amplitudes by at most $O(1/\sqrt{N})$, how many queries are needed?

**(c)** Make this argument more precise. (This is challenging!)

---

### Problem 7: Cryptographic Implications

**(a)** AES-128 uses 128-bit keys. How many classical operations are needed to brute-force search all keys?

**(b)** How many Grover iterations would a quantum computer need?

**(c)** What key length provides 128 bits of security against quantum attack?

**(d)** Why is this called "Grover's attack" on symmetric cryptography?

---

### Problem 8: SAT Oracle Design

Consider the Boolean formula in CNF (Conjunctive Normal Form):
$$\phi(x_1, x_2, x_3) = (x_1 \lor x_2) \land (\neg x_1 \lor x_3) \land (\neg x_2 \lor \neg x_3)$$

**(a)** How many possible assignments $(x_1, x_2, x_3) \in \{0,1\}^3$?

**(b)** Find all satisfying assignments by exhaustive search.

**(c)** Design an oracle circuit that marks satisfying assignments.

*Hint:* Build the oracle in layers:
1. **Clause evaluation:** For each clause, compute whether it's satisfied into an ancilla. For example, $(x_1 \lor x_2)$ is satisfied unless both are 0, so use a Toffoli with $x_1$ and $x_2$ as controls (after flipping them) targeting an ancilla.
2. **AND the clauses:** Use another Toffoli to compute the AND of all clause ancillas.
3. **Phase flip:** Apply Z to the final ancilla (or use phase kickback).
4. **Uncompute:** Reverse steps 1-2 to clean up ancillas.

**(d)** How many ancilla qubits does your circuit need?

**(e)** If this formula had $n$ variables and $m$ clauses (each with at most $k$ literals), estimate the oracle complexity in terms of $n$, $m$, $k$.

**(f)** Compare the total complexity of Grover + your oracle to classical brute force for $n = 100$ variables.

---

### Problem 9: Implementation

**(a)** Implement Grover's algorithm in Qiskit for $N = 16$ with solution at $x = 9$.

**(b)** Run with 0, 1, 2, 3, 4, 5 iterations and plot the success probability.

**(c)** Where is the maximum? Does it match the theoretical prediction?

**(d)** Implement amplitude estimation to estimate the number of solutions for a mystery oracle.

---

### Problem 10: Grover vs Classical

**(a)** For what value of $N$ does Grover with $\sqrt{N}$ queries equal classical with $N$ queries at $N = 10^6$ operations? (Assume 1 query = 1 operation.)

**(b)** If quantum operations are 1000× slower than classical operations, for what $N$ does Grover become advantageous?

**(c)** If the oracle requires $n$ gates to evaluate and $N = 2^n$, compare total gate complexity of classical brute force vs Grover.

---

### Problem 11: Amplitude Amplification (General)

Amplitude amplification generalizes Grover to the case where the initial state is not uniform.

**(a)** Suppose we start with state $|\psi\rangle$ (not necessarily $|s\rangle$) where $|\langle w|\psi\rangle|^2 = a$ (overlap with solution space). What is the optimal number of iterations?

**(b)** This is useful when a classical algorithm can generate "good guesses" with probability $a > 1/N$. If $a = 1/100$, how many iterations are needed?

**(c)** Compare to Grover with uniform start ($a = M/N$). When is amplitude amplification advantageous?

---

### Problem 12: Practical Considerations

**(a)** You have a quantum computer with 50 qubits and can run circuits of depth 1000. What is the largest $N$ for which you can run Grover?

**(b)** If each Grover iteration requires 100 gates, estimate the maximum $N$.

**(c)** Current quantum computers have gate error rates around 0.1%. After how many gates does the error become significant? What does this imply for Grover?

**(d)** Discuss: when will Grover's algorithm provide practical speedups on real hardware?

---

### Problem 13: Quantum Counting

Quantum counting uses phase estimation on the Grover iterate $G$ to estimate the number of solutions $M$.

**(a)** The Grover iterate $G = D \cdot O_f$ has eigenvalues $e^{\pm 2i\theta}$ where $\sin\theta = \sqrt{M/N}$. Verify this for the 2D subspace spanned by $\{|w\rangle, |w^\perp\rangle\}$.

**(b)** If phase estimation gives us $\theta$ with precision $\delta$, what is the error in our estimate of $M$? (Use $M = N\sin^2\theta$ and propagate errors.)

**(c)** To estimate $M$ with relative error $\epsilon$ (i.e., $|\hat{M} - M| \leq \epsilon M$), what precision $\delta$ do we need in $\theta$?

**(d)** Phase estimation requires $O(1/\delta)$ controlled applications of $G$. Each application of $G$ uses 1 oracle query. What is the total query complexity to estimate $M$ with relative error $\epsilon$?

**(e)** Compare to classical counting: to estimate $M$ with relative error $\epsilon$, how many samples/queries does classical counting need?

---

### Problem 14: Fixed-Point Grover

Standard Grover can overshoot if we run too many iterations.

**(a)** For $N = 1000$ and $M = 1$, calculate the success probability after $k = 25$ iterations (optimal is about 25) and after $k = 50$ iterations.

**(b)** If you don't know $M$ and guess $k$ randomly between 1 and 50, what is your expected success probability? (Average over both $k$ and the measurement outcome.)

**(c)** The "guess and double" strategy runs Grover with $k = 1, 2, 4, 8, \ldots$ iterations, measuring after each. Argue that this finds a solution in $O(\sqrt{N/M})$ total queries.

**(d)** Why might fixed-point amplitude amplification (which never overshoots) be preferable to "guess and double" in practice?

---

## Looking Ahead

Grover showed us that quantum computers offer a quadratic speedup for the most general search problem. The speedup is modest but broadly applicable.

Next lecture, we tackle the crown jewel of quantum algorithms: **Shor's algorithm for factoring**. Unlike Grover's quadratic speedup, Shor achieves an exponential speedup — and breaks the RSA cryptosystem that secures most of the internet.