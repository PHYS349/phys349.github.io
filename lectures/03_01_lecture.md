# Lecture 3.1: Two Qubits and the Tensor Product

## The Turning Point

Everything changes when we have more than one qubit.

In Part 2, we mastered the single qubit: a wave over two configurations, complex amplitudes, rotations on the Bloch sphere, interference, measurement. Beautiful physics, and the foundation of quantum sensing.

But a single qubit is not enough for quantum computing. With one qubit, you have 2 configurations. With two qubits, you have 4. With three, you have 8.

With $N$ qubits, you have $2^N$ configurations.

This exponential scaling is where quantum computing gets its power — and where things become truly strange. A system of just 50 qubits has more configurations than there are atoms in the Earth. No classical computer can even write down the full quantum state.

Today we learn how to describe multi-qubit systems, and we'll meet **entanglement** — correlations that have no classical explanation.

---

## Where Do Two-Qubit Systems Come From?

Before diving into mathematics, let's ground this in physics. Where do we actually find systems with two qubits?

**Two electron spins:** Take two electrons (perhaps in a molecule, or in two nearby atoms). Each electron has spin-1/2, giving two qubits. The spins can interact through exchange coupling or dipole-dipole interactions.

**Two polarized photons:** In a process called *spontaneous parametric down-conversion*, a single high-energy photon enters a special crystal and splits into two lower-energy photons. The polarizations of these "daughter" photons are entangled — this is how most Bell test experiments are done.

**Two trapped ions:** In a linear ion trap (like those used by IonQ and Quantinuum), individual ions are held in a line by electric fields. Each ion's internal electronic states form a qubit. The ions interact through their shared motion — they're connected by "phonon springs."

**Two superconducting circuits:** On a chip cooled to 15 millikelvin, artificial atoms made of superconducting circuits sit next to each other. They interact through capacitive coupling or shared microwave resonators.

In each case, the key is:
1. Two systems, each with two states (qubits)
2. The ability to create correlations between them (entanglement)

Now let's build the mathematics.

---

## Combining Systems: The Classical Warm-Up

Before quantum mechanics, let's think classically.

### One Bit

A single classical bit has two possible values:
$$
\{0, 1\}
$$

### Two Bits

Two classical bits have four possible values — all possible pairs:
$$
\{00, 01, 10, 11\}
$$

The first digit tells you the state of bit A, the second tells you bit B.

### The Pattern

When you combine systems, you take all possible combinations. If system A has $n_A$ configurations and system B has $n_B$ configurations, the combined system has $n_A \times n_B$ configurations.

For bits:
- 1 bit: $2$ configurations
- 2 bits: $2 \times 2 = 4$ configurations
- 3 bits: $2 \times 2 \times 2 = 8$ configurations
- $N$ bits: $2^N$ configurations

This isn't quantum mechanics yet — it's just combinatorics. But quantum mechanics inherits this structure.

---

## Two Qubits: The Configuration Space

Now apply our mantra from Part 2:

> A quantum state is a list of configurations, each with a complex amplitude.

For one qubit, the configurations are $|0\rangle$ and $|1\rangle$. The state is:
$$
|\psi\rangle = \alpha|0\rangle + \beta|1\rangle
$$

For two qubits, the configurations are all pairs:
$$
|00\rangle, \quad |01\rangle, \quad |10\rangle, \quad |11\rangle
$$

A general two-qubit state is a wave over these four configurations:
$$
|\Psi\rangle = c_{00}|00\rangle + c_{01}|01\rangle + c_{10}|10\rangle + c_{11}|11\rangle
$$

with normalization:
$$
|c_{00}|^2 + |c_{01}|^2 + |c_{10}|^2 + |c_{11}|^2 = 1
$$

The notation $|ab\rangle$ means: "qubit 1 is in state $a$, qubit 2 is in state $b$."

```{figure} ./03_01_lecture_files/configuration_tree.svg
:width: 400px
:name: config-tree

Building two-qubit configurations: every choice for qubit A pairs with every choice for qubit B.
```

```{admonition} The Core Idea
:class: important

A two-qubit state assigns a complex amplitude to each of the four joint configurations.

The probabilities of measurement outcomes are $|c_{ab}|^2$.
```

---

## The Tensor Product: How to Combine Quantum Systems

We need a mathematical operation that builds the joint configuration space from the individual spaces. This is the **tensor product**, written $\otimes$.

### Example First: Combining Two Superpositions

