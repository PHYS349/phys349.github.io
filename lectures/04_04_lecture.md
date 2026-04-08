---
kernelspec:
  name: python3
  display_name: Python 3
---

# Lecture 4.4 — Quantum Teleportation and Superdense Coding

## Where We Left Off

In Lecture 4.3, we saw how the laws of quantum mechanics enable secure key distribution. We ended with a problem: fiber loss limits QKD to a few hundred kilometers, and the long-term solution — quantum repeaters — requires a protocol we haven't learned yet: **teleportation**.

Today we ask a different question: **if Alice and Bob already share entanglement, what can they do with it?**

The answer is remarkable. Entanglement is not just a strange correlation — it is a **resource**. With one shared Bell pair, Alice and Bob can:

- **Teleport** an unknown qubit from Alice to Bob using only local operations and two classical bits
- **Superdense code**: send two classical bits by transmitting only one qubit

These two protocols are among the most beautiful results in quantum information. They are simple enough to derive exactly, but deep enough to reveal what entanglement is really for. They also connect directly to the quantum repeaters we need for long-distance QKD.

------------------------------------------------------------------------

## Part 1: Bell States as a Resource

Before we build either protocol, we need one key tool: the **Bell basis**.

### The Four Bell States

We met these in Lecture 3.2, and we've been using them ever since. The four Bell states are:

$$
|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle), \qquad |\Phi^-\rangle = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle)
$$

$$
|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle), \qquad |\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)
$$

These are not just four interesting examples. They form an **orthonormal basis** for the two-qubit Hilbert space — just like $\{|00\rangle, |01\rangle, |10\rangle, |11\rangle\}$, but entangled. Any two-qubit state can be written as a superposition of Bell states.

### Pauli Operations Map Between Bell States

Here is the fact that makes both teleportation and superdense coding work. If Alice and Bob share $|\Phi^+\rangle$ and Alice applies a **single-qubit Pauli gate** to her half, the shared state transforms into a different Bell state:

| Alice applies | Shared state becomes |
|:-------------:|:--------------------:|
| $I$ | $\lvert\Phi^+\rangle$ |
| $Z$ | $\lvert\Phi^-\rangle$ |
| $X$ | $\lvert\Psi^+\rangle$ |
| $XZ$ | $-\lvert\Psi^-\rangle$ |

Let's verify one: $Z$ on Alice's qubit of $|\Phi^+\rangle$:

$$
(Z \otimes I)\frac{1}{\sqrt{2}}(|00\rangle + |11\rangle) = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle) = |\Phi^-\rangle \quad \checkmark
$$

The four Pauli operations $\{I, X, Z, XZ\}$ map $|\Phi^+\rangle$ to all four Bell states. Since Bell states are orthogonal, they are **perfectly distinguishable** by measurement. This simple fact is the engine behind both protocols.

```{code-cell} python
:tags: [hide-input]
from qiskit.quantum_info import Statevector, Operator
from qiskit.circuit.library import CXGate
import numpy as np

# Verify all four Pauli-Bell mappings
phi_plus = Statevector.from_label('00').evolve(Operator.from_label('HI')).evolve(Operator(CXGate()))

paulis = {'I': 'II', 'Z': 'ZI', 'X': 'XI', 'XZ': 'XI'}  # Applied to qubit 0 (left/Alice)
bell_labels = ['Φ+', 'Φ-', 'Ψ+', 'Ψ-']

# Manually construct
I2 = np.eye(4)
Z_I = np.kron(np.array([[1,0],[0,-1]]), np.eye(2))
X_I = np.kron(np.array([[0,1],[1,0]]), np.eye(2))
XZ_I = X_I @ Z_I

ops = [('I', I2), ('Z', Z_I), ('X', X_I), ('XZ', XZ_I)]

print("Alice applies → Shared state")
print("-" * 40)
for name, op in ops:
    result = Statevector(op @ phi_plus.data)
    # Identify which Bell state
    print(f"  {name:>2}  →  {result}")
```

### Creating and Measuring Bell States

The circuit to create $|\Phi^+\rangle$ from $|00\rangle$ is H on the first qubit followed by CNOT:

$$
|00\rangle \xrightarrow{H \otimes I} \frac{1}{\sqrt{2}}(|00\rangle + |10\rangle) \xrightarrow{\text{CNOT}} \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle) = |\Phi^+\rangle
$$

