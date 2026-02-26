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

$$P(0,0), \quad P(0,1), \quad P(1,0), \quad P(1,1)$$

That's $2 \times 2 = 4$ outcomes. For $N$ marbles: $2^N$ outcomes. This combinatorial explosion isn't quantum mechanics yet — it's just counting. But quantum mechanics inherits this structure.

The question is: **what joint probability distribution describes the pair?**

The individual marginals ($P_A$ and $P_B$) don't tell you the answer. The correlations matter.

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

For independent systems, the joint probability factors:
$$P_{AB}(i, j) = P_A(i) \cdot P_B(j)$$

Let's check. The marginals of distribution (C) are $P_A(0) = 0.5$ and $P_A(1) = 0.5$, and likewise $P_B(0) = 0.5$, $P_B(1) = 0.5$. The product gives $P_A(0) \cdot P_B(0) = 0.25 \neq 0.5 = P(0,0)$.

The joint distribution **cannot be written as a product of marginals.** These marbles are **correlated**.

This is a classical correlation — like Bertlmann's mismatched socks (one always pink, one always blue). The correlation was established when someone prepared the pair. There's nothing mysterious here: a hidden variable (the preparation procedure) explains everything.

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

```{admonition} Classical Correlations
:class: note

Classical correlations arise from shared history or common causes. Learning about one part of a correlated system reveals information about the other part, but doesn't affect it.

The information was always there — you just didn't know it.
```

Keep this in your back pocket: we're about to see quantum states with exactly the same measurement statistics — outcomes 00 and 11, or outcomes 01 and 10, each 50/50. The question will be: **is the explanation the same?** Can we always say "the answer was determined in advance"?

---

## Part 2: Now Qubits

### One Qubit (Review)

A single qubit lives in a 2-dimensional Hilbert space:
$$|\Psi\rangle = c_0|0\rangle + c_1|1\rangle$$

It's a wave over two configurations, fully described by a point on the Bloch sphere — 2 real degrees of freedom (after normalization and global phase). We can represent it as a column vector:

$$|\Psi\rangle = \begin{pmatrix} c_0 \\ c_1 \end{pmatrix}$$


### Quantum Mechanics Is a Wave over Configurations

Here is the key conceptual leap. For a single qubit, the state is a superposition over two possibilities:

$$|\psi\rangle = c_0|0\rangle + c_1|1\rangle$$

Each possibility gets a complex amplitude. The squared magnitudes give probabilities.

For two qubits, the state is a superposition over all four configurations of the pair:

$$|\Psi\rangle = c_{00}|00\rangle + c_{01}|01\rangle + c_{10}|10\rangle + c_{11}|11\rangle$$

Each configuration gets its own complex amplitude. This is the essential structure of quantum mechanics: **it assigns a complex number (an amplitude) to every possible configuration of the system.** The probabilities are the squared magnitudes: $P(01) = |c_{01}|^2$, etc.

For $N$ qubits, there are $2^N$ configurations, each with its own amplitude. This exponential growth is the fundamental resource of quantum computing — and the reason quantum systems are hard to simulate classically.

Quantum dynamics is a wave over configurations.  Everything that can happen does happen, just with some complex amplitude (amplitude and phase).


---




### Two Indepedent Qubits: The Tensor Product

Now suppose we have two qubits: qubit A in state $|\Psi\rangle = a|0\rangle + b|1\rangle$ and qubit B in state $|\phi\rangle = c|0\rangle + d|1\rangle$.

How do we describe the joint system? We need a mathematical operation that combines configuration spaces. This is the **tensor product**, written $\otimes$:

$$|\Psi\rangle \otimes |\phi\rangle = (a|0\rangle + b|1\rangle) \otimes (c|0\rangle + d|1\rangle)$$

The tensor product distributes just like ordinary multiplication — $(a + b)(c + d) = ac + ad + bc + bd$:

$$= ac|00\rangle + ad|01\rangle + bc|10\rangle + bd|11\rangle$$

where $|0\rangle \otimes |0\rangle \equiv |00\rangle$, and so on.

