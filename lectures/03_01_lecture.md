# Lecture 3.1 — Two Qubits and the Tensor Product

## The Turning Point

Everything changes when we have more than one qubit.

In Chapter 2, we mastered the single qubit: a wave over two configurations, complex amplitudes, rotations on the Bloch sphere, interference, measurement. Beautiful physics, and the foundation of quantum sensing.

But a single qubit is not enough for quantum computing. With one qubit, you have 2 configurations. With two qubits, you have 4. With three, you have 8. With $N$ qubits, you have $2^N$ configurations.

This exponential scaling is where quantum computing gets its power — and where things become truly strange. A system of just 50 qubits has more configurations than there are atoms in the Earth. No classical computer can even write down the full quantum state.

Today we learn how to describe multi-qubit systems, and we'll meet **entanglement** — correlations that have no classical explanation.

---

## Part 1: Classical Correlations

Before quantum mechanics, let's build our intuition with something tangible.

### Two Marbles

I have two marbles: marble A (yours) and marble B (mine). Each marble is either blue (0) or red (1), chosen at random.

**Individual probability distributions:**

$$P_A(0) = 0.5, \quad P_A(1) = 0.5$$
$$P_B(0) = 0.5, \quad P_B(1) = 0.5$$

Since there are two marbles, each with two possible colors, the **joint** system has four possible outcomes — all possible pairs:

$$(0,0), \quad (0,1), \quad (1,0), \quad (1,1)$$

That's $2 \times 2 = 4$ outcomes. For $N$ marbles: $2^N$ outcomes. This combinatorial explosion isn't quantum mechanics yet — it's just counting. But quantum mechanics inherits this structure.

### Joint Probability Distributions

A **joint probability distribution** $P(i, j)$ assigns a probability to each of the four outcomes. It must satisfy:

$$P(i, j) \geq 0, \qquad \sum_{i,j} P(i,j) = 1$$

The **marginal distributions** are obtained by summing over the other variable:

$$P_A(i) = \sum_j P(i, j), \qquad P_B(j) = \sum_i P(i, j)$$

The marginals tell you about each marble individually. But they don't fully determine the joint distribution — different joint distributions can have the same marginals. The extra information is in the **correlations** between the marbles.

The key question is: **what joint probability distribution describes a given pair?**

---

### Correlated Outcomes

```{admonition} iClicker
:class: tip

We draw many pairs and measure. Every time, we observe same-color pairs: both blue or both red.

Which probability distribution describes this?

|  | $P(0,0)$ | $P(0,1)$ | $P(1,0)$ | $P(1,1)$ |
|--|--|--|--|--|
| **(A)** | 0 | 0 | 0.5 | 0.5 |
| **(B)** | 0.25 | 0.25 | 0.25 | 0.25 |
| **(C)** | 0.5 | 0 | 0 | 0.5 |
```

The answer is **(C)**: outcomes (0,0) and (1,1) each with probability 1/2. We never see different colors.

**Are these marbles independent?**

Two systems are **independent** (uncorrelated) if the joint probability factors into a product of the marginals:
$$P(i, j) = P_A(i) \cdot P_B(j)$$

Let's check. The marginals of distribution (C) are $P_A(0) = 0.5$, $P_A(1) = 0.5$, and $P_B(0) = 0.5$, $P_B(1) = 0.5$. The product gives $P_A(0) \cdot P_B(0) = 0.25 \neq 0.5 = P(0,0)$.

The joint distribution cannot be written as a product of marginals. These marbles are **classically correlated**.

---

### Anti-Correlated Outcomes

```{admonition} iClicker
:class: tip

Now we draw pairs and always see opposite colors: blue-red or red-blue.

Which probability distribution describes this?

|  | $P(0,0)$ | $P(0,1)$ | $P(1,0)$ | $P(1,1)$ |
|--|--|--|--|--|
| **(A)** | 0 | 0.5 | 0.5 | 0 |
| **(B)** | 0.5 | 0 | 0 | 0.5 |
| **(C)** | 0.25 | 0.25 | 0.25 | 0.25 |
```

The answer is **(A)**: outcomes (0,1) and (1,0) each with probability 1/2.

