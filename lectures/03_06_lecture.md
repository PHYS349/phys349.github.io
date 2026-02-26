# Lecture 3.4: Two-Qubit Gates and Hardware

## From Theory to Implementation

We've established that entanglement is real, non-classical, and powerful. Now we learn how to **create** it.

Single-qubit gates (H, X, Z, rotations) manipulate individual qubits. But they cannot create entanglement — a product state remains a product state under local operations.

To create entanglement, we need **two-qubit gates**: operations that couple qubits together. Today we'll study the most important two-qubit gates and see how they're physically implemented in real quantum computers.

---

## Why Two-Qubit Gates?

### The Limitation of Single-Qubit Gates

Suppose Alice has qubit A and Bob has qubit B, initially in state $|0\rangle_A \otimes |0\rangle_B = |00\rangle$.

If Alice applies any single-qubit gate $U_A$ to her qubit, and Bob applies any gate $U_B$ to his:

$$
(U_A \otimes U_B)|00\rangle = U_A|0\rangle \otimes U_B|0\rangle = |\psi\rangle_A \otimes |\phi\rangle_B
$$

The result is still a **product state**. No entanglement.

```{admonition} Single-Qubit Gates Cannot Create Entanglement
:class: warning

If you start with a product state and apply only single-qubit gates (even different gates to each qubit), you always get a product state.

Entanglement requires **interaction** between qubits.
```

### The Power of Two-Qubit Gates

A two-qubit gate $U$ acts on the 4-dimensional space of both qubits together:

$$
U: \mathbb{C}^4 \to \mathbb{C}^4
$$

Such a gate can transform a product state into an entangled state — this is what makes quantum computing powerful.

### What Makes a Gate "Entangling"?

Not every two-qubit gate creates entanglement. For example:
- $I \otimes I$ does nothing
- $X \otimes Y$ applies X to qubit 1 and Y to qubit 2 — still just single-qubit operations in parallel

```{admonition} Entangling Gates
:class: tip

A two-qubit gate $U$ is **entangling** if there exists some product state $|\psi\rangle \otimes |\phi\rangle$ such that $U(|\psi\rangle \otimes |\phi\rangle)$ is entangled.

A gate is **non-entangling** if it maps every product state to a product state. This happens exactly when $U = U_A \otimes U_B$ (tensor product of single-qubit gates).
```

The gates we'll study (CNOT, CZ, SWAP, iSWAP) are all entangling — they can create entanglement from product states. This is what makes them useful for quantum computing.

---

## The CNOT Gate

The most important two-qubit gate is the **Controlled-NOT** (CNOT or CX).

### Definition

CNOT has a **control** qubit and a **target** qubit:
- If control = $|0\rangle$: do nothing to target
- If control = $|1\rangle$: flip the target (apply X)

In the computational basis:

$$
\text{CNOT}|00\rangle = |00\rangle
$$
$$
\text{CNOT}|01\rangle = |01\rangle
$$
$$
\text{CNOT}|10\rangle = |11\rangle
$$
$$
\text{CNOT}|11\rangle = |10\rangle
$$

The control qubit (first) is unchanged. The target qubit (second) is flipped if and only if the control is $|1\rangle$.

### Matrix Representation

$$
\text{CNOT} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{pmatrix}
$$

The rows/columns are ordered as $|00\rangle, |01\rangle, |10\rangle, |11\rangle$.

### Circuit Symbol

```{figure} ./03_04_lecture_files/cnot_symbol.svg
:width: 200px
:name: cnot-symbol

CNOT gate: solid dot on control, ⊕ on target.
```

### Creating a Bell State

CNOT is the key ingredient for creating entanglement:

$$
|00\rangle \xrightarrow{H \otimes I} \frac{|0\rangle + |1\rangle}{\sqrt{2}} \otimes |0\rangle = \frac{|00\rangle + |10\rangle}{\sqrt{2}}
$$

$$
\xrightarrow{\text{CNOT}} \frac{|00\rangle + |11\rangle}{\sqrt{2}} = |\Phi^+\rangle
$$

The Hadamard creates a superposition; the CNOT "copies" the control state to the target, creating entanglement.

```{figure} ./03_04_lecture_files/bell_state_circuit.svg
:width: 300px
:name: bell-circuit

Circuit to create $|\Phi^+\rangle$: H on qubit 0, then CNOT.
```

### CNOT is Universal

A remarkable theorem:

```{admonition} Universality
:class: important

**Any** quantum computation can be built from:
- Single-qubit gates
- CNOT gates

This is called a **universal gate set**. CNOT + arbitrary single-qubit rotations can approximate any unitary transformation to arbitrary precision.
```

---

## The CZ Gate

The **Controlled-Z** (CZ) gate is another fundamental two-qubit gate.

### Definition

CZ applies a Z gate to the target if the control is $|1\rangle$:

$$
\text{CZ}|00\rangle = |00\rangle
$$
$$
\text{CZ}|01\rangle = |01\rangle
$$
$$
\text{CZ}|10\rangle = |10\rangle
$$
$$
\text{CZ}|11\rangle = -|11\rangle
$$

Only the $|11\rangle$ state picks up a minus sign.

### Matrix Representation

$$
\text{CZ} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & -1 \end{pmatrix}
$$

### CZ is Symmetric!

Unlike CNOT, CZ is **symmetric** in the two qubits:

$$
\text{CZ}_{12} = \text{CZ}_{21}
$$

It doesn't matter which qubit you call "control" and which "target" — the gate is the same either way.

This makes CZ natural for certain hardware implementations where the physical interaction is symmetric.

### Circuit Symbol

```{figure} ./03_04_lecture_files/cz_symbol.svg
:width: 200px
:name: cz-symbol

CZ gate: solid dots on both qubits (reflecting symmetry).
```

### Relation to CNOT

CNOT and CZ are related by Hadamard gates:

$$
\text{CNOT}_{12} = (I \otimes H) \cdot \text{CZ}_{12} \cdot (I \otimes H)
$$

So if your hardware natively implements CZ, you can build CNOT by sandwiching it with Hadamards on the target qubit.

---

## The SWAP Gate

The **SWAP** gate exchanges the states of two qubits.

### Definition

$$
\text{SWAP}|ab\rangle = |ba\rangle
$$

Explicitly:
$$
\text{SWAP}|00\rangle = |00\rangle
$$
$$
\text{SWAP}|01\rangle = |10\rangle
$$
$$
\text{SWAP}|10\rangle = |01\rangle
$$
$$
\text{SWAP}|11\rangle = |11\rangle
$$

### Matrix Representation

$$
\text{SWAP} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}
$$

### SWAP from CNOTs

SWAP can be built from three CNOTs:

$$
\text{SWAP} = \text{CNOT}_{12} \cdot \text{CNOT}_{21} \cdot \text{CNOT}_{12}
$$

```{figure} ./03_04_lecture_files/swap_from_cnot.svg
:width: 350px
:name: swap-cnot

SWAP = three CNOTs with alternating control/target.
```

### The $\sqrt{\text{SWAP}}$ Gate

The square root of SWAP is also useful:

$$
\sqrt{\text{SWAP}} \cdot \sqrt{\text{SWAP}} = \text{SWAP}
$$

This gate creates partial entanglement and is native to some hardware platforms.

---

## The iSWAP Gate

The **iSWAP** gate swaps the $|01\rangle$ and $|10\rangle$ components while adding a phase of $i$:

$$
\text{iSWAP}|00\rangle = |00\rangle
$$
$$
\text{iSWAP}|01\rangle = i|10\rangle
$$
$$
\text{iSWAP}|10\rangle = i|01\rangle
$$
$$
\text{iSWAP}|11\rangle = |11\rangle
$$

### Matrix Representation

$$
\text{iSWAP} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 0 & i & 0 \\ 0 & i & 0 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}
$$

### Why iSWAP Matters

The iSWAP gate arises naturally from the **XY coupling** Hamiltonian:

$$
H_{XY} = \frac{J}{2}(\sigma_x \otimes \sigma_x + \sigma_y \otimes \sigma_y)
$$

When you evolve under this Hamiltonian for time $t = \pi/(2J)$:

$$
e^{-iH_{XY}t} = \text{iSWAP}
$$

This is exactly what happens in many superconducting qubit architectures when two transmons are tuned into resonance!

### iSWAP Creates Entanglement

Let's verify that iSWAP is entangling. Apply it to $|01\rangle$:

$$
\text{iSWAP}|01\rangle = i|10\rangle
$$

That's still a product state. But apply it to $|+0\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |10\rangle)$:

$$
\text{iSWAP}|+0\rangle = \frac{1}{\sqrt{2}}(|00\rangle + i|01\rangle)
$$

This is entangled! (You can verify with the determinant test.)

### The $\sqrt{\text{iSWAP}}$ Gate

Google's Sycamore processor uses a gate closely related to $\sqrt{\text{iSWAP}}$:

$$
\sqrt{\text{iSWAP}} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & \frac{1}{\sqrt{2}} & \frac{i}{\sqrt{2}} & 0 \\ 0 & \frac{i}{\sqrt{2}} & \frac{1}{\sqrt{2}} & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}
$$

Two applications give iSWAP: $\sqrt{\text{iSWAP}} \cdot \sqrt{\text{iSWAP}} = \text{iSWAP}$.

This "partial swap" is often the natural gate duration for a given coupling strength, making it efficient to implement.

---

## Controlled-U Gates

More generally, we can control any single-qubit gate $U$:

$$
\text{C-}U = |0\rangle\langle 0| \otimes I + |1\rangle\langle 1| \otimes U
$$

This means:
- If control = $|0\rangle$: apply $I$ to target (do nothing)
- If control = $|1\rangle$: apply $U$ to target

### Examples