Let's start with a concrete example. Suppose qubit A is in state:
$$
|\psi\rangle_A = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) = |+\rangle
$$

And qubit B is in state:
$$
|\phi\rangle_B = |0\rangle
$$

What's the combined state? We need to pair every configuration of A with every configuration of B:

- A in $|0\rangle$, B in $|0\rangle$ → amplitude = $\frac{1}{\sqrt{2}} \times 1 = \frac{1}{\sqrt{2}}$
- A in $|0\rangle$, B in $|1\rangle$ → amplitude = $\frac{1}{\sqrt{2}} \times 0 = 0$
- A in $|1\rangle$, B in $|0\rangle$ → amplitude = $\frac{1}{\sqrt{2}} \times 1 = \frac{1}{\sqrt{2}}$
- A in $|1\rangle$, B in $|1\rangle$ → amplitude = $\frac{1}{\sqrt{2}} \times 0 = 0$

So the combined state is:
$$
\frac{1}{\sqrt{2}}|00\rangle + 0|01\rangle + \frac{1}{\sqrt{2}}|10\rangle + 0|11\rangle = \frac{|00\rangle + |10\rangle}{\sqrt{2}}
$$

Notice the pattern: **amplitudes multiply** when we combine configurations.

### The General Rule: Definition on Basis States

The tensor product is the operation that formalizes this:
$$
|a\rangle \otimes |b\rangle \equiv |ab\rangle
$$

So:
$$
|0\rangle \otimes |0\rangle = |00\rangle
$$
$$
|0\rangle \otimes |1\rangle = |01\rangle
$$
$$
|1\rangle \otimes |0\rangle = |10\rangle
$$
$$
|1\rangle \otimes |1\rangle = |11\rangle
$$

The tensor product is just a formal way of saying "pair up all configurations."

### Linearity: Distribute Like Algebra

The tensor product is **bilinear** — it distributes over addition like ordinary multiplication.

For general states $|\psi\rangle_A = \alpha|0\rangle + \beta|1\rangle$ and $|\phi\rangle_B = \gamma|0\rangle + \delta|1\rangle$:

$$
|\psi\rangle_A \otimes |\phi\rangle_B = (\alpha|0\rangle + \beta|1\rangle) \otimes (\gamma|0\rangle + \delta|1\rangle)
$$

Distribute just like $(a + b)(c + d) = ac + ad + bc + bd$:
$$
= \alpha\gamma|00\rangle + \alpha\delta|01\rangle + \beta\gamma|10\rangle + \beta\delta|11\rangle
$$

```{admonition} The Tensor Product Rule
:class: tip

When you combine independent quantum systems:
- Every configuration of A pairs with every configuration of B
- Amplitudes multiply

This is the operational meaning of $\otimes$.
```

### Dimensions Multiply

One qubit lives in a 2-dimensional complex vector space (spanned by $|0\rangle$ and $|1\rangle$).

Two qubits live in a $2 \times 2 = 4$ dimensional space.

Three qubits: $2 \times 2 \times 2 = 8$ dimensions.

In general:
$$
N \text{ qubits} \Rightarrow 2^N \text{ dimensional Hilbert space}
$$

This exponential growth is the mathematical source of quantum computing's power — and the reason classical simulation becomes impossible for large systems.

| Qubits | Configurations | Complex numbers needed |
|--------|----------------|----------------------|
| 1 | 2 | 2 |
| 2 | 4 | 4 |
| 10 | 1,024 | 1,024 |
| 20 | ~1 million | ~1 million |
| 50 | ~$10^{15}$ | ~$10^{15}$ |
| 300 | ~$10^{90}$ | More than atoms in universe |

```{admonition} Why This Matters for Quantum Computing
:class: important

A classical computer simulating 50 qubits must store $2^{50} \approx 10^{15}$ complex numbers — about a petabyte of memory. Simulating 100 qubits is impossible classically.

A quantum computer with 50 qubits just... has 50 qubits. It represents the full $2^{50}$-dimensional state naturally.

This is the source of quantum computational power: the state space grows exponentially, but the physical resources (number of qubits) grow only linearly.

The catch: we can only extract classical information through measurement. Quantum algorithms must cleverly arrange for useful information to be accessible.
```

---

## Two-Qubit States as Vectors

Just like single qubits, we can represent two-qubit states as column vectors.

### The Computational Basis

We order the basis states as:
$$
|00\rangle, \quad |01\rangle, \quad |10\rangle, \quad |11\rangle
$$