This describes **two independent particles that have never interacted.** Each qubit has its own state, the amplitudes are just products of individual amplitudes ($\Psi_{ij} = u_i v_j$), and knowledge about one qubit tells you nothing about the other. This is the quantum analog of independent marbles.

You will see several equivalent notations:
$$|0\rangle \otimes |1\rangle \;=\; |0\rangle|1\rangle \;=\; |0,1\rangle \;=\; |01\rangle$$

We'll mostly use the compact form $|01\rangle$.





### General 2 qubit state 

For two qubits, the state is a superposition over all four configurations of the pair:

$$|\Psi\rangle = c_{00}|00\rangle + c_{01}|01\rangle + c_{10}|10\rangle + c_{11}|11\rangle$$
How much information does a two-qubit state carry?

$$\text{4 complex numbers} \;\to\; 8 \text{ real DOF}$$
$$\text{normalization} \;\to\; -1$$
$$\text{global phase} \;\to\; -1$$

$$\boxed{2 \text{ qubits} = 6 \text{ DOF}}$$

Now compare: two *independent* qubits, each on its own Bloch sphere, have $2 + 2 = 4$ degrees of freedom. Where do the extra 2 come from?

$$\underbrace{6}_{\text{two-qubit state}} \;=\; \underbrace{2}_{\text{Bloch sphere A}} \;+\; \underbrace{2}_{\text{Bloch sphere B}} \;+\; \underbrace{2}_{\text{correlations}}$$

Those extra 2 degrees of freedom encode correlations between the qubits. A product state like $|+\rangle \otimes |0\rangle$ uses only the 4 Bloch sphere DOFs — the correlations are zero, and the qubits are independent. But some two-qubit states use all 6 DOFs. These are the entangled states, and they're what we'll explore next.























### The Kronecker Product (Vector Form)

As column vectors, the tensor product becomes the **Kronecker product**:

$$\begin{pmatrix} u_0 \\ u_1 \end{pmatrix} \otimes \begin{pmatrix} v_0 \\ v_1 \end{pmatrix} = \begin{pmatrix} u_0 v_0 \\ u_0 v_1 \\ u_1 v_0 \\ u_1 v_1 \end{pmatrix}$$

Multiply each component of the first vector by the entire second vector, and stack the results. Two $n = 2$ vectors produce one $n = 4$ vector. For $N$ qubits: dimension $2^N$.

| Qubits | Configurations | Complex numbers needed |
|--------|----------------|----------------------|
| 1 | 2 | 2 |
| 2 | 4 | 4 |
| 10 | 1,024 | 1,024 |
| 20 | ~1 million | ~1 million |
| 50 | ~$10^{15}$ | ~$10^{15}$ |
| 300 | ~$10^{90}$ | More than atoms in the universe |

A classical computer simulating 50 qubits must store $2^{50} \approx 10^{15}$ complex numbers — about a petabyte of memory. A quantum computer with 50 qubits just... has 50 qubits. This is the source of quantum computational power: the state space grows exponentially, but the physical resources grow linearly.

We can also arrange the amplitudes as a $2 \times 2$ **amplitude table**:

$$C = \begin{pmatrix} u_0 v_0 & u_0 v_1 \\ u_1 v_0 & u_1 v_1 \end{pmatrix}$$

Rows correspond to qubit A's state ($|0\rangle$ or $|1\rangle$), columns to qubit B's. For a product state, this matrix is an outer product of two vectors: $C = \vec{u}\,\vec{v}^T$.

<!-- 
## Homework

### Problem 1: Tensor Product Practice

Compute the following tensor products and write the result as a 4-component column vector.

**(a)** $|0\rangle \otimes |1\rangle$

**(b)** $|1\rangle \otimes |+\rangle$ where $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$

**(c)** $|+\rangle \otimes |-\rangle$ where $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$

**(d)** $|+\rangle \otimes |+\rangle$

---

### Problem 2: The Amplitude Table and Determinant Test

