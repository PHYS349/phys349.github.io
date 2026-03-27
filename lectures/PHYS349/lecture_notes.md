# Lecture Notes — Working Draft

## From Watrous: Controlled Operations and SWAP via Tensor Products

Source: quantum.cloud.ibm.com/learning/en/courses/basics-of-quantum-information/multiple-systems/quantum-information

### Controlled-Unitary Operations (Formal Definition)

The key idea: a controlled-U gate is built from **projectors tensored with operations**. Given a control qubit Q and a target system R with unitary $U$:

$$C_U = |0\rangle\langle 0| \otimes I_R \;+\; |1\rangle\langle 1| \otimes U$$

Read this as: "If the control qubit is $|0\rangle$, do nothing to the target. If the control is $|1\rangle$, apply $U$ to the target."

This is elegant because it uses the projectors $|0\rangle\langle 0|$ and $|1\rangle\langle 1|$ that the students already know from measurement theory (lecture 02_03, Malus's law). Now the same projectors build gates.

### Controlled-NOT (CNOT / CX)

Set $U = X$ (the Pauli-X / NOT gate):

$$\text{CNOT} = |0\rangle\langle 0| \otimes I + |1\rangle\langle 1| \otimes X$$

Action on basis states:
- $|00\rangle \to |00\rangle$
- $|01\rangle \to |01\rangle$
- $|10\rangle \to |11\rangle$
- $|11\rangle \to |10\rangle$

In matrix form (computational basis order $|00\rangle, |01\rangle, |10\rangle, |11\rangle$):

$$\text{CNOT} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{pmatrix}$$

**Connection to course:** Students know $|0\rangle\langle 0|$ and $|1\rangle\langle 1|$ as projectors from lecture 02_03. They know $X$ as a Pauli matrix from 02_05. The CNOT is literally just combining these two things they already know.

### Controlled-Z (CZ)

Set $U = Z$:

$$\text{CZ} = |0\rangle\langle 0| \otimes I + |1\rangle\langle 1| \otimes Z$$

$$\text{CZ} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & -1 \end{pmatrix}$$

**Nice observation:** CZ only affects $|11\rangle \to -|11\rangle$. It's symmetric — doesn't matter which qubit is "control" and which is "target." This is *not* true for CNOT.

### The SWAP Gate

For two systems X and Y:

$$\text{SWAP}|a\rangle|b\rangle = |b\rangle|a\rangle$$

Written in Dirac notation:

$$\text{SWAP} = \sum_{c,d} |c\rangle\langle d| \otimes |d\rangle\langle c|$$

For two qubits:

$$\text{SWAP} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}$$

This swaps the middle two rows/columns ($|01\rangle \leftrightarrow |10\rangle$), leaving $|00\rangle$ and $|11\rangle$ alone.

**Key decomposition:** SWAP = three CNOTs:

$$\text{SWAP} = \text{CNOT}_{12} \cdot \text{CNOT}_{21} \cdot \text{CNOT}_{12}$$