(This is "lexicographic" order, like counting in binary: 0, 1, 2, 3.)

Each basis state becomes a 4-component column:
$$
|00\rangle = \begin{pmatrix} 1 \\ 0 \\ 0 \\ 0 \end{pmatrix}, \quad
|01\rangle = \begin{pmatrix} 0 \\ 1 \\ 0 \\ 0 \end{pmatrix}, \quad
|10\rangle = \begin{pmatrix} 0 \\ 0 \\ 1 \\ 0 \end{pmatrix}, \quad
|11\rangle = \begin{pmatrix} 0 \\ 0 \\ 0 \\ 1 \end{pmatrix}
$$

A general state:
$$
|\Psi\rangle = c_{00}|00\rangle + c_{01}|01\rangle + c_{10}|10\rangle + c_{11}|11\rangle = \begin{pmatrix} c_{00} \\ c_{01} \\ c_{10} \\ c_{11} \end{pmatrix}
$$

### The Tensor Product of Vectors

There's also a concrete formula for the tensor product of two column vectors. If:
$$
|\psi\rangle = \begin{pmatrix} \alpha \\ \beta \end{pmatrix}, \qquad |\phi\rangle = \begin{pmatrix} \gamma \\ \delta \end{pmatrix}
$$

Then:
$$
|\psi\rangle \otimes |\phi\rangle = \begin{pmatrix} \alpha \\ \beta \end{pmatrix} \otimes \begin{pmatrix} \gamma \\ \delta \end{pmatrix} = \begin{pmatrix} \alpha\gamma \\ \alpha\delta \\ \beta\gamma \\ \beta\delta \end{pmatrix}
$$

The rule: multiply each component of the first vector by the entire second vector, and stack the results.

You can verify this matches our "distribute like algebra" calculation from before.

---

## Product States vs Entangled States

Now we reach the key conceptual divide.

### Product (Separable) States

A two-qubit state is called a **product state** (or **separable**) if it can be written as:
$$
|\Psi\rangle = |\psi\rangle_A \otimes |\phi\rangle_B
$$

for some single-qubit states $|\psi\rangle_A$ and $|\phi\rangle_B$.

In a product state, each qubit has its own independent quantum state. They're not correlated in any special way.

**Example 1:** Both qubits in $|0\rangle$
$$
|00\rangle = |0\rangle \otimes |0\rangle \quad \checkmark \text{ product}
$$

**Example 2:** Qubit A in superposition, qubit B in $|0\rangle$
$$
\frac{|00\rangle + |10\rangle}{\sqrt{2}} = \left(\frac{|0\rangle + |1\rangle}{\sqrt{2}}\right) \otimes |0\rangle = |+\rangle \otimes |0\rangle \quad \checkmark \text{ product}
$$

**Example 3:** Both qubits in superposition
$$
\frac{|00\rangle + |01\rangle + |10\rangle + |11\rangle}{2} = \frac{|0\rangle + |1\rangle}{\sqrt{2}} \otimes \frac{|0\rangle + |1\rangle}{\sqrt{2}} = |+\rangle \otimes |+\rangle \quad \checkmark \text{ product}
$$

### Entangled States

A two-qubit state is **entangled** if it is NOT a product state — it cannot be written as $|\psi\rangle_A \otimes |\phi\rangle_B$ for any choice of single-qubit states.

**Example:** 
$$
|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}
$$

Can this be written as a product? Let's try. Suppose:
$$
\frac{|00\rangle + |11\rangle}{\sqrt{2}} = (\alpha|0\rangle + \beta|1\rangle) \otimes (\gamma|0\rangle + \delta|1\rangle)
$$

Expanding the right side:
$$
= \alpha\gamma|00\rangle + \alpha\delta|01\rangle + \beta\gamma|10\rangle + \beta\delta|11\rangle
$$

Comparing coefficients:
- $c_{00} = \alpha\gamma = \frac{1}{\sqrt{2}}$
- $c_{01} = \alpha\delta = 0$
- $c_{10} = \beta\gamma = 0$
- $c_{11} = \beta\delta = \frac{1}{\sqrt{2}}$

From $c_{01} = 0$: either $\alpha = 0$ or $\delta = 0$.
From $c_{10} = 0$: either $\beta = 0$ or $\gamma = 0$.

But if $\alpha = 0$, then $c_{00} = 0$, contradiction.
If $\delta = 0$, then $c_{11} = 0$, contradiction.

Similarly for the other cases. **There is no solution.**