The **reverse** circuit — CNOT followed by H — is a **Bell measurement**: it maps each Bell state back to a unique computational basis state, so a standard measurement in the $Z$-basis distinguishes all four.

| Bell state | After CNOT + H | Measurement outcome |
|:----------:|:--------------:|:-------------------:|
| $\lvert\Phi^+\rangle$ | $\lvert 00\rangle$ | 00 |
| $\lvert\Psi^+\rangle$ | $\lvert 01\rangle$ | 01 |
| $\lvert\Phi^-\rangle$ | $\lvert 10\rangle$ | 10 |
| $\lvert\Psi^-\rangle$ | $\lvert 11\rangle$ | 11 |

```{admonition} iClicker
:class: question
Alice and Bob share $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$. Alice applies $Z$ to her qubit. Which Bell state results?

A) $|\Phi^+\rangle$ &emsp; B) $|\Phi^-\rangle$ &emsp; C) $|\Psi^+\rangle$ &emsp; D) $|\Psi^-\rangle$
```

```{admonition} A Bell Pair Is a Resource
:class: important
A shared Bell pair is not just "two correlated qubits." It is a reusable information-theoretic resource — an **e-bit** — that can be spent to accomplish tasks impossible with classical resources alone. Both protocols today consume one e-bit per use.
```

------------------------------------------------------------------------

## Part 2: The Problem — How Do You Send a Quantum State?

Suppose Alice has a qubit in some unknown state $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$ and wants Bob to have that same state. How can she do this?

**Attempt 1 — Send the qubit physically.** Works if Alice has a quantum channel, but fiber loss makes this impractical beyond a few hundred kilometers (Lecture 4.3).

**Attempt 2 — Measure and send the result.** Alice measures $|\psi\rangle$, gets 0 or 1, and tells Bob. But measurement **destroys** the superposition. One classical bit cannot encode two continuous parameters ($\alpha$, $\beta$). The information is irretrievably lost.

**Attempt 3 — Copy the qubit.** The no-cloning theorem (Lecture 4.1) forbids this.

Classical communication alone can't do it. Direct quantum transmission is impractical at long range. Copying is forbidden. What if Alice and Bob share a Bell pair?

```{admonition} iClicker
:class: question
Alice wants to send Bob an unknown qubit $|\psi\rangle$. Why can't she just measure it and send the result?

A) Measurement is too slow &emsp; B) Measurement collapses the state — she can't recover $\alpha$ and $\beta$ from one measurement

C) Bob doesn't have a quantum computer &emsp; D) The no-cloning theorem prevents measurement
```

------------------------------------------------------------------------

## Part 3: The Teleportation Protocol

### The Setup

Alice holds three qubits:
- **Qubit $C$**: the unknown state $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$
- **Qubit $A$**: her half of a Bell pair $|\Phi^+\rangle_{AB}$

Bob holds:
- **Qubit $B$**: the other half of the Bell pair

### The Full Derivation

**Step 0 — Initial state:**

$$
|\Psi_0\rangle = |\psi\rangle_C \otimes |\Phi^+\rangle_{AB} = \frac{1}{\sqrt{2}}\Big[\alpha|000\rangle + \alpha|011\rangle + \beta|100\rangle + \beta|111\rangle\Big]
$$

where the ordering is $|C\, A\, B\rangle$.

**Step 1 — Alice applies CNOT** (qubit $C$ controls, qubit $A$ target). CNOT flips $A$ when $C = 1$:

$$
|\Psi_1\rangle = \frac{1}{\sqrt{2}}\Big[\alpha|000\rangle + \alpha|011\rangle + \beta|110\rangle + \beta|101\rangle\Big]
$$

**Step 2 — Alice applies Hadamard to qubit $C$.** Using $H|0\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ and $H|1\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$:

$$
|\Psi_2\rangle = \frac{1}{2}\Big[|00\rangle_{CA}(\alpha|0\rangle + \beta|1\rangle)_B + |01\rangle_{CA}(\alpha|1\rangle + \beta|0\rangle)_B + |10\rangle_{CA}(\alpha|0\rangle - \beta|1\rangle)_B + |11\rangle_{CA}(\alpha|1\rangle - \beta|0\rangle)_B\Big]
$$

