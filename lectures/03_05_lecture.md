---
kernelspec:
  name: python3
  display_name: Python 3
---

# Lecture 3.5 — Quantum Circuits and the No-Cloning Theorem

## From States to Circuits

In Chapters 2 and 3 we built the physics of quantum information: qubits, superposition, measurement, entanglement, Bell's theorem. We know *what* quantum states are and *how* they behave.

Now we need a language for *doing things* with them. How do you build a Bell state? How do you move quantum information from one qubit to another? How do you combine simple operations into a protocol?

The answer is **quantum circuits** — the standard computational framework for quantum information. The idea is borrowed from classical electrical engineering: information flows along wires through a network of gates. Today we translate that idea into quantum mechanics, and discover a fundamental constraint that has no classical analog: **you cannot copy a quantum state**.

------------------------------------------------------------------------

## Part 1: Classical Circuits

### Bits, Wires, and Gates

A classical circuit has three ingredients:

-   **Wires** carry bits (0 or 1)
-   **Gates** perform operations on bits
-   Information flows left to right

The basic logic gates and their truth tables:

**NOT gate** ($\neg$): flips a bit.

$$
\begin{array}{c|c}
a & \neg a \\
\hline
0 & 1 \\
1 & 0
\end{array}
$$

**AND gate** ($\wedge$): outputs 1 only if both inputs are 1.

$$
\begin{array}{c|c}
ab & a \wedge b \\
\hline
00 & 0 \\
01 & 0 \\
10 & 0 \\
11 & 1
\end{array}
$$

**XOR gate** ($\oplus$): outputs 1 if the inputs differ.

$$
\begin{array}{c|c}
ab & a \oplus b \\
\hline
00 & 0 \\
01 & 1 \\
10 & 1 \\
11 & 0
\end{array}
$$

XOR is addition modulo 2. We'll see this operation again and again in quantum algorithms.

Here's what these classical gates look like as circuit symbols:

```{code-cell} python
:tags: [hide-input]
import schemdraw
from schemdraw import logic
import matplotlib.pyplot as plt

fig, axes = plt.subplots(1, 3, figsize=(10, 2.5))

with schemdraw.Drawing(canvas=axes[0], show=False) as d:
    d.config(fontsize=14)
    d.add(logic.Not().label('NOT', 'top', ofst=0.3))

with schemdraw.Drawing(canvas=axes[1], show=False) as d2:
    d2.config(fontsize=14)
    d2.add(logic.And().label('AND', 'top', ofst=0.3))

with schemdraw.Drawing(canvas=axes[2], show=False) as d3:
    d3.config(fontsize=14)
    d3.add(logic.Xor().label('XOR', 'top', ofst=0.3))

for ax in axes:
    ax.axis('off')
    ax.set_aspect('equal')

plt.tight_layout()
plt.show()
```

### Fan-Out: Copying Is Free

In classical circuits, you can freely **copy** a bit. If a wire carries the value 1, you can split it into two wires that both carry 1. This is called **fan-out**, and it's so natural in classical computing that we barely think about it.

Keep this in mind. It will not survive the transition to quantum mechanics.

### The Classical Half-Adder

A useful example: the **half-adder** computes the sum and carry of two bits.

-   **Sum** = $a \oplus b$ (XOR)
-   **Carry** = $a \wedge b$ (AND)

So $1 + 1 = 10$ in binary: sum bit is 0, carry bit is 1. This tiny circuit is the foundation of all classical arithmetic. We'll build its quantum version in the next lecture.

```{admonition} iClicker
:class: question

A half-adder takes inputs $a = 1$, $b = 1$. What is (Sum, Carry)?

A) (0, 0) &emsp; B) (1, 0) &emsp; C) (1, 1) &emsp; D) (0, 1)
```