Therefore $|\Phi^+\rangle$ cannot be factored. It is entangled.

```{admonition} What Entanglement Means
:class: warning

Entanglement is not "we don't know which product state it is."

Entanglement means the quantum state has a structure that **cannot** be decomposed into independent parts. The two qubits are correlated in a way that has no classical explanation.
```

---

## The Amplitude Table: A Visual Test for Entanglement

Here's a beautiful way to see entanglement.

Arrange the four amplitudes of a two-qubit state as a $2 \times 2$ table:

$$
C = \begin{pmatrix} c_{00} & c_{01} \\ c_{10} & c_{11} \end{pmatrix}
$$

The rows correspond to qubit A ($|0\rangle_A$ or $|1\rangle_A$).
The columns correspond to qubit B ($|0\rangle_B$ or $|1\rangle_B$).

### Product States Have Rank 1

If the state is a product:
$$
|\Psi\rangle = (\alpha|0\rangle + \beta|1\rangle) \otimes (\gamma|0\rangle + \delta|1\rangle)
$$

Then the amplitude table is:
$$
C = \begin{pmatrix} \alpha\gamma & \alpha\delta \\ \beta\gamma & \beta\delta \end{pmatrix} = \begin{pmatrix} \alpha \\ \beta \end{pmatrix} \begin{pmatrix} \gamma & \delta \end{pmatrix}
$$

This is an **outer product** of two vectors — a rank-1 matrix.

### The Determinant Test

For a $2 \times 2$ matrix, rank 1 is equivalent to determinant zero:
$$
\det(C) = c_{00}c_{11} - c_{01}c_{10} = 0 \quad \Leftrightarrow \quad \text{separable}
$$

So:

```{admonition} Separability Test
:class: tip

For a two-qubit state with amplitude table $C$:

$$
\det(C) = c_{00}c_{11} - c_{01}c_{10}
$$

- If $\det(C) = 0$: the state is **separable** (product)
- If $\det(C) \neq 0$: the state is **entangled**
```

**Example:** For $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$:

$$
C = \begin{pmatrix} 1/\sqrt{2} & 0 \\ 0 & 1/\sqrt{2} \end{pmatrix}
$$

$$
\det(C) = \frac{1}{\sqrt{2}} \cdot \frac{1}{\sqrt{2}} - 0 \cdot 0 = \frac{1}{2} \neq 0
$$

Entangled! ✓

**Example:** For $|+\rangle \otimes |0\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |10\rangle)$:

$$
C = \begin{pmatrix} 1/\sqrt{2} & 0 \\ 1/\sqrt{2} & 0 \end{pmatrix}
$$

$$
\det(C) = \frac{1}{\sqrt{2}} \cdot 0 - 0 \cdot \frac{1}{\sqrt{2}} = 0
$$

Separable! ✓

---

## The Bell States: Maximally Entangled

The simplest and most important entangled states are the four **Bell states**:

$$
|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}
$$

$$
|\Phi^-\rangle = \frac{|00\rangle - |11\rangle}{\sqrt{2}}
$$

$$
|\Psi^+\rangle = \frac{|01\rangle + |10\rangle}{\sqrt{2}}
$$

$$
|\Psi^-\rangle = \frac{|01\rangle - |10\rangle}{\sqrt{2}}
$$

### Why These Four?

The Bell states aren't arbitrary — they have a beautiful structure.

**Orthonormal basis:** The four Bell states form an orthonormal basis for the two-qubit Hilbert space. Any two-qubit state can be written as a superposition of Bell states, just like any single-qubit state can be written in the $\{|0\rangle, |1\rangle\}$ basis.

**Related by local operations:** All four Bell states are related by single-qubit Pauli gates:

$$
|\Phi^-\rangle = (Z \otimes I)|\Phi^+\rangle
$$
$$
|\Psi^+\rangle = (X \otimes I)|\Phi^+\rangle
$$
$$
|\Psi^-\rangle = (XZ \otimes I)|\Phi^+\rangle = (iY \otimes I)|\Phi^+\rangle
$$

So if you can create $|\Phi^+\rangle$, you can create any Bell state by applying a Pauli gate to one qubit.

**Naming convention:**
- $\Phi$ (Phi) states: same bits (00 or 11)
- $\Psi$ (Psi) states: different bits (01 or 10)
- $+$ superscript: positive superposition
- $-$ superscript: negative superposition (relative phase of $\pi$)

### Why "Maximally Entangled"?