```{admonition} Check Yourself
:class: tip
Verify this by expanding all eight terms from Step 1 after the Hadamard and regrouping by the $CA$ indices. It's pure algebra — careful bookkeeping of signs — and worth doing once by hand.
```

Recognizing the Pauli operators acting on $|\psi\rangle$:

$$
|\Psi_2\rangle = \frac{1}{2}\Big[|00\rangle \cdot I|\psi\rangle + |01\rangle \cdot X|\psi\rangle + |10\rangle \cdot Z|\psi\rangle + |11\rangle \cdot XZ|\psi\rangle\Big]
$$

This is the key result. Alice's CNOT + H has rewritten the three-qubit state so that **each possible measurement outcome for Alice's qubits is paired with Bob's qubit in a state related to $|\psi\rangle$ by a known Pauli operation**.

**Step 3 — Alice measures** qubits $C$ and $A$. She gets one of $\{00, 01, 10, 11\}$, each with probability $1/4$.

**Step 4 — Classical communication.** Alice sends her two bits to Bob (phone, email, carrier pigeon — any classical channel).

**Step 5 — Bob's correction:**

| Alice's result | Bob's state | Bob applies | Result |
|:--------------:|:-----------:|:-----------:|:------:|
| 00 | $\alpha\lvert 0\rangle + \beta\lvert 1\rangle$ | $I$ | $\lvert\psi\rangle$ |
| 01 | $\alpha\lvert 1\rangle + \beta\lvert 0\rangle$ | $X$ | $\lvert\psi\rangle$ |
| 10 | $\alpha\lvert 0\rangle - \beta\lvert 1\rangle$ | $Z$ | $\lvert\psi\rangle$ |
| 11 | $\alpha\lvert 1\rangle - \beta\lvert 0\rangle$ | $ZX$ | $\lvert\psi\rangle$ |

After correction, Bob's qubit is in state $|\psi\rangle$. The quantum state has been **teleported**.

```{admonition} iClicker
:class: question
In teleportation, Alice measures her two qubits and gets result $(1, 0)$. What gate must Bob apply?

A) $I$ &emsp; B) $X$ &emsp; C) $Z$ &emsp; D) $XZ$
```

```{admonition} Why does Alice's measurement not reveal $\alpha$ and $\beta$?
:class: note
Alice's four outcomes each occur with probability $1/4$, **regardless** of what $|\psi\rangle$ is. Her measurement tells her which Pauli correction Bob needs — nothing about the amplitudes. This is essential: if Alice could learn the state, she could prepare a copy, violating no-cloning.
```

### The Circuit

```{code-cell} python
:tags: [hide-input]
from qiskit import QuantumCircuit
import matplotlib.pyplot as plt

qc = QuantumCircuit(3, 2)

# Create Bell pair (A=1, B=2)
qc.h(1)
qc.cx(1, 2)
qc.barrier(label="Bell pair")

# Alice's operations on C=0 and A=1
qc.cx(0, 1)
qc.h(0)
qc.barrier(label="Alice")

# Measure
qc.measure(0, 0)
qc.measure(1, 1)

# Bob's corrections
with qc.if_test((1, 1)):
    qc.x(2)
with qc.if_test((0, 1)):
    qc.z(2)

fig, ax = plt.subplots(figsize=(10, 3))
qc.draw("mpl", ax=ax)
ax.set_title("Quantum Teleportation Circuit", fontsize=12)
plt.tight_layout()
plt.show()
```

```{admonition} iClicker
:class: question
After Alice measures but before she sends her classical bits to Bob, what does Bob know about his qubit?

A) It's already in state $|\psi\rangle$ &emsp; B) It's in one of four states, equally likely — he doesn't know which

C) It's still entangled with Alice's qubits &emsp; D) It's in state $|0\rangle$
```

```{admonition} iClicker — Hard Question
:class: question
Alice teleports $|+\rangle$ to Bob. She measures her two qubits and gets $(1, 0)$. Before Bob applies his correction, his qubit is in state:

A) $|+\rangle$ &emsp; B) $|-\rangle$ &emsp; C) $|0\rangle$ &emsp; D) $|1\rangle$
```

------------------------------------------------------------------------

## Part 4: Why Teleportation Doesn't Break Physics