```{code-cell} python
:tags: [hide-input]
import schemdraw
from schemdraw import logic

with schemdraw.Drawing() as d:
    d.config(fontsize=12)
    # XOR gate for Sum (placed first, at top)
    xor = d.add(logic.Xor().at((4, 0)).right())
    d.add(logic.Line().right().length(1.5).label('Sum', 'right'))

    # AND gate for Carry (below)
    andg = d.add(logic.And().at((4, -3)).right())
    d.add(logic.Line().right().length(1.5).label('Carry', 'right'))

    # Input a: horizontal wire, then branch to both gates
    d.add(logic.Line().at((0, 0)).right().length(1.5).label('a', 'left'))
    d.add(logic.Dot())
    d.add(logic.Line().right().to(xor.in1))
    d.add(logic.Line().at((1.5, 0)).down().to((1.5, andg.in1[1])))
    d.add(logic.Line().right().to(andg.in1))

    # Input b: horizontal wire, then branch to both gates
    d.add(logic.Line().at((0, -1)).right().length(2.5).label('b', 'left'))
    d.add(logic.Dot())
    d.add(logic.Line().right().to(xor.in2))
    d.add(logic.Line().at((2.5, -1)).down().to((2.5, andg.in2[1])))
    d.add(logic.Line().right().to(andg.in2))
```

------------------------------------------------------------------------

## Part 2: Quantum Circuits

### The Three Ingredients

A quantum circuit also has three ingredients:

-   **Wires** carry qubits (horizontal lines)
-   **Gates** are unitary operations on qubits
-   **Measurements** collapse qubits to classical bits

Information flows left to right. Applying a sequence of gates composes their unitaries: if gate $A$ comes first and gate $B$ comes second, the total operation is $BA$ (matrix multiplication is right to left, even though the circuit reads left to right).

### Single-Qubit Gates (Review)

You already know these from Chapter 2. Now they appear as boxes on a wire:

**Pauli-X** (NOT gate): flips $|0\rangle \leftrightarrow |1\rangle$.

$$
X = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}
$$

Drawn as a box labeled $X$, or sometimes as a circle with a plus: $\oplus$.

**Hadamard**: creates superposition.

$$
H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}
$$

$$
H|0\rangle = \frac{|0\rangle + |1\rangle}{\sqrt{2}} = |+\rangle, \qquad H|1\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}} = |-\rangle
$$

**Pauli-Z**: phase flip on $|1\rangle$.

$$
Z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}
$$

Drawn as a box labeled $Z$. Note that $Z|0\rangle = |0\rangle$ and $Z|1\rangle = -|1\rangle$.

**S and T gates**: smaller phase rotations. $S$ applies a phase of $i$ to $|1\rangle$; $T$ applies $e^{i\pi/4}$.

$$
S = \begin{pmatrix} 1 & 0 \\ 0 & i \end{pmatrix}, \qquad T = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\pi/4} \end{pmatrix}
$$

These are less familiar, but they appear in many quantum algorithms. The Hadamard and T gate together are sufficient to approximate any single-qubit unitary to arbitrary precision — a fact we won't prove but is worth knowing.

Here's what these gates look like as circuit elements — a sequence of $T$, $H$, $S$, $H$ applied to a single qubit:

```{code-cell} python
:tags: [hide-input]
from qiskit import QuantumCircuit

qc = QuantumCircuit(1)
qc.h(0)
qc.s(0)
qc.h(0)
qc.t(0)
qc.draw("mpl")
```

The circuit reads left to right: Hadamard first, then $S$, then another Hadamard, then $T$. The total unitary is $THSH$ (right to left in matrix multiplication).

```{code-cell} python
:tags: [hide-input]
import matplotlib.pyplot as plt
import numpy as np

fig, axes = plt.subplots(1, 3, figsize=(10, 3))

labels = ['0', '1']

# Histogram A: all |0⟩
axes[0].bar(labels, [1000, 0], color=['#1f77b4', '#1f77b4'])
axes[0].set_title('A', fontsize=16, fontweight='bold')
axes[0].set_ylim(0, 1100)
axes[0].set_ylabel('Counts')

# Histogram B: 50/50
axes[1].bar(labels, [512, 488], color=['#1f77b4', '#1f77b4'])
axes[1].set_title('B', fontsize=16, fontweight='bold')
axes[1].set_ylim(0, 1100)

# Histogram C: all |1⟩
axes[2].bar(labels, [0, 1000], color=['#1f77b4', '#1f77b4'])
axes[2].set_title('C', fontsize=16, fontweight='bold')
axes[2].set_ylim(0, 1100)

plt.tight_layout()
plt.show()
```

