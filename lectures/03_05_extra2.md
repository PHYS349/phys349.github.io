# Lecture 3.5 — The Circuit Model

## From Physics to Computation

We've spent four lectures on what quantum mechanics *says*: entanglement, Bell states, correlations, the EPR argument, Bell's theorem. We've worked entirely in the language of states, operators, and tensor products.

Now we shift gears. The question is no longer "what does quantum mechanics predict?" but **"how do we build and manipulate quantum states?"**

The answer is the **circuit model** — a way to describe quantum computation as a sequence of operations (called **gates**) acting on quantum bits (called **wires**). The circuit model is to quantum computing what Boolean logic circuits are to classical computing: a universal framework for expressing any computation.

### Conventions

A quantum circuit is read **left to right**:

```
q0: ──────────────────
q1: ──────────────────
```

Each horizontal line is a **wire** representing one qubit. Every qubit starts in the state $|0\rangle$ unless otherwise specified. Gates appear as labeled boxes or symbols on the wires, and time flows from left to right.

---

## Part 1: Single-Qubit Gates

You've already seen the Pauli matrices and the Hadamard as operators acting on qubit states. Now we draw them as **circuit elements** — boxes on wires.

### The $X$ Gate (NOT / Bit Flip)

$$X = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$$

Action: $X|0\rangle = |1\rangle$, $X|1\rangle = |0\rangle$. It flips a qubit, just like a classical NOT gate.

```
     ┌───┐
q0: ─┤ X ├─
     └───┘
```

### The $Z$ Gate (Phase Flip)

$$Z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}$$

Action: $Z|0\rangle = |0\rangle$, $Z|1\rangle = -|1\rangle$. It leaves $|0\rangle$ alone and multiplies $|1\rangle$ by $-1$. This has no classical analog — it changes the **phase** of the state without changing the measurement probabilities in the Z-basis.

```
     ┌───┐
q0: ─┤ Z ├─
     └───┘
```

### The Hadamard Gate $H$

$$H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$$

Action:
$$H|0\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) = |+\rangle, \qquad H|1\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle) = |-\rangle.$$

One gate creates superposition. The Hadamard is the workhorse of quantum computing — nearly every quantum algorithm starts with Hadamard gates.

```
     ┌───┐
q0: ─┤ H ├─
     └───┘
```

Notice that $H$ is its own inverse: $H^2 = I$. Applying Hadamard twice returns you to the original state.

### The $R_y(\theta)$ Gate (Rotation)

$$R_y(\theta) = \begin{pmatrix} \cos(\theta/2) & -\sin(\theta/2) \\ \sin(\theta/2) & \cos(\theta/2) \end{pmatrix}$$

This rotates the qubit state by angle $\theta$ about the $y$-axis on the Bloch sphere. It generalizes the relationship between measurement bases: $R_y(\pi/2)$ maps $|0\rangle \to |+\rangle$ (like $H$ up to a phase), while $R_y(\theta)$ for arbitrary $\theta$ lets us reach any axis in the $xz$-plane.

We'll need $R_y$ for Bell tests with measurement axes that aren't exactly along $X$ or $Z$.

### Summary: Gate $\leftrightarrow$ Matrix $\leftrightarrow$ Circuit Symbol

| Gate | Matrix | Action on $|0\rangle$ | Action on $|1\rangle$ |
|---|---|---|---|
| $X$ | $\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$ | $|1\rangle$ | $|0\rangle$ |
| $Z$ | $\begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}$ | $|0\rangle$ | $-|1\rangle$ |
| $H$ | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$ | $|+\rangle$ | $|-\rangle$ |

Each of these is a $2 \times 2$ unitary matrix. In the circuit model, any single-qubit gate is a $2 \times 2$ unitary drawn as a box on one wire.

---

## Part 2: The CNOT Gate

### The First Two-Qubit Gate

Everything so far has been single-qubit operations — manipulating one qubit at a time. But the whole point of quantum computing is **entanglement**, and for that we need gates that act on two qubits simultaneously.

The most important two-qubit gate is the **CNOT** (controlled-NOT):

```
q0: ──■──
      │
q1: ──⊕──
```