(This isn't in Watrous here but is a standard result worth showing.)

### Operations on One Qubit of a Pair

To apply $U$ to qubit X while leaving qubit Y alone:

$$U \otimes I_Y$$

Example: Hadamard on first qubit of a two-qubit system:

$$H \otimes I = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 0 & 1 & 0 \\ 0 & 1 & 0 & 1 \\ 1 & 0 & -1 & 0 \\ 0 & 1 & 0 & -1 \end{pmatrix}$$

### Toffoli Gate (CCX) — Double Controlled-NOT

$$\text{CCX} = (|00\rangle\langle 00| + |01\rangle\langle 01| + |10\rangle\langle 10|) \otimes I + |11\rangle\langle 11| \otimes X$$

"Apply X to target only when *both* control qubits are 1."

This is the quantum version of a classical AND gate (it's reversible, unlike classical AND).

### Fredkin Gate (CSWAP) — Controlled-SWAP

Control qubit + two target qubits:
- If control = $|0\rangle$: do nothing
- If control = $|1\rangle$: SWAP the two targets

---

## Pedagogical Notes

**Why this formalism is nice for PHYS 349:**

1. The $C_U = |0\rangle\langle 0| \otimes I + |1\rangle\langle 1| \otimes U$ formula directly connects projective measurement (which they learned in Ch. 2–3) to gate construction. Same math, different application.

2. Students can *derive* the CNOT matrix themselves from this formula — it's not just handed to them as a $4\times 4$ matrix.

3. The tensor product structure ($U \otimes I$ for single-qubit gates on multi-qubit systems) makes explicit what "this gate acts on qubit 1 and ignores qubit 2" means mathematically.

4. This builds naturally toward circuits: once they know CNOT and H, they can build Bell states: $(H \otimes I) \cdot \text{CNOT} |00\rangle = |\Phi^+\rangle$.

---

## Other Content from This Page (Already Covered in PHYS 349)

The page also covers tensor products of states, Bell states, GHZ/W states, partial measurement, and entanglement — all of which map to existing lectures 03_01 through 03_04. The new material for your students is primarily the **gate formalism** above.

---

## From Watrous: Multiple Systems — Qiskit Implementation

Source: quantum.cloud.ibm.com/learning/en/courses/basics-of-quantum-information/multiple-systems/qiskit-implementation

**Definitely want this — first real multi-qubit Qiskit code.**

### Key Qiskit Classes for Multi-Qubit Systems

**Statevector class:**
- `Statevector.from_label("0")`, `from_label("+")`, `from_label("r")` etc.
- `.tensor()` method for building multi-qubit states: `zero.tensor(one)` → $|01\rangle$
- `^` shorthand operator: `plus ^ minus_i`
- `.evolve(operator)` — apply a unitary to a state
- `.measure([0])` — partial measurement on specific qubits

**Operator class:**
- `Operator.from_label("H")`, `"I"`, `"X"` etc.
- `.tensor()` or `^` for building multi-qubit operators: `H ^ I ^ X`

### Building CNOT from Matrix

```python
from qiskit.quantum_info import Statevector, Operator

CX = Operator([[1,0,0,0], [0,1,0,0], [0,0,0,1], [0,0,1,0]])
psi = Statevector.from_label("+").tensor(Statevector.from_label("0"))
result = psi.evolve(CX)  # Produces Bell state |Φ⁺⟩
```

### Partial Measurement

```python
result, new_state = w.measure([0])      # Measure qubit 0 (rightmost)
result, new_state = w.measure([0, 1])   # Measure qubits 0 and 1
```

**Qiskit qubit ordering:** qubit 0 is the **rightmost** position in kets. So for $|q_1 q_0\rangle$, `measure([0])` measures $q_0$.

### Pedagogical Value

This page is the bridge from the formal tensor product math (previous section) to actual code. Students see that `Operator.from_label("H").tensor(Operator.from_label("I"))` literally is $H \otimes I$. The Statevector/Operator classes make the linear algebra tangible.

---

## From Watrous: Quantum Circuits

Source: quantum.cloud.ibm.com/learning/en/courses/basics-of-quantum-information/quantum-circuits/circuits

### The Circuit Model

Quantum circuits are a computational model where:
- **Wires** represent qubits (horizontal lines, information flows left → right)
- **Gates** represent unitary operations on those qubits
- Circuits are **acyclic** — no feedback loops (unlike classical sequential logic)

### Classical Analogy: Boolean Circuits

Watrous starts with Boolean circuits to motivate the quantum version:
- Wires carry bits (0 or 1), gates are logic operations (NOT, AND, OR, XOR)
- **Fan-out** (copying a bit to use in multiple gates) is free in classical circuits
- **Key distinction:** Fan-out is NOT available in quantum circuits (no-cloning theorem). You cannot copy an arbitrary qubit state. This is a fundamental difference between classical and quantum computation.

### Quantum Circuit Components

Three building blocks:
1. **Qubit wires** — horizontal lines, each carrying one qubit
2. **Unitary gates** — boxes/symbols on wires representing unitary operations
3. **Standard basis measurements** — meter symbol, collapses qubit to $|0\rangle$ or $|1\rangle$, outputs a classical bit (drawn as a double-line wire)

### Gate Symbol Conventions

| Gate | Symbol |
|------|--------|
| Single-qubit $U$ | Square box labeled "$U$" |
| Pauli-X / NOT | Circle with plus ($\oplus$), or box labeled $X$ |
| CNOT | Solid dot (control) connected by vertical line to $\oplus$ (target) |
| SWAP | Two $\times$ symbols connected by a vertical line |
| Controlled-$U$ | Solid dot (control) connected to box "$U$" on target |
| Toffoli (CCX) | Two solid dots connected to $\oplus$ |
| Measurement | Meter symbol, output is double wire (classical bit) |

### Qiskit Qubit Ordering (IMPORTANT)

Qiskit uses a specific convention that can be confusing:
- **Top wire** = qubit index 0 = **rightmost** position in ket notation
- **Bottom wire** = highest index = **leftmost** position in ket notation
- For a 2-qubit system with $q_0$ on top and $q_1$ on bottom, the state is written $|q_1 q_0\rangle$

This means when you see a circuit diagram with qubit 0 on top, the tensor product order is reversed from what you might expect: the matrix for "H on qubit 0, I on qubit 1" is $I \otimes H$ (not $H \otimes I$) in the standard math convention.

**Worth spending time on this in class** — students will hit this confusion immediately when using Qiskit.

### Bell State Creation Circuit (The E-bit Circuit)

The canonical two-qubit circuit: Hadamard on qubit 0, then CNOT with qubit 0 as control and qubit 1 as target.

The composite unitary is:

$$U = \text{CNOT} \cdot (H \otimes I)$$

Acting on computational basis states:

| Input | Output |
|-------|--------|
| $\|00\rangle$ | $\|\Phi^+\rangle = \frac{1}{\sqrt{2}}(\|00\rangle + \|11\rangle)$ |
| $\|01\rangle$ | $\|\Psi^+\rangle = \frac{1}{\sqrt{2}}(\|01\rangle + \|10\rangle)$ |
| $\|10\rangle$ | $\|\Phi^-\rangle = \frac{1}{\sqrt{2}}(\|00\rangle - \|11\rangle)$ |
| $\|11\rangle$ | $-\|\Psi^-\rangle = -\frac{1}{\sqrt{2}}(\|01\rangle - \|10\rangle)$ |

**Connection to course:** Students already know Bell states from lecture 03_01. Now they see *how to make them* with a circuit. This is the payoff — the formalism from the previous sections turns into something you can build and run.

### Measurement in Circuits

- Measurement gates produce a classical bit output (double-line wire)
- After measurement, the qubit collapses to the post-measurement state ($|0\rangle$ or $|1\rangle$)
- Classical bits can be used for classical control (e.g., "if measurement result = 1, apply X")
- This is essential for teleportation and QKD protocols

### No Fan-out / No Cloning

Classical circuits freely copy bits (fan-out). Quantum circuits **cannot** — the no-cloning theorem forbids copying an unknown quantum state. This is not just a practical limitation; it's a theorem.

This has real consequences:
- Can't broadcast quantum information
- Need entanglement + classical communication to "move" quantum states (teleportation)
- Makes quantum error correction fundamentally different from classical error correction

---

### Pedagogical Notes for Circuits Section

**What's genuinely new for PHYS 349 students:**

1. The **circuit model** itself — wires, gates, left-to-right flow. This is the computational framework they'll use for the rest of the course.

2. **Qiskit qubit ordering** — will cause confusion, worth a dedicated example showing that Qiskit's convention differs from textbook convention.

3. The **e-bit circuit** (H then CNOT → Bell state) — they know Bell states, but haven't seen how to *construct* them from primitive gates.

4. **No-cloning → no fan-out** distinction from classical circuits. They may have heard of no-cloning but seeing it as a computational constraint (you can't just wire-split a qubit) makes it concrete.

5. **Measurement producing classical bits** and the double-wire notation — sets up teleportation and QKD where classical communication is essential.

**What overlaps with existing lectures:**
- Gate definitions (CNOT, CZ, SWAP, Toffoli, etc.) — already captured in the section above
- Bell states themselves — covered in 03_01
- Basic quantum state notation — covered in Ch. 1–2

---

## From Watrous: Limitations on Quantum Information

Source: quantum.cloud.ibm.com/learning/en/courses/basics-of-quantum-information/quantum-circuits/limitations-on-quantum-information

**Most of this page is already covered (inner products, global phase, orthogonality). The new material worth keeping is the no-cloning theorem proof and the non-distinguishability result.**

### No-Cloning Theorem (Formal Statement + Proof)

**Theorem:** There does not exist a quantum state $|\phi\rangle$ of Y and a unitary operation $U$ on the pair (X, Y) such that

$$U(|\psi\rangle \otimes |\phi\rangle) = |\psi\rangle \otimes |\psi\rangle$$

for every quantum state $|\psi\rangle$ of X.

**Proof (by contradiction via linearity):**

Suppose such a $U$ exists. Then for two orthonormal states $|a\rangle$ and $|b\rangle$:

$$U(|a\rangle \otimes |\phi\rangle) = |a\rangle \otimes |a\rangle$$
$$U(|b\rangle \otimes |\phi\rangle) = |b\rangle \otimes |b\rangle$$

Now consider the superposition $|\psi\rangle = \frac{1}{\sqrt{2}}(|a\rangle + |b\rangle)$. By **linearity of $U$**:

$$U(|\psi\rangle \otimes |\phi\rangle) = \frac{1}{\sqrt{2}}\bigl(|a\rangle \otimes |a\rangle + |b\rangle \otimes |b\rangle\bigr)$$

But if $U$ is a cloner, it should produce:

$$|\psi\rangle \otimes |\psi\rangle = \frac{1}{2}\bigl(|a\rangle \otimes |a\rangle + |a\rangle \otimes |b\rangle + |b\rangle \otimes |a\rangle + |b\rangle \otimes |b\rangle\bigr)$$

These are not equal. Contradiction. $\square$

**Key points for students:**
- Cloning is a **nonlinear** operation — but quantum mechanics only allows linear (unitary) evolution. That's the fundamental tension.
- You **can** clone known basis states (e.g., CNOT copies $|0\rangle$ and $|1\rangle$ perfectly). The theorem says you can't clone an **arbitrary unknown** state.
- This is why quantum teleportation requires destroying the original — and why QKD is secure (an eavesdropper can't copy the qubits without disturbing them).

### Non-Orthogonal States Cannot Be Perfectly Distinguished

**Theorem:** If $\langle\phi|\psi\rangle \neq 0$, no measurement procedure can distinguish $|\psi\rangle$ from $|\phi\rangle$ with certainty.

**Proof sketch (contrapositive):** Suppose a circuit with unitary $U$ and ancilla qubits perfectly distinguishes the two states:

$$U(|0\cdots0\rangle \otimes |\psi\rangle) = |\gamma_0\rangle \otimes |0\rangle$$
$$U(|0\cdots0\rangle \otimes |\phi\rangle) = |\delta_1\rangle \otimes |1\rangle$$

Taking inner products and using unitarity ($U^\dagger U = I$ preserves inner products):

$$\langle\gamma_0|\delta_1\rangle \cdot \underbrace{\langle 0|1\rangle}_{=\,0} = \langle\psi|\phi\rangle$$

So $\langle\psi|\phi\rangle = 0$. The states must be orthogonal. $\square$

**Why this matters for the course:**
- Directly connects to quantum key distribution: an eavesdropper measuring non-orthogonal states (e.g., BB84's rectilinear vs. diagonal bases) **must** introduce errors, because they can't perfectly determine which state was sent.
- Reinforces that quantum information is fundamentally different from classical: you can always distinguish distinct classical states, but not distinct quantum states.

### Already Covered in PHYS 349

- Global vs. relative phase (lecture 02_01–02_02)
- Inner products and bra-ket notation (lecture 02_03)
- Orthonormality (lecture 02_03–02_04)

---

## From Watrous: Entanglement in Action — Introduction

Source: quantum.cloud.ibm.com/learning/en/courses/basics-of-quantum-information/entanglement-in-action/introduction

**Mostly framing/motivation, but sets up three key protocols.**

### Entanglement as a Resource

The key conceptual shift: entanglement is a **consumable resource** (like fuel). Protocols use up shared entangled pairs ("e-bits") to accomplish tasks that are impossible classically.

The Bell state $|\phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$ represents **one e-bit** of entanglement.

### Three Protocols Introduced

1. **Quantum Teleportation** — transmit a qubit using 1 e-bit + 2 classical bits
2. **Superdense Coding** — transmit 2 classical bits using 1 e-bit + 1 qubit
3. **CHSH Game** — demonstrate quantum advantage / Bell inequality violation

### Entanglement vs. Classical Correlation

Watrous makes the important distinction: the entangled state $|\phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$ looks superficially similar to the classically correlated probabilistic mixture "50% $|00\rangle$, 50% $|11\rangle$" — but they enable fundamentally different capabilities.

**Connection to PHYS 349:** Students have seen Bell states (03_01) and Bell inequality violation (03_04). The conceptual leap here is viewing entanglement as a **resource you spend** to accomplish information-processing tasks. This is the quantum information perspective.

---

## From Watrous: Quantum Teleportation

Source: quantum.cloud.ibm.com/learning/en/courses/basics-of-quantum-information/entanglement-in-action/quantum-teleportation

**This is entirely new material for the course. Core protocol — definitely include.**

### Setup

- Alice has qubit Q in unknown state $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$ that she wants to send to Bob
- Alice and Bob share an entangled pair (A, B) in the Bell state $|\phi^+\rangle$
- Alice holds qubits Q and A; Bob holds qubit B
- They also have a classical communication channel

### The Protocol (Three Steps)

**Step 1 — Alice's operations:**
- Apply CNOT with Q as control, A as target
- Apply Hadamard to Q

**Step 2 — Alice measures:**
- Measure both Q and A in the computational basis → outcomes $(b, a)$
- Send the two classical bits to Bob

**Step 3 — Bob's corrections:**

| Alice's result $(a, b)$ | Bob applies |
|--------------------------|-------------|
| $(0, 0)$ | $I$ (nothing) |
| $(0, 1)$ | $Z$ |
| $(1, 0)$ | $X$ |
| $(1, 1)$ | $ZX$ |

Bob's qubit B is now in state $\alpha|0\rangle + \beta|1\rangle$. Done.

### Full Mathematical Derivation

**Initial state** (three qubits: B, A, Q in Qiskit ordering):

$$|\pi_0\rangle = |\phi^+\rangle_{AB} \otimes |\psi\rangle_Q = \frac{1}{\sqrt{2}}\bigl(\alpha|000\rangle + \alpha|110\rangle + \beta|001\rangle + \beta|111\rangle\bigr)$$

**After CNOT** (Q controls A):

$$|\pi_1\rangle = \frac{1}{\sqrt{2}}\bigl(\alpha|000\rangle + \alpha|110\rangle + \beta|011\rangle + \beta|101\rangle\bigr)$$

**After Hadamard on Q:**

$$|\pi_2\rangle = \frac{1}{2}\bigl[(\alpha|0\rangle + \beta|1\rangle)|00\rangle + (\alpha|0\rangle - \beta|1\rangle)|01\rangle + (\alpha|1\rangle + \beta|0\rangle)|10\rangle + (\alpha|1\rangle - \beta|0\rangle)|11\rangle\bigr]$$

Reading off each measurement outcome (each with probability $1/4$):

| Measurement $(a,b)$ | Bob's state before correction | Correction | Bob's final state |
|---------------------|-------------------------------|------------|-------------------|
| $00$ | $\alpha|0\rangle + \beta|1\rangle$ | $I$ | $\alpha|0\rangle + \beta|1\rangle$ |
| $01$ | $\alpha|0\rangle - \beta|1\rangle$ | $Z$ | $\alpha|0\rangle + \beta|1\rangle$ |
| $10$ | $\beta|0\rangle + \alpha|1\rangle$ | $X$ | $\alpha|0\rangle + \beta|1\rangle$ |
| $11$ | $\beta|0\rangle - \alpha|1\rangle$ | $ZX$ | $\alpha|0\rangle + \beta|1\rangle$ |

### Key Properties

1. **The e-bit is consumed:** After the protocol, qubits Q and A have collapsed to computational basis states. The entanglement is "used up."

2. **No-cloning respected:** The original state of Q is **destroyed** by Alice's measurement. The quantum information is transferred, not copied.

3. **Alice learns nothing:** All four measurement outcomes occur with probability $1/4$ regardless of $\alpha$ and $\beta$. The measurement results are completely random — they carry zero information about the teleported state.

4. **Classical communication is essential:** Without the 2 classical bits, Bob has a maximally mixed state (he can't do anything useful). The classical bits tell him which Pauli correction to apply. This is why teleportation doesn't violate special relativity — the quantum information only becomes accessible after the classical message arrives at light speed or slower.

5. **Works for entangled inputs too:** If Q is entangled with some external system R in state $\alpha|0\rangle_Q|\gamma_0\rangle_R + \beta|1\rangle_Q|\gamma_1\rangle_R$, after teleportation Bob's qubit inherits the entanglement: $\alpha|0\rangle_B|\gamma_0\rangle_R + \beta|1\rangle_B|\gamma_1\rangle_R$. The protocol preserves all correlations.

### Pedagogical Notes

**Why this is a great lecture for PHYS 349:**

1. It synthesizes everything from the first half: Bell states, tensor products, CNOT, Hadamard, projective measurement, classical bits — all in one protocol.

2. The math is completely accessible. Students can work through the three-qubit state evolution line by line. It's a beautiful exercise in "just multiply the matrices and regroup terms."

3. It makes the no-cloning theorem feel like a feature, not a bug: teleportation is *designed* around the constraint that you can't copy quantum states.

4. Natural connection to QKD: both protocols use shared entanglement + classical communication. Teleportation moves quantum information; QKD extracts secure classical information.

5. The "Alice learns nothing" property is surprising and worth emphasizing — it's the same randomness that makes quantum mechanics useful for cryptography.

**Possible in-class Qiskit demo:** Build the teleportation circuit, run it on a simulator, verify that Bob's qubit always matches Alice's input state regardless of which measurement outcomes occurred.

---

## From Watrous: Superdense Coding

Source: quantum.cloud.ibm.com/learning/en/courses/basics-of-quantum-information/entanglement-in-action/superdense-coding

**New material — the "dual" of teleportation. Definitely include.**

### The Protocol

**Goal:** Alice sends 2 classical bits to Bob by transmitting only 1 qubit, using a pre-shared e-bit.

**Setup:** Alice and Bob share $|\phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$. Alice holds qubit A, Bob holds qubit B.

### Step 1 — Alice Encodes

Alice wants to send two classical bits $(c, d)$. She applies gates to her qubit A:
- If $d = 1$: apply $Z$
- If $c = 1$: apply $X$

This maps the shared Bell state to one of the four Bell states:

| Bits $(c,d)$ | Alice applies | Resulting state |
|---------------|---------------|-----------------|
| $00$ | $I$ | $|\phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$ |
| $01$ | $Z$ | $|\phi^-\rangle = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle)$ |
| $10$ | $X$ | $|\psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle)$ |
| $11$ | $XZ$ | $|\psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$ |

Alice then sends qubit A to Bob.

### Step 2 — Bob Decodes

Bob now holds both qubits. He applies the **reverse of the Bell state creation circuit**:
1. CNOT with A as control, B as target
2. Hadamard on A
3. Measure both qubits in the computational basis

The Bell states map back to computational basis states:

$$|\phi^+\rangle \to |00\rangle, \quad |\phi^-\rangle \to |01\rangle, \quad |\psi^+\rangle \to |10\rangle, \quad |\psi^-\rangle \to |11\rangle$$

Bob reads off $(c, d)$ directly from his measurement results.

### Why This Works (Holevo's Theorem Connection)

**Holevo's theorem** says that without shared entanglement, sending a single qubit can communicate **at most 1 classical bit**. Superdense coding beats this bound by using pre-shared entanglement — effectively doubling the classical capacity.

### Duality with Teleportation

| | Teleportation | Superdense Coding |
|---|---|---|
| **Sends** | 1 qubit (quantum info) | 2 classical bits |
| **Uses** | 1 e-bit + 2 classical bits | 1 e-bit + 1 qubit sent |
| **Direction** | Quantum → quantum | Classical → classical |

These are "dual" protocols: teleportation trades classical bits + entanglement for quantum communication; superdense coding trades a qubit + entanglement for extra classical communication.

### Pedagogical Notes

- Bob's decoding circuit is literally the **inverse** of the Bell state creation circuit (H then CNOT → CNOT then H). Students should recognize this.
- The encoding step is elegant: all four Bell states are related by single-qubit Pauli operations on one half of the pair. This connects to the Bell basis being a "Pauli orbit" of $|\phi^+\rangle$.
- Pair this with teleportation in the same lecture or back-to-back lectures — the duality is very satisfying.

---

## From Watrous: CHSH Game

Source: quantum.cloud.ibm.com/learning/en/courses/basics-of-quantum-information/entanglement-in-action/chsh-game

**Students have seen Bell inequality violation (lecture 03_04), but this game-theoretic framing is new and connects to quantum advantage.**

### The Game

A referee gives Alice question $x \in \{0,1\}$ and Bob question $y \in \{0,1\}$, chosen uniformly at random. They cannot communicate. Each responds with an answer $a, b \in \{0,1\}$.

**Winning condition:**

$$a \oplus b = x \wedge y$$

In words:
- If $(x,y) \in \{(0,0), (0,1), (1,0)\}$: Alice and Bob win if $a = b$ (same answers)
- If $(x,y) = (1,1)$: Alice and Bob win if $a \neq b$ (different answers)

### Classical Bound

**Best classical strategy wins with probability $3/4$.**

Proof: Any deterministic strategy is a fixed choice of $a(x)$ and $b(y)$. If the strategy wins for the three "match" cases, then $a(0) = b(0) = b(1) = a(1)$, which forces $a \oplus b = 0$ for $(1,1)$, but the winning condition requires $a \oplus b = 1$. So at least one of the four cases must lose. Probabilistic strategies (mixtures of deterministic ones) can't beat $3/4$ either.

### Quantum Strategy

Alice and Bob share $|\phi^+\rangle$ and use measurement in rotated bases.

**Rotation unitary:** For angle $\theta$:

$$U_\theta = \begin{pmatrix} \cos\theta & \sin\theta \\ -\sin\theta & \cos\theta \end{pmatrix}$$

This rotates the measurement basis so that measuring in the $\{|0\rangle, |1\rangle\}$ basis after applying $U_\theta$ is equivalent to measuring in $\{|\psi_\theta\rangle, |\psi_{\theta+\pi/2}\rangle\}$ where $|\psi_\theta\rangle = \cos\theta|0\rangle + \sin\theta|1\rangle$.

**Angle choices:**
- Alice: $\alpha = 0$ if $x = 0$, $\quad \alpha = \pi/4$ if $x = 1$
- Bob: $\beta = \pi/8$ if $y = 0$, $\quad \beta = -\pi/8$ if $y = 1$

**Key formula:** For shared $|\phi^+\rangle$ measured in bases at angles $\alpha$ and $\beta$:

$$\Pr(a = b) = \cos^2(\alpha - \beta), \qquad \Pr(a \neq b) = \sin^2(\alpha - \beta)$$

### Winning Probability Calculation

| $(x,y)$ | Need | Angle difference $\alpha - \beta$ | Probability |
|----------|------|----------------------------------|-------------|
| $(0,0)$ | $a = b$ | $0 - \pi/8 = -\pi/8$ | $\cos^2(\pi/8)$ |
| $(0,1)$ | $a = b$ | $0 - (-\pi/8) = \pi/8$ | $\cos^2(\pi/8)$ |
| $(1,0)$ | $a = b$ | $\pi/4 - \pi/8 = \pi/8$ | $\cos^2(\pi/8)$ |
| $(1,1)$ | $a \neq b$ | $\pi/4 - (-\pi/8) = 3\pi/8$ | $\sin^2(3\pi/8) = \cos^2(\pi/8)$ |

All four cases give the same winning probability!

$$\Pr(\text{win}) = \cos^2(\pi/8) = \frac{2 + \sqrt{2}}{4} \approx 0.854$$

### Tsirelson's Bound

This is **optimal** — no quantum strategy can do better. This upper bound is known as **Tsirelson's inequality**, the quantum analog of the classical Bell/CHSH bound.

| Strategy | Max win probability |
|----------|-------------------|
| Classical | $3/4 = 0.75$ |
| Quantum | $\frac{2+\sqrt{2}}{4} \approx 0.854$ |
| No-signaling (theoretical max) | $1.0$ |

### Pedagogical Notes

**Connection to existing PHYS 349 content:** Lecture 03_04 covers Bell inequality violation. The CHSH game reframes this as a *computational advantage*: entanglement lets you win a cooperative game more often than any classical strategy allows. Same physics, different perspective.

**What's new here:**
1. The game-theoretic framing — quantum advantage as winning probability
2. The explicit angle optimization ($\pi/8$ spacing)
3. Tsirelson's bound — there's a quantum limit too, not just a classical one
4. The $\cos^2(\alpha - \beta)$ correlation formula for Bell state measurements — beautiful and general

**Nice in-class demo:** Run the CHSH game in Qiskit many times, tally win rates, compare classical ($\leq 75\%$) vs. quantum ($\approx 85\%$).

---

## From Watrous: Entanglement in Action — Qiskit Implementation

Source: quantum.cloud.ibm.com/learning/en/courses/basics-of-quantum-information/entanglement-in-action/qiskit-implementation

**The code for all three protocols. Essential for the Qiskit-heavy second half of the course.**

### New Qiskit Concepts Introduced

**QuantumRegister / ClassicalRegister:**
```python
from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister

qubit = QuantumRegister(1, "Q")      # Alice's message qubit
ebit0 = QuantumRegister(1, "A")      # Alice's half of e-bit
ebit1 = QuantumRegister(1, "B")      # Bob's half of e-bit
a = ClassicalRegister(1, "a")        # Classical bit for measurement
b = ClassicalRegister(1, "b")        # Classical bit for measurement

circuit = QuantumCircuit(qubit, ebit0, ebit1, a, b)
```

**barrier():** Visual separator in circuit diagrams, also prevents transpiler optimization across the barrier. Useful for showing protocol stages.

**if_test() — Classical Conditioning:**
```python
with circuit.if_test((a, 1)):
    circuit.x(ebit1)      # Bob applies X if a=1
with circuit.if_test((b, 1)):
    circuit.z(ebit1)      # Bob applies Z if b=1
```

This is the Qiskit way to implement "measure then classically control" — essential for teleportation and QKD.

### Teleportation Circuit in Qiskit

```python
# Prepare e-bit
circuit.h(ebit0)
circuit.cx(ebit0, ebit1)
circuit.barrier()

# Alice's operations
circuit.cx(qubit, ebit0)
circuit.h(qubit)
circuit.barrier()

# Measurements
circuit.measure(ebit0, a)
circuit.measure(qubit, b)
circuit.barrier()

# Bob's corrections
with circuit.if_test((a, 1)):
    circuit.x(ebit1)
with circuit.if_test((b, 1)):
    circuit.z(ebit1)
```

**Verification trick:** Apply a random unitary $U$ to prepare the input state, run teleportation, then apply $U^\dagger$ to Bob's output. If teleportation worked, measuring should give $|0\rangle$ with certainty.

### Superdense Coding Circuit

```python
# Prepare e-bit
circuit.h(0)
circuit.cx(0, 1)

# Alice encodes (c, d)
if d == "1":
    circuit.z(0)
if c == "1":
    circuit.x(0)

# Bob decodes (reverse Bell circuit)
circuit.cx(0, 1)
circuit.h(0)
circuit.measure_all()
```

### CHSH Game Circuit

```python
import numpy as np

def u_theta(theta):
    """Rotation matrix for measuring in rotated basis."""
    return np.array([
        [np.cos(theta), np.sin(theta)],
        [-np.sin(theta), np.cos(theta)]
    ])

# Alice's angle: 0 or π/4
# Bob's angle: π/8 or -π/8
# Apply U_alpha to Alice's qubit, U_beta to Bob's qubit, then measure
```

### Simulation

```python
from qiskit_aer import AerSimulator

result = AerSimulator().run(circuit, shots=1000).result()
counts = result.get_counts()
```

### Pedagogical Notes

**Key Qiskit patterns students learn here:**
1. Named registers (`QuantumRegister`, `ClassicalRegister`) — organizing multi-party protocols
2. `barrier()` — structuring circuits into protocol phases
3. `if_test()` — classical control flow (measure-then-act), the basis for all adaptive protocols
4. `compose()` — building complex circuits from subcircuits
5. `AerSimulator` — running circuits and collecting statistics

These are the building blocks for everything that follows: QKD circuits, algorithm implementations, error correction.

---

# IBM Quantum Learning Classroom Modules

The following are from the standalone classroom modules (quantum.cloud.ibm.com/learning/en/modules). These are designed for undergraduate classroom use with Qiskit code, assessment questions, and instructor answer keys.

---

## Module: Get Started with Qiskit in the Classroom

Source: quantum.cloud.ibm.com/learning/en/modules/quantum-mechanics/get-started-with-qiskit

**Good first-day Qiskit module. Covers basics and includes a quantum half-adder as a capstone exercise.**

### What It Covers

1. **Classical → Quantum comparison:** bits vs qubits, classical gates (NOT, AND, OR, XOR) vs quantum gates
2. **Single-qubit gates:** X, H, Z, T with matrices and Qiskit code
3. **Multi-qubit gates:** CNOT (`qc.cx(0,1)`), SWAP (`qc.swap(0,1)`), Toffoli (`qc.ccx(0,1,2)`)
4. **Measurement:** `qc.measure(0, 0)` — destructive, probabilistic, collapses to eigenstate
5. **Qiskit Patterns workflow:** Map → Optimize → Execute → Post-process

### Key Qiskit Code Patterns

```python
from qiskit import QuantumCircuit

# Basic circuit
qc = QuantumCircuit(1, 1)   # 1 qubit, 1 classical bit
qc.h(0)                      # Hadamard
qc.measure(0, 0)             # Measure

# Transpile for hardware
from qiskit.transpiler.preset_passmanagers import generate_preset_pass_manager
pm = generate_preset_pass_manager(target=target, optimization_level=3)
qc_isa = pm.run(qc)

# Execute with SamplerV2
from qiskit_ibm_runtime import SamplerV2 as Sampler
sampler = Sampler(mode=backend)
job = sampler.run([qc_isa], shots=100)
counts = job.result()[0].data.meas.get_counts()

# Visualize
from qiskit.visualization import plot_histogram
plot_histogram(counts)
```

### Quantum Half-Adder (Nice In-Class Exercise)

```python
qc = QuantumCircuit(4)
# Set inputs a, b on qubits 0, 1
if a: qc.x(0)
if b: qc.x(1)

# Sum (XOR) → qubit 2
qc.cx(0, 2)
qc.cx(1, 2)

# Carry (AND) → qubit 3
qc.ccx(0, 1, 3)

qc.measure_all()
```

Students verify: $A + B = S + 2C$ for all input combinations.

### Little-Endian Convention

Qiskit uses little-endian: qubit 0 is the **rightmost** (least significant) bit. So `qc.cx(0, 1)` means q₀ controls q₁, and in the output bitstring, q₀ is on the right.

### What Overlaps with PHYS 349

Most of the quantum mechanics content (superposition, measurement, etc.) is review. **The new value is the Qiskit workflow** — transpilation, SamplerV2, running on real hardware, and the half-adder exercise.

---

## Module: Superposition with Qiskit

Source: quantum.cloud.ibm.com/learning/en/modules/quantum-mechanics/superposition-with-qiskit

**Good for introducing Bloch sphere visualization and the two Qiskit primitives (Sampler vs Estimator).**

### Key New Concepts

**Sampler vs Estimator — Two Qiskit Primitives:**

| Primitive | What it does | Output |
|-----------|-------------|--------|
| `SamplerV2` | Repeats circuit many times, samples outcomes | Counts dictionary (`{"0": 512, "1": 488}`) |
| `EstimatorV2` | Computes expectation value of an observable | Single number ($\langle Z \rangle$, $\langle X \rangle$, etc.) |

```python
# Sampler usage
from qiskit_ibm_runtime import SamplerV2 as Sampler
sampler = Sampler(mode=backend)
job = sampler.run([qc_isa], shots=1000)
counts = job.result()[0].data.meas.get_counts()

# Estimator usage
from qiskit.quantum_info import Pauli
from qiskit_ibm_runtime import EstimatorV2 as Estimator
obs = Pauli("Z")
estimator = Estimator(mode=backend)
job = estimator.run([[qc_isa, obs_isa]])
expectation_value = job.result()[0].data.evs
```

**Bloch Sphere Visualization:**
```python
from qiskit.visualization import plot_bloch_multivector
plot_bloch_multivector(statevector)
```

General single-qubit state: $|\psi\rangle = \cos(\theta/2)|0\rangle + e^{i\phi}\sin(\theta/2)|1\rangle$

**Interference Demo:** Apply H, then phase gate P($\pi$), then H again:
- The $|0\rangle$ amplitudes interfere constructively
- The $|1\rangle$ amplitudes interfere destructively
- Result: always measure $|1\rangle$

This is a great demo — students see that a phase shift (invisible in Z-measurement) becomes visible through interference.

### Gates Introduced

- **H:** Creates equal superposition
- **P(φ):** Phase gate, applies $e^{i\phi}$ to $|1\rangle$ component. `qc.p(np.pi, 0)`
- **SX:** $\sqrt{X}$ gate, 90° rotation around x-axis. `qc.sx(0)`
- Double-H returns to original state (H is its own inverse)

### What Overlaps with PHYS 349

Superposition, Bloch sphere, measurement probabilities — all review. **New value:** Sampler vs Estimator distinction, Bloch sphere plotting in Qiskit, interference demo with phase gate.

---

## Module: Stern-Gerlach Measurements with Qiskit

Source: quantum.cloud.ibm.com/learning/en/modules/quantum-mechanics/stern-gerlach-measurements-with-qiskit

**Excellent module — maps the physical Stern-Gerlach experiment directly onto qubit measurements.**

### Five Progressive Experiments

1. **Single measurement:** Initialize $|\psi\rangle = [a, b]$ via `qc.initialize([a, b])`, measure in Z-basis
2. **Ensemble measurement:** Repeat with `shots=100+` to observe probabilistic distribution
3. **Random spin initialization:** Use `qc.rx(θ, 0)` and `qc.rz(φ, 0)` with random angles to simulate "oven" producing random spin states
4. **Repeated measurements:** Two consecutive measurements on the same qubit → always get same result (collapse)
5. **Basis rotation:** Apply H before measurement to measure in X-basis instead of Z-basis

### Measurement in Different Bases

| Basis | Gate before measurement | Eigenstates |
|-------|------------------------|-------------|
| Z (default) | None | $\|0\rangle, \|1\rangle$ |
| X | Hadamard (H) | $\|+\rangle = \frac{1}{\sqrt{2}}(\|0\rangle + \|1\rangle)$, $\|-\rangle = \frac{1}{\sqrt{2}}(\|0\rangle - \|1\rangle)$ |
| Y | $S^\dagger$ then H | $\|+i\rangle = \frac{1}{\sqrt{2}}(\|0\rangle + i\|1\rangle)$, $\|-i\rangle = \frac{1}{\sqrt{2}}(\|0\rangle - i\|1\rangle)$ |

### Connection to Spin Operators

$$S_z = \frac{\hbar}{2}\sigma_z, \quad S_x = \frac{\hbar}{2}\sigma_x, \quad S_y = \frac{\hbar}{2}\sigma_y$$

Eigenvalues: $\pm\hbar/2$ for each component.

### Key Qiskit Methods

- `qc.initialize([a, b])` — prepare arbitrary single-qubit state
- `qc.rx(theta, 0)`, `qc.rz(phi, 0)` — parameterized rotation gates
- Consecutive measurements: `qc.measure(0, 0)` then `qc.measure(0, 1)` — demonstrates collapse

### Pedagogical Value

This maps beautifully onto your existing Stern-Gerlach / Malus's law lectures (02_01–02_03). Students who learned the physics now see it implemented as quantum circuits. The "two consecutive measurements always agree" experiment is a powerful demonstration of wavefunction collapse.

### What Overlaps with PHYS 349

The physics is entirely covered (Stern-Gerlach, spin-1/2, measurement). **New value:** Qiskit implementation of these experiments, parameterized gates, basis rotation technique for measuring in X and Y bases, and the collapse demo.

---

## Module: Exploring Uncertainty with Qiskit

Source: quantum.cloud.ibm.com/learning/en/modules/quantum-mechanics/exploring-uncertainty-with-qiskit

**Nice experimental verification of uncertainty relations on actual quantum hardware.**

### Key Equations

**Robertson uncertainty relation (generalized Heisenberg):**

$$\Delta A \cdot \Delta B \geq \frac{1}{2}|\langle [A, B] \rangle|$$

**Pauli commutation relations:**

$$[X, Z] = -2iY \implies \Delta X \cdot \Delta Z \geq |\langle Y \rangle|$$
$$[X, Y] = 2iZ \implies \Delta X \cdot \Delta Y \geq |\langle Z \rangle|$$
$$[Y, Z] = 2iX \implies \Delta Y \cdot \Delta Z \geq |\langle X \rangle|$$

**Uncertainty for Pauli operators** (since $S^2 = I$):

$$(\Delta S)^2 = 1 - \langle S \rangle^2$$

So $\Delta S = \sqrt{1 - \langle S \rangle^2}$ — uncertainty is fully determined by the expectation value.

### Experimental Protocol

1. Prepare states $|\psi(\theta)\rangle = R_Y(\theta)|0\rangle$ for $\theta \in [0, 2\pi]$
2. Measure $\langle X \rangle$, $\langle Y \rangle$, $\langle Z \rangle$ using EstimatorV2
3. Compute $\Delta X$, $\Delta Y$, $\Delta Z$ from expectation values
4. Plot $\Delta A \cdot \Delta B$ vs $\frac{1}{2}|\langle [A,B] \rangle|$ — verify the product always exceeds the bound

### Basis Rotation for Measuring Different Observables

| Observable | Circuit modification before measurement |
|-----------|----------------------------------------|
| Z | None (direct measurement) |
| X | Apply H |
| Y | Apply $S^\dagger$ then H |

### Pedagogical Value

Students see uncertainty relations verified experimentally on real IBM hardware (ibm_sherbrooke). The key insight: for states along the Z-axis ($\theta = 0$ or $\pi$), $\Delta Z = 0$ but $\Delta X = \Delta Y = 1$ — maximum certainty in one component means maximum uncertainty in the others.

### What Overlaps with PHYS 349

Uncertainty relations, commutators, Pauli matrices — all review from lectures 02_04–02_05. **New value:** Experimental verification on quantum hardware via EstimatorV2, the $(\Delta S)^2 = 1 - \langle S\rangle^2$ formula, and the systematic sweep over states.

---

## Module: Bell's Inequality with Qiskit

Source: quantum.cloud.ibm.com/learning/en/modules/quantum-mechanics/bells-inequality-with-qiskit

**Overlaps significantly with lecture 03_04 but has a clean Qiskit implementation.**

### Setup

Anti-correlated Bell state: $|\psi\rangle = \frac{1}{\sqrt{2}}(|+\rangle_L|-\rangle_R - |-\rangle_L|+\rangle_R)$

Prepared from $|00\rangle$:
```python
qc.x(0)          # |0⟩ → |1⟩
qc.x(1)
qc.h(0)          # Create superposition
qc.cx(0, 1)      # Entangle
```

### Three Measurement Axes at 120° Separation

Basis rotation via $R_Y(\theta)$:
- Axis 1: $\theta = 0$ (Z-basis)
- Axis 2: $\theta = -2\pi/3$
- Axis 3: $\theta = -4\pi/3$

### The Inequality

**Hidden variables bound:** $P_\text{same} \leq 4/9 \approx 0.444$

**Quantum prediction:** $P_\text{same} = 1/2 = 0.500$

For measurement angle $\theta$: $P_\text{same} = \sin^2(\theta/2)$

Averaging over three 120° configurations: $P_\text{same,QM} = 1/2 > 4/9$. Violation confirmed.

### Pedagogical Value

This is a different Bell inequality formulation than CHSH (which uses correlators). The 120° three-axis version with $P_\text{same}$ is perhaps more physically intuitive — students can picture three Stern-Gerlach orientations. Good complement to the CHSH game from Watrous.

### What Overlaps with PHYS 349

Bell states, Bell inequality — covered in 03_04. **New value:** Qiskit circuit implementation, $R_Y(\theta)$ for basis rotation, running on real hardware, and the specific three-axis formulation.