```{admonition} iClicker
:class: question

You prepare $|0\rangle$, apply $H$, then measure. After 1000 shots, which histogram do you get?

A) Histogram A &emsp; B) Histogram B &emsp; C) Histogram C
```

------------------------------------------------------------------------

### Multi-Qubit Gates

This is where circuits become powerful. To act on more than one qubit, we use the **tensor product** structure from Lecture 3.1.

**Applying a gate to one qubit of a pair.** If we apply $U$ to one qubit and do nothing to the other, we tensor with the identity. For example, $U$ on the first qubit and identity on the second gives $U \otimes I$; identity on the first and $U$ on the second gives $I \otimes U$.

Example: identity on the first qubit, Hadamard on the second:

$$
I \otimes H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 & 0 & 0 \\ 1 & -1 & 0 & 0 \\ 0 & 0 & 1 & 1 \\ 0 & 0 & 1 & -1 \end{pmatrix}.
$$

The block-diagonal structure makes sense: the first qubit's state selects which $2\times 2$ block we're in, and within each block, $H$ acts on the second qubit.

### The Controlled-NOT (CNOT)

The most important two-qubit gate. It has a **control** qubit and a **target** qubit:

- If the control is $|0\rangle$: do nothing.
- If the control is $|1\rangle$: apply $X$ (NOT) to the target.

In Dirac notation, using the projector formalism from Lecture 3.1:

$$
\text{CNOT} = |0\rangle\langle 0| \otimes I + |1\rangle\langle 1| \otimes X.
$$

Read this as: "project the control onto $|0\rangle$, do nothing to the target; project the control onto $|1\rangle$, flip the target." The projectors $|0\rangle\langle 0|$ and $|1\rangle\langle 1|$ are exactly the same objects you used in measurement theory (Lecture 2.3). Now they build gates.

Action on the computational basis:

$$
|00\rangle \to |00\rangle, \quad |01\rangle \to |01\rangle, \quad |10\rangle \to |11\rangle, \quad |11\rangle \to |10\rangle
$$

In matrix form:

$$
\text{CNOT} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{pmatrix}
$$

Circuit symbol: a solid dot on the control wire connected by a vertical line to $\oplus$ on the target wire.

```{code-cell} python
:tags: [hide-input]
qc = QuantumCircuit(2)
qc.cx(0, 1)
qc.draw("mpl")
```

```{admonition} Check Yourself
:class: tip

Verify that $\text{CNOT}$ is unitary: compute $\text{CNOT}^\dagger \cdot \text{CNOT}$ and confirm you get the identity.
```

```{admonition} iClicker
:class: question

What is CNOT $|00\rangle$?

A) $|00\rangle$ &emsp; B) $|01\rangle$ &emsp; C) $|10\rangle$ &emsp; D) $|11\rangle$
```

```{admonition} iClicker
:class: question

Start with $|00\rangle$. Apply $H$ to the first qubit, then CNOT. What is the output state?

A) $|00\rangle$ &emsp; B) $|01\rangle + |10\rangle$ &emsp; C) $\frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$ &emsp; D) $\frac{1}{\sqrt{2}}(|00\rangle + |10\rangle)$
```

```{code-cell} python
:tags: [hide-input]
import matplotlib.pyplot as plt

fig, axes = plt.subplots(1, 4, figsize=(12, 3))

labels = ['00', '01', '10', '11']

# A: only |00⟩ and |11⟩
axes[0].bar(labels, [500, 0, 0, 500], color='#1f77b4')
axes[0].set_title('A', fontsize=16, fontweight='bold')
axes[0].set_ylim(0, 1100)
axes[0].set_ylabel('Counts')

# B: all four equal
axes[1].bar(labels, [250, 250, 250, 250], color='#1f77b4')
axes[1].set_title('B', fontsize=16, fontweight='bold')
axes[1].set_ylim(0, 1100)

# C: only |00⟩
axes[2].bar(labels, [1000, 0, 0, 0], color='#1f77b4')
axes[2].set_title('C', fontsize=16, fontweight='bold')
axes[2].set_ylim(0, 1100)

# D: only |00⟩ and |10⟩
axes[3].bar(labels, [500, 0, 500, 0], color='#1f77b4')
axes[3].set_title('D', fontsize=16, fontweight='bold')
axes[3].set_ylim(0, 1100)

plt.tight_layout()
plt.show()
```