- **CNOT** = C-X (controlled-X)
- **CZ** = C-Z (controlled-Z)
- **CY** = C-Y (controlled-Y)
- **C-Phase($\phi$)**: apply phase $e^{i\phi}$ to target if control is $|1\rangle$

### Controlled-Phase Gate

The controlled-phase gate C-P($\phi$) is particularly important:

$$
\text{C-P}(\phi) = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & e^{i\phi} \end{pmatrix}
$$

When $\phi = \pi$, this is CZ.

---

## Two-Qubit Gate Decomposition

Any two-qubit gate can be decomposed into at most **three CNOT gates** plus single-qubit gates.

This is the **KAK decomposition** (Cartan decomposition):

$$
U = (A_1 \otimes B_1) \cdot e^{i(c_1 X \otimes X + c_2 Y \otimes Y + c_3 Z \otimes Z)} \cdot (A_2 \otimes B_2)
$$

where $A_1, A_2, B_1, B_2$ are single-qubit gates.

The middle part (the "interaction" part) requires at most 3 CNOTs to implement.

```{admonition} Gate Compilation
:class: note

When you write a quantum circuit with arbitrary two-qubit gates, the compiler decomposes them into the native gates of your hardware. Understanding native gates helps you write more efficient circuits.
```

---

## Physical Implementation: How Do We Make Two-Qubit Gates?

Now for the exciting part: how do real quantum computers implement these gates?

The key insight: **two-qubit gates require physical interaction between qubits**.

Different platforms use different interactions:

| Platform | Qubit Type | Native 2Q Gate | Interaction |
|----------|------------|----------------|-------------|
| Superconducting | Transmon | CZ or iSWAP | Capacitive coupling |
| Trapped ions | Ion spin | Mølmer-Sørensen | Collective motion |
| Neutral atoms | Atomic states | CZ | Rydberg blockade |
| Photons | Polarization | CZ | Nonlinear optics |
| Spin qubits | Electron spin | $\sqrt{\text{SWAP}}$ | Exchange interaction |

### Native Gate Sets by Platform

When you write a quantum circuit, it gets **compiled** to the native gates of your hardware. Knowing the native gates helps you write efficient circuits.

| Platform | Company | Native 2Q Gate | Building CNOT |
|----------|---------|----------------|---------------|
| Superconducting | IBM | ECR (echoed cross-resonance) | 1 ECR + single-qubit |
| Superconducting | Google | $\sqrt{\text{iSWAP}}$ + CZ | ~2 native gates |
| Superconducting | Rigetti | CZ | CNOT = H-CZ-H |
| Trapped ions | IonQ | XX (Mølmer-Sørensen) | 1 XX + single-qubit |
| Trapped ions | Quantinuum | ZZ | 1 ZZ + single-qubit |
| Neutral atoms | QuEra, Pasqal | CZ (Rydberg) | CNOT = H-CZ-H |

```{admonition} Why Native Gates Matter
:class: note

A circuit using "textbook" gates (CNOT, Toffoli, etc.) must be recompiled to native gates. This can increase the gate count:

- CNOT on IBM hardware: 1 ECR + 2 single-qubit gates
- Toffoli (CCX) on any hardware: 6+ two-qubit gates
- SWAP: 3 CNOTs = potentially 6+ native gates

Writing circuits with native gates in mind can significantly improve performance.
```

Let's explore the three major platforms in detail.

---

## Superconducting Qubits

### The Qubit

Superconducting qubits use circuits made of superconducting materials (often aluminum) cooled to ~15 millikelvin.

The **transmon** qubit is the most common type:
- A Josephson junction acts as a nonlinear inductor
- Combined with a capacitor, it forms an anharmonic oscillator
- The lowest two energy levels form the qubit: $|0\rangle$ and $|1\rangle$

```{figure} ./03_04_lecture_files/transmon_schematic.svg
:width: 300px
:name: transmon

Transmon qubit: Josephson junction (X) with capacitor (||).
```

### Single-Qubit Gates

Single-qubit gates are implemented with microwave pulses:
- Frequency matches the qubit transition (~5 GHz)
- Pulse duration and amplitude control the rotation angle
- Pulse phase controls the rotation axis (X vs Y)

Gates take ~20-50 nanoseconds.

### Two-Qubit Gates: Capacitive Coupling

Two transmons are coupled by placing them near each other or connecting them with a capacitor or resonator.

The coupling Hamiltonian includes terms like:

$$
H_{\text{int}} \propto \sigma_x^{(1)} \sigma_x^{(2)} + \sigma_y^{(1)} \sigma_y^{(2)}
$$

This is an **XY coupling** (also called transverse coupling).

### Implementing CZ

One common approach for CZ:

1. **Tune one qubit's frequency** temporarily
2. When the frequencies become resonant, the qubits interact
3. The $|11\rangle$ state acquires a phase
4. Tune the qubit back

This takes ~30-100 nanoseconds.

### Implementing iSWAP