Are they correlated? Try factoring: $P(i,j) = P_A(i) \cdot P_B(j)$. With $P_A(0) = 0.5$ and $P_B(0) = 0.5$, the product gives $P(0,0) = 0.25 \neq 0$. **Doesn't work.** We cannot write this as a product. The marbles are classically correlated — this time, anti-correlated.

---

### Classical Correlations Are Not Unusual

```{admonition} Classical Correlations
:class: note

Classical correlations arise from shared history or common causes. Learning about one part of a correlated system reveals information about the other part, but doesn't affect it. The information was always there — you just didn't know it.
```

There's nothing strange about classical correlations — they happen all the time. If I put one red marble and one blue marble into two boxes and shuffle them, the boxes are anti-correlated. Opening one box tells you the color of the other. This is perfectly described by a **hidden variable**: the shuffling procedure determined which marble went where, and opening a box just reveals a pre-existing fact.

Any classical correlation, no matter how strong, can be explained by a hidden variable. The correlation was established in the past, encoded in some shared information, and measurement just reads it out. This is the picture Einstein wanted for quantum mechanics too.

What we'll discover in the next lecture — through Bell's theorem — is that quantum mechanics produces correlations that are **too strong** to be explained this way. It's not that quantum systems are correlated (classical systems are too). It's that the *amount* and *structure* of quantum correlations exceed what any hidden variable model can produce.

Keep this in your back pocket: we're about to see quantum states with exactly the same measurement statistics as these marbles — outcomes 00 and 11, or 01 and 10, each 50/50. The question will be: **is the explanation the same?**

---

## Part 2: Now Qubits

### One Qubit (Review)

A single qubit lives in a 2-dimensional Hilbert space:
$$|\Psi\rangle = c_0|0\rangle + c_1|1\rangle$$

It's a wave over two configurations, fully described by a point on the Bloch sphere — 2 real degrees of freedom (after normalization and global phase). We can represent it as a column vector:

$$|\Psi\rangle = \begin{pmatrix} c_0 \\ c_1 \end{pmatrix}$$

---

### Quantum Mechanics Is a Wave over Configurations

Here is the key conceptual leap.

For a single qubit, the state is a superposition over two possibilities:
$$|\psi\rangle = c_0|0\rangle + c_1|1\rangle$$

Each possibility gets a complex amplitude. The squared magnitudes give probabilities.

For two qubits, the state is a superposition over all four configurations of the pair:
$$|\Psi\rangle = c_{00}|00\rangle + c_{01}|01\rangle + c_{10}|10\rangle + c_{11}|11\rangle$$

Each configuration gets its own complex amplitude. This is the essential structure of quantum mechanics: **it assigns a complex number — an amplitude with magnitude and phase — to every possible configuration of the system.** The probabilities are the squared magnitudes: $P(01) = |c_{01}|^2$, etc.

For $N$ qubits, there are $2^N$ configurations, each with its own amplitude. This exponential growth is the fundamental resource of quantum computing — and the reason quantum systems are hard to simulate classically.

> Quantum dynamics is a wave over configurations. Everything that can happen does happen, just with some complex amplitude (magnitude and phase).

---

### Two Independent Qubits: The Tensor Product

Now suppose we have two qubits: qubit A in state $|\psi\rangle = a|0\rangle + b|1\rangle$ and qubit B in state $|\phi\rangle = c|0\rangle + d|1\rangle$.

How do we describe the joint system? We need a mathematical operation that combines configuration spaces. This is the **tensor product**, written $\otimes$:

$$|\psi\rangle \otimes |\phi\rangle = (a|0\rangle + b|1\rangle) \otimes (c|0\rangle + d|1\rangle)$$

The tensor product distributes just like ordinary multiplication — $(a + b)(c + d) = ac + ad + bc + bd$:

$$= ac|00\rangle + ad|01\rangle + bc|10\rangle + bd|11\rangle$$

where $|0\rangle \otimes |0\rangle \equiv |00\rangle$, and so on.

This describes **two independent particles that have never interacted.** Each qubit has its own state, the amplitudes are just products of individual amplitudes ($\Psi_{ij} = u_i v_j$), and knowledge about one qubit tells you nothing about the other. This is the quantum analog of independent marbles.