```{admonition} iClicker
:class: question

Same circuit: $|00\rangle \to H \otimes I \to \text{CNOT}$, then measure both qubits 1000 times. Which histogram do you see?

A) Histogram A &emsp; B) Histogram B &emsp; C) Histogram C &emsp; D) Histogram D
```

We can also verify the CNOT matrix directly with Qiskit's `Operator` class. But watch out — Qiskit's qubit ordering puts qubit 0 as the *rightmost* bit in the ket (we'll discuss this more in Part 4). So `cx(0, 1)` produces a matrix with rows/columns ordered as $|q_1\, q_0\rangle$, which shuffles the middle two rows compared to the textbook convention:

```{code-cell} python
from qiskit.quantum_info import Operator
import numpy as np

# Qiskit ordering: qubit 0 = rightmost in ket
qc_cnot = QuantumCircuit(2)
qc_cnot.cx(0, 1)
op = Operator(qc_cnot)
print("CNOT matrix (Qiskit ordering |q₁ q₀⟩):")
print(np.real(op.data).astype(int))

# To get the textbook matrix, swap the qubit order:
qc_cnot2 = QuantumCircuit(2)
qc_cnot2.cx(1, 0)
op2 = Operator(qc_cnot2)
print("\nCNOT matrix (textbook ordering |q₀ q₁⟩):")
print(np.real(op2.data).astype(int))
```

### The Controlled-Z (CZ)

Replace $X$ with $Z$ in the controlled-gate formula:

$$
\text{CZ} = |0\rangle\langle 0| \otimes I + |1\rangle\langle 1| \otimes Z = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & -1 \end{pmatrix}
$$

The only effect: $|11\rangle \to -|11\rangle$. Everything else is unchanged.

Notice something interesting: the CZ matrix is symmetric under exchange of the two qubits. It doesn't matter which qubit you call "control" and which you call "target." This is *not* true for CNOT.

Circuit symbol: solid dots on both wires, connected by a vertical line.

```{code-cell} python
:tags: [hide-input]
qc = QuantumCircuit(2)
qc.cz(0, 1)
qc.draw("mpl")
```

### The SWAP Gate

Exchanges two qubits:

$$
\text{SWAP}|a\rangle|b\rangle = |b\rangle|a\rangle
$$

$$
\text{SWAP} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}
$$