**Maximum determinant:** Each Bell state has $|\det(C)| = 1/2$, the maximum possible for a normalized state. The determinant measures "how entangled" — Bell states are as entangled as two qubits can be.

**Maximum uncertainty locally:** If you have $|\Phi^+\rangle$ and measure only qubit A, you get $|0\rangle$ or $|1\rangle$ with exactly 50/50 probability. Qubit A alone is in a maximally mixed state — you have zero information about it. Same for qubit B.

**Maximum correlation jointly:** Despite each qubit being completely random individually, the results are perfectly correlated:
- In $|\Phi^+\rangle$: always get $00$ or $11$, never $01$ or $10$
- In $|\Psi^+\rangle$: always get $01$ or $10$, never $00$ or $11$

```{admonition} Where Does the Information Live?
:class: important

In a Bell state:
- Each qubit alone carries **zero** information (completely random)
- The **correlation** between qubits carries **all** the information

This is radically different from classical systems, where information lives in the individual parts.
```

### The Bell Basis

Since the four Bell states form a basis, we can measure in the "Bell basis" instead of the computational basis. A Bell measurement asks: "Which of the four Bell states is this?"

This is crucial for quantum teleportation and other protocols. We'll see how to implement Bell measurements with circuits in a later lecture.

---

## Creating Bell States (Preview)

We'll study quantum gates in detail soon, but here's a preview of how to create a Bell state.

### The Recipe

1. Start with $|00\rangle$ (both qubits in ground state)
2. Apply a **Hadamard gate** (H) to the first qubit
3. Apply a **CNOT gate** (controlled-NOT) with qubit 1 as control and qubit 2 as target

Let's trace through this:

**Step 1:** Initial state
$$
|00\rangle
$$

**Step 2:** Hadamard on qubit 1
$$
H \otimes I \, |00\rangle = \frac{|0\rangle + |1\rangle}{\sqrt{2}} \otimes |0\rangle = \frac{|00\rangle + |10\rangle}{\sqrt{2}}
$$

This is still a product state.

**Step 3:** CNOT (flips qubit 2 if qubit 1 is $|1\rangle$)
$$
\text{CNOT} \left( \frac{|00\rangle + |10\rangle}{\sqrt{2}} \right) = \frac{|00\rangle + |11\rangle}{\sqrt{2}} = |\Phi^+\rangle
$$

The CNOT "ties together" the two qubits: when qubit 1 is $|1\rangle$, qubit 2 gets flipped to $|1\rangle$ too.

```{figure} ./03_01_lecture_files/bell_state_circuit.svg
:width: 350px
:name: bell-circuit-preview

Circuit to create $|\Phi^+\rangle$: Hadamard followed by CNOT.
```

The transition from Step 2 to Step 3 is where entanglement is born. A product state becomes entangled.

---

## What Does Measurement Look Like?

### Measuring Both Qubits

For the state $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$, the probabilities are:

$$
P(00) = |c_{00}|^2 = \frac{1}{2}
$$
$$
P(01) = |c_{01}|^2 = 0
$$
$$
P(10) = |c_{10}|^2 = 0
$$
$$
P(11) = |c_{11}|^2 = \frac{1}{2}
$$

You get 00 or 11, each with probability 1/2. Never 01 or 10.

### Measuring Only One Qubit: The Key to Understanding Entanglement

What if Alice measures just qubit 1, while Bob has qubit 2 far away?

Let's work through this carefully for $|\Phi^+\rangle$.

**Step 1: Rewrite the state to separate qubit 1**

$$
|\Phi^+\rangle = \frac{1}{\sqrt{2}}|0\rangle_1 \otimes |0\rangle_2 + \frac{1}{\sqrt{2}}|1\rangle_1 \otimes |1\rangle_2
$$

**Step 2: Apply the measurement rule**

When Alice measures qubit 1 in the computational basis:

- **Probability of getting $|0\rangle$:** The amplitude for qubit 1 being in $|0\rangle$ is $\frac{1}{\sqrt{2}}$ (from the first term). So $P(0) = |1/\sqrt{2}|^2 = 1/2$.

- **Probability of getting $|1\rangle$:** The amplitude for qubit 1 being in $|1\rangle$ is $\frac{1}{\sqrt{2}}$ (from the second term). So $P(1) = |1/\sqrt{2}|^2 = 1/2$.

**Step 3: State after measurement**

