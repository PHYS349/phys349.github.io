---
kernelspec:
  name: python3
  display_name: Python 3
---

# Lecture 5.2 — Quantum Error Correction: Protecting Quantum Information

## Where We Left Off

In the last lecture we surveyed the physical platforms that implement quantum gates: superconducting transmons driven by microwave pulses, trapped ions manipulated with lasers, neutral atoms in optical tweezer arrays, and photonic qubits routed through beam splitters. Every one of these platforms faces the same fundamental enemy: noise. Qubits decohere. Gates are not perfect. Measurements introduce errors. Back in Lecture 2.7, we quantified this with $T_1$ (energy relaxation) and $T_2$ (dephasing), and we saw that even the best qubits lose their quantum information on timescales of microseconds to seconds.

This raises an urgent question: if qubits are so fragile, how can we ever build quantum computers that run long algorithms like Shor's factoring? The answer is **quantum error correction**, a set of techniques that use redundancy and entanglement to protect quantum information from noise, without ever looking at (and thus destroying) the quantum state.

Today we'll build the theory from the ground up, starting with the reasons why classical error correction strategies don't work for qubits, and ending with a working error-correcting code in Qiskit.

------------------------------------------------------------------------

## Part 1: Why Error Correction Is Different for Qubits

### Classical Error Correction: Just Copy It

In classical computing, error correction is conceptually simple. To protect a bit against a random flip, you copy it:

$$0 \to 000, \qquad 1 \to 111$$

If noise flips one of the three bits, say $000 \to 010$, you take a majority vote and recover the original. This is the **3-bit repetition code**, and it can correct any single-bit error.

Simple. Elegant. And completely impossible for qubits.

### Three Obstacles

Three features of quantum mechanics seem to make quantum error correction impossible:

1. The **no-cloning theorem**. We proved in Lecture 4.0 that you cannot copy an unknown quantum state: there is no unitary that takes $|\psi\rangle|0\rangle \to |\psi\rangle|\psi\rangle$ for arbitrary $|\psi\rangle$. So the classical strategy of "just copy the bit three times" appears dead on arrival.

2. Measurement collapses the state. To detect a classical error, you read the bits. But measuring a qubit in superposition $\alpha|0\rangle + \beta|1\rangle$ collapses it to $|0\rangle$ or $|1\rangle$, destroying the very information you're trying to protect.

3. Errors are continuous. A classical bit can only be in state 0 or 1, so the only error is a flip. But a qubit can undergo any rotation: $|\psi\rangle \to e^{i\epsilon}R_x(\delta)|\psi\rangle$. There seem to be infinitely many types of errors to correct.

None of these obstacles is actually fatal. No-cloning prevents copying, but we can entangle qubits to spread information across multiple qubits without copying. Measurement collapse is avoided by measuring correlations (parities between qubits) instead of the qubits themselves. And continuous errors are handled by a remarkable theorem: correcting a discrete set of errors (the Pauli errors $X$, $Y$, $Z$) automatically corrects all continuous errors too.

Let's see how each of these workarounds plays out.

### The Key Insight: Partial Measurement and Redundancy

The idea that makes everything work starts with a question: can you learn something about errors without learning anything about the quantum state itself?

Yes, if you are careful about what you measure. The trick is a **partial measurement**. Instead of measuring the data qubits directly (which would collapse their superposition), you measure only an auxiliary degree of freedom that carries information about the error but none about the encoded data.

Here is the setup. Suppose you encode one logical qubit into three physical qubits, so the information lives in the joint state of all three. A single bit-flip error changes one qubit relative to the other two. If you could ask the question "are qubits 0 and 1 the same or different?" you would learn whether an error hit one of them, but you would learn nothing about whether the encoded state was $|000\rangle$ or $|111\rangle$ (or any superposition of the two). The answer to the parity question is the same for both logical basis states.

To implement this, you bring in an **ancilla** qubit, run a CNOT from each data qubit into the ancilla, and measure only the ancilla. The ancilla absorbs the parity information and, when measured, collapses onto 0 (same) or 1 (different). The data qubits are never measured, so their superposition survives.

This is the central trick of quantum error correction: extract information about the error without extracting information about the encoded state. The redundancy in the encoding is what creates a space where error information lives separately from the logical quantum information. In the next section, we build this idea into a concrete code.