It swaps the middle two rows: $|01\rangle \leftrightarrow |10\rangle$, while $|00\rangle$ and $|11\rangle$ are unchanged (they're already symmetric).

SWAP can be decomposed into three CNOTs:

$$
\text{SWAP} = \text{CNOT}_{12} \cdot \text{CNOT}_{21} \cdot \text{CNOT}_{12}
$$

Circuit symbol: two $\times$ marks connected by a vertical line.

```{code-cell} python
:tags: [hide-input]
qc = QuantumCircuit(2)
qc.swap(0, 1)
qc.draw("mpl")
```

We can verify the SWAP = three CNOTs decomposition:

```{code-cell} python
:tags: [hide-input]
qc_swap_decomposed = QuantumCircuit(2)
qc_swap_decomposed.cx(0, 1)
qc_swap_decomposed.cx(1, 0)
qc_swap_decomposed.cx(0, 1)
qc_swap_decomposed.draw("mpl")
```

### General Controlled-$U$

For any single-qubit unitary $U$:

$$
C_U = |0\rangle\langle 0| \otimes I + |1\rangle\langle 1| \otimes U
$$

CNOT is $C_X$. CZ is $C_Z$. This generalizes to any gate. Circuit symbol: solid dot connected to a box labeled $U$.

### The Toffoli Gate (CCX)

A three-qubit gate with two controls and one target: flip the target only if **both** controls are $|1\rangle$.

$$
\text{CCX} = (|00\rangle\langle 00| + |01\rangle\langle 01| + |10\rangle\langle 10|) \otimes I + |11\rangle\langle 11| \otimes X
$$

The Toffoli gate is the quantum version of a classical AND gate (it computes AND reversibly). Circuit symbol: two solid dots connected to $\oplus$.

```{code-cell} python
:tags: [hide-input]
qc = QuantumCircuit(3)
qc.ccx(0, 1, 2)
qc.draw("mpl")
```

### Measurement

A measurement gate is drawn as a meter symbol. It collapses the qubit to $|0\rangle$ or $|1\rangle$ and outputs a **classical bit**, drawn as a double wire.

After measurement, the qubit's quantum information is destroyed — it's now a definite classical value. Classical bits from measurements can be used to control later operations (we'll use this in teleportation).

```{code-cell} python
:tags: [hide-input]
qc = QuantumCircuit(2, 2)
qc.h(0)
qc.cx(0, 1)
qc.measure([0, 1], [0, 1])
qc.draw("mpl")
```

The double lines coming out of the meter symbols are **classical bit** wires.

### Circuit Symbol Summary

| Gate             | Symbol                                     |
|------------------|--------------------------------------------|
| Single-qubit $U$ | Box labeled "$U$"                          |
| $X$ (NOT)        | $\oplus$ or box labeled $X$                |
| CNOT             | Solid dot — vertical line — $\oplus$       |
| CZ               | Solid dot — vertical line — solid dot      |
| SWAP             | $\times$ — vertical line — $\times$        |
| Toffoli (CCX)    | Two solid dots — vertical line — $\oplus$  |
| Controlled-$U$   | Solid dot — vertical line — box "$U$"      |
| Measurement      | Meter symbol → double wire (classical bit) |

------------------------------------------------------------------------

## Part 3: Building Bell States

Now we use circuits to build something we already know: Bell states. The point is that the formalism we just set up actually *produces* the entangled states from Chapter 3.

### The E-bit Circuit

Start with two qubits in $|00\rangle$. Apply Hadamard to the first qubit, then CNOT with the first qubit as control.

```{code-cell} python
:tags: [hide-input]
bell_circuit = QuantumCircuit(2)
bell_circuit.h(0)
bell_circuit.cx(0, 1)
bell_circuit.draw("mpl")
```

**Step 1: Hadamard on the first qubit.**

$$
(H \otimes I)|00\rangle = \left(\frac{|0\rangle + |1\rangle}{\sqrt{2}}\right) \otimes |0\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |10\rangle)
$$

After the Hadamard, the first qubit is in a superposition. The second qubit is still $|0\rangle$. This state is **not** entangled — it's a product state.

```{admonition} iClicker
:class: question

Starting from $|00\rangle$, after applying $H$ to the first qubit (before the CNOT), the state is:

A) $\frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$ &emsp; B) $\frac{1}{\sqrt{2}}(|00\rangle + |10\rangle)$ &emsp; C) $\frac{1}{\sqrt{2}}(|00\rangle + |01\rangle)$ &emsp; D) $|{+}{+}\rangle$
```

**Step 2: CNOT (first qubit controls second qubit).**

$$
\text{CNOT} \cdot \frac{1}{\sqrt{2}}(|00\rangle + |10\rangle) = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle) = |\Phi^+\rangle
$$

The CNOT correlates the second qubit with the first: whenever the first is $|1\rangle$, it flips the second to $|1\rangle$ too. The result is a Bell state — maximally entangled.

```{admonition} Check Yourself
:class: tip

Verify this by multiplying the $4\times 4$ CNOT matrix by the vector $\frac{1}{\sqrt{2}}(1, 0, 1, 0)^T$. You should get $\frac{1}{\sqrt{2}}(1, 0, 0, 1)^T$.
```

Let's verify with Qiskit — the `Statevector` class tracks the quantum state through each gate:

```{code-cell} python
from qiskit.quantum_info import Statevector

# Build the Bell state circuit
qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)

# Compute the output state
psi = Statevector.from_instruction(qc)
psi.draw("text")
```

That's $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$.

```{admonition} iClicker
:class: question

You measure both qubits of $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$. Which outcomes are possible?

A) Only $|00\rangle$ &emsp; B) $|00\rangle$ or $|11\rangle$ &emsp; C) $|00\rangle$ or $|01\rangle$ &emsp; D) All four basis states equally
```