- **If Alice gets 0:** The state collapses to the branch where qubit 1 is $|0\rangle$. The full state becomes $|0\rangle_1 \otimes |0\rangle_2 = |00\rangle$. Bob's qubit is now definitely $|0\rangle$.

- **If Alice gets 1:** The state collapses to $|1\rangle_1 \otimes |1\rangle_2 = |11\rangle$. Bob's qubit is now definitely $|1\rangle$.

### The Puzzle

Before Alice measures, Bob's qubit has no definite state — it's entangled. After Alice measures, Bob's qubit has a definite state — it's either $|0\rangle$ or $|1\rangle$.

Alice's measurement seems to "affect" Bob's qubit instantly, even if they're light-years apart!

This is the "spooky action at a distance" that bothered Einstein. We'll explore it deeply in the next lecture.

```{admonition} A Subtlety: Bob Doesn't Notice
:class: note

Although Alice's measurement determines Bob's qubit, Bob can't tell this happened. From Bob's perspective:

- Before Alice measures: if Bob measures, he gets 0 or 1 with 50/50 probability
- After Alice measures: if Bob measures, he still gets 0 or 1 with 50/50 probability

The statistics look the same to Bob. He only learns his qubit's state is correlated with Alice's when they compare results later (through ordinary classical communication).

This is why entanglement can't be used for faster-than-light communication.
```

### What State is a Single Entangled Qubit In?

Here's a deep question: before any measurement, what is the "state" of qubit 2 alone in the Bell state $|\Phi^+\rangle$?

The answer is: **it doesn't have a pure state**. You cannot write qubit 2's state as $\alpha|0\rangle + \beta|1\rangle$ for any $\alpha, \beta$.