### No Faster-Than-Light Communication

Bob's qubit is in a random state until he receives Alice's two classical bits. Without Alice's message, his reduced density matrix is $\frac{I}{2}$ — the maximally mixed state, carrying zero information about $|\psi\rangle$. The classical bits travel at most at the speed of light.

Teleportation requires **both** entanglement and classical communication. Neither alone suffices. This is fully consistent with special relativity and the no-signaling theorem (Lecture 3.3).

### No-Cloning Is Respected

Alice's original qubit is **destroyed** by her measurement. After measuring, qubits $C$ and $A$ are in one of $\{|00\rangle, |01\rangle, |10\rangle, |11\rangle\}$. The superposition $\alpha|0\rangle + \beta|1\rangle$ no longer exists anywhere on Alice's side. The quantum information has **moved**, not been copied.

### The E-Bit Is Consumed

The Bell pair is used up. After teleportation, the three qubits are in a product state — no entanglement remains. To teleport again, Alice and Bob need a **fresh** Bell pair. Entanglement is a resource that gets spent.

```{admonition} Resource Accounting
:class: important
**Teleportation consumes:** 1 e-bit + 2 classical bits sent

**Teleportation produces:** 1 qubit transmitted

**Teleportation destroys:** Alice's original qubit
```

```{admonition} Common Misconceptions
:class: warning
**"Teleportation moves matter."** No — the physical particle stays put. Only the quantum state is transferred.

**"Teleportation copies the state."** No — Alice's original is destroyed. This is transfer, not cloning.

**"Teleportation is faster than light."** No — Bob cannot use his qubit until Alice's classical bits arrive.

**"Entanglement alone sends messages."** No — entanglement creates correlations, but cannot transmit controllable information without classical communication.
```

```{admonition} iClicker
:class: question
Does teleportation allow faster-than-light communication?

A) Yes — Bob gets the state instantly &emsp; B) No — Bob needs Alice's classical bits, which travel at light speed or slower

C) Only if they use more entangled pairs &emsp; D) It depends on the distance
```

```{admonition} iClicker
:class: question
After teleportation, Alice's original qubit is:

A) Unchanged &emsp; B) Still in state $|\psi\rangle$ &emsp; C) Destroyed — collapsed into a computational basis state &emsp; D) Entangled with Bob's qubit
```

------------------------------------------------------------------------

## Part 5: Teleportation in Qiskit

### The Deferred Measurement Approach

Instead of classically conditioned gates (which are tricky to simulate), we use the **deferred measurement principle**: replace classical conditioning with quantum-controlled gates (CX and CZ from Alice's qubits to Bob's), then defer all measurements to the end. The physics is identical.

```{code-cell} python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector, partial_trace
import numpy as np

def teleportation_circuit(theta, phi):
    """Teleportation circuit for input state R_y(theta) R_z(phi) |0>.
    Uses deferred measurement (CX, CZ instead of classical conditioning)."""
    qc = QuantumCircuit(3)

    # Prepare Alice's unknown state on qubit 0
    qc.ry(theta, 0)
    qc.rz(phi, 0)
    qc.barrier(label="ψ ready")

    # Bell pair on qubits 1 and 2
    qc.h(1)
    qc.cx(1, 2)
    qc.barrier(label="Bell pair")

    # Alice: CNOT then Hadamard
    qc.cx(0, 1)
    qc.h(0)
    qc.barrier(label="Alice done")

    # Bob's corrections (deferred)
    qc.cx(1, 2)
    qc.cz(0, 2)

    return qc

qc = teleportation_circuit(np.pi/4, 0.0)
qc.draw("mpl")
```

### Statevector Verification

We verify that Bob's qubit (qubit 2) ends up in the correct state by tracing out Alice's qubits:

```{code-cell} python
def verify_teleportation(theta, phi):
    """Check that qubit 2 matches the target state."""
    qc = teleportation_circuit(theta, phi)
    sv = Statevector.from_instruction(qc)

    # Trace out Alice's qubits (0 and 1)
    rho_bob = partial_trace(sv, [0, 1])

    # Target state
    qc_target = QuantumCircuit(1)
    qc_target.ry(theta, 0)
    qc_target.rz(phi, 0)
    target = Statevector.from_instruction(qc_target)
    rho_target = np.outer(target.data, target.data.conj())

    fidelity = np.real(np.trace(rho_bob.data @ rho_target))
    return fidelity

# Test several states
print(f"{'State':>35}  {'Fidelity':>10}")
print("-" * 49)
for label, th, ph in [
    ("cos(π/8)|0⟩ + sin(π/8)|1⟩", np.pi/4, 0.0),
    ("|+⟩", np.pi/2, 0.0),
    ("|1⟩", np.pi, 0.0),
    ("general (θ=1.2, φ=0.7)", 1.2, 0.7),
    ("general (θ=2.5, φ=3.1)", 2.5, 3.1),
]:
    print(f"{label:>35}  {verify_teleportation(th, ph):>10.6f}")
```

Fidelity 1.000000 for every state — teleportation works perfectly in the ideal case.

### The $U^\dagger$ Verification Trick

On real (noisy) hardware, we can't inspect the statevector. Instead, if the input state was prepared by $U|0\rangle$, we apply $U^\dagger$ to Bob's qubit after teleportation. If the protocol worked, Bob should measure $|0\rangle$ with certainty:

```{code-cell} python
from qiskit_aer import AerSimulator

theta, phi = np.pi / 3, np.pi / 5

qc = QuantumCircuit(3, 1)
qc.ry(theta, 0)
qc.rz(phi, 0)
qc.barrier()
qc.h(1)
qc.cx(1, 2)
qc.barrier()
qc.cx(0, 1)
qc.h(0)
qc.barrier()
qc.cx(1, 2)
qc.cz(0, 2)
qc.barrier()

# Apply U† to Bob's qubit
qc.rz(-phi, 2)
qc.ry(-theta, 2)
qc.measure(2, 0)

sim = AerSimulator()
result = sim.run(qc, shots=10000).result()
counts = result.get_counts()
print(f"After teleportation + U†, Bob measures: {counts}")
print(f"Probability of |0⟩: {counts.get('0', 0) / 10000:.4f}")
```

```{admonition} The $U^\dagger$ Trick
:class: tip
This is a general technique for verifying quantum protocols. If the input was $U|0\rangle$, then applying $U^\dagger$ to a perfect output gives $|0\rangle$. Any deviation from 100% tells you the fidelity. It works on real hardware without needing statevector access.
```

------------------------------------------------------------------------

## Part 6: Superdense Coding

### The Dual Question

Teleportation uses entanglement to transmit **quantum** information:

$$\text{1 e-bit} + \text{2 classical bits} \longrightarrow \text{transmit 1 qubit}$$

Can entanglement boost **classical** communication instead? The answer is **superdense coding** (Bennett and Wiesner, 1992):

$$\text{1 e-bit} + \text{1 qubit sent} \longrightarrow \text{transmit 2 classical bits}$$

```{admonition} iClicker
:class: question
Without pre-shared entanglement, how many classical bits can one transmitted qubit carry?

A) 0 &emsp; B) 1 &emsp; C) 2 &emsp; D) Unlimited
```

### Why Entanglement Is Needed

Without entanglement, sending one qubit can carry at most one classical bit. You might think: Alice can prepare one of four states — say $|0\rangle$, $|1\rangle$, $|+\rangle$, $|-\rangle$ — encoding two bits. But Bob **cannot perfectly distinguish** all four, because $|0\rangle$ and $|+\rangle$ are not orthogonal: $|\langle 0|+\rangle|^2 = 1/2$. No measurement can perfectly tell apart four non-orthogonal states.

With a shared Bell pair, the situation changes completely. The four Bell states **are** orthogonal. Alice's single-qubit Pauli operations map $|\Phi^+\rangle$ to all four Bell states, and Bob can perfectly distinguish them with a Bell measurement. The entanglement provides the extra degree of freedom.

```{admonition} iClicker
:class: question
Why can't Alice encode 2 bits in four single-qubit states (like $|0\rangle, |1\rangle, |+\rangle, |-\rangle$) and skip the entanglement?

A) A qubit can't exist in four states &emsp; B) Non-orthogonal states cannot all be perfectly distinguished by measurement

C) Bell states are impossible to measure &emsp; D) The no-cloning theorem forbids it
```

### The Protocol