### All Four Bell States

The same circuit with different inputs produces all four Bell states:

| Input | After $H \otimes I$ | After CNOT | Output |
|---------------|----------------------------|---------------|---------------|
| $\|00\rangle$ | $\frac{1}{\sqrt{2}}(\|00\rangle + \|10\rangle)$ | $\frac{1}{\sqrt{2}}(\|00\rangle + \|11\rangle)$ | $\|\Phi^+\rangle$ |
| $\|01\rangle$ | $\frac{1}{\sqrt{2}}(\|01\rangle + \|11\rangle)$ | $\frac{1}{\sqrt{2}}(\|01\rangle + \|10\rangle)$ | $\|\Psi^+\rangle$ |
| $\|10\rangle$ | $\frac{1}{\sqrt{2}}(\|00\rangle - \|10\rangle)$ | $\frac{1}{\sqrt{2}}(\|00\rangle - \|11\rangle)$ | $\|\Phi^-\rangle$ |
| $\|11\rangle$ | $\frac{1}{\sqrt{2}}(\|01\rangle - \|11\rangle)$ | $\frac{1}{\sqrt{2}}(\|01\rangle - \|10\rangle)$ | $-\|\Psi^-\rangle$ |

The circuit is a **unitary map from the computational basis to the Bell basis**. It converts product states into maximally entangled states.

Let's verify all four with Qiskit:

```{code-cell} python
inputs = ["00", "01", "10", "11"]
bell_names = ["|Φ⁺⟩", "|Ψ⁺⟩", "|Φ⁻⟩", "-|Ψ⁻⟩"]

for label, name in zip(inputs, bell_names):
    qc = QuantumCircuit(2)
    # Prepare input state
    if label[1] == "1":  # qubit 0 (rightmost bit)
        qc.x(0)
    if label[0] == "1":  # qubit 1 (leftmost bit)
        qc.x(1)
    # Apply Bell circuit
    qc.h(0)
    qc.cx(0, 1)
    psi = Statevector.from_instruction(qc)
    print(f"|{label}⟩ → {name}: {psi}")
```

### The Reverse: Bell Measurement

Running the circuit backward — first CNOT, then Hadamard — takes Bell states *back* to the computational basis. This is how you measure in the Bell basis: apply CNOT then H, then measure in the standard basis.

This reverse circuit is exactly what Bob does in superdense coding (coming in Lecture 3.8). Keep it in mind.

```{code-cell} python
:tags: [hide-input]
# The reverse circuit: Bell measurement
qc_reverse = QuantumCircuit(2)
qc_reverse.cx(0, 1)
qc_reverse.h(0)
qc_reverse.draw("mpl")
```

```{code-cell} python
# Verify: apply reverse circuit to |Φ⁺⟩ → should give |00⟩
phi_plus = Statevector.from_label("00")

# Forward: create Bell state
qc_bell = QuantumCircuit(2)
qc_bell.h(0)
qc_bell.cx(0, 1)
phi_plus = phi_plus.evolve(qc_bell)
print(f"After Bell circuit: {phi_plus}")

# Reverse: undo it
qc_rev = QuantumCircuit(2)
qc_rev.cx(0, 1)
qc_rev.h(0)
result = phi_plus.evolve(qc_rev)
print(f"After reverse: {result}")
```

------------------------------------------------------------------------

## Part 4: Qiskit's Qubit Ordering Convention

Before we go further, there's a bookkeeping issue that will cause confusion if we don't address it now.

**Qiskit's convention:**

- The **top** wire in a circuit diagram is qubit 0.
- Qubit 0 corresponds to the **rightmost** position in a ket.
- The **bottom** wire has the highest index and corresponds to the **leftmost** position.

So for two qubits with $q_0$ on top and $q_1$ on bottom, Qiskit writes the state as $|q_1\, q_0\rangle$.

This means: when you apply $H$ to the top wire (qubit 0) and $I$ to the bottom wire (qubit 1), the matrix is $I \otimes H$ — **not** $H \otimes I$.

```{admonition} Warning
:class: warning

Qiskit's convention reverses the tensor product order from what most textbooks use. You **will** get confused by this at some point. It's not wrong — it's a choice of convention — but you need to be aware of it.
```