Arrange the amplitudes of a two-qubit state as a $2 \times 2$ matrix:
$$C = \begin{pmatrix} C_{00} & C_{01} \\ C_{10} & C_{11} \end{pmatrix}$$

**(a)** Show that if $|\Psi\rangle = |\psi\rangle_A \otimes |\phi\rangle_B$ is a product state, then $C$ is a rank-1 matrix (i.e., $C = \vec{u}\,\vec{v}^T$, the outer product of two vectors).

**(b)** For a $2 \times 2$ matrix, rank 1 is equivalent to $\det(C) = 0$. Use this to verify that $|+\rangle \otimes |0\rangle$ is separable.

**(c)** Show that $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$ has $\det(C) = 1/2 \neq 0$, and is therefore entangled.

**(d)** Compute $\det(C)$ for all four Bell states. What do you notice about $|\det(C)|$?

---

### Problem 3: Identifying Product vs. Entangled

For each state, compute $\det(C)$ of the amplitude table. If separable, write it as $|\psi\rangle \otimes |\phi\rangle$. If entangled, state the determinant.

**(a)** $|\Psi\rangle = \frac{1}{2}(|00\rangle + |01\rangle + |10\rangle + |11\rangle)$

**(b)** $|\Psi\rangle = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle)$

**(c)** $|\Psi\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |01\rangle)$

**(d)** $|\Psi\rangle = \frac{1}{2}(|00\rangle + |01\rangle + |10\rangle - |11\rangle)$

---

### Problem 4: Bell State Measurements

**(a)** Write down the ZZ measurement probabilities for all four Bell states. Verify that $|\Phi^+\rangle$ and $|\Phi^-\rangle$ are indistinguishable under ZZ, and so are $|\Psi^+\rangle$ and $|\Psi^-\rangle$.

**(b)** Using the X-basis expansions derived in lecture (for both $\Psi$ and $\Phi$ pairs), write down the XX measurement probabilities for all four Bell states. Verify that XX distinguishes the $+/-$ pairs that ZZ cannot.

**(c)** Using the correlation table from lecture, which single measurement basis (ZZ, XX, or YY) can distinguish the most Bell states? Which pairs remain ambiguous?

**(d)** **YY for the $\Psi$ states.** The Y basis states are $|+i\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ and $|-i\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$. Express $|\Psi^+\rangle$ and $|\Psi^-\rangle$ in the Y basis and verify the YY column of the correlation table.

**(e)** Why do you need at least two measurement bases to identify all four Bell states?

---

### Problem 5: Partial Measurement

When you measure only one qubit of a two-qubit state, the other qubit's state changes. To find the post-measurement state, project onto the measured outcome and renormalize: apply $(|a\rangle\langle a| \otimes I)$ to the joint state.

**(a)** For $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$: if Alice measures qubit A and gets $|0\rangle$, what state is qubit B in? What if she gets $|1\rangle$?

**(b)** Repeat for $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$.

**(c)** Before Alice measures, what is the "state" of qubit B alone in $|\Phi^+\rangle$? Can it be described by a state vector $\alpha|0\rangle + \beta|1\rangle$? Explain. (We'll return to this when we study density matrices.)

**(d)** **No-signaling check.** From Bob's perspective alone (without knowing Alice's result), does he see different measurement statistics depending on whether Alice has measured or not? Consider: if Alice measures Z, she gets 0 or 1 with 50% each, and Bob's qubit becomes $|0\rangle$ or $|1\rangle$ accordingly. If Bob then measures Z, what are his probabilities? Compare to what he'd see if Alice hadn't measured at all. (This is why entanglement can't be used for faster-than-light communication.)

---

### Problem 6: Creating Entanglement

**(a)** Explain in words why a product gate $A \otimes B$ (where $A$ acts on qubit A and $B$ acts on qubit B, independently) cannot create entanglement from a product state.

**(b)** The CNOT gate is defined by: $|00\rangle \to |00\rangle$, $|01\rangle \to |01\rangle$, $|10\rangle \to |11\rangle$, $|11\rangle \to |10\rangle$. Can CNOT be written as $A \otimes B$ for any single-qubit gates $A$ and $B$? (Hint: consider what happens to $|10\rangle$ vs $|00\rangle$.)