The filled dot ($\bullet$) marks the **control** qubit, and the circled plus ($\oplus$) marks the **target** qubit.

### Truth Table

CNOT flips the target if and only if the control is $|1\rangle$:

| Input | Output |
|---|---|
| $|00\rangle$ | $|00\rangle$ |
| $|01\rangle$ | $|01\rangle$ |
| $|10\rangle$ | $|11\rangle$ |
| $|11\rangle$ | $|10\rangle$ |

In words: the control qubit is never changed, and the target qubit is XOR'd with the control.

### Matrix Representation

In the computational basis $\{|00\rangle, |01\rangle, |10\rangle, |11\rangle\}$:

$$\text{CNOT} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{pmatrix}.$$

This is a $4 \times 4$ unitary matrix acting on the two-qubit Hilbert space $\mathbb{C}^2 \otimes \mathbb{C}^2$.

### CNOT Is Classical on Basis States

When both inputs are computational basis states, CNOT is just a reversible classical logic gate (a controlled-NOT, familiar from classical computing). There is nothing quantum about it in this regime.

**The magic happens when the control is in superposition.** We'll see this in the next section.

---

## Part 3: Building Bell States

### The H + CNOT Circuit

Here is the most important two-gate circuit in all of quantum computing:

```
     ┌───┐
q0: ─┤ H ├──■──
     └───┘  │
q1: ────────⊕──
```

Hadamard on qubit 0, then CNOT with qubit 0 as control and qubit 1 as target. Let's trace the state step by step.

**Initial state:**

$$|00\rangle$$

**After $H$ on qubit 0:**

$$|00\rangle \xrightarrow{H \otimes I} \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) \otimes |0\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |10\rangle).$$

At this point, qubit 0 is in superposition but the two qubits are still in a **product state** — no entanglement yet. You can check: $\alpha\delta - \beta\gamma = \frac{1}{\sqrt{2}} \cdot 0 - 0 \cdot \frac{1}{\sqrt{2}} = 0$, so the determinant test (from Lecture 3.2) says this is separable.

**After CNOT:**

CNOT acts on each branch of the superposition:

$$\text{CNOT}|00\rangle = |00\rangle, \qquad \text{CNOT}|10\rangle = |11\rangle.$$

Therefore:

$$\frac{1}{\sqrt{2}}(|00\rangle + |10\rangle) \xrightarrow{\text{CNOT}} \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle) = |\Phi^+\rangle.$$

This is a **Bell state**. The qubits are now entangled. Check the determinant: $\alpha = \delta = 1/\sqrt{2}$, $\beta = \gamma = 0$, so $\alpha\delta - \beta\gamma = 1/2 \neq 0$. Entangled. ✓

```{admonition} Superposition + CNOT = Entanglement
:class: important

When the control qubit is in a definite state ($|0\rangle$ or $|1\rangle$), CNOT acts classically — it either flips or doesn't flip. No entanglement is created.

When the control qubit is in **superposition**, CNOT correlates the two qubits: the $|0\rangle$ branch leaves the target alone, while the $|1\rangle$ branch flips it. The result is a state where the two qubits cannot be described independently.

This is the fundamental mechanism by which quantum circuits create entanglement.
```

### All Four Bell States

By choosing different inputs or adding single-qubit gates, we can build all four Bell states from the same H + CNOT structure:

| Initial state | Circuit | Output |
|---|---|---|
| $|00\rangle$ | $H$, CNOT | $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$ |
| $|10\rangle$ | $H$, CNOT | $|\Phi^-\rangle = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle)$ |
| $|01\rangle$ | $H$, CNOT | $|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle)$ |
| $|11\rangle$ | $H$, CNOT | $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$ |

To start in $|10\rangle$ instead of $|00\rangle$, apply $X$ on qubit 0 before the Hadamard. To start in $|01\rangle$, apply $X$ on qubit 1. To start in $|11\rangle$, apply $X$ on both.

The **singlet state** $|\Psi^-\rangle$ — the state we've been using for Bell's theorem — is just:

```
     ┌───┐┌───┐
q0: ─┤ X ├┤ H ├──■──
     └───┘└───┘  │
q1: ─┤ X ├───────⊕──
     └───┘
```