You will see several equivalent notations:
$$|0\rangle \otimes |1\rangle \;=\; |0\rangle|1\rangle \;=\; |0,1\rangle \;=\; |01\rangle$$

We'll mostly use the compact form $|01\rangle$.

---

### The General Two-Qubit State

The tensor product describes two independent, non-interacting particles. But the most general two-qubit state is richer:

$$|\Psi\rangle = C_{00}|00\rangle + C_{01}|01\rangle + C_{10}|10\rangle + C_{11}|11\rangle$$

with four complex amplitudes satisfying $\sum|C_{ij}|^2 = 1$.

For a tensor product state, the amplitudes factor: $C_{ij} = u_i v_j$. But in general, the four amplitudes can be *anything* (subject to normalization). When they don't factor, the state describes particles whose correlations go beyond anything a product description can capture. These are **entangled states**, and they arise from interactions between particles or from correlated creation processes (like photon pair generation).

### Counting Degrees of Freedom

How much information does a two-qubit state carry?

$$\text{4 complex numbers} \;\to\; 8 \text{ real DOF}$$
$$\text{normalization} \;\to\; -1$$
$$\text{global phase} \;\to\; -1$$

$$\boxed{2 \text{ qubits} = 6 \text{ DOF}}$$

Now compare: two *independent* qubits, each on its own Bloch sphere, have $2 + 2 = 4$ degrees of freedom. Where do the extra 2 come from?

$$\underbrace{6}_{\text{two-qubit state}} \;=\; \underbrace{2}_{\text{Bloch sphere A}} \;+\; \underbrace{2}_{\text{Bloch sphere B}} \;+\; \underbrace{2}_{\text{correlations}}$$

Those extra 2 degrees of freedom encode **correlations between the qubits**. A product state like $|+\rangle \otimes |0\rangle$ uses only the 4 Bloch sphere DOFs — the correlations are zero, and the qubits are independent. But some two-qubit states use all 6 DOFs. These are the entangled states, and they're what we'll explore next.

---

### The Tensor Product as a Matrix and a Vector

For two qubits with states $\vec{u} = \begin{pmatrix} u_0 \\ u_1 \end{pmatrix}$ and $\vec{v} = \begin{pmatrix} v_0 \\ v_1 \end{pmatrix}$, the tensor product defines a set of amplitudes:

$$\Psi_{ij} = u_i \, v_j$$

We can arrange these amplitudes in two equivalent ways.

**As a $2 \times 2$ matrix** (the amplitude table):

$$C = \begin{pmatrix} u_0 v_0 & u_0 v_1 \\ u_1 v_0 & u_1 v_1 \end{pmatrix} = \vec{u}\,\vec{v}^T$$

Rows correspond to qubit A's state ($|0\rangle_A$ or $|1\rangle_A$), columns to qubit B's. For a product state, this matrix is an **outer product** of two vectors: $C = \vec{u}\,\vec{v}^T$.

**As a $4$-component column vector:**

$$|\Psi\rangle = \begin{pmatrix} u_0 v_0 \\ u_0 v_1 \\ u_1 v_0 \\ u_1 v_1 \end{pmatrix} = \begin{pmatrix} c_{00} \\ c_{01} \\ c_{10} \\ c_{11} \end{pmatrix}$$

This is simply the matrix entries read off row by row into a single column. Both representations contain the same information. The vector form generalizes to $N$ qubits (a column of $2^N$ entries), while the matrix form is specific to two qubits — but it will be very useful when we introduce the **determinant test for entanglement** in the next lecture.

For $N$ qubits, the state vector has $2^N$ components. A classical computer simulating 50 qubits must store $2^{50} \approx 10^{15}$ complex numbers — about a petabyte of memory. A quantum computer with 50 qubits just... has 50 qubits. This is the source of quantum computational power: the state space grows exponentially, but the physical resources grow linearly.

---

## Summary

1. **Classical correlations** arise when two systems share a common cause. The joint probability $P(i,j)$ cannot be written as $P_A(i) \cdot P_B(j)$, but there's nothing mysterious — a hidden variable explains it. Classical correlations are perfectly normal; what Bell's theorem will show is that quantum correlations are *stronger* than any hidden variable can produce.

