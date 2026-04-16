---
kernelspec:
  name: python3
  display_name: Python 3
---

# Lecture 5.2 — Quantum Error Correction: Protecting Quantum Information

## Where We Left Off

In the last lecture we surveyed the physical platforms that implement quantum gates: superconducting transmons driven by microwave pulses, trapped ions manipulated with lasers, neutral atoms in optical tweezer arrays, and photonic qubits routed through beam splitters. Every one of these platforms faces the same fundamental enemy — **noise**. Qubits decohere. Gates aren't perfect. Measurements introduce errors. Back in Lecture 2.7, we quantified this with $T_1$ (energy relaxation) and $T_2$ (dephasing), and we saw that even the best qubits lose their quantum information on timescales of microseconds to seconds.

This raises an urgent question: if qubits are so fragile, how can we ever build quantum computers that run long algorithms like Shor's factoring? The answer is **quantum error correction** — a set of techniques that use redundancy and entanglement to protect quantum information from noise, without ever looking at (and thus destroying) the quantum state.

Today we'll build the theory from the ground up, starting with the reasons why classical error correction strategies don't work for qubits, and ending with a working error-correcting code in Qiskit.

------------------------------------------------------------------------

## Part 1: Why Error Correction Is Different for Qubits

### Classical Error Correction: Just Copy It

In classical computing, error correction is conceptually simple. To protect a bit against a random flip, you copy it:

$$
0 \to 000, \qquad 1 \to 111
$$

If noise flips one of the three bits — say $000 \to 010$ — you take a **majority vote** and recover the original. This is the **3-bit repetition code**, and it can correct any single-bit error.

Simple. Elegant. And completely impossible for qubits.

### Three Obstacles

Three features of quantum mechanics seem to make quantum error correction impossible:

**1. No-cloning theorem.** We proved in Lecture 4.0 that you cannot copy an unknown quantum state: there is no unitary that takes $|\psi\rangle|0\rangle \to |\psi\rangle|\psi\rangle$ for arbitrary $|\psi\rangle$. So the classical strategy of "just copy the bit three times" appears dead on arrival.

**2. Measurement collapses the state.** To detect a classical error, you read the bits. But measuring a qubit in superposition $\alpha|0\rangle + \beta|1\rangle$ collapses it to $|0\rangle$ or $|1\rangle$, destroying the very information you're trying to protect.

**3. Errors are continuous.** A classical bit can only be in state 0 or 1, so the only error is a flip. But a qubit can undergo any rotation: $|\psi\rangle \to e^{i\epsilon}R_x(\delta)|\psi\rangle$. There seem to be infinitely many types of errors to correct.

```{admonition} iClicker
:class: question
Which of these three obstacles do you think is the most fundamental barrier to quantum error correction?

A) No-cloning — you can't copy quantum states &emsp; B) Measurement collapse — you can't look without disturbing &emsp; C) Continuous errors — infinitely many error types &emsp; D) They're all equally fatal
```

```{dropdown} Answer
**D) None of them are actually fatal!** All three obstacles are real, but all three have clever workarounds. No-cloning prevents copying, but we can *entangle* qubits to spread information across multiple qubits without copying. Measurement collapse is avoided by measuring *parities* (correlations between qubits) instead of the qubits themselves. And continuous errors are handled by a remarkable theorem: correcting a discrete set of errors (the Pauli errors $X$, $Y$, $Z$) automatically corrects all continuous errors too.
```

### The Key Insight: Measure Correlations, Not Data

Here's the idea that makes everything work. Suppose you encode a qubit into three qubits. You don't measure the qubits themselves — that would collapse the state. Instead, you measure **parity**: "are qubits 1 and 2 the same or different?" This tells you whether an error flipped one of them *without revealing what state they're in*.

An ancilla qubit acts as a parity detector. A CNOT from each data qubit into the ancilla computes their parity into the ancilla, which you then measure. The data qubits are never measured directly.

This is the central trick of quantum error correction: **extract information about the error without extracting information about the encoded state.**

------------------------------------------------------------------------

## Part 2: The Bit-Flip Code

### Encoding Without Cloning

The 3-qubit bit-flip code encodes one logical qubit into three physical qubits. The encoding is:

$$
|0\rangle \to |0_L\rangle = |000\rangle, \qquad |1\rangle \to |1_L\rangle = |111\rangle
$$

For a general state $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$, the encoded state is:

$$
|\psi_L\rangle = \alpha|000\rangle + \beta|111\rangle
$$

This is **not** cloning! The state $\alpha|0\rangle + \beta|1\rangle$ has not been copied three times. Instead, the three qubits are **entangled** — if you measure any single qubit, you get no information about $\alpha$ and $\beta$. The information is stored *in the correlations* between the qubits, not in any individual qubit. This is a GHZ-type state, just like the ones we studied in Lecture 4.1.

The encoding circuit is simple: two CNOT gates from the data qubit to two fresh ancilla qubits.

```{code-cell} python
:tags: [hide-input]
from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister
from qiskit.quantum_info import Statevector, Operator
from qiskit_aer import AerSimulator
import numpy as np
import matplotlib.pyplot as plt
```

```{code-cell} python
# Encoding circuit for the 3-qubit bit-flip code
def encode_bit_flip():
    """Encode qubit 0 into qubits 0, 1, 2."""
    qc = QuantumCircuit(3)
    qc.cx(0, 1)
    qc.cx(0, 2)
    return qc

enc = encode_bit_flip()
enc.draw(output="mpl", style="iqp")
```

```{code-cell} python
# Verify: encoding |+⟩ = (|0⟩ + |1⟩)/√2
# Should give (|000⟩ + |111⟩)/√2 — a GHZ state
qc = QuantumCircuit(3)
qc.h(0)  # Prepare |+⟩
qc.compose(encode_bit_flip(), inplace=True)

sv = Statevector(qc)
print("Encoded |+⟩:")
sv.draw("text")
```

```{admonition} Check Yourself
:class: tip
What state do you get if you encode $|1\rangle$ (apply X to qubit 0 before the encoding circuit)? Verify with Qiskit.
```

### Error Model: Single Bit-Flips

The bit-flip code protects against a single $X$ (bit-flip) error on any one of the three qubits. After encoding, one of four things can happen:

| Error | State after error |
|:-----:|:------------------|
| None | $\alpha|000\rangle + \beta|111\rangle$ |
| $X$ on qubit 0 | $\alpha|100\rangle + \beta|011\rangle$ |
| $X$ on qubit 1 | $\alpha|010\rangle + \beta|101\rangle$ |
| $X$ on qubit 2 | $\alpha|001\rangle + \beta|110\rangle$ |

Notice that in every case, the superposition of $\alpha$ and $\beta$ is preserved — the error just moved some bits around.

### Syndrome Measurement

To detect which error occurred, we measure **parities** using ancilla qubits. Two parity checks tell us everything we need:

- **Parity of qubits 0 and 1:** Are they the same ($|00\rangle$ or $|11\rangle$) or different ($|01\rangle$ or $|10\rangle$)?
- **Parity of qubits 1 and 2:** Same or different?

The results form a 2-bit **syndrome**:

| Syndrome ($s_1 s_2$) | Meaning | Correction |
|:---------------------:|:--------|:-----------|
| 00 | No error | Do nothing |
| 10 | Qubits 0,1 differ but 1,2 agree → qubit 0 flipped | $X$ on qubit 0 |
| 11 | Both pairs differ → qubit 1 flipped | $X$ on qubit 1 |
| 01 | Qubits 0,1 agree but 1,2 differ → qubit 2 flipped | $X$ on qubit 2 |

```{admonition} iClicker
:class: question
In the 3-qubit bit-flip code, the syndrome measurement tells you *which* qubit had an error. Does it also tell you what the encoded state $\alpha$ and $\beta$ are?

A) Yes — the syndrome reveals everything &emsp; B) No — the syndrome only reveals error information &emsp; C) It depends on the error &emsp; D) It reveals $|\alpha|^2$ but not the phase
```

```{dropdown} Answer
**B) No — the syndrome only reveals error information.** This is the whole point! The parity measurement extracts information about *which qubit flipped* without revealing *what the logical state is*. The values of $\alpha$ and $\beta$ remain hidden in the entanglement between the data qubits. This is what distinguishes quantum error correction from classical — we can diagnose errors while keeping the quantum information intact.
```

### Full Circuit in Qiskit

Let's build the complete encode → error → syndrome → correct pipeline:

```{code-cell} python
def bit_flip_code(error_qubit=None):
    """
    Full 3-qubit bit-flip code circuit.
    
    Parameters:
        error_qubit: which qubit to flip (0, 1, 2, or None for no error)
    Returns:
        QuantumCircuit with 5 qubits (3 data + 2 ancilla) and 2 classical bits
    """
    data = QuantumRegister(3, 'data')
    ancilla = QuantumRegister(2, 'anc')
    syndrome = ClassicalRegister(2, 'syn')
    qc = QuantumCircuit(data, ancilla, syndrome)
    
    # --- Encode ---
    # Prepare an interesting state on qubit 0
    qc.ry(np.pi/3, data[0])  # |ψ⟩ = cos(π/6)|0⟩ + sin(π/6)|1⟩
    qc.barrier(label="Prep")
    
    # Encode: CNOT from qubit 0 to qubits 1, 2
    qc.cx(data[0], data[1])
    qc.cx(data[0], data[2])
    qc.barrier(label="Encode")
    
    # --- Introduce error ---
    if error_qubit is not None:
        qc.x(data[error_qubit])
    qc.barrier(label=f"Error: X on q{error_qubit}" if error_qubit is not None else "No error")
    
    # --- Syndrome measurement ---
    # Parity of data[0] and data[1] → ancilla[0]
    qc.cx(data[0], ancilla[0])
    qc.cx(data[1], ancilla[0])
    
    # Parity of data[1] and data[2] → ancilla[1]
    qc.cx(data[1], ancilla[1])
    qc.cx(data[2], ancilla[1])
    
    qc.measure(ancilla, syndrome)
    qc.barrier(label="Syndrome")
    
    # --- Correction ---
    # syndrome = 10 → flip data[0]
    # syndrome = 11 → flip data[1]
    # syndrome = 01 → flip data[2]
    with qc.if_test((syndrome, 0b10)):
        qc.x(data[0])
    with qc.if_test((syndrome, 0b11)):
        qc.x(data[1])
    with qc.if_test((syndrome, 0b01)):
        qc.x(data[2])
    qc.barrier(label="Correct")
    
    return qc

# Show the circuit with an error on qubit 1
qc_demo = bit_flip_code(error_qubit=1)
qc_demo.draw(output="mpl", style="iqp", fold=-1)
```

```{code-cell} python
# Verify with Statevector: compare encoded state before error
# to decoded state after correction

# First, let's check what the correct encoded state looks like
qc_no_error = QuantumCircuit(3)
qc_no_error.ry(np.pi/3, 0)
sv_original = Statevector(qc_no_error)
print("Original single-qubit state:")
print(f"  |0⟩ amplitude: {sv_original.data[0]:.4f}")
print(f"  |1⟩ amplitude: {sv_original.data[1]:.4f}")

# Now encode, introduce error on qubit 1, and measure syndrome
# We'll use Statevector to track the 5-qubit state
qc_test = QuantumCircuit(5)  # 3 data + 2 ancilla
qc_test.ry(np.pi/3, 0)      # Prepare |ψ⟩
qc_test.cx(0, 1)             # Encode
qc_test.cx(0, 2)
qc_test.x(1)                 # Error: flip qubit 1

# Syndrome measurement (as unitary, before actual measurement)
qc_test.cx(0, 3)             # Parity 0-1
qc_test.cx(1, 3)
qc_test.cx(1, 4)             # Parity 1-2
qc_test.cx(2, 4)

sv_after_syndrome = Statevector(qc_test)
print("\nState after error + syndrome computation:")
print("(Ancilla qubits 3,4 encode the syndrome)")
for i, amp in enumerate(sv_after_syndrome.data):
    if abs(amp) > 0.01:
        bits = format(i, '05b')
        print(f"  |{bits}⟩: {amp:.4f}")
```

```{admonition} Note on Mid-Circuit Measurement
:class: note
The `if_test` syntax in Qiskit implements **dynamic circuits** — the correction gates are applied conditionally based on the measurement results from the syndrome qubits. This is a real feature of modern quantum hardware (IBM's Eagle and Heron processors support mid-circuit measurement). On older hardware without this capability, you'd need to defer the measurement to the end and use quantum-controlled gates instead.
```

------------------------------------------------------------------------

## Part 3: The Phase-Flip Code

### A Different Kind of Error

The bit-flip code corrects $X$ errors — qubits flipping from $|0\rangle$ to $|1\rangle$ and vice versa. But qubits can also suffer **phase errors**: a $Z$ gate turns $|+\rangle$ into $|-\rangle$ without flipping the computational basis state. In the $Z$ basis, a phase error is invisible:

$$
Z|0\rangle = |0\rangle, \qquad Z|1\rangle = -|1\rangle
$$

If your state is $\alpha|0\rangle + \beta|1\rangle$, a phase error gives $\alpha|0\rangle - \beta|1\rangle$. The probabilities $|\alpha|^2$ and $|\beta|^2$ don't change — but the relative phase is flipped, which matters for interference and entanglement.

### The Hadamard Trick

Here's the elegant insight: a phase flip in the $Z$ basis is a bit flip in the $X$ basis. Since $HZH = X$, we can convert the phase-flip problem into a bit-flip problem:

1. Apply $H$ to each qubit.
2. Run the bit-flip code.
3. Apply $H$ to each qubit again.

The encoding becomes:

$$
|0\rangle \to |{+}{+}{+}\rangle, \qquad |1\rangle \to |{-}{-}{-}\rangle
$$

or equivalently:

$$
|\psi_L\rangle = \alpha|{+}{+}{+}\rangle + \beta|{-}{-}{-}\rangle
$$

Now a $Z$ error on any single qubit can be detected and corrected by the same syndrome measurement, but in the Hadamard basis.

```{code-cell} python
# Phase-flip code: H → bit-flip code → H
def encode_phase_flip():
    """Encode qubit 0 against single phase-flip errors."""
    qc = QuantumCircuit(3)
    # First encode in the Hadamard basis
    qc.cx(0, 1)
    qc.cx(0, 2)
    qc.h(0)
    qc.h(1)
    qc.h(2)
    return qc

pf = encode_phase_flip()
pf.draw(output="mpl", style="iqp")
```

```{code-cell} python
# Verify: encode |0⟩ in the phase-flip code
# Should give |+++⟩ = (|0⟩+|1⟩)(|0⟩+|1⟩)(|0⟩+|1⟩) / 2√2
qc = QuantumCircuit(3)
qc.compose(encode_phase_flip(), inplace=True)

sv = Statevector(qc)
print("Encoded |0⟩ in phase-flip code:")
for i, amp in enumerate(sv.data):
    if abs(amp) > 0.01:
        print(f"  |{format(i, '03b')}⟩: {amp:.4f}")
```

```{admonition} iClicker
:class: question
The bit-flip code corrects $X$ errors. The phase-flip code corrects $Z$ errors. To correct *both* types of errors simultaneously, what would you do?

A) Run both codes at the same time on the same qubits &emsp; B) Use the bit-flip code and hope for the best &emsp; C) Concatenate: encode with one code, then encode each qubit again with the other &emsp; D) It's impossible — you can't correct both
```

```{dropdown} Answer
**C) Concatenate.** Peter Shor's insight in 1995 was to nest the two codes: first encode each qubit against phase-flips (3 qubits per logical qubit), then encode each of those 3 qubits against bit-flips (3 qubits each). This gives $3 \times 3 = 9$ physical qubits per logical qubit and can correct *any* single-qubit error.
```

------------------------------------------------------------------------

## Part 4: The Shor Code — Correcting Any Error

### Concatenation: Nesting Two Codes

The **Shor code** (1995) was the first quantum error-correcting code that corrects arbitrary single-qubit errors. It uses 9 physical qubits per logical qubit, arranged in a two-level encoding:

**Level 1 (phase-flip protection):** Encode the logical qubit into 3 blocks:

$$
|0_L\rangle = \frac{1}{2\sqrt{2}}\left(|000\rangle + |111\rangle\right)\left(|000\rangle + |111\rangle\right)\left(|000\rangle + |111\rangle\right)
$$

$$
|1_L\rangle = \frac{1}{2\sqrt{2}}\left(|000\rangle - |111\rangle\right)\left(|000\rangle - |111\rangle\right)\left(|000\rangle - |111\rangle\right)
$$

**Level 2 (bit-flip protection within each block):** Each block of 3 qubits is itself a bit-flip code.

A bit-flip ($X$) on any qubit is caught by the bit-flip code within that block. A phase-flip ($Z$) on any qubit flips the sign of one block, which is caught by the phase-flip code across the three blocks.

### The Discretization of Errors

But what about the third obstacle — continuous errors? A qubit might not suffer a clean $X$ or $Z$ flip. It might rotate by a small angle: $|\psi\rangle \to (\cos\epsilon \cdot I + i\sin\epsilon \cdot X)|\psi\rangle$.

Here's the key theorem that makes quantum error correction practical:

```{admonition} Discretization of Errors
:class: important
Any single-qubit error can be written as a linear combination of the Pauli operators $\{I, X, Y, Z\}$. If a code can correct the discrete errors $X$, $Y$, and $Z$, it automatically corrects *all* single-qubit errors, including continuous rotations.
```

Why? Because the syndrome measurement **projects** the continuous error onto one of the discrete Pauli errors. When you measure the syndrome, you're asking "did an $X$, $Y$, $Z$, or no error occur?" The measurement collapses the error (not the data!) into one of these discrete possibilities, which you then correct.

This is profoundly counterintuitive — the act of error detection simplifies the error. It's another example of how measurement in quantum mechanics isn't just passive observation; it's an active process that shapes the state.

Since $Y = iXZ$, correcting $X$ and $Z$ errors suffices to handle $Y$ errors too. The Shor code can correct any single-qubit error on any of its 9 physical qubits.

```{code-cell} python
# Demonstration: a small rotation error is "discretized" by syndrome measurement
# We'll show this with the bit-flip code for simplicity

# Apply a small rotation error (not a clean X flip) to qubit 1
theta_error = 0.3  # small angle

qc = QuantumCircuit(5)
qc.ry(np.pi/3, 0)       # Prepare |ψ⟩
qc.cx(0, 1)              # Encode
qc.cx(0, 2)

# Small rotation error on qubit 1 (continuous error!)
qc.rx(theta_error, 1)

# Syndrome measurement
qc.cx(0, 3)
qc.cx(1, 3)
qc.cx(1, 4)
qc.cx(2, 4)

sv = Statevector(qc)

# What are the possible syndrome outcomes and their probabilities?
print(f"Continuous error: Rx({theta_error:.2f}) on qubit 1")
print(f"Error decomposition: cos({theta_error/2:.3f})·I + i·sin({theta_error/2:.3f})·X")
print(f"  P(no error detected) = cos²({theta_error/2:.3f}) = {np.cos(theta_error/2)**2:.4f}")
print(f"  P(X error detected)  = sin²({theta_error/2:.3f}) = {np.sin(theta_error/2)**2:.4f}")
print()

# Compute probabilities for each syndrome
for syn_val in range(4):
    prob = 0
    for i in range(32):
        anc_bits = (i >> 3) & 0b11  # extract ancilla bits (qubits 3,4)
        if anc_bits == syn_val:
            prob += abs(sv.data[i])**2
    if prob > 0.001:
        print(f"  Syndrome {format(syn_val, '02b')}: P = {prob:.4f}", end="")
        if syn_val == 0:
            print("  (no error → state is the correct encoded state)")
        elif syn_val == 3:
            print("  (qubit 1 flipped → apply X correction)")
        else:
            print()
```

```{admonition} Physical Intuition
:class: tip
Think of error discretization like the photoelectric effect: light is continuous (electromagnetic waves), but when it interacts with an atom, the energy exchange is quantized into discrete photons. Similarly, qubit errors are continuous rotations, but when they interact with the syndrome measurement, the error is "quantized" into discrete Pauli errors. The measurement projects the error, just as the atom projects the light.
```

------------------------------------------------------------------------

## Part 5: Error Correction on Real Hardware

### From Theory to Experiment

In theory, the bit-flip code is straightforward. In practice, every gate and measurement introduces its own errors. The cure has side effects.

Let's simulate what happens when we run the bit-flip code with realistic noise. We'll use a simple noise model: each CNOT gate has a small probability of introducing an error, and each measurement has a small probability of returning the wrong result.

```{code-cell} python
from qiskit_aer.noise import NoiseModel, depolarizing_error

def run_bit_flip_experiment(error_rate=0.0, gate_error=0.0, shots=10000):
    """
    Run the bit-flip code with optional noise.
    
    Parameters:
        error_rate: probability of a bit-flip error on data qubit 0
        gate_error: depolarizing error rate per CNOT gate
        shots: number of simulation shots
    Returns:
        success_rate: fraction of shots where final measurement matches expected
    """
    data = QuantumRegister(3, 'data')
    anc = QuantumRegister(2, 'anc')
    syn = ClassicalRegister(2, 'syn')
    out = ClassicalRegister(1, 'out')
    qc = QuantumCircuit(data, anc, syn, out)
    
    # Prepare |1⟩ for easy verification
    qc.x(data[0])
    
    # Encode
    qc.cx(data[0], data[1])
    qc.cx(data[0], data[2])
    qc.barrier()
    
    # Introduce error with given probability (we simulate by always applying
    # and using the error_rate as a conceptual parameter)
    if error_rate > 0:
        qc.x(data[0])  # deterministic error for demonstration
    qc.barrier()
    
    # Syndrome measurement
    qc.cx(data[0], anc[0])
    qc.cx(data[1], anc[0])
    qc.cx(data[1], anc[1])
    qc.cx(data[2], anc[1])
    qc.measure(anc, syn)
    
    # Correction
    with qc.if_test((syn, 0b10)):
        qc.x(data[0])
    with qc.if_test((syn, 0b11)):
        qc.x(data[1])
    with qc.if_test((syn, 0b01)):
        qc.x(data[2])
    
    # Decode and measure
    qc.cx(data[0], data[2])
    qc.cx(data[0], data[1])
    qc.measure(data[0], out[0])
    
    # Set up noise model
    simulator = AerSimulator()
    if gate_error > 0:
        noise_model = NoiseModel()
        cx_error = depolarizing_error(gate_error, 2)
        noise_model.add_all_qubit_quantum_error(cx_error, ['cx'])
        result = simulator.run(qc, noise_model=noise_model, shots=shots).result()
    else:
        result = simulator.run(qc, shots=shots).result()
    
    counts = result.get_counts()
    
    # Count successes (should measure |1⟩ since we prepared |1⟩)
    success = 0
    for bitstring, count in counts.items():
        # The output bit is the last classical register
        out_bit = bitstring.split()[-1] if ' ' in bitstring else bitstring[-1]
        if out_bit == '1':
            success += count
    
    return success / shots

# Compare: with and without error correction
print("Bit-flip code performance")
print("=" * 50)

# Perfect gates, with error
rate = run_bit_flip_experiment(error_rate=1.0, gate_error=0.0)
print(f"Perfect gates + X error on q0:   success = {rate:.1%}")

# Perfect gates, no error
rate = run_bit_flip_experiment(error_rate=0.0, gate_error=0.0)
print(f"Perfect gates, no error:         success = {rate:.1%}")

# Noisy gates, with error
for ge in [0.001, 0.01, 0.05]:
    rate = run_bit_flip_experiment(error_rate=1.0, gate_error=ge)
    print(f"Gate error = {ge:.3f} + X error:    success = {rate:.1%}")
```

### The Threshold Theorem

The results above illustrate a key tension: error correction uses extra gates, and each gate introduces its own errors. If the gate errors are too large, the correction process introduces *more* errors than it fixes.

The **threshold theorem** resolves this:

```{admonition} Quantum Error Correction Threshold
:class: important
There exists a threshold error rate $p_{\text{th}}$ such that if the physical error rate per gate $p < p_{\text{th}}$, then error correction can reduce the logical error rate to **arbitrarily low** levels by using enough physical qubits. The overhead grows only polynomially in the desired logical error rate.
```

For surface codes (the most promising architecture for near-term devices), the threshold is approximately $p_{\text{th}} \approx 1\%$. Current hardware is approaching this threshold: IBM's Heron processors achieve two-qubit gate errors around 0.3–1%, and Google's Willow processor demonstrated error correction improving with system size for the first time in December 2024.

### Surface Codes: The Path Forward

The repetition code we studied corrects only one type of error ($X$ or $Z$, not both). The Shor code corrects both but uses 9 qubits per logical qubit. Modern quantum error correction uses **surface codes**, which arrange physical qubits on a 2D grid:

- **Data qubits** sit at the vertices.
- **Ancilla qubits** sit at the faces and edges, measuring $X$-type and $Z$-type parities.
- A distance-$d$ surface code uses $\sim 2d^2$ physical qubits to encode 1 logical qubit and correct up to $\lfloor(d-1)/2\rfloor$ errors.

For a distance-3 surface code: 17 physical qubits protect 1 logical qubit against any single error. For a distance-5 code: about 49 qubits, correcting up to 2 errors.

```{admonition} iClicker
:class: question
To run Shor's algorithm and factor a 2048-bit RSA key, estimates suggest we'd need about 20 million physical qubits with surface code error correction. If each logical qubit requires about 1,000 physical qubits (with the code distance needed), roughly how many *logical* qubits does Shor's algorithm need?

A) ~200 &emsp; B) ~2,000 &emsp; C) ~20,000 &emsp; D) ~200,000
```

```{dropdown} Answer
**C) ~20,000.** With 20 million physical qubits and ~1,000 physical qubits per logical qubit, you get about 20,000 logical qubits. This gives you a sense of the enormous overhead: most of the qubits in a fault-tolerant quantum computer are dedicated to error correction, not computation. The actual algorithm only needs a few thousand logical operations, but protecting them requires a thousandfold redundancy.
```

------------------------------------------------------------------------

## Part 6: The Bigger Picture

### Where We Are: The NISQ Era

The current era of quantum computing is called **NISQ** — Noisy Intermediate-Scale Quantum. We have processors with 50–1,000+ qubits, but they're too noisy for long computations and too small for full error correction. This is where Grover's algorithm, variational methods (like VQE), and small demonstrations live.

### The Road Ahead

The path from NISQ to fault-tolerant quantum computing requires three things happening simultaneously:

1. **More qubits** — scaling from hundreds to millions.
2. **Better qubits** — pushing error rates below the threshold.
3. **Error correction** — the software layer that bridges the gap.

Progress is real: Google's 2024 result showed that a distance-5 surface code outperformed a distance-3 code, the first time error correction was shown to improve with code distance. IBM's roadmap targets error-corrected systems by 2029.

### Full-Stack Perspective

Looking back at the whole course, we can now see the full stack of quantum computing:

| Layer | What We Covered | Key Lectures |
|:-----:|:----------------|:-------------|
| Physics | Waves, interference, superposition | 2.1–2.3 |
| Qubits | Bloch sphere, Rabi/Ramsey, decoherence | 2.4–2.7 |
| Entanglement | Bell states, CHSH, nonlocality | 3.1–3.4 |
| Protocols | QKD, teleportation, superdense coding | 4.2–4.4 |
| Algorithms | Deutsch-Jozsa, Grover's | 4.5–4.6 |
| Hardware | Superconducting, ions, atoms, photons | 4.7 |
| Error Correction | Repetition codes, Shor code, surface codes | **Today** |

From a single beam splitter in the Mach-Zehnder interferometer to a million-qubit fault-tolerant quantum computer — it's all the same physics of superposition and interference, applied at increasing scales of complexity.

------------------------------------------------------------------------

## Summary

- **Three obstacles** seem to prevent quantum error correction: no-cloning, measurement collapse, and continuous errors. All three have elegant workarounds.

- **The bit-flip code** encodes $\alpha|0\rangle + \beta|1\rangle$ as $\alpha|000\rangle + \beta|111\rangle$ and uses syndrome measurement (parity checks on ancilla qubits) to detect and correct single $X$ errors *without* measuring the data qubits.

- **The phase-flip code** uses the same idea in the Hadamard basis to correct $Z$ errors.

- **The Shor code** concatenates both codes (9 qubits per logical qubit) and corrects any single-qubit error. The **discretization theorem** ensures that correcting Pauli errors suffices for continuous errors too.

- **The threshold theorem** guarantees that if physical error rates are below a threshold (~1% for surface codes), we can make logical error rates arbitrarily small. Real hardware is approaching this threshold.

- **Surface codes** are the leading approach for fault-tolerant quantum computing, using 2D arrays of qubits with local parity measurements.

------------------------------------------------------------------------

## What's Next

With error correction, we've covered every layer of the quantum computing stack. For the remaining weeks, you'll apply what you've learned to a **final project** — pick a topic from anywhere in the course, go deeper, build something, and present it to the class. The project list includes implementing algorithms on real IBM hardware, exploring error correction codes, quantum sensing experiments, and more. Details will be posted this week.