In these lectures, we will mostly follow the textbook convention ($|q_1\, q_2\rangle$ with the first qubit on the left). When we write Qiskit code, we'll note where the ordering flips.

Here's a concrete example. We apply $H$ to qubit 0 (top wire) and $I$ to qubit 1 (bottom wire). The matrix Qiskit produces is $I \otimes H$, not $H \otimes I$:

```{code-cell} python
qc = QuantumCircuit(2)
qc.h(0)   # Hadamard on qubit 0 (top wire)
print("Circuit:")
print(qc.draw())
print("\nMatrix (note: this is I⊗H, not H⊗I):")
print(np.round(Operator(qc).data, 3).real)
```

------------------------------------------------------------------------

## Part 5: The No-Cloning Theorem

### Can We Copy a Qubit?

In classical circuits, fan-out is free: if a wire carries 1, you can split it into two wires that both carry 1. Can we do the same with quantum states?

Let's try. We want a **cloning machine**: a unitary $U$ acting on two qubits (the original and a blank) such that

$$
U(|\psi\rangle \otimes |0\rangle) = |\psi\rangle \otimes |\psi\rangle
$$

for every possible qubit state $|\psi\rangle$.

### It Works for Basis States

Consider $|\psi\rangle = |0\rangle$: we need $U(|00\rangle) = |00\rangle$. That's just the identity on $|00\rangle$. Fine.

Consider $|\psi\rangle = |1\rangle$: we need $U(|10\rangle) = |11\rangle$. That's a CNOT! The CNOT gate "copies" basis states perfectly:

$$
\text{CNOT}|00\rangle = |00\rangle, \qquad \text{CNOT}|10\rangle = |11\rangle.
$$

So far so good. **CNOT clones** $|0\rangle$ and $|1\rangle$.

### It Fails for Superpositions

Now try $|\psi\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) = |+\rangle$.

```{admonition} iClicker
:class: question

CNOT applied to $|+\rangle \otimes |0\rangle$ produces:

A) $|+\rangle \otimes |+\rangle$ &emsp; B) $\frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$ &emsp; C) $|00\rangle$ &emsp; D) Cannot be determined
```

If $U$ is a cloning machine, it should produce

$$
U(|+\rangle \otimes |0\rangle) = |+\rangle \otimes |+\rangle = \frac{1}{2}(|00\rangle + |01\rangle + |10\rangle + |11\rangle).
$$

But $U$ is linear (it's a unitary matrix), so we can compute what it actually does:

$$
U\left(\frac{|0\rangle + |1\rangle}{\sqrt{2}} \otimes |0\rangle\right) = \frac{1}{\sqrt{2}}\bigl(U|00\rangle + U|10\rangle\bigr) = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle) = |\Phi^+\rangle.
$$

We get a **Bell state**, not two copies of $|+\rangle$. The CNOT *entangles* the two qubits instead of cloning the first one.

Let's see this directly:

```{code-cell} python
# CNOT "clones" basis states
# Qiskit label "XY" means |X⟩ ⊗ |Y⟩ with qubit 1 = X, qubit 0 = Y
# We use cx(1, 0): qubit 1 (left/first) controls qubit 0 (right/second)
for label in ["0", "1"]:
    sv = Statevector.from_label(label + "0")  # |label⟩ ⊗ |0⟩
    qc = QuantumCircuit(2)
    qc.cx(1, 0)  # first qubit controls second
    result = sv.evolve(qc)
    print(f"CNOT |{label}0⟩ = {result}")

print()

# CNOT FAILS to clone |+⟩
sv_plus = Statevector.from_label("+0")  # |+⟩ ⊗ |0⟩
qc = QuantumCircuit(2)
qc.cx(1, 0)
result = sv_plus.evolve(qc)
print(f"CNOT |+0⟩ = {result}")
print("This is |Φ⁺⟩ (a Bell state), NOT |+⟩⊗|+⟩!")
print()

# What |+⟩⊗|+⟩ would actually look like
plus_plus = Statevector.from_label("++")
print(f"|+⟩⊗|+⟩ = {plus_plus}")
print("↑ Four equal amplitudes — clearly different from the Bell state above.")
```