Or equivalently, we can build it as $H$, CNOT, then $Z$ on qubit 0 and $X$ on qubit 1:

```
     ┌───┐     ┌───┐
q0: ─┤ H ├──■──┤ Z ├─
     └───┘  │  └───┘
q1: ────────⊕──┤ X ├─
               └───┘
```

Both give $|\Psi^-\rangle$. (Verify this in the homework.)

We just **built** the state we've been studying for four lectures.

---

## Part 4: Measurement in Circuits

### The Measurement Symbol

Measurement is drawn as a meter icon on a wire:

```
     ┌─┐
q0: ─┤M├─
     └─┘
```

A measurement collapses the qubit to $|0\rangle$ or $|1\rangle$ and records the classical outcome (0 or 1) in a classical bit register.

In the circuit model (and on real quantum hardware), measurement is always in the **computational basis** (the Z-basis). This might seem restrictive — after all, Bell tests require measurements in X, or along axes at $45°$. But it's not a limitation at all.

### Measuring in Other Bases: The Pre-Rotation Trick

To measure along an axis $\hat{n}$ other than Z, we apply a **rotation** that maps $\hat{n}$ to $\hat{z}$, and then measure in Z.

Why does this work? Measuring the observable $\sigma_n$ and getting eigenvalue $\pm 1$ is mathematically identical to first rotating so that $\hat{n}$ aligns with $\hat{z}$, and then measuring $\sigma_z$. The rotation doesn't change the physics — it just changes which basis the Z-measurement projects onto.

**X-basis measurement:**

Apply $H$ before measuring. Since $H|+\rangle = |0\rangle$ and $H|-\rangle = |1\rangle$, a Z-measurement after $H$ distinguishes $|+\rangle$ from $|-\rangle$ — exactly what an X-measurement does.

```
     ┌───┐┌─┐
q0: ─┤ H ├┤M├─    ← This measures in the X-basis
     └───┘└─┘
```

**Measurement along $Q$ (at $45°$ between Z and X):**

Apply $R_y(-\pi/4)$ before measuring. This rotates the $Q$-axis to align with Z.

```
     ┌──────────┐┌─┐
q0: ─┤ Ry(-π/4) ├┤M├─    ← This measures in the Q-basis
     └──────────┘└─┘
```

### The General Rule

| Desired measurement basis | Gate before measurement |
|---|---|
| Z | None |
| X | $H$ |
| $Q_+$ (at $+45°$ from Z) | $R_y(-\pi/4)$ |
| $Q_-$ (at $-45°$ from Z) | $R_y(+\pi/4)$ |
| General axis at angle $\theta$ from Z | $R_y(-\theta)$ |

The minus sign in $R_y(-\theta)$ is because we're **undoing** the tilt: rotating the measurement axis back to Z.

### Connection to the Correlation Table

In Lecture 3.3, we computed the correlation table for the singlet state with measurements in various bases. Now we can see how each entry corresponds to a specific circuit:

- **$E(Z,Z) = -1$:** Prepare singlet, measure both qubits. No pre-rotation needed.

- **$E(X,X) = -1$:** Prepare singlet, apply $H$ to both qubits, then measure.

- **$E(Z,X) = 0$:** Prepare singlet, apply $H$ to Bob's qubit only, then measure.

The correlation table from Lecture 3.3 is a table of circuits.

---

## Part 5: Putting It Together — A Bell Test Circuit

### The Experiment as a Circuit

We now have everything we need to implement a Bell test as a quantum circuit. The procedure is:

1. **Prepare** the singlet state (H + CNOT + Z + X).
2. **Rotate** each qubit into the desired measurement basis.
3. **Measure** both qubits in Z.
4. **Compute** the correlation $E$ from the measurement statistics.

Here's what the circuit looks like when Alice measures Z and Bob measures in the $Q_+$ basis ($45°$ from Z):

```
     ┌───┐     ┌───┐                ┌─┐
q0: ─┤ H ├──■──┤ Z ├────────────────┤M├──  Alice: Z-basis (no rotation)
     └───┘  │  └───┘                └─┘
q1: ────────⊕──┤ X ├──┤ Ry(-π/4) ├──┤M├──  Bob: Q₊-basis
               └───┘  └──────────┘  └─┘
```