**Step 1 — Encoding.** Alice applies a Pauli to her half of $|\Phi^+\rangle$ based on her two-bit message:

| Message $(b_1, b_2)$ | Alice applies | Shared state becomes |
|:---------------------:|:-------------:|:--------------------:|
| 00 | $I$ | $\lvert\Phi^+\rangle$ |
| 01 | $X$ | $\lvert\Psi^+\rangle$ |
| 10 | $Z$ | $\lvert\Phi^-\rangle$ |
| 11 | $ZX$ | $-\lvert\Psi^-\rangle$ |

**Step 2 — Transmission.** Alice sends her qubit to Bob. Now Bob has both qubits.

**Step 3 — Decoding.** Bob applies the inverse Bell circuit (CNOT then H) and measures. Each Bell state maps to a unique computational basis outcome, recovering Alice's message.

```{code-cell} python
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator

def superdense_coding(b1, b2):
    """Alice sends two classical bits (b1, b2) to Bob."""
    qc = QuantumCircuit(2, 2)

    # Bell pair
    qc.h(0)
    qc.cx(0, 1)
    qc.barrier(label="Bell pair")

    # Alice encodes on qubit 0
    if b2 == 1:
        qc.x(0)
    if b1 == 1:
        qc.z(0)
    qc.barrier(label="Alice encodes")

    # Bob decodes: inverse Bell circuit
    qc.cx(0, 1)
    qc.h(0)
    qc.barrier(label="Bob decodes")

    qc.measure([0, 1], [0, 1])
    return qc

# Test all four messages
sim = AerSimulator()
print("Message  →  Bob measures")
print("-" * 30)
for b1 in [0, 1]:
    for b2 in [0, 1]:
        qc = superdense_coding(b1, b2)
        result = sim.run(qc, shots=1000).result()
        counts = result.get_counts()
        print(f"  ({b1},{b2})   →  {counts}")
```

### The Duality

Teleportation and superdense coding exchange quantum and classical resources:

| | Teleportation | Superdense Coding |
|---|---|---|
| **Transmits** | 1 unknown qubit | 2 classical bits |
| **Consumes** | 1 e-bit + 2 classical bits sent | 1 e-bit + 1 qubit sent |
| **Alice does** | Bell measurement | Pauli encoding |
| **Bob does** | Pauli correction | Bell measurement |

The deep point: **entanglement is a resource that can be spent to enhance either quantum or classical communication.** Teleportation converts entanglement + classical bits into a qubit transfer. Superdense coding converts entanglement + a qubit transfer into two classical bits.

------------------------------------------------------------------------

## Part 7: Entanglement Swapping and Quantum Repeaters

### Teleportation as a Building Block

What happens if the "unknown state" Alice teleports is itself one half of an entangled pair? Then teleportation becomes **entanglement swapping** — creating entanglement between particles that have never interacted.

### The Protocol

Alice–Charlie share $|\Phi^+\rangle$. Charlie–Bob share a separate $|\Phi^+\rangle$. Charlie performs a Bell measurement on his two qubits (one from each pair) and communicates the result classically. After corrections, **Alice and Bob are now entangled** — even though no quantum information was ever sent directly between them.

```{code-cell} python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector, partial_trace
import numpy as np

# 4 qubits: Alice(0), Charlie_L(1), Charlie_R(2), Bob(3)
qc = QuantumCircuit(4)

# Bell pair 1: Alice — Charlie_L
qc.h(0)
qc.cx(0, 1)

# Bell pair 2: Charlie_R — Bob
qc.h(2)
qc.cx(2, 3)
qc.barrier(label="Two Bell pairs")

# Charlie's Bell measurement (qubits 1 and 2)
qc.cx(1, 2)
qc.h(1)
qc.barrier(label="Charlie BSM")

# Corrections on Bob (deferred)
qc.cx(2, 3)
qc.cz(1, 3)

# Check: are Alice and Bob entangled?
sv = Statevector.from_instruction(qc)
rho_AB = partial_trace(sv, [1, 2])

bell_plus = np.array([1, 0, 0, 1]) / np.sqrt(2)
rho_bell = np.outer(bell_plus, bell_plus.conj())
fidelity = np.real(np.trace(rho_AB.data @ rho_bell))
print(f"Fidelity of Alice-Bob state with |Φ+⟩: {fidelity:.6f}")
```