### The General Proof

This isn't special to CNOT. **No** unitary can clone arbitrary states.

```{admonition} Theorem (No-Cloning)
:class: important

There is no unitary $U$ and no blank state $|\phi\rangle$ such that $U(|\psi\rangle \otimes |\phi\rangle) = |\psi\rangle \otimes |\psi\rangle$ for every quantum state $|\psi\rangle$.
```

**Proof.** Suppose such a $U$ exists. Pick two orthogonal states $|a\rangle$ and $|b\rangle$. Then by assumption:

$$
U(|a\rangle \otimes |\phi\rangle) = |a\rangle \otimes |a\rangle
$$

$$
U(|b\rangle \otimes |\phi\rangle) = |b\rangle \otimes |b\rangle
$$

Now apply $U$ to the superposition $|\psi\rangle = \frac{1}{\sqrt{2}}(|a\rangle + |b\rangle)$.

By **linearity**:

$$
U\left(\frac{|a\rangle + |b\rangle}{\sqrt{2}} \otimes |\phi\rangle\right) = \frac{1}{\sqrt{2}}\bigl(|a\rangle \otimes |a\rangle + |b\rangle \otimes |b\rangle\bigr)
$$

But if $U$ clones, it should produce:

$$
\frac{|a\rangle + |b\rangle}{\sqrt{2}} \otimes \frac{|a\rangle + |b\rangle}{\sqrt{2}} = \frac{1}{2}\bigl(|aa\rangle + |ab\rangle + |ba\rangle + |bb\rangle\bigr)
$$

These are not the same state. The linearity result has two terms; the cloning result has four. **Contradiction.** $\square$

The essential point: **cloning is a nonlinear operation, but quantum mechanics only allows linear (unitary) evolution.** That single sentence is the entire no-cloning theorem.

### Why No-Cloning Matters

This is not just a mathematical curiosity. No-cloning has profound consequences:

-   **Quantum key distribution works.** An eavesdropper cannot copy the qubits Alice sends to Bob. Any interception disturbs the states and introduces detectable errors. (This is the subject of Lecture 3.7.)

-   **Teleportation must destroy the original.** When Alice teleports a qubit to Bob, her qubit is destroyed by measurement. If cloning were possible, teleportation would create two copies — violating no-cloning.

-   **Quantum error correction is hard.** Classically, you protect data by making backup copies. You can't copy quantum states, so quantum error correction requires fundamentally different strategies (encoding information across entangled qubits).

-   **Fan-out is not free.** Unlike classical circuits, quantum circuits cannot simply split a wire. Every use of a qubit's state must be accounted for. This is a basic constraint on quantum circuit design.

------------------------------------------------------------------------

## Summary

-   **Classical circuits:** wires carry bits, gates perform logic, fan-out (copying) is free.
-   **Quantum circuits:** wires carry qubits, gates are unitaries, measurement outputs classical bits.
-   **Key gates:**
    -   Single-qubit: $X$, $H$, $Z$, $S$, $T$ (review from Chapter 2).
    -   Two-qubit: CNOT ($C_X$), CZ ($C_Z$), SWAP; general controlled-$U$.
    -   Three-qubit: Toffoli (CCX).
    -   All built from the projector formula: $C_U = |0\rangle\langle 0| \otimes I + |1\rangle\langle 1| \otimes U$.
-   **Bell state circuit:** $H$ then CNOT converts $|00\rangle \to |\Phi^+\rangle$. Different inputs give all four Bell states.
-   **Qiskit ordering:** top wire = qubit 0 = rightmost in ket. Watch the tensor product order.
-   **No-cloning theorem:** no unitary can copy an arbitrary quantum state. Proof: linearity of quantum mechanics. Consequences for QKD, teleportation, error correction, and circuit design.

------------------------------------------------------------------------

## What's Next

Next lecture we'll get hands-on with Qiskit: build circuits, run them on a simulator and real quantum hardware, and see measurement statistics. After that, we'll use circuits + no-cloning to build our first quantum protocols: quantum key distribution and teleportation.

# HOMEWORK
Homework will be added shortly. 