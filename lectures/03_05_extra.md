
## Lecture 3.5 — Quantum Key Distribution

### The No-Cloning Theorem

Before diving into quantum cryptography, we need a result that is both profound and remarkably easy to prove: **quantum states cannot be copied.**

**Theorem (No-Cloning):** There is no unitary operation $U$ that can clone an arbitrary quantum state:

$$U|\psi\rangle|0\rangle = |\psi\rangle|\psi\rangle \quad \text{for all } |\psi\rangle$$

**Proof:** Suppose such a $U$ exists. Then for two states $|\psi\rangle$ and $|\phi\rangle$:

$$U|\psi\rangle|0\rangle = |\psi\rangle|\psi\rangle, \qquad U|\phi\rangle|0\rangle = |\phi\rangle|\phi\rangle$$

Take the inner product of both sides (unitarity preserves inner products):

$$\langle\psi|\phi\rangle\langle 0|0\rangle = \langle\psi|\phi\rangle\langle\psi|\phi\rangle$$

$$\langle\psi|\phi\rangle = \langle\psi|\phi\rangle^2$$

This equation has only two solutions: $\langle\psi|\phi\rangle = 0$ (orthogonal) or $\langle\psi|\phi\rangle = 1$ (identical). A cloning machine that works for two non-orthogonal states is impossible. $\square$

Three lines. One of the most important results in quantum information, and the proof is just the linearity of quantum mechanics.

**Why it matters:** In classical communication, Eve can always copy a signal without disturbing it — she taps the line, copies the bits, and neither Alice nor Bob ever knows. In quantum communication, this is impossible. Any attempt by Eve to extract information necessarily disturbs the quantum state. This is the physical foundation of quantum cryptography.

---

### The Problem: Sharing Secrets

Alice and Bob want to communicate privately. The gold standard of cryptography is the **one-time pad**: if they share a secret random key as long as the message, and each bit is used only once, the encryption is provably unbreakable — no amount of computational power can crack it.

The problem isn't encryption. The problem is **key distribution**: how do Alice and Bob get a shared secret key in the first place, especially if they can only communicate over channels that an eavesdropper (Eve) might be monitoring?

Classically, this is fundamentally hard. Any classical signal Eve intercepts can be copied without Alice or Bob knowing. Quantum mechanics changes this, because quantum states cannot be copied (no-cloning) and measurement disturbs the state.

---

### BB84: The First Quantum Key Distribution Protocol

Bennett and Brassard (1984) proposed the first QKD protocol. It doesn't require entanglement — just single qubits.

#### The Protocol

**Step 1 — Transmission.** Alice generates random bits and, for each bit, randomly chooses one of two bases to encode it:

| Bit | Z basis encoding | X basis encoding |
|---|---|---|
| 0 | $\|0\rangle$ | $\|+\rangle$ |
| 1 | $\|1\rangle$ | $\|-\rangle$ |

She sends the encoded qubit to Bob through a quantum channel.

**Step 2 — Measurement.** Bob doesn't know which basis Alice used, so he randomly chooses Z or X for each qubit and records his result.

**Step 3 — Sifting.** Over a public classical channel, Alice and Bob announce which *basis* they used for each qubit (but not the bit values). They keep only the rounds where they happened to choose the same basis — on average, half the rounds. On those rounds, their bit values are guaranteed to agree.

**Step 4 — Error estimation.** They sacrifice a random subset of their sifted key and compare bit values publicly. If the error rate is below a threshold, they proceed. If it's above the threshold, an eavesdropper may be present, and they abort.

**Step 5 — Privacy amplification.** They apply classical post-processing to distill a shorter but highly secure final key.

#### Why It's Secure

Suppose Eve intercepts a qubit, measures it, and sends a replacement to Bob.

- If Eve guesses the right basis: she gets the correct bit and sends the right state. No disturbance.
- If Eve guesses the wrong basis: she gets a random result, and sends Bob a state in the wrong basis. When Bob measures in the correct basis, he gets the wrong answer 50% of the time.

Eve guesses wrong half the time, and when she does, she introduces a 50% error on those bits. Net effect: Eve's attack introduces a **25% error rate** on the sifted key. Alice and Bob can detect this in Step 4.

The key insight: **any attempt to gain information about the key necessarily disturbs it.** This is not a technological limitation — it's a law of physics (no-cloning). Eve cannot copy the qubit and measure her copy without disturbing Alice's.

---

### E91: Entanglement-Based QKD

Ekert (1991) proposed a fundamentally different approach that uses entangled pairs and connects security directly to Bell's theorem.

#### The Protocol