The XY coupling naturally implements iSWAP when qubits are on resonance for the right duration:

$$
e^{-i H_{\text{XY}} t} \propto \text{iSWAP} \quad \text{at } t = \pi/4J
$$

Some systems (like Google's Sycamore) use iSWAP-like gates as the native operation.

### The Cross-Resonance Gate (IBM)

IBM uses a different approach called **cross-resonance**:

1. Drive qubit A at the frequency of qubit B
2. The drive "leaks" through the coupling to affect qubit B
3. The effect on B depends on the state of A — this creates entanglement

The cross-resonance gate doesn't require tuning qubit frequencies, which simplifies control. It naturally produces a gate related to CNOT.

```{admonition} Key Takeaway: Superconducting Qubits
:class: tip

**Strengths:** Fast gates (~50 ns), scalable fabrication, high qubit counts

**Weaknesses:** Short coherence (~100 μs), requires 15 mK cooling, frequency crowding

**Native gates:** CZ, iSWAP, or cross-resonance depending on architecture

**Best for:** Circuits with many gates but moderate depth, NISQ algorithms
```

### Pros and Cons

**Advantages:**
- Fast gates (10-100 ns)
- High connectivity possible
- Lithographic fabrication (scalable manufacturing)

**Disadvantages:**
- Requires extreme cooling (~15 mK)
- Short coherence times (~100 μs)
- Frequency crowding with many qubits

---

## Trapped Ion Qubits

### The Qubit

Individual ions (e.g., $^{171}$Yb$^+$, $^{43}$Ca$^+$) are trapped by electromagnetic fields in a vacuum.

The qubit is encoded in:
- **Hyperfine states** (ground state sublevels), or
- **Optical states** (ground + metastable excited state)

Ions are held ~5-10 μm apart, arranged in a line.

### Single-Qubit Gates

Implemented with laser pulses or microwaves:
- Resonant with the qubit transition
- High fidelity (>99.9%)
- Gate time: ~1-10 μs

### Two-Qubit Gates: Shared Motion

Ions in a trap repel each other electrostatically. This couples their motion — they oscillate together like masses connected by springs.

The collective motion has quantum mechanical modes (phonons).

### The Mølmer-Sørensen Gate

The most common two-qubit gate:

1. Apply laser beams that couple the qubit state to the motional state
2. The coupling is designed so that the motion returns to its original state
3. But the qubit states accumulate a phase that depends on both qubits

The result is an entangling gate equivalent to:

$$
\text{MS} = e^{-i \frac{\pi}{4} \sigma_x^{(1)} \sigma_x^{(2)}}
$$

This can be converted to CNOT with single-qubit rotations.

Gate time: ~50-200 μs.

```{admonition} Key Takeaway: Trapped Ion Qubits
:class: tip

**Strengths:** Highest fidelity (~99.9%), longest coherence (seconds), all-to-all connectivity

**Weaknesses:** Slow gates (~100 μs), scaling challenges, complex vacuum/laser systems

**Native gates:** XX (Mølmer-Sørensen) or ZZ gates

**Best for:** High-precision algorithms, error correction research, small-scale quantum advantage
```

### Pros and Cons

**Advantages:**
- Long coherence times (seconds to minutes)
- High gate fidelity (>99.9%)
- All-to-all connectivity (any ion can interact with any other)

**Disadvantages:**
- Slow gates (~100 μs)
- Scaling is challenging (long chains are hard to control)
- Requires ultra-high vacuum

---

## Neutral Atom Qubits

### The Qubit

Individual neutral atoms (e.g., rubidium, cesium) are trapped by focused laser beams called **optical tweezers**.

The qubit is encoded in hyperfine ground states (similar to trapped ions).

Atoms can be arranged in 1D, 2D, or 3D arrays with ~5 μm spacing.

### Single-Qubit Gates

Implemented with:
- Global microwave pulses (same rotation on all qubits)
- Individual laser beams (addressing specific atoms)

Gate time: ~1 μs.

### Two-Qubit Gates: Rydberg Blockade

Here's the clever trick:

1. **Rydberg states** are highly excited atomic states (principal quantum number n ~ 50-100)
2. Rydberg atoms have huge electric dipole moments
3. Two nearby Rydberg atoms repel each other strongly

```{figure} ./03_04_lecture_files/rydberg_blockade.svg
:width: 400px
:name: rydberg-blockade

Rydberg blockade: If one atom is excited to Rydberg state, nearby atoms cannot be excited.
```

### The Rydberg CZ Gate

1. Try to excite both atoms to a Rydberg state simultaneously
2. If both atoms are in $|1\rangle$: blockade prevents double excitation → pick up a phase
3. If one or both atoms are in $|0\rangle$: no blockade → no extra phase

The result is a CZ gate:

$$
|11\rangle \to -|11\rangle
$$

while other basis states are unchanged.

Gate time: ~100 ns - 1 μs.

```{admonition} Key Takeaway: Neutral Atom Qubits
:class: tip

**Strengths:** Identical qubits, flexible 2D/3D geometry, long coherence, fast Rydberg gates

**Weaknesses:** Atom loss, finite Rydberg lifetime, laser complexity

**Native gates:** CZ via Rydberg blockade

**Best for:** Quantum simulation, optimization problems, scaling to many qubits
```

### Pros and Cons

**Advantages:**
- Identical qubits (atoms are all the same)
- Flexible array geometries (2D, 3D)
- Long coherence times (~1 s)
- Fast Rydberg gates

**Disadvantages:**
- Atom loss (atoms occasionally escape)
- Rydberg state decay
- Laser stabilization requirements

---

## Comparing Platforms

| Property | Superconducting | Trapped Ions | Neutral Atoms |
|----------|-----------------|--------------|---------------|
| Qubit count (2024) | ~1000 | ~50 | ~1000 |
| T1 (relaxation) | ~100 μs | ~seconds | ~seconds |
| T2 (coherence) | ~100 μs | ~seconds | ~1 s |
| 1Q gate time | ~20 ns | ~1 μs | ~1 μs |
| 2Q gate time | ~50 ns | ~100 μs | ~500 ns |
| 2Q gate fidelity | ~99.5% | ~99.9% | ~99.5% |
| Connectivity | Nearest-neighbor | All-to-all | Programmable |

Each platform has trade-offs. The "best" choice depends on the application.

---

## Entanglement in Practice: Circuit Depth and Fidelity

In real hardware, gates aren't perfect and qubits decohere. Understanding these limitations is essential.

### Gate Count vs Circuit Depth

Two important metrics for quantum circuits:

**Gate count:** Total number of gates in the circuit.

**Circuit depth:** Number of time steps (layers), where gates in each layer execute in parallel.

```{admonition} Example: Depth vs Count
:class: note

A circuit applying H to each of 100 qubits:
- **Gate count:** 100
- **Circuit depth:** 1 (all gates in parallel)

A circuit applying H, then T, then S to one qubit:
- **Gate count:** 3
- **Circuit depth:** 3 (sequential)
```

For decoherence, **depth matters more than count**. Parallel gates don't add to the total time, so they don't give decoherence more opportunity to corrupt the state.

### Connectivity and SWAP Overhead

Real quantum computers have **limited connectivity** — not every qubit can directly interact with every other qubit.

| Platform | Typical Connectivity |
|----------|---------------------|
| IBM superconducting | Heavy-hex lattice (degree 2-3) |
| Google superconducting | 2D grid (degree 4) |
| Rigetti superconducting | Octagonal lattice |
| Trapped ions | All-to-all |
| Neutral atoms | Programmable (can rearrange) |

**The problem:** If your circuit needs a CNOT between qubits 1 and 5, but they're not connected, you must insert SWAP gates to move the qubit states until they're adjacent.

**The cost:** Each SWAP = 3 CNOTs. A circuit with 10 "logical" CNOTs might need 30+ "physical" CNOTs after routing!

```{figure} ./03_04_lecture_files/swap_insertion.svg
:width: 400px
:name: swap-insertion

SWAP insertion: To apply CNOT between non-adjacent qubits, we must route through intermediate qubits.
```

This is why all-to-all connectivity (trapped ions) or programmable connectivity (neutral atoms) can be advantageous despite slower gates.

### Gate Fidelity

The **fidelity** of a gate measures how close the actual operation is to the ideal:

$$
F = |\langle \psi_{\text{ideal}} | \psi_{\text{actual}} \rangle|^2
$$

- $F = 1$: perfect gate
- $F = 0.99$: 1% error per gate
- $F = 0.999$: 0.1% error per gate

### Error Accumulation: A Worked Example

In a circuit with $n$ two-qubit gates, errors compound:

$$
F_{\text{total}} \approx F^n
$$

**Example:** You want to run a 50-CNOT circuit.

With $F = 0.99$ (99% fidelity):
$$
F_{\text{total}} = 0.99^{50} \approx 0.60
$$
About 40% of runs have at least one error!

With $F = 0.995$ (99.5% fidelity):
$$
F_{\text{total}} = 0.995^{50} \approx 0.78
$$
Better, but still significant errors.

With $F = 0.999$ (99.9% fidelity):
$$
F_{\text{total}} = 0.999^{50} \approx 0.95
$$
Now we're talking! This is why the push from 99% to 99.9% is so important.

```{admonition} The Error Correction Threshold
:class: important

For fault-tolerant quantum computing with error correction, we need gate fidelities above a **threshold** — typically around 99% for surface codes.

Current hardware is at or just above this threshold. The next challenge is maintaining high fidelity as we scale to more qubits.
```

### State of the Art (2024)

- Superconducting: ~99.5% two-qubit fidelity
- Trapped ions: ~99.9% two-qubit fidelity
- Neutral atoms: ~99.5% two-qubit fidelity

The threshold for fault-tolerant quantum computing is roughly ~99%, so we're just reaching the necessary regime.

---

## Lab: Two-Qubit Gates in Qiskit

Let's explore two-qubit gates in simulation.

### Setup

```{code-block} python
import numpy as np
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector, Operator
from qiskit_aer import AerSimulator
import matplotlib.pyplot as plt

simulator = AerSimulator()
```

### CNOT Gate Action

```{code-block} python
def show_cnot_action():
    """Demonstrate CNOT on all basis states"""
    print("CNOT Gate Action:")
    print("="*40)
    
    basis_states = ['00', '01', '10', '11']
    
    for bs in basis_states:
        # Create basis state
        qc = QuantumCircuit(2)
        if bs[0] == '1':
            qc.x(1)  # First qubit (control)
        if bs[1] == '1':
            qc.x(0)  # Second qubit (target)
        
        # Apply CNOT
        qc.cx(1, 0)  # Control=1, Target=0
        
        # Get output state
        sv = Statevector(qc)
        
        # Find which basis state it is
        for i, amp in enumerate(sv):
            if abs(amp) > 0.9:
                out = format(i, '02b')[::-1]  # Reverse for our convention
                print(f"  |{bs}⟩ → |{out}⟩")

show_cnot_action()
```

### Creating All Four Bell States

```{code-block} python
def create_bell_states():
    """Create all four Bell states"""
    bell_states = {}
    
    # |Φ+⟩ = (|00⟩ + |11⟩)/√2
    qc = QuantumCircuit(2)
    qc.h(0)
    qc.cx(0, 1)
    bell_states['Φ+'] = Statevector(qc)
    
    # |Φ-⟩ = (|00⟩ - |11⟩)/√2
    qc = QuantumCircuit(2)
    qc.h(0)
    qc.cx(0, 1)
    qc.z(0)
    bell_states['Φ-'] = Statevector(qc)
    
    # |Ψ+⟩ = (|01⟩ + |10⟩)/√2
    qc = QuantumCircuit(2)
    qc.h(0)
    qc.cx(0, 1)
    qc.x(1)
    bell_states['Ψ+'] = Statevector(qc)
    
    # |Ψ-⟩ = (|01⟩ - |10⟩)/√2
    qc = QuantumCircuit(2)
    qc.h(0)
    qc.cx(0, 1)
    qc.x(1)
    qc.z(0)
    bell_states['Ψ-'] = Statevector(qc)
    
    print("Bell States:")
    print("="*40)
    for name, sv in bell_states.items():
        print(f"\n|{name}⟩:")
        for i, amp in enumerate(sv):
            if abs(amp) > 1e-10:
                label = format(i, '02b')
                print(f"  |{label}⟩: {amp:.4f}")
    
    return bell_states

bell_states = create_bell_states()
```

### CNOT vs CZ Comparison

```{code-block} python
def compare_cnot_cz():
    """Show the relationship between CNOT and CZ"""
    print("\nCNOT vs CZ Comparison:")
    print("="*40)
    
    # CNOT matrix
    qc_cnot = QuantumCircuit(2)
    qc_cnot.cx(0, 1)
    cnot = Operator(qc_cnot).data
    
    # CZ matrix
    qc_cz = QuantumCircuit(2)
    qc_cz.cz(0, 1)
    cz = Operator(qc_cz).data
    
    print("\nCNOT matrix:")
    print(np.round(cnot.real, 2))
    
    print("\nCZ matrix:")
    print(np.round(cz.real, 2))
    
    # Verify: CNOT = (I⊗H) CZ (I⊗H)
    qc_cnot_from_cz = QuantumCircuit(2)
    qc_cnot_from_cz.h(1)
    qc_cnot_from_cz.cz(0, 1)
    qc_cnot_from_cz.h(1)
    cnot_from_cz = Operator(qc_cnot_from_cz).data
    
    print("\nCNOT from CZ (H-CZ-H on target):")
    print(np.round(cnot_from_cz.real, 2))
    
    print("\nAre they equal?", np.allclose(cnot, cnot_from_cz))

compare_cnot_cz()
```

### SWAP from Three CNOTs

```{code-block} python
def verify_swap():
    """Verify SWAP = CNOT · CNOT · CNOT"""
    print("\nSWAP Decomposition:")
    print("="*40)
    
    # Direct SWAP
    qc_swap = QuantumCircuit(2)
    qc_swap.swap(0, 1)
    swap_direct = Operator(qc_swap).data
    
    # SWAP from CNOTs
    qc_cnot_swap = QuantumCircuit(2)
    qc_cnot_swap.cx(0, 1)
    qc_cnot_swap.cx(1, 0)
    qc_cnot_swap.cx(0, 1)
    swap_from_cnot = Operator(qc_cnot_swap).data
    
    print("SWAP matrix (direct):")
    print(np.round(swap_direct.real, 2))
    
    print("\nSWAP from 3 CNOTs:")
    print(np.round(swap_from_cnot.real, 2))
    
    print("\nAre they equal?", np.allclose(swap_direct, swap_from_cnot))

verify_swap()
```

### Entanglement Verification

```{code-block} python
def verify_entanglement():
    """Verify that CNOT creates entanglement but single-qubit gates don't"""
    print("\nEntanglement Creation:")
    print("="*40)
    
    def is_entangled(sv):
        """Check if a 2-qubit state is entangled using the determinant test"""
        amps = np.array(sv)
        C = np.array([[amps[0], amps[1]], [amps[2], amps[3]]])
        det = np.abs(np.linalg.det(C))
        return det > 1e-10
    
    # Start with |00⟩
    qc_base = QuantumCircuit(2)
    sv_base = Statevector(qc_base)
    print(f"|00⟩ entangled? {is_entangled(sv_base)}")
    
    # Apply only single-qubit gates
    qc_single = QuantumCircuit(2)
    qc_single.h(0)
    qc_single.t(0)
    qc_single.rx(0.5, 1)
    qc_single.ry(0.3, 0)
    sv_single = Statevector(qc_single)
    print(f"After single-qubit gates? {is_entangled(sv_single)}")
    
    # Apply CNOT
    qc_cnot = QuantumCircuit(2)
    qc_cnot.h(0)
    qc_cnot.cx(0, 1)
    sv_cnot = Statevector(qc_cnot)
    print(f"After H + CNOT? {is_entangled(sv_cnot)}")

verify_entanglement()
```

### Gate Count for Different Circuits

```{code-block} python
def count_cnots():
    """Show CNOT counts for various operations"""
    print("\nCNOT Counts for Common Operations:")
    print("="*40)
    
    operations = [
        ("CNOT", 1),
        ("CZ (via CNOT)", 1),  # CZ = H-CNOT-H
        ("SWAP", 3),
        ("Toffoli (CCX)", 6),
        ("Fredkin (CSWAP)", 8),
    ]
    
    for name, count in operations:
        print(f"  {name}: {count} CNOT(s)")
    
    print("\nNote: Minimizing CNOT count is crucial because")
    print("two-qubit gates have lower fidelity than single-qubit gates.")

count_cnots()
```

---

## Summary

Today we learned how to create and control entanglement:

1. **Two-qubit gates** are necessary for creating entanglement
   - Single-qubit gates preserve product states
   - An entangling gate can transform product states into entangled states

2. **CNOT** (controlled-NOT) is the canonical entangling gate
   - Flips target if control is $|1\rangle$
   - CNOT + single-qubit gates = universal gate set

3. **CZ** (controlled-Z) is symmetric and often hardware-native
   - Adds a minus sign to $|11\rangle$
   - Related to CNOT by Hadamards

4. **SWAP** exchanges qubit states (3 CNOTs)

5. **iSWAP** arises from XY coupling and is native to some architectures
   - Google's Sycamore uses $\sqrt{\text{iSWAP}}$

6. **Native gate sets** vary by platform:
   - IBM: cross-resonance / ECR
   - Google: $\sqrt{\text{iSWAP}}$ + CZ
   - IonQ: XX (Mølmer-Sørensen)
   - Neutral atoms: CZ (Rydberg)

7. **Physical implementations:**
   - **Superconducting:** capacitive coupling, fast but short-lived
   - **Trapped ions:** shared motion, slow but high-fidelity
   - **Neutral atoms:** Rydberg blockade, scalable arrays

8. **Practical considerations:**
   - **Circuit depth** (not just gate count) determines decoherence impact
   - **Connectivity** limits which qubits can interact; SWAP overhead can be significant
   - **Gate fidelity** compounds: $F_{\text{total}} \approx F^n$

---

## Homework

### Problem 1: CNOT Action

**(a)** Compute $\text{CNOT}|+0\rangle$ where $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$.

**(b)** Is the result a product state or entangled? Verify using the determinant test.

**(c)** Compute $\text{CNOT}|{+}{+}\rangle$. Is this entangled?

**(d)** Find a two-qubit product state that remains a product state after CNOT.

---

### Problem 2: Creating Bell States

Starting from $|00\rangle$, give circuits to create:

**(a)** $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$

**(b)** $|\Phi^-\rangle = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle)$

**(c)** $|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle)$

**(d)** $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$

Use only H, X, Z, and CNOT gates.

---

### Problem 3: CZ Symmetry

**(a)** Write out the CZ matrix in the computational basis.

**(b)** Show that $\text{CZ}_{12} = \text{CZ}_{21}$ by computing both matrices explicitly.

**(c)** For CNOT, is $\text{CNOT}_{12} = \text{CNOT}_{21}$? Prove or give a counterexample.

**(d)** Verify that $\text{CNOT} = (I \otimes H) \cdot \text{CZ} \cdot (I \otimes H)$ by matrix multiplication.

---

### Problem 4: SWAP Decomposition

**(a)** Verify by matrix multiplication that $\text{SWAP} = \text{CNOT}_{01} \cdot \text{CNOT}_{10} \cdot \text{CNOT}_{01}$.

**(b)** Can you build SWAP from just two CNOTs? Try some combinations and explain why three seems to be the minimum.

**(c)** Show that SWAP preserves entanglement: if $|\psi\rangle$ is entangled, so is $\text{SWAP}|\psi\rangle$.

---

### Problem 5: Controlled-U Decomposition

A general controlled-U gate can be written as:

$$
\text{C-}U = |0\rangle\langle 0| \otimes I + |1\rangle\langle 1| \otimes U
$$