### From Bitstrings to Correlations

Each run of the circuit produces a two-bit outcome: `00`, `01`, `10`, or `11`. We convert bits to $\pm 1$ eigenvalues:

$$0 \to +1, \qquad 1 \to -1.$$

The correlation function is:

$$E = \langle A \cdot B \rangle = P(\texttt{00}) + P(\texttt{11}) - P(\texttt{01}) - P(\texttt{10}).$$

The logic: outcomes `00` and `11` have the product $a \cdot b = +1$ (both same sign), while `01` and `10` have $a \cdot b = -1$.

### Example: ZZ Measurement of the Singlet

The singlet $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$ has amplitudes only on $|01\rangle$ and $|10\rangle$. So the measurement outcomes are `01` and `10` with equal probability, and never `00` or `11`:

$$E(Z,Z) = 0 + 0 - 0.5 - 0.5 = -1.$$

Perfect anti-correlation. ✓

### Example: XX Measurement of the Singlet

Apply $H \otimes H$ before measuring. From Lecture 3.3, we know the singlet is anti-correlated in every basis, so we expect only outcomes corresponding to opposite X-eigenvalues. The circuit gives `01` and `10` (in the rotated frame) with equal probability:

$$E(X,X) = -1.$$

✓

### The Full Bell Test

For the CHSH version of Bell's test (which we'll analyze next lecture), we need four circuits with these measurement settings:

| Circuit | Alice | Bob | Angle between axes |
|---|---|---|---|
| 1 | Z ($0°$) | $Q_+$ ($+45°$) | $45°$ |
| 2 | Z ($0°$) | $Q_-$ ($-45°$) | $45°$ |
| 3 | X ($90°$) | $Q_+$ ($+45°$) | $45°$ |
| 4 | X ($90°$) | $Q_-$ ($-45°$) | $135°$ |

Using $E(\hat{a}, \hat{b}) = -\cos\theta$:

| Circuit | $\theta$ | $E = -\cos\theta$ |
|---|---|---|
| 1 | $45°$ | $-1/\sqrt{2} \approx -0.707$ |
| 2 | $45°$ | $-1/\sqrt{2} \approx -0.707$ |
| 3 | $45°$ | $-1/\sqrt{2} \approx -0.707$ |
| 4 | $135°$ | $+1/\sqrt{2} \approx +0.707$ |

The CHSH combination $S = E_1 + E_2 + E_3 - E_4 = -2\sqrt{2} \approx -2.83$ violates the classical bound $|S| \leq 2$. We'll prove this bound next lecture. In the homework, you'll build these four circuits in Qiskit and verify the violation numerically.

```{admonition} Aspect's Experiment, as a Circuit
:class: note

Aspect's 1982 Bell test did exactly this — with photons instead of qubits, polarizers instead of $R_y$ gates, and coincidence counters instead of classical bit registers. The circuit model is a direct map of the physical experiment.
```

---

## Part 6: The Circuit Model in General

### What We've Established

The circuit model provides a systematic recipe for quantum computation:

1. **Initialize** qubits in $|0\rangle$.
2. **Apply gates** (unitary operations) — single-qubit gates like $H$, $X$, $Z$, $R_y(\theta)$ and two-qubit gates like CNOT.
3. **Measure** in the computational basis, possibly after pre-rotations.

Any quantum computation can be expressed in this framework. The circuit model is **universal**: any unitary operation on $n$ qubits can be decomposed into a sequence of single-qubit gates and CNOTs.

### Gates We'll Use Going Forward

As we move into quantum algorithms, we'll encounter more gates. Here's a preview of the most important ones:

**Single-qubit gates we've seen:**

| Gate | Matrix | Key property |
|---|---|---|
| $X$ | $\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$ | Bit flip |
| $Z$ | $\begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}$ | Phase flip |
| $H$ | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$ | Creates superposition |
| $R_y(\theta)$ | $\begin{pmatrix} \cos\theta/2 & -\sin\theta/2 \\ \sin\theta/2 & \cos\theta/2 \end{pmatrix}$ | Rotation about Y-axis |