An entangled qubit is not in any definite quantum state on its own — its state is only defined relative to the other qubit. This is described mathematically by a **density matrix** (which we'll study later), not a state vector.

This is what "entanglement" really means: the whole is not the sum of its parts.

---

## Lab: Two-Qubit States in Qiskit

Let's create and explore two-qubit states.

```{admonition} Qiskit Qubit Ordering Convention
:class: warning

Qiskit uses **little-endian** ordering: qubit 0 is the *rightmost* bit in output strings.

So if you create a circuit with `qc.x(0)` (flip qubit 0), the output state is $|01\rangle$ in our notation, but Qiskit displays it as `'10'` (reading right-to-left).

In this lab, we'll be careful to note when this matters. The state vectors are always correct — it's just the string labels that can be confusing.
```

### Setup

```{code-block} python
import numpy as np
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit_aer import AerSimulator

simulator = AerSimulator()
```

### Creating Product States

```{code-block} python
# Product state: |+⟩ ⊗ |0⟩
qc_product = QuantumCircuit(2)
qc_product.h(0)  # Hadamard on qubit 0 -> |+⟩
# Qubit 1 stays in |0⟩

state_product = Statevector(qc_product)
print("Product state |+⟩|0⟩:")
print(state_product)

# The amplitudes
print("\nAmplitudes:")
for i, amp in enumerate(state_product):
    label = format(i, '02b')  # Binary label
    if abs(amp) > 1e-10:
        print(f"  |{label}⟩: {amp:.4f}")
```

### Creating a Bell State

```{code-block} python
# Bell state |Φ+⟩ = (|00⟩ + |11⟩)/√2
qc_bell = QuantumCircuit(2)
qc_bell.h(0)      # Hadamard on qubit 0
qc_bell.cx(0, 1)  # CNOT: control=0, target=1

state_bell = Statevector(qc_bell)
print("Bell state |Φ+⟩:")
print(state_bell)

print("\nAmplitudes:")
for i, amp in enumerate(state_bell):
    label = format(i, '02b')
    if abs(amp) > 1e-10:
        print(f"  |{label}⟩: {amp:.4f}")
```

### The Amplitude Table

```{code-block} python
def amplitude_table(statevector):
    """Display a two-qubit state as a 2x2 amplitude table."""
    amps = np.array(statevector)
    table = np.array([[amps[0], amps[1]], 
                      [amps[2], amps[3]]])
    return table

def check_entanglement(statevector):
    """Check if a two-qubit state is entangled using the determinant test."""
    table = amplitude_table(statevector)
    det = table[0,0]*table[1,1] - table[0,1]*table[1,0]
    return det, abs(det) > 1e-10

# Test product state
print("Product state |+⟩|0⟩:")
table = amplitude_table(state_product)
print(table)
det, entangled = check_entanglement(state_product)
print(f"Determinant: {det:.4f}")
print(f"Entangled: {entangled}")

print("\n" + "="*40 + "\n")

# Test Bell state
print("Bell state |Φ+⟩:")
table = amplitude_table(state_bell)
print(table)
det, entangled = check_entanglement(state_bell)
print(f"Determinant: {det:.4f}")
print(f"Entangled: {entangled}")
```

### Measuring Bell State Correlations

```{code-block} python
# Create Bell state and measure both qubits
qc_measure = QuantumCircuit(2, 2)
qc_measure.h(0)
qc_measure.cx(0, 1)
qc_measure.measure([0, 1], [0, 1])

# Run many times
job = simulator.run(qc_measure, shots=1000)
counts = job.result().get_counts()

print("Measurement results for |Φ+⟩:")
for outcome, count in sorted(counts.items()):
    print(f"  |{outcome}⟩: {count} times ({count/10:.1f}%)")
```

### Creating All Four Bell States

```{code-block} python
def create_bell_state(which):
    """Create one of the four Bell states.
    
    which: 'Phi+', 'Phi-', 'Psi+', or 'Psi-'
    """
    qc = QuantumCircuit(2)
    
    # Start with H on qubit 0
    qc.h(0)
    
    # Apply CNOT
    qc.cx(0, 1)
    
    # Now we have |Φ+⟩. Modify as needed:
    if which == 'Phi-':
        qc.z(0)  # Adds relative phase -1 to |11⟩
    elif which == 'Psi+':
        qc.x(1)  # Swaps |00⟩↔|01⟩ and |11⟩↔|10⟩
    elif which == 'Psi-':
        qc.x(1)
        qc.z(0)
    
    return Statevector(qc)

# Display all four
bell_names = ['Phi+', 'Phi-', 'Psi+', 'Psi-']
for name in bell_names:
    state = create_bell_state(name)
    print(f"|{name}⟩:")
    for i, amp in enumerate(state):
        label = format(i, '02b')
        if abs(amp) > 1e-10:
            print(f"  |{label}⟩: {amp:.4f}")
    print()
```

---

## Entanglement as a Resource (Preview)

Entanglement might seem like a philosophical curiosity — weird correlations that Einstein didn't like. But it's far more than that.

Entanglement is a **resource** that can be created, distributed, stored, and consumed to accomplish tasks impossible with classical physics:

**Quantum teleportation:** Send an unknown quantum state from Alice to Bob using only classical communication — if they share entanglement.

**Superdense coding:** Send two classical bits from Alice to Bob by transmitting only one qubit — if they share entanglement.

**Quantum key distribution:** Create a secret key that's guaranteed secure by the laws of physics — using entanglement.

**Quantum algorithms:** Many quantum speedups (including Shor's algorithm for factoring) rely on entanglement between qubits during computation.

We'll explore these protocols in coming lectures. For now, remember:

> Entanglement is not just a strange phenomenon to explain — it's a tool to exploit.

---

## Summary

Today we took the crucial step from one qubit to two:

1. **The tensor product** combines configuration spaces
   - Every configuration of A pairs with every configuration of B
   - Amplitudes multiply
   - Dimensions multiply: $2 \times 2 = 4$

2. **Two-qubit states** live in a 4-dimensional Hilbert space
   - Basis: $|00\rangle, |01\rangle, |10\rangle, |11\rangle$
   - General state: 4 complex amplitudes

3. **Product states** factor as $|\psi\rangle_A \otimes |\phi\rangle_B$
   - Each qubit has its own independent state
   - Amplitude table has rank 1 (determinant = 0)

4. **Entangled states** do not factor
   - The qubits are correlated in a non-classical way
   - Amplitude table has rank 2 (determinant ≠ 0)

5. **Bell states** are maximally entangled
   - Each qubit alone is random
   - But they're perfectly correlated
   - "Information lives in the correlations"

6. **Creating entanglement**: H + CNOT turns a product state into a Bell state

---

## Homework

### Problem 1: Tensor Product Practice

Compute the following tensor products and write the result as a 4-component column vector.

**(a)** $|0\rangle \otimes |1\rangle$

**(b)** $|1\rangle \otimes |+\rangle$ where $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$

**(c)** $|+\rangle \otimes |-\rangle$ where $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$

**(d)** $|+\rangle \otimes |+\rangle$

---

### Problem 2: Identifying Product vs Entangled

For each state, determine whether it is a product state or entangled. If product, write it as $|\psi\rangle \otimes |\phi\rangle$. If entangled, compute the determinant of the amplitude table.

**(a)** $|\Psi\rangle = \frac{1}{2}(|00\rangle + |01\rangle + |10\rangle + |11\rangle)$

**(b)** $|\Psi\rangle = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle)$

**(c)** $|\Psi\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |01\rangle)$