**Setup:** A source distributes Bell pairs $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$ — one qubit to Alice, one to Bob. (The source could even be controlled by Eve — it doesn't matter.)

**Step 1 — Measurement.** On each round, Alice and Bob each randomly choose a measurement axis. Their settings are:

$$\text{Alice: } a_1 = 0°, \quad a_2 = 45°, \quad a_3 = 90°$$
$$\text{Bob: } b_1 = 45°, \quad b_2 = 90°, \quad b_3 = 135°$$

All six angles lie in the xz-plane, evenly spaced at $45°$:

$$\underset{a_1}{\overset{0°}{|}} \quad\quad \underset{a_2, b_1}{\overset{45°}{|}} \quad\quad \underset{a_3, b_2}{\overset{90°}{|}} \quad\quad \underset{b_3}{\overset{135°}{|}}$$

The key feature: Alice's and Bob's settings **partially overlap** ($a_2 = b_1 = 45°$ and $a_3 = b_2 = 90°$) but also include non-overlapping settings that test Bell's inequality.

**Step 2 — Sifting into two groups:**

- **Key generation rounds:** Alice and Bob used the **same** measurement axis: $(a_2, b_1)$ both at $45°$, or $(a_3, b_2)$ both at $90°$. These give $E = -\cos(0°) = -1$, meaning perfectly anti-correlated outcomes. Bob flips his bit → shared secret key bits.

- **Bell test rounds:** The remaining setting combinations — $(a_1, b_1)$, $(a_1, b_3)$, $(a_2, b_2)$, $(a_3, b_1)$, etc. — are used to compute the CHSH parameter $S$.

**Step 3 — Bell test.** Alice and Bob compute $|S|$ from the Bell test rounds. If $|S| > 2$, the correlations are genuinely quantum and no eavesdropper could have predetermined the outcomes. If $|S| \leq 2$, the correlations are classically explainable and the key may be compromised — they abort.

**Step 4 — Privacy amplification.** Same as BB84.

#### Why the Bell Test Certifies Security

This is the beautiful connection between foundations and applications. Suppose Eve intercepts both qubits, measures them, and sends replacement qubits to Alice and Bob. Eve now has classical information about the outcomes. But the replacement qubits she sends are in some separable (product) state — or at best, a state whose correlations Eve partially knows.

Here's the key: any state whose correlations Eve can predict is constrained by the CHSH inequality. If Alice and Bob observe $|S| > 2$, their correlations are **stronger than anything Eve could have manufactured or predicted classically.** The Bell violation certifies that:

1. The qubits were genuinely entangled.
2. The measurement outcomes were not predetermined.
3. Eve could not have full information about the key.

The more $|S|$ exceeds 2, the less information Eve can have — and the more secret key Alice and Bob can extract.

---

### Device-Independent QKD

E91 leads to an even stronger idea: **device-independent QKD (DI-QKD)**.

In standard QKD (including BB84), security proofs assume that Alice's and Bob's devices work as specified — that their qubit source is honest, their detectors measure what they claim to measure, etc. But what if Eve manufactured the devices? What if there's a hardware backdoor?

DI-QKD removes this assumption entirely. The security guarantee comes from one thing alone: **the observed Bell violation.** The reasoning is:

- A Bell violation certifies that the correlations are non-classical.
- Non-classical correlations cannot be fully known to any third party (this is a theorem).
- Therefore, some of Alice and Bob's shared bits must be private — regardless of what's inside the boxes.

Alice and Bob treat their devices as **black boxes**. They don't need to know or trust anything about the internal workings. They only need to verify that the input-output statistics violate a Bell inequality.

This is the strongest form of cryptographic security known. It rests on only two assumptions:

1. **Quantum mechanics is correct** (specifically, the no-signaling principle).
2. **Alice's and Bob's labs are secure** (Eve can't look at their screens).

No assumptions about the devices, the source, or the quantum channel.

---

### Comparison of QKD Protocols

| | BB84 | E91 | DI-QKD |
|---|---|---|---|
| **Entanglement required?** | No | Yes | Yes |
| **Security based on** | No-cloning + measurement disturbance | Bell inequality violation | Bell inequality violation |
| **Trust devices?** | Yes | Partially | **No** |
| **Key rate** | High | Moderate | Low (current technology) |
| **Experimental status** | Deployed commercially | Demonstrated | Proof-of-principle |

#### The Practical Tradeoff

DI-QKD is the theoretically ideal protocol, but it's experimentally demanding — you need high-fidelity entangled pairs, low-loss channels, and efficient detectors to close the detection loophole. Current implementations achieve low key rates over short distances. BB84 is much easier to implement and is already deployed in commercial systems (with trusted devices).

The field is moving toward closing this gap. As entangled photon sources and detectors improve, DI-QKD may become practical — giving us cryptographic security guaranteed by the laws of physics, with no trust in hardware required.

#### Qiskit Exercise

Simulate the BB84 protocol: Alice prepares random qubits in random bases (Z or X), Bob measures in random bases. Implement sifting (discard mismatched bases) and verify that the sifted key matches perfectly. Then add an eavesdropper: Eve intercepts each qubit, measures in a random basis, and forwards her result to Bob. Measure the error rate on the sifted key — you should find approximately 25%. Plot the error rate as a function of the fraction of qubits Eve intercepts.

---

### The Big Picture

This chapter has traced a remarkable arc:

| Concept | What it means |
|---|---|
| **Tensor product** | Two qubits live in a 4D Hilbert space, richer than two separate Bloch spheres |
| **Entanglement** | Correlations that cannot be explained by independent qubit states |
| **Bell states** | Four maximally entangled basis states, distinguished by measurements in different bases |
| **EPR** | Einstein argued these correlations imply hidden variables |
| **Bell's theorem** | Hidden variables predict $\|S\| \leq 2$; quantum mechanics predicts $2\sqrt{2}$ |
| **Experiments** | Nature violates the inequality — local realism is ruled out |
| **No-cloning** | Quantum states cannot be copied — the foundation of quantum cryptography |
| **QKD** | The Bell violation becomes a *resource* for provably secure communication |

The thread connecting all of this is the idea we started with in Chapter 2: **quantum mechanics assigns complex amplitudes to configurations, and interference between those amplitudes produces correlations that no classical theory can reproduce.** For one qubit, this gives us the Bloch sphere and quantum gates. For two qubits, it gives us entanglement, Bell violations, and the beginnings of quantum information science.

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




---

### Two Non-Independent Particles

The tensor product describes particles that have never interacted — independent qubits, each minding its own business. But the most general two-qubit state is richer:

$$|\Psi\rangle = C_{00}|00\rangle + C_{01}|01\rangle + C_{10}|10\rangle + C_{11}|11\rangle$$

with four independent complex amplitudes and normalization $\sum|C_{ij}|^2 = 1$.

For a tensor product state, the amplitudes factor: $C_{ij} = u_i v_j$. But in general, the four amplitudes $C_{00}, C_{01}, C_{10}, C_{11}$ can be anything (subject to normalization). When they *don't* factor — when the amplitude table $C$ cannot be written as an outer product $\vec{u}\,\vec{v}^T$ — the state describes particles that are **correlated in a way that goes beyond any product description**. These are entangled states, born from interactions between the particles (or from correlated creation, like photon pair generation).

This is our mantra from Chapter 2, extended to two particles:

> A quantum state is a wave over configurations.

For one qubit, the configurations are $|0\rangle$ and $|1\rangle$. For two qubits, the configurations are all four pairs: $|00\rangle, |01\rangle, |10\rangle, |11\rangle$. Each configuration gets a complex amplitude — a magnitude and a phase. In quantum mechanics, everything that can happen does happen, just with some amplitude and phase.

The notation $|ab\rangle$ means: "qubit A is in state $a$, qubit B is in state $b$." The probability of measuring outcome $(a, b)$ is $|C_{ab}|^2$. -->




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



### Problem 1: Classical vs Quantum Correlations

**(a)** Bertlmann randomly chooses to put the pink sock on his left foot or right foot with equal probability. Write down the joint probability distribution $P(\text{left}, \text{right})$ for sock colors.

**(b)** What is the correlation $E = \langle L \cdot R \rangle$ if we assign pink $= +1$ and blue $= -1$?

**(c)** Now Bertlmann randomly rotates both socks before putting them on, so each foot independently gets pink or blue with 50% probability. What is the correlation now?

**(d)** Explain why case (a) is analogous to entanglement measured in one basis, but case (c) is what the hidden variable model predicts for other bases.

---

### Problem 2: Verifying the X-Basis Expansion

Prove that $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle) = \frac{1}{\sqrt{2}}(|++\rangle + |--\rangle)$.

**(a)** Express $|0\rangle$ and $|1\rangle$ in terms of $|+\rangle$ and $|-\rangle$.

**(b)** Substitute into $|00\rangle$ and expand.

**(c)** Substitute into $|11\rangle$ and expand.

**(d)** Add and simplify to get $|00\rangle + |11\rangle = |++\rangle + |--\rangle$.

---

### Problem 3: The Singlet State

Consider the singlet state $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$.

**(a)** What are the probabilities of the four outcomes when both qubits are measured in the Z basis?

**(b)** Show that $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|-+\rangle - |+-\rangle)$ in the X basis.