**Two-qubit gates:**

| Gate | Circuit symbol | Key property |
|---|---|---|
| CNOT | $\bullet$—$\oplus$ | Entangling gate, flips target if control is $|1\rangle$ |

**Gates we'll introduce later** (when we need them for algorithms):

| Gate | What it does |
|---|---|
| $S$ | Phase gate: $|1\rangle \to i|1\rangle$ (quarter turn) |
| $T$ | $\pi/8$ gate: $|1\rangle \to e^{i\pi/4}|1\rangle$ |
| Toffoli (CCNOT) | Three-qubit gate: flips target if both controls are $|1\rangle$ |

We'll introduce $S$, $T$, and Toffoli when we encounter algorithms that use them.

---

## Summary

1. **The circuit model** represents quantum computation as gates on wires, read left to right. Every qubit starts in $|0\rangle$.

2. **Single-qubit gates** are $2 \times 2$ unitary matrices drawn as boxes on one wire: $X$ (bit flip), $Z$ (phase flip), $H$ (superposition), $R_y(\theta)$ (rotation).

3. **CNOT** is the fundamental two-qubit gate. It acts classically on basis states. The quantum magic is that CNOT on a superposed control creates **entanglement**.

4. **Bell states** are built by $H$ + CNOT, the simplest entangling circuit. Different inputs or follow-up gates give all four Bell states. Four gates build the singlet.

5. **Measurement** in the circuit model is always in Z. To measure in another basis, apply a rotation gate first: $H$ for X-basis, $R_y(-\theta)$ for an axis at angle $\theta$ from Z.

6. **A Bell test is a circuit:** prepare singlet → rotate into measurement bases → measure → compute correlations from bitstring statistics.

---

## Homework

### Problem 1: Single-Qubit Gate Identities

**(a)** Verify that $H^2 = I$ by matrix multiplication.

**(b)** Show that $HXH = Z$ and $HZH = X$. What does this mean? *(Hint: conjugating by H swaps the X and Z bases.)*

**(c)** Show that $R_y(\pi/2)$ maps $|0\rangle \to \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$. Compare this to $H|0\rangle$. Are they the same state?

---

### Problem 2: CNOT on Various Inputs

**(a)** Verify the CNOT truth table by applying the $4 \times 4$ matrix to each basis state $|00\rangle$, $|01\rangle$, $|10\rangle$, $|11\rangle$.

**(b)** Compute CNOT$(|+\rangle \otimes |0\rangle)$ by expanding $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ and applying CNOT to each term. Verify you get $|\Phi^+\rangle$.

**(c)** Compute CNOT$(|{-}\rangle \otimes |0\rangle)$. What state do you get?

**(d)** Compute CNOT$(|0\rangle \otimes |+\rangle)$. Is the result entangled? *(Hint: try the determinant test.)*

---

### Problem 3: Building All Four Bell States

**(a)** Starting from $|00\rangle$, trace through the circuit $H$ (qubit 0) → CNOT to verify you get $|\Phi^+\rangle$.

**(b)** Starting from $|10\rangle$ (i.e., apply $X$ on qubit 0, then $H$ on qubit 0, then CNOT), trace through to get $|\Phi^-\rangle$.

**(c)** Repeat for $|01\rangle \to |\Psi^+\rangle$ and $|11\rangle \to |\Psi^-\rangle$.

**(d)** Verify that the alternative singlet circuit ($H$, CNOT, $Z$ on qubit 0, $X$ on qubit 1) also gives $|\Psi^-\rangle$ starting from $|00\rangle$.

---

### Problem 4: Measurement Basis Rotations

**(a)** Show that measuring $H|0\rangle$ in the Z-basis gives outcomes 0 and 1 with equal probability. Then show that applying $H$ first (i.e., $H \cdot H|0\rangle = |0\rangle$) and measuring in Z gives outcome 0 with certainty. This confirms that $H$-then-measure is an X-basis measurement.

**(b)** The state $|0\rangle$ has $\langle \sigma_x \rangle = 0$, meaning X-basis measurement gives 50/50 outcomes. Verify this by computing the probabilities of getting $|+\rangle$ and $|-\rangle$ directly (expand $|0\rangle$ in the X-eigenbasis). Then verify the same result using the circuit: apply $H$, measure in Z, and check you get 50/50.