**(d)** $|\Psi\rangle = \frac{1}{2}(|00\rangle + |01\rangle + |10\rangle - |11\rangle)$

---

### Problem 3: The Amplitude Table

For each state, write the $2 \times 2$ amplitude table $C$ and compute $\det(C)$.

**(a)** $|\Phi^-\rangle = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle)$

**(b)** $|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle)$

**(c)** $|0\rangle \otimes |+\rangle$

**(d)** A state with $c_{00} = c_{11} = \frac{1}{2}$ and $c_{01} = c_{10} = \frac{1}{2}$

---

### Problem 4: Bell State Measurements

Consider the Bell state $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$.

**(a)** What are the possible outcomes if both qubits are measured in the computational basis? What are the probabilities?

**(b)** If Alice measures qubit 1 and gets $|0\rangle$, what state is qubit 2 in?

**(c)** If Alice measures qubit 1 and gets $|1\rangle$, what state is qubit 2 in?

**(d)** Compared to $|\Phi^+\rangle$, are the correlations the same or different?

---

### Problem 5: Three Qubits

The tensor product extends to more qubits.

**(a)** How many basis states are there for three qubits? List them all.

**(b)** Write the state $|+\rangle \otimes |0\rangle \otimes |1\rangle$ as a sum of computational basis states.

**(c)** Consider the three-qubit state:
$$
|GHZ\rangle = \frac{1}{\sqrt{2}}(|000\rangle + |111\rangle)
$$
This is called the Greenberger-Horne-Zeilinger state. Is it a product state? How would you test this?

**(d)** If you measure all three qubits of $|GHZ\rangle$ in the computational basis, what outcomes are possible?

---

### Problem 6: Partial Measurement

When you measure only one qubit of a two-qubit state, the other qubit's state changes.

Consider $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$.

**(a)** If you measure qubit 1 and get $|0\rangle$, what is the (normalized) state of the full system afterward?

**(b)** What if you measure qubit 1 and get $|1\rangle$?

**(c)** Before the measurement, what is the "state" of qubit 2 alone? Can it be described by a single-qubit state vector?

**(d)** This is related to the concept of "reduced density matrix" (which we'll study later). For now, just note: an entangled qubit does not have its own pure state.

---

### Problem 7: Creating Entanglement (Conceptual)

**(a)** Explain in words why a product gate $A \otimes B$ (where $A$ acts on qubit 1 and $B$ acts on qubit 2) cannot create entanglement from a product state.

**(b)** The CNOT gate is defined by: $|00\rangle \to |00\rangle$, $|01\rangle \to |01\rangle$, $|10\rangle \to |11\rangle$, $|11\rangle \to |10\rangle$. Can CNOT be written as $A \otimes B$ for any single-qubit gates $A$ and $B$? (Hint: consider what happens to $|10\rangle$ vs $|00\rangle$.)

**(c)** Apply CNOT to the product state $|+\rangle \otimes |+\rangle$. Is the result entangled?

---

### Problem 8: Qiskit Exploration

**(a)** Use Qiskit to create the state $\frac{1}{\sqrt{2}}(|01\rangle + |10\rangle) = |\Psi^+\rangle$. Measure it 1000 times and verify you only get 01 and 10.

**(b)** Create the three-qubit GHZ state $\frac{1}{\sqrt{2}}(|000\rangle + |111\rangle)$ using the circuit:
- H on qubit 0
- CNOT with control 0, target 1
- CNOT with control 0, target 2

Measure all three qubits and verify the correlations.

**(c)** Create a product state of your choice and verify using the determinant test that it is separable.

---

## Looking Ahead

We've established that entangled states exist and are fundamentally different from product states. The correlations in a Bell state are perfect: measuring one qubit instantly determines the other.

But wait — doesn't that violate relativity? If Alice and Bob are far apart, and Alice's measurement instantly affects Bob's qubit, isn't that faster-than-light communication?

Next lecture, we'll explore this puzzle. We'll meet Einstein's EPR argument, see why he thought quantum mechanics must be incomplete, and discover what Bell's theorem tells us about the nature of reality.

Spoiler: entanglement is real, it's weird, but it doesn't let you send messages faster than light.