Alice and Bob are entangled with fidelity 1.000 — and they never interacted directly.

```{admonition} iClicker
:class: question
In entanglement swapping, Charlie performs a Bell measurement on his two qubits. After this, Alice and Bob are entangled even though:

A) They shared a Bell pair all along &emsp; B) Charlie sent them entangled photons

C) They never directly interacted — the entanglement was "swapped" via Charlie's measurement &emsp; D) This violates the no-cloning theorem
```

### Quantum Repeaters

This is the building block for **quantum repeaters**. To create entanglement over 1,000 km, break the path into 50 km segments. Create Bell pairs across each short segment, then chain them together via entanglement swapping:

$$
A \underset{50\,\text{km}}{\longleftrightarrow} C_1 \underset{50\,\text{km}}{\longleftrightarrow} C_2 \underset{50\,\text{km}}{\longleftrightarrow} \cdots \underset{50\,\text{km}}{\longleftrightarrow} B
$$

After entanglement swapping at each relay, Alice and Bob share a Bell pair over 1,000 km — without any single photon traveling more than 50 km. Each relay needs a **quantum memory** to store its half of the Bell pair while waiting, which is why quantum repeaters are not yet deployed. But teleportation is the protocol that makes the concept possible.

### Stepping Back: Entanglement as a Resource

We began the course with qubits as abstract state vectors and Bell states as interesting examples. Now those ideas are operational:

- **Bell states** are a computational basis and a communication resource
- **Pauli gates** move between Bell states — the engine of both protocols
- **Bell measurements** extract global information from entangled systems
- **Teleportation** transfers quantum states; **superdense coding** boosts classical capacity
- **Entanglement swapping** connects remote nodes into networks

The real lesson is not two clever circuits. It is that **entanglement is a usable resource that changes what communication can do**.

------------------------------------------------------------------------

## Summary

- **The four Bell states** form an orthonormal basis. Pauli operations on one qubit map $|\Phi^+\rangle$ to all four Bell states — this is the key tool for both protocols.

- **Quantum teleportation** transmits an unknown qubit using 1 e-bit + 2 classical bits. Alice performs a Bell measurement (CNOT + H + measure); Bob applies a Pauli correction ($I$, $X$, $Z$, or $ZX$). The original is destroyed, no cloning is violated, and no information travels faster than light.

- **Superdense coding** is the dual: 1 e-bit + 1 qubit sent → 2 classical bits. Alice encodes with a Pauli; Bob decodes with a Bell measurement. Entanglement doubles the classical capacity because the four Bell states are orthogonal.

- **Entanglement swapping** is teleportation applied to one half of an entangled pair, creating entanglement between particles that never interacted. This is the building block for quantum repeaters and the future quantum internet.

- **Entanglement is a resource** — consumed in each use, enabling tasks impossible with classical resources alone.

------------------------------------------------------------------------

## What's Next

Next lecture: **Quantum Hardware Platforms** — how qubits are physically built (superconducting circuits, trapped ions, neutral atoms, photonics) and how the gates we've been writing on paper are actually implemented in the lab.

------------------------------------------------------------------------

## References

- C. H. Bennett, G. Brassard, C. Crépeau, R. Jozsa, A. Peres, and W. K. Wootters, "Teleporting an unknown quantum state via dual classical and Einstein-Podolsky-Rosen channels," *Phys. Rev. Lett.* **70**, 1895 (1993).
- C. H. Bennett and S. J. Wiesner, "Communication via one- and two-particle operators on Einstein-Podolsky-Rosen states," *Phys. Rev. Lett.* **69**, 2881 (1992).
- D. Bouwmeester, J.-W. Pan, K. Mattle, M. Eibl, H. Weinfurter, and A. Zeilinger, "Experimental quantum teleportation," *Nature* **390**, 575 (1997).
- M. Żukowski, A. Zeilinger, M. A. Horne, and A. K. Ekert, "Event-ready-detectors Bell experiment via entanglement swapping," *Phys. Rev. Lett.* **71**, 4287 (1993).
- J.-W. Pan, D. Bouwmeester, H. Weinfurter, and A. Zeilinger, "Experimental entanglement swapping: Entangling photons that never interacted," *Phys. Rev. Lett.* **80**, 3891 (1998).