**(c)** Apply CNOT to the product state $|+\rangle \otimes |+\rangle$. Write the result as a sum of computational basis states. Is it entangled? Use the determinant test.

**(d)** The circuit Hadamard-then-CNOT creates $|\Phi^+\rangle$ from $|00\rangle$. Show that different inputs to the same circuit produce all four Bell states:

| Input | Output |
|---|---|
| $\|00\rangle$ | $\|\Phi^+\rangle$ |
| $\|10\rangle$ | $\|\Phi^-\rangle$ |
| $\|01\rangle$ | $\|\Psi^+\rangle$ |
| $\|11\rangle$ | $\|\Psi^-\rangle$ |

Verify at least two of these by tracing through the circuit.

---

### Problem 7: Three Qubits

The tensor product extends to more than two qubits.

**(a)** How many basis states are there for three qubits? List them all.

**(b)** Write the state $|+\rangle \otimes |0\rangle \otimes |1\rangle$ as a sum of computational basis states.

**(c)** Consider the three-qubit state:
$$|GHZ\rangle = \frac{1}{\sqrt{2}}(|000\rangle + |111\rangle)$$
This is called the Greenberger-Horne-Zeilinger state. Is it a product state? How would you test this? (Hint: the $2 \times 2$ determinant test doesn't directly apply. Try to factor it.)

**(d)** If you measure all three qubits of $|GHZ\rangle$ in the computational basis, what outcomes are possible?

---

### Problem 8: Qiskit Exploration

```{admonition} Qiskit Qubit Ordering
:class: warning

Qiskit uses **little-endian** ordering: qubit 0 is the *rightmost* bit in output strings. So `qc.x(0)` produces state $|01\rangle$ in our notation, but Qiskit displays it as `'10'`. The state vectors are always correct — it's just the string labels that can be confusing.
```

```python
# Setup
import numpy as np
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit_aer import AerSimulator

simulator = AerSimulator()
```

**(a)** Create the Bell state $|\Phi^+\rangle$ in Qiskit using Hadamard + CNOT. Print the statevector and verify it matches $\frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$. Then add measurements and run 10,000 shots. Verify you only get 00 and 11.

```python
# Starter code
qc_bell = QuantumCircuit(2)
qc_bell.h(0)
qc_bell.cx(0, 1)

state = Statevector(qc_bell)
print("Statevector:", state)
```

**(b)** Create $|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle)$. (Hint: start from $|\Phi^+\rangle$ and apply $X$ to one qubit.) Measure 10,000 times and verify you only get 01 and 10.

**(c)** Create the GHZ state $\frac{1}{\sqrt{2}}(|000\rangle + |111\rangle)$ using H on qubit 0, then CNOT(0,1), then CNOT(0,2). Measure all three qubits and verify the correlations.

**(d)** Write a function that takes a two-qubit `Statevector` and computes $\det(C)$ of the amplitude table. Test it on a product state and an entangled state.

```python
def det_test(statevector):
    """Compute det(C) for a two-qubit state."""
    amps = np.array(statevector)
    C = np.array([[amps[0], amps[1]],
                  [amps[2], amps[3]]])
    return C[0,0]*C[1,1] - C[0,1]*C[1,0]

# Test on a product state: |+⟩|0⟩
qc_prod = QuantumCircuit(2)
qc_prod.h(0)
print("Product state det:", det_test(Statevector(qc_prod)))

# Test on Bell state
print("Bell state det:", det_test(state))
```

**(e)** **XX measurement of Bell states.** Create $|\Phi^+\rangle$ and measure both qubits in the X basis (apply H to both qubits before measuring). Verify that you only see 00 and 11 (meaning $++$ and $--$). Then do the same for $|\Phi^-\rangle$ and verify you see 01 and 10 (meaning $+-$ and $-+$). Compare to the correlation table from lecture.