**(c)** What are the probabilities of the four outcomes when both qubits are measured in the X basis?

**(d)** For the Y basis, $|+i\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ and $|-i\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$. Show that the singlet has perfect anti-correlation in the Y basis too.

---

### Problem 4: The Correlation Function

For the singlet state, the correlation function is $E(\hat{a}, \hat{b}) = -\hat{a} \cdot \hat{b} = -\cos\theta$.

**(a)** What is $E$ when Alice and Bob measure along the same axis ($\theta = 0$)?

**(b)** What is $E$ when they measure along perpendicular axes ($\theta = 90°$)?

**(c)** What is $E$ when they measure along opposite axes ($\theta = 180°$)?

**(d)** At what angle $\theta$ is $E = -1/2$?

---

### Problem 5: Designing Hidden Variable Models

The simple hidden variable model (mixture of $|00\rangle$ and $|11\rangle$) failed because it predicted no X-basis correlations. Let's try to do better.

**(a)** Design a hidden variable model where each pair is assigned one of four hidden states:
- State A: "return +1 for both Z and X measurements" for both qubits
- State B: "return -1 for both Z and X measurements" for both qubits  
- State C: "return +1 for Z, -1 for X" for both qubits
- State D: "return -1 for Z, +1 for X" for both qubits

With equal probability (25% each), what does this model predict for:
- Z-basis correlation?
- X-basis correlation?

**(b)** Can you choose different probabilities for states A, B, C, D to match the quantum predictions (perfect correlation in both bases)? Try to find probabilities that work, or explain why it's impossible.

**(c)** Now consider a third basis, Y. In quantum mechanics, $|\Phi^+\rangle$ also has perfect correlation in Y. Can your model from part (b) also give perfect Y-correlation? Why or why not?

**(d)** This illustrates why hidden variable models fail: they can match quantum predictions in one or two bases, but not all simultaneously. Explain in your own words why adding more bases makes the hidden variable task harder.

---

### Problem 6: No-Signaling

Alice and Bob share $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$.

**(a)** If Alice measures in the Z basis, what are Bob's probabilities of getting 0 or 1 when he measures in Z?

**(b)** If Alice measures in the X basis instead, what are Bob's probabilities of getting 0 or 1 when he measures in Z? 

*Hint: Consider all possible Alice outcomes and weight by probability.*

**(c)** Compare your answers to (a) and (b). Can Bob tell which measurement Alice performed by looking at his own results?

**(d)** Explain why this result guarantees that entanglement cannot be used for faster-than-light communication.

---

### Problem 7: EPR Argument Analysis

Reconstruct the EPR argument in your own words.

**(a)** What is the "locality" assumption?

**(b)** What is the "realism" assumption (that measurement reveals pre-existing values)?

**(c)** How do these assumptions, combined with quantum predictions, lead to the conclusion that quantum mechanics is "incomplete"?

**(d)** Bell's theorem shows that quantum mechanics violates certain inequalities derived from local realism. What does this imply about the EPR assumptions?

---

### Problem 8: Qiskit Exploration

**(a)** Create the singlet state $|\Psi^-\rangle$ in Qiskit and verify its amplitudes.

**(b)** Measure it 10,000 times in the Z basis. Verify perfect anti-correlation.

**(c)** Measure it 10,000 times in the X basis (apply H to both qubits before measuring). Verify perfect anti-correlation.

**(d)** Measure with Alice in Z and Bob at angle $\theta = 45°$ (apply $R_y(\pi/4)$ to Bob's qubit before measuring). Calculate the correlation $E$ from your results and compare to the theoretical prediction $E = -\cos(45°) = -1/\sqrt{2}$.

---

### Problem 9: Conceptual Questions

**(a)** Alice and Bob share an entangled state. Alice measures and gets a result. Has anything "physical" happened to Bob's qubit? Discuss different interpretations.

**(b)** Why can't we use the perfect correlations in entanglement to send a message faster than light?

**(c)** Einstein called entanglement "spooky action at a distance." Is there actually any "action" (physical influence) at a distance? What would Einstein say? What would a modern physicist say?

**(d)** If quantum correlations can't be explained by hidden variables (as Bell's theorem suggests), what are the alternatives?

---

## Looking Ahead

We've seen that quantum correlations are stronger than classical correlations — they persist across multiple measurement bases. And we've previewed Bell's key insight: this strength can be quantified, leading to testable inequalities.

Next lecture, we'll derive the CHSH inequality and prove that quantum mechanics violates it. This is the mathematical heart of the "quantum vs classical" distinction — and the foundation for understanding why quantum computers might outperform classical ones.