2. **Quantum mechanics is a wave over configurations.** A two-qubit state assigns a complex amplitude to each of the four joint configurations $|00\rangle, |01\rangle, |10\rangle, |11\rangle$. For $N$ qubits: $2^N$ amplitudes.

3. **The tensor product** $\otimes$ describes two independent qubits: amplitudes multiply ($\Psi_{ij} = u_i v_j$), and the result can be written as a $2 \times 2$ matrix (outer product $\vec{u}\,\vec{v}^T$) or a 4-component vector.

4. **A general two-qubit state** has **6 real DOF** = 2 (Bloch sphere A) + 2 (Bloch sphere B) + 2 (correlations). The extra DOFs encode correlations — and when the amplitudes don't factor, we call the state **entangled**.

<!-- ---

## Homework

### Problem 1: Joint Probability Distributions

Two classical bits are drawn from a joint distribution.

**(a)** If $P(0,0) = 1/3$, $P(0,1) = 1/6$, $P(1,0) = 1/6$, $P(1,1) = 1/3$, compute the marginals $P_A(0), P_A(1), P_B(0), P_B(1)$.

**(b)** Is this distribution independent (i.e., does $P(i,j) = P_A(i) \cdot P_B(j)$)?

**(c)** Design a joint distribution over $(i,j) \in \{0,1\}^2$ that has marginals $P_A(0) = P_A(1) = 1/2$ and $P_B(0) = P_B(1) = 1/2$ but is **not** independent.

**(d)** Design one that has the same marginals and **is** independent.

---

### Problem 2: Tensor Product Practice

Compute the following tensor products and write the result as a 4-component column vector.

**(a)** $|0\rangle \otimes |1\rangle$

**(b)** $|1\rangle \otimes |+\rangle$ where $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$

**(c)** $|+\rangle \otimes |-\rangle$ where $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$

**(d)** $|+\rangle \otimes |+\rangle$

---

### Problem 3: The Amplitude Table

For each tensor product in Problem 2, write the result as a $2 \times 2$ amplitude table $C$ (rows = qubit A, columns = qubit B). Verify that each table is an outer product $C = \vec{u}\,\vec{v}^T$ — i.e., that $C_{ij} = u_i v_j$.

---

### Problem 4: Degrees of Freedom

**(a)** How many real degrees of freedom does a general $N$-qubit state have? (Remember to subtract normalization and global phase.)

**(b)** How many DOF do $N$ independent qubits have (each on its own Bloch sphere)?

**(c)** For $N = 3$: how many DOF does a general 3-qubit state have? How many for three independent Bloch spheres? How many "correlation DOFs" are there?

**(d)** As $N$ grows, which grows faster — the total DOFs or the independent-qubit DOFs? What does this say about the importance of correlations in large quantum systems?

---

### Problem 5: Product or Not?

For each two-qubit state, try to write it as $|\psi\rangle_A \otimes |\phi\rangle_B$. If you can, give the factors. If you can't, explain why.

**(a)** $|\Psi\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |10\rangle)$

**(b)** $|\Psi\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$

**(c)** $|\Psi\rangle = \frac{1}{2}(|00\rangle + |01\rangle + |10\rangle + |11\rangle)$

**(d)** $|\Psi\rangle = \frac{1}{2}(|00\rangle + |01\rangle + |10\rangle - |11\rangle)$

---

### Problem 6: Three Qubits

**(a)** How many computational basis states are there for three qubits? List them all.

**(b)** Write $|+\rangle \otimes |0\rangle \otimes |1\rangle$ as a sum of computational basis states.

**(c)** The GHZ state is $|GHZ\rangle = \frac{1}{\sqrt{2}}(|000\rangle + |111\rangle)$. Is it a product state? Try to factor it.

**(d)** If you measure all three qubits of $|GHZ\rangle$ in the computational basis, what outcomes are possible?

---

## Looking Ahead

We've built the mathematical framework for two-qubit states: tensor products, the amplitude table, and the $6 = 4 + 2$ DOF counting that reveals where correlations live.

But we haven't yet explored what makes quantum correlations *different* from classical ones. Next lecture, we'll introduce the Bell states — the four maximally entangled two-qubit states — and discover that their correlations persist across *every* measurement basis simultaneously. This is something no classical system can do, and it leads directly to Einstein's challenge, Bell's theorem, and the foundations of quantum information science. -->