**(c)** What gate before measurement implements a Y-basis measurement? *(Hint: you need a rotation that maps $|+i\rangle$ and $|-i\rangle$ to $|0\rangle$ and $|1\rangle$.)*

---

### Problem 5: Bell Test Circuits in Qiskit

This problem uses Qiskit. Install the packages listed below if you haven't already.

```python
# pip install qiskit qiskit-aer numpy pylatexenc
```

**(a)** Write a Qiskit circuit that prepares the singlet state $|\Psi^-\rangle$. Use `Statevector.from_instruction(qc)` (from `qiskit.quantum_info`) to verify that the state vector is $[0, \; 1/\sqrt{2}, \; -1/\sqrt{2}, \; 0]$. Draw the circuit using `qc.draw("mpl")`.

**(b)** Add Z-basis measurements on both qubits. Run 10,000 shots with `StatevectorSampler` (from `qiskit.primitives`) and verify that only outcomes `01` and `10` appear (perfect anti-correlation). Compute $E(Z,Z)$ from the counts.

**(c)** Modify the circuit to measure in the X-basis (apply H to both qubits before measurement). Run 10,000 shots and verify $E(X,X) = -1$.

**(d)** Measure Alice in Z and Bob in X. Verify $E(Z,X) \approx 0$.

```{admonition} Qiskit Bit Ordering
:class: warning

Qiskit uses **little-endian** ordering: in a bitstring like `"01"`, the rightmost character is qubit 0 and the leftmost is qubit 1. So `bitstring[0]` is qubit 1 (Bob) and `bitstring[1]` is qubit 0 (Alice). Getting this backwards will flip the sign of your correlations.
```

---

### Problem 6: CHSH Violation in Qiskit

**(a)** Write a function `compute_E(counts, shots)` that takes a dictionary of bitstring counts and returns the correlation $E = P(\texttt{00}) + P(\texttt{11}) - P(\texttt{01}) - P(\texttt{10})$.

**(b)** Build four circuits for the CHSH measurement configurations: Alice $\in \{Z, X\}$, Bob $\in \{Q_+, Q_-\}$, where $Q_{\pm}$ are the axes at $\pm 45°$ from Z (use $R_y(\mp\pi/4)$ as the pre-rotation). Run each with 10,000 shots and report the four $E$ values.

**(c)** Compute $S = E(Z, Q_+) + E(Z, Q_-) + E(X, Q_+) - E(X, Q_-)$. Verify that $|S| \approx 2\sqrt{2} \approx 2.83$, violating the classical bound $|S| \leq 2$.

**(d)** Sweep Bob's measurement angle $\alpha$ from $0$ to $2\pi$. For each $\alpha$, measure $E(Z, \alpha)$ and plot it against $\alpha$. Overlay the theoretical curve $E = -\cos\alpha$. Do they match?

---

### Problem 7: Shot Noise (Optional)

**(a)** Run the CHSH test from Problem 6 with $N = 100$, $1{,}000$, and $10{,}000$ shots. For each, report $|S|$. How does the statistical uncertainty scale with $N$?

**(b)** Repeat the $N = 100$ experiment 100 times. Plot a histogram of the measured $|S|$ values. What fraction of trials give $|S| > 2$?

**(c)** Experimentalists report the number of standard deviations by which $|S|$ exceeds 2. Estimate this for $N = 10{,}000$.

---

## Looking Ahead

We now have all the tools to do quantum computing: states, gates, circuits, and measurement. The circuit model is the language we'll use for the rest of the course.

Next lecture: we **prove** the CHSH inequality $|S| \leq 2$ for local hidden variables, derive the quantum maximum $|S| = 2\sqrt{2}$ (the Tsirelson bound), and understand why $2\sqrt{2}$ and not $4$ — even quantum mechanics has limits on how strong correlations can be.

After that, we move to **quantum algorithms**: Deutsch's algorithm, then Grover's search, then the quantum Fourier transform and Shor's factoring. All of them are circuits built from the gates we learned today.