------------------------------------------------------------------------

## Part 2: The 3-Qubit Bit-Flip Code

### Encoding Without Cloning

The 3-qubit bit-flip code encodes one logical qubit into three physical qubits. The encoding is:

$$|0\rangle \to |0_L\rangle = |000\rangle, \qquad |1\rangle \to |1_L\rangle = |111\rangle$$

For a general state $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$, the encoded state is:

$$|\psi_L\rangle = \alpha|000\rangle + \beta|111\rangle$$

This is not cloning. The state $\alpha|0\rangle + \beta|1\rangle$ has not been copied three times. Instead, the three qubits are entangled: if you measure any single qubit, you get no information about $\alpha$ and $\beta$. The information is stored in the correlations between the qubits, not in any individual qubit. This is a GHZ-type state, just like the ones we studied in Lecture 4.1.

The encoding circuit is simple: two CNOT gates from the data qubit to two fresh ancilla qubits. The following Qiskit code builds this circuit and draws it. The first CNOT copies the $|0\rangle/|1\rangle$ value of qubit 0 into qubit 1, and the second copies it into qubit 2, creating the entangled triple.

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

Let's verify the encoding works. We prepare $|+\rangle = (|0\rangle + |1\rangle)/\sqrt{2}$ on qubit 0 and then run the encoding circuit. The result should be the GHZ state $(|000\rangle + |111\rangle)/\sqrt{2}$, which is exactly $\alpha|000\rangle + \beta|111\rangle$ with $\alpha = \beta = 1/\sqrt{2}$.

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

### What Happens When an Error Hits

Now suppose the encoding is done and one of the three qubits suffers a bit-flip ($X$ error). The bit-flip code protects against a single $X$ error on any one of the three qubits. After encoding, one of four things can happen:

|     Error      | State after error                                          |
|:--------------:|:-----------------------------------------------------------|
|      None      | $\alpha\vert 000\rangle + \beta\vert 111\rangle$           |
| $X$ on qubit 0 | $\alpha\vert 100\rangle + \beta\vert 011\rangle$           |
| $X$ on qubit 1 | $\alpha\vert 010\rangle + \beta\vert 101\rangle$           |
| $X$ on qubit 2 | $\alpha\vert 001\rangle + \beta\vert 110\rangle$           |

Notice that in every case, the superposition of $\alpha$ and $\beta$ is preserved. The error just moved some bits around. The key question is: how do we figure out which qubit flipped without measuring the data qubits directly?

This is where the partial-measurement idea from Part 1 becomes concrete. We measure the **parity** of pairs of data qubits. The parity of two qubits is a single classical bit that answers: "are these two qubits the same, or different?" If both are $|00\rangle$ or both are $|11\rangle$, the parity is 0 (same). If they are $|01\rangle$ or $|10\rangle$, the parity is 1 (different). Critically, the parity tells you nothing about which of the two "same" states ($|00\rangle$ vs. $|11\rangle$) or which of the two "different" states ($|01\rangle$ vs. $|10\rangle$) you have. That is exactly the information we want to keep hidden.

Two parity checks are enough to locate a single bit-flip among three qubits:

- Parity of qubits 0 and 1: same or different?
- Parity of qubits 1 and 2: same or different?

The results form a 2-bit **syndrome**:

| Syndrome ($s_1 s_2$) | Meaning | Correction |
|:--------------------:|:--------|:-----------|
| 00 | No error | Do nothing |
| 10 | Qubits 0,1 differ but 1,2 agree, so qubit 0 flipped | $X$ on qubit 0 |
| 11 | Both pairs differ, so qubit 1 flipped | $X$ on qubit 1 |
| 01 | Qubits 0,1 agree but 1,2 differ, so qubit 2 flipped | $X$ on qubit 2 |

The syndrome tells you which qubit had an error, but it does not reveal what the encoded state $\alpha$ and $\beta$ are. The parity measurement extracts information about which qubit flipped without revealing what the logical state is. The values of $\alpha$ and $\beta$ remain hidden in the entanglement between the data qubits. This is what distinguishes quantum error correction from classical: we can diagnose errors while keeping the quantum information intact.

### Measuring Parity with a Circuit

How do you actually measure a parity on a quantum computer? You cannot just "look" at two qubits and ask if they match. Instead, you use a small circuit built from two CNOT gates and one extra qubit.

Start with a fresh **ancilla** qubit in the state $|0\rangle$. Run a CNOT from qubit A into the ancilla, then a CNOT from qubit B into the ancilla. Recall that a CNOT flips the target whenever the control is $|1\rangle$. So after both gates:

- If A and B are in the same state ($|00\rangle$ or $|11\rangle$), the ancilla gets flipped either zero or two times and ends up back at $|0\rangle$. Parity = 0 (same).
- If A and B are in different states ($|01\rangle$ or $|10\rangle$), the ancilla gets flipped exactly once and ends up at $|1\rangle$. Parity = 1 (different).

The ancilla now holds the XOR of A and B. You measure only the ancilla and leave the data qubits alone. Here is the circuit:

```{code-cell} python
# The parity-check circuit: two CNOTs compute XOR into an ancilla
qc_parity_demo = QuantumCircuit(3)  # qubit 0 = A, qubit 1 = B, qubit 2 = ancilla
qc_parity_demo.cx(0, 2)   # CNOT: A → ancilla
qc_parity_demo.cx(1, 2)   # CNOT: B → ancilla
# After this, ancilla = A ⊕ B (the parity)
qc_parity_demo.draw(output="mpl", style="iqp")
```

Why does this work without disturbing the data? Think about what information the ancilla carries. It records whether A and B agree or disagree. But it does not record which of the two "agree" states ($|00\rangle$ vs. $|11\rangle$) or which of the two "disagree" states ($|01\rangle$ vs. $|10\rangle$) you have. When you measure the ancilla and get 0, you know the data qubits matched, but you have learned nothing about whether they were both $|0\rangle$ or both $|1\rangle$. The superposition between those two cases survives. This is exactly the partial measurement we need: error information comes out, logical information stays hidden.

Let's verify this on the encoded state. We prepare $\alpha|000\rangle + \beta|111\rangle$, flip qubit 1 to simulate an error, and then run the two-CNOT parity check on qubits 0 and 1. The ancilla should come out $|1\rangle$ (they disagree), while $\alpha$ and $\beta$ survive in the data qubits.

```{code-cell} python
# Parity check in action: encode, introduce error, check parity of (q0, q1)
qc_parity = QuantumCircuit(4)  # qubits 0,1,2 = data; qubit 3 = ancilla

# Encode: |ψ⟩ = cos(π/6)|000⟩ + sin(π/6)|111⟩
qc_parity.ry(np.pi/3, 0)
qc_parity.cx(0, 1)
qc_parity.cx(0, 2)

# Error: flip qubit 1
qc_parity.x(1)

# Parity check: CNOT from qubit 0 and qubit 1 into ancilla (qubit 3)
qc_parity.cx(0, 3)
qc_parity.cx(1, 3)

sv = Statevector(qc_parity)
print("State after error on qubit 1 + parity of (q0, q1) in ancilla q3:")
for i, amp in enumerate(sv.data):
    if abs(amp) > 0.01:
        bits = format(i, '04b')
        print(f"  |{bits}⟩: {amp:.4f}   (ancilla = {bits[0]})")
print("\nAncilla is always 1 → qubits 0 and 1 disagree (error detected).")
print("But α and β are still intact in the data qubits.")
```

The full error-correction circuit uses this same trick twice: one ancilla for the parity of qubits 0 and 1, and a second ancilla for the parity of qubits 1 and 2. Together, the two parity bits give the syndrome that pinpoints which qubit (if any) was flipped.

### Building the Full Circuit in Qiskit

Now let's put the whole pipeline together: prepare a state, encode it, introduce an error, measure both syndromes, and apply the correction. The function below builds a circuit with five qubits (three data qubits and two ancilla qubits for the syndrome) and two classical bits to hold the syndrome result.

The circuit proceeds in stages. First, it prepares the state $|\psi\rangle = \cos(\pi/6)|0\rangle + \sin(\pi/6)|1\rangle$ on data qubit 0. Then the two CNOT gates encode this into the three-qubit logical state. An $X$ gate on the chosen data qubit simulates the error. Next, four CNOT gates compute the two parities into the ancilla qubits (exactly the two-CNOT trick from the example above, done twice). The ancillas are measured to get the syndrome. Finally, `if_test` blocks apply the appropriate $X$ correction conditioned on the syndrome result.

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

Let's verify that the syndrome computation correctly identifies the error. The code below tracks the full 5-qubit statevector through encoding, error, and syndrome extraction (without collapsing the state via measurement). You should see that the ancilla qubits end up in the state $|11\rangle$ (syndrome = 11), indicating that qubit 1 was flipped, while the data qubits still carry the original $\alpha$ and $\beta$ in their amplitudes.

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
The `if_test` syntax in Qiskit implements **dynamic circuits**: the correction gates are applied conditionally based on the measurement results from the syndrome qubits. This is a real feature of modern quantum hardware (IBM's Eagle and Heron processors support mid-circuit measurement). On older hardware without this capability, you would need to defer the measurement to the end and use quantum-controlled gates instead.
```

------------------------------------------------------------------------

## Part 3: The Phase-Flip Code

The bit-flip code handles $X$ errors, where a qubit flips between $|0\rangle$ and $|1\rangle$. But real qubits also suffer phase errors. In this section, we ask: can we adapt the same encoding-and-syndrome strategy to protect against $Z$ errors instead? The answer turns out to be surprisingly simple, and it sets up the final step where we combine both protections into a single code.

### A Different Kind of Error

A $Z$ gate turns $|+\rangle$ into $|-\rangle$ without flipping the computational basis state. In the $Z$ basis, a **phase error** is invisible:

$$Z|0\rangle = |0\rangle, \qquad Z|1\rangle = -|1\rangle$$

If your state is $\alpha|0\rangle + \beta|1\rangle$, a phase error gives $\alpha|0\rangle - \beta|1\rangle$. The probabilities $|\alpha|^2$ and $|\beta|^2$ don't change, but the relative phase is flipped, which matters for interference and entanglement.

### The Hadamard Trick

Here's the elegant insight: a phase flip in the $Z$ basis is a bit flip in the $X$ basis. Since $HZH = X$, we can convert the phase-flip problem into a bit-flip problem:

1. Apply $H$ to each qubit.
2. Run the bit-flip code.
3. Apply $H$ to each qubit again.

The encoding becomes:

$$|0\rangle \to |{+}{+}{+}\rangle, \qquad |1\rangle \to |{-}{-}{-}\rangle$$

or equivalently:

$$|\psi_L\rangle = \alpha|{+}{+}{+}\rangle + \beta|{-}{-}{-}\rangle$$

Now a $Z$ error on any single qubit can be detected and corrected by the same syndrome measurement, but in the Hadamard basis.

The following code builds the phase-flip encoding circuit. It first runs the same two CNOT gates as the bit-flip code, then applies Hadamard to all three qubits to rotate from the $Z$ basis into the $X$ basis.

```{code-cell} python
# Phase-flip code: bit-flip encoding → H on all qubits
def encode_phase_flip():
    """Encode qubit 0 against single phase-flip errors."""
    qc = QuantumCircuit(3)
    # First encode in the computational basis
    qc.cx(0, 1)
    qc.cx(0, 2)
    # Then rotate into the Hadamard basis
    qc.h(0)
    qc.h(1)
    qc.h(2)
    return qc

pf = encode_phase_flip()
pf.draw(output="mpl", style="iqp")
```

Let's verify: encoding $|0\rangle$ should produce $|{+}{+}{+}\rangle$, which is an equal superposition of all eight computational basis states.

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

So the bit-flip code corrects $X$ errors, and the phase-flip code corrects $Z$ errors. The natural next question: can we correct both at the same time? Peter Shor's insight in 1995 was to concatenate the two codes: first encode each qubit against phase-flips (3 qubits per logical qubit), then encode each of those 3 qubits against bit-flips (3 qubits each). This gives $3 \times 3 = 9$ physical qubits per logical qubit and can correct any single-qubit error.

------------------------------------------------------------------------

## Part 4: The Shor Code — Correcting Any Error

### Concatenation: Nesting Two Codes

The **Shor code** (1995) was the first quantum error-correcting code that corrects arbitrary single-qubit errors. It uses 9 physical qubits per logical qubit, arranged in a two-level encoding:

Level 1 (phase-flip protection): encode the logical qubit into 3 blocks:

$$|0_L\rangle = \frac{1}{2\sqrt{2}}\left(|000\rangle + |111\rangle\right)\left(|000\rangle + |111\rangle\right)\left(|000\rangle + |111\rangle\right)$$

$$|1_L\rangle = \frac{1}{2\sqrt{2}}\left(|000\rangle - |111\rangle\right)\left(|000\rangle - |111\rangle\right)\left(|000\rangle - |111\rangle\right)$$

Level 2 (bit-flip protection within each block): each block of 3 qubits is itself a bit-flip code.

A bit-flip ($X$) on any qubit is caught by the bit-flip code within that block. A phase-flip ($Z$) on any qubit flips the sign of one block, which is caught by the phase-flip code across the three blocks.

### The Discretization of Errors

But what about the third obstacle from Part 1, continuous errors? A qubit might not suffer a clean $X$ or $Z$ flip. It might rotate by a small angle: $|\psi\rangle \to (\cos\epsilon \cdot I + i\sin\epsilon \cdot X)|\psi\rangle$.

Here's the key theorem that makes quantum error correction practical:

```{admonition} Discretization of Errors
:class: important
Any single-qubit error can be written as a linear combination of the Pauli operators $\{I, X, Y, Z\}$. If a code can correct the discrete errors $X$, $Y$, and $Z$, it automatically corrects all single-qubit errors, including continuous rotations.
```

Why? Because the syndrome measurement projects the continuous error onto one of the discrete Pauli errors. When you measure the syndrome, you're asking "did an $X$, $Y$, $Z$, or no error occur?" The measurement collapses the error (not the data!) into one of these discrete possibilities, which you then correct.

This is profoundly counterintuitive: the act of error detection simplifies the error. It's another example of how measurement in quantum mechanics isn't just passive observation; it's an active process that shapes the state.

Since $Y = iXZ$, correcting $X$ and $Z$ errors suffices to handle $Y$ errors too. The Shor code can correct any single-qubit error on any of its 9 physical qubits.

Let's see this discretization in action. The code below applies a small rotation $R_x(\theta)$ (not a clean $X$ flip) to qubit 1 of a bit-flip-encoded state, then computes the syndrome. You'll see that the syndrome measurement produces only two possible outcomes: "no error" (with probability $\cos^2(\theta/2)$) or "X error on qubit 1" (with probability $\sin^2(\theta/2)$). The continuous error has been projected onto discrete alternatives.

```{code-cell} python
# Demonstration: a small rotation error is "discretized" by syndrome measurement
# We'll show this with the bit-flip code for simplicity

# Apply a small rotation error (not a clean X flip) to qubit 1
theta_error = 0.3  # small angle

qc = QuantumCircuit(5)
qc.ry(np.pi/3, 0)   # Prepare |ψ⟩
qc.cx(0, 1)          # Encode
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

The code below simulates what happens when we run the bit-flip code with realistic noise. It uses a simple noise model: each CNOT gate has a small probability of introducing a depolarizing error. The function builds the full encode, error, syndrome, correct, decode pipeline, then runs it with 10,000 shots and reports the fraction of shots where the final measurement matches the expected result.

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
print(f"Perfect gates + X error on q0:  success = {rate:.1%}")

# Perfect gates, no error
rate = run_bit_flip_experiment(error_rate=0.0, gate_error=0.0)
print(f"Perfect gates, no error:        success = {rate:.1%}")

# Noisy gates, with error
for ge in [0.001, 0.01, 0.05]:
    rate = run_bit_flip_experiment(error_rate=1.0, gate_error=ge)
    print(f"Gate error = {ge:.3f} + X error:   success = {rate:.1%}")
```

### The Threshold Theorem

The results above illustrate a key tension: error correction uses extra gates, and each gate introduces its own errors. If the gate errors are too large, the correction process introduces more errors than it fixes.

The **threshold theorem** resolves this:

```{admonition} Quantum Error Correction Threshold
:class: important
There exists a threshold error rate $p_{\text{th}}$ such that if the physical error rate per gate $p < p_{\text{th}}$, then error correction can reduce the logical error rate to arbitrarily low levels by using enough physical qubits. The overhead grows only polynomially in the desired logical error rate.
```

For **surface codes** (the most promising architecture for near-term devices), the threshold is approximately $p_{\text{th}} \approx 1\%$. Current hardware is approaching this threshold: IBM's Heron processors achieve two-qubit gate errors around 0.3%, and Google's Willow processor demonstrated error correction improving with system size for the first time in December 2024.

### Surface Codes: The Path Forward

The repetition code we studied corrects only one type of error ($X$ or $Z$, not both). The Shor code corrects both but uses 9 qubits per logical qubit. Modern quantum error correction uses surface codes, which arrange physical qubits on a 2D grid:

- Data qubits sit at the vertices.
- Ancilla qubits sit at the faces and edges, measuring $X$-type and $Z$-type parities.
- A distance-$d$ surface code uses $\sim 2d^2$ physical qubits to encode 1 logical qubit and correct up to $\lfloor(d-1)/2\rfloor$ errors.

For a distance-3 surface code: 17 physical qubits protect 1 logical qubit against any single error. For a distance-5 code: about 49 qubits, correcting up to 2 errors.

To put the overhead in perspective: estimates suggest that running Shor's algorithm to factor a 2048-bit RSA key would require about 20 million physical qubits with surface code error correction. If each logical qubit requires about 1,000 physical qubits (at the necessary code distance), the algorithm itself needs roughly 20,000 logical qubits. Most of the qubits in a fault-tolerant quantum computer would be dedicated to error correction, not computation.

------------------------------------------------------------------------

## Part 6: The Bigger Picture

### Where We Are: The NISQ Era

The current era of quantum computing is called **NISQ** (Noisy Intermediate-Scale Quantum). We have processors with 50 to 1,000+ qubits, but they're too noisy for long computations and too small for full error correction. This is where Grover's algorithm, variational methods (like VQE), and small demonstrations live.

### The Road Ahead

The path from NISQ to fault-tolerant quantum computing requires three things happening simultaneously:

1. More qubits, scaling from hundreds to millions.
2. Better qubits, pushing error rates below the threshold.
3. Error correction, the software layer that bridges the gap.

Progress is real: Google's 2024 result showed that a distance-5 surface code outperformed a distance-3 code, the first time error correction was shown to improve with code distance. IBM's roadmap targets error-corrected systems by 2029.

### Full-Stack Perspective

Looking back at the whole course, we can now see the full stack of quantum computing:

|      Layer       | What We Covered                            | Key Lectures |
|:----------------:|:-------------------------------------------|:-------------|
|     Physics      | Waves, interference, superposition         | 2.1–2.3      |
|      Qubits      | Bloch sphere, Rabi/Ramsey, decoherence     | 2.4–2.7      |
|   Entanglement   | Bell states, CHSH, nonlocality             | 3.1–3.4      |
|    Protocols     | QKD, teleportation, superdense coding      | 4.2–4.4      |
|    Algorithms    | Deutsch-Jozsa, Grover's                    | 4.5–4.6      |
|     Hardware     | Superconducting, ions, atoms, photons      | 5.1          |
| Error Correction | Repetition codes, Shor code, surface codes | Today        |

From a single beam splitter in the Mach-Zehnder interferometer to a million-qubit fault-tolerant quantum computer, it's all the same physics of superposition and interference, applied at increasing scales of complexity.

------------------------------------------------------------------------

## Summary

- Three obstacles seem to prevent quantum error correction: no-cloning, measurement collapse, and continuous errors. All three have elegant workarounds.

- The bit-flip code encodes $\alpha|0\rangle + \beta|1\rangle$ as $\alpha|000\rangle + \beta|111\rangle$ and uses syndrome measurement (parity checks on ancilla qubits) to detect and correct single $X$ errors without measuring the data qubits.

- The phase-flip code uses the same idea in the Hadamard basis to correct $Z$ errors.

- The Shor code concatenates both codes (9 qubits per logical qubit) and corrects any single-qubit error. The **discretization theorem** ensures that correcting Pauli errors suffices for continuous errors too.

- The **threshold theorem** guarantees that if physical error rates are below a threshold (~1% for surface codes), we can make logical error rates arbitrarily small. Real hardware is approaching this threshold.

- Surface codes are the leading approach for fault-tolerant quantum computing, using 2D arrays of qubits with local parity measurements.

------------------------------------------------------------------------

## What's Next

With error correction, we've covered every layer of the quantum computing stack. For the remaining weeks, you'll apply what you've learned to a final project: pick a topic from the project menu, run it on real IBM hardware, and present your results to the class. Details are on the [Final Project page](final_project.md).