**(a)** Verify this gives the CNOT matrix when $U = X$.

**(b)** Write the matrix for C-Y (controlled-Y).

**(c)** Show that any controlled-U can be built from CNOT and single-qubit gates using:
$$\text{C-}U = (I \otimes A) \cdot \text{CNOT} \cdot (I \otimes B) \cdot \text{CNOT} \cdot (I \otimes C)$$
where $ABC = I$ and $e^{i\phi}AXBXC = U$ for some phase $\phi$.

*Hint for (c):* Start by noting that when the control is $|0\rangle$, the target sees $ABC = I$. When the control is $|1\rangle$, the CNOTs apply X gates, so the target sees $AXBXC$. For a rotation $U = e^{-i\theta \hat{n} \cdot \vec{\sigma}/2}$, try $A = R_z(\alpha)R_y(\beta/2)$, $B = R_y(-\beta/2)R_z(-(\alpha+\gamma)/2)$, $C = R_z((\gamma-\alpha)/2)$ where $\alpha, \beta, \gamma$ are Euler angles for $U$.

---

### Problem 6: Gate Fidelity

Suppose a two-qubit gate has fidelity $F = 0.995$ (i.e., 0.5% error per gate).

**(a)** If a circuit uses 10 two-qubit gates, what is the expected overall fidelity?

**(b)** How many two-qubit gates can you use before the fidelity drops below 50%?

**(c)** If you improve to $F = 0.999$, how does the answer to (b) change?

**(d)** Why is this analysis important for quantum error correction?

---

### Problem 7: Hardware Comparison

**(a)** A superconducting qubit has T2 = 100 μs and two-qubit gate time = 50 ns. How many two-qubit gates can be performed before decoherence significantly affects the state?

**(b)** A trapped ion qubit has T2 = 1 s and two-qubit gate time = 100 μs. Same question.

**(c)** Which system can do more gates before decoherence? Which has faster absolute runtime for 100 gates?

**(d)** What other factors besides gate count and speed matter for choosing a platform?

---

### Problem 8: Rydberg Blockade

In neutral atom qubits, the Rydberg blockade occurs when two nearby atoms cannot both be excited to the Rydberg state due to strong dipole-dipole interactions.

**(a)** If the interaction energy is $V = C_6/r^6$ where $r$ is the distance between atoms, and $C_6$ is a constant, how does the blockade radius $r_b$ (where $V = \hbar\Omega$, with $\Omega$ the laser Rabi frequency) scale with $\Omega$?

**(b)** Typical values: $C_6 \approx 2\pi \times 100$ GHz·μm$^6$, $\Omega \approx 2\pi \times 1$ MHz. Estimate the blockade radius.

**(c)** If atoms are spaced 5 μm apart, is this within the blockade radius? What does this mean for implementing gates?

---

### Problem 9: Circuit Compilation

You want to implement the gate $U = e^{-i\frac{\pi}{8} Z \otimes Z}$ on hardware that only has CNOT and single-qubit rotations.

**(a)** Show that $Z \otimes Z = \text{CNOT} \cdot (I \otimes Z) \cdot \text{CNOT}$.

**(b)** Using the identity $e^{-i\theta Z} = R_z(2\theta)$, find a circuit for $U$.

**(c)** Your circuit should use 2 CNOTs. Can you do it with fewer?

---

### Problem 10: Connectivity Constraints

Consider a 5-qubit linear chain where qubit $i$ can only interact with qubit $i \pm 1$:

```
Q0 — Q1 — Q2 — Q3 — Q4
```

**(a)** You want to apply CNOT with control=Q0 and target=Q4. How many SWAP gates are needed to bring them adjacent?

**(b)** Each SWAP costs 3 CNOTs. What is the total CNOT cost for the operation in (a), including the final CNOT?

**(c)** After the operation, the qubit states have been permuted. Write down where each logical qubit ends up.

**(d)** On a device with all-to-all connectivity (like trapped ions), how many CNOTs would the same operation require? What's the speedup factor?

---

## Looking Ahead

We now have all the tools to build quantum circuits: single-qubit gates, two-qubit gates, and the knowledge of how they're physically implemented.

Next lecture, we'll use these tools for **quantum protocols**: superdense coding, teleportation, and entanglement swapping. These are the "hello world" programs of quantum information — beautiful demonstrations of what entanglement can do.