---
kernelspec:
  name: python3
  display_name: Python 3
---

# Lecture 4.1 — Circuit Challenges and the CHSH Game

## Where We Left Off

Last lecture we built the quantum circuit formalism: wires carry qubits, gates are unitaries, and measurement produces classical bits. We constructed Bell states from a Hadamard and a CNOT, and proved the no-cloning theorem — quantum mechanics forbids copying arbitrary states because cloning is a nonlinear operation.

Today we put circuits to work. We'll build a quantum version of a classical arithmetic circuit, extend Bell states to three qubits, and then play a game that demonstrates quantum advantage in a precise, quantitative way.

------------------------------------------------------------------------

## Part 1: The Quantum Half-Adder

### Reversibility

In Lecture 3.5 we saw the classical half-adder: two input bits $a$ and $b$, two outputs Sum $= a \oplus b$ and Carry $= a \wedge b$. That circuit has two inputs and two outputs — no information is lost.

But the AND gate by itself is **irreversible**: two input bits produce one output bit, so information is destroyed. In quantum mechanics, all operations are unitary, which means they must be **reversible** — you can always run them backward. This is a fundamental constraint.

To build quantum arithmetic, we need to implement classical logic gates *reversibly*. The trick: instead of overwriting, write the result into an extra "target" qubit initialized to $|0\rangle$.

### Building the Half-Adder

We need four qubits:

- Qubits 0, 1: inputs $a$ and $b$
- Qubit 2: target for the **sum** ($a \oplus b$)
- Qubit 3: target for the **carry** ($a \wedge b$)

The sum is XOR — and we already know a gate that does this. CNOT flips the target when the control is $|1\rangle$, so two CNOTs (one controlled by $a$, one by $b$) compute $a \oplus b$ into the target qubit:

$$
|a\rangle|b\rangle|0\rangle \xrightarrow{\text{CNOT}_{0 \to 2}} |a\rangle|b\rangle|a\rangle \xrightarrow{\text{CNOT}_{1 \to 2}} |a\rangle|b\rangle|a \oplus b\rangle
$$

The carry is AND — and the Toffoli gate (CCX) computes exactly this. It flips the target only when *both* controls are $|1\rangle$:

$$
|a\rangle|b\rangle|0\rangle \xrightarrow{\text{CCX}} |a\rangle|b\rangle|a \wedge b\rangle
$$

Putting it together:

```{code-cell} python
:tags: [hide-input]
from qiskit import QuantumCircuit

# Quantum half-adder circuit
qc_adder = QuantumCircuit(4)

# Sum: XOR via two CNOTs into qubit 2
qc_adder.cx(0, 2)
qc_adder.cx(1, 2)

# Carry: AND via Toffoli into qubit 3
qc_adder.ccx(0, 1, 3)

qc_adder.draw("mpl")
```

The inputs $a$ and $b$ (qubits 0 and 1) pass through unchanged — the operation is reversible. The sum and carry appear on qubits 2 and 3.

Let's verify it works for all four input combinations:

```{code-cell} python
from qiskit.quantum_info import Statevector

for a in [0, 1]:
    for b in [0, 1]:
        qc = QuantumCircuit(4)
        if a: qc.x(0)
        if b: qc.x(1)
        # Sum
        qc.cx(0, 2)
        qc.cx(1, 2)
        # Carry
        qc.ccx(0, 1, 3)
        sv = Statevector.from_instruction(qc)
        # Extract the output: find which basis state has amplitude 1
        probs = sv.probabilities_dict()
        outcome = list(probs.keys())[0]
        # Qiskit ordering: outcome = q3 q2 q1 q0
        carry_out = int(outcome[0])
        sum_out = int(outcome[1])
        print(f"a={a}, b={b} → Sum={sum_out}, Carry={carry_out}  (binary: {carry_out}{sum_out} = {a+b})")
```

Every row matches the classical half-adder: $0+0=0$, $0+1=1$, $1+0=1$, $1+1=10$ (binary).

The key insight: **quantum circuits can do everything classical circuits can do**, as long as we make things reversible by adding target qubits. The Toffoli gate alone is classically universal — you can build any classical Boolean function from Toffolis.

------------------------------------------------------------------------

## Part 2: GHZ States — Entanglement Beyond Two Qubits

### From Bell to GHZ

Bell states entangle two qubits. What happens with three?

The **Greenberger–Horne–Zeilinger (GHZ) state** is the natural extension:

$$
|\text{GHZ}\rangle = \frac{1}{\sqrt{2}}(|000\rangle + |111\rangle)
$$

All three qubits are either all $|0\rangle$ or all $|1\rangle$ — maximally correlated, just like a Bell state but across three particles.

### Building GHZ with a Circuit

The construction is a direct extension of the Bell state circuit. Start with $|000\rangle$, apply Hadamard to the first qubit, then CNOT from the first to the second, and another CNOT from the first to the third:

```{code-cell} python
:tags: [hide-input]
qc_ghz = QuantumCircuit(3)
qc_ghz.h(0)
qc_ghz.cx(0, 1)
qc_ghz.cx(0, 2)
qc_ghz.draw("mpl")
```

Let's trace the state:

**Step 1: Hadamard on qubit 0.**

$$
(H \otimes I \otimes I)|000\rangle = \frac{1}{\sqrt{2}}(|000\rangle + |100\rangle)
$$

**Step 2: CNOT from qubit 0 to qubit 1.**

$$
\frac{1}{\sqrt{2}}(|000\rangle + |100\rangle) \to \frac{1}{\sqrt{2}}(|000\rangle + |110\rangle)
$$

The CNOT flips qubit 1 whenever qubit 0 is $|1\rangle$.

**Step 3: CNOT from qubit 0 to qubit 2.**

$$
\frac{1}{\sqrt{2}}(|000\rangle + |110\rangle) \to \frac{1}{\sqrt{2}}(|000\rangle + |111\rangle) = |\text{GHZ}\rangle
$$

```{code-cell} python
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(3)
qc.h(0)
qc.cx(0, 1)
qc.cx(0, 2)

psi = Statevector.from_instruction(qc)
psi.draw("text")
```

### GHZ Entanglement Is Fragile

What happens if you measure just one qubit of a GHZ state? If the first qubit comes out $|0\rangle$, the remaining two qubits collapse to $|00\rangle$ — a product state, not entangled at all. Similarly, measuring $|1\rangle$ collapses the rest to $|11\rangle$.

This is qualitatively different from Bell states. If you measure one qubit of $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$, the other qubit collapses to a definite state too — but the two-qubit state was only bipartite to begin with.

For GHZ, the three-way entanglement is completely destroyed by measuring *any* single qubit. This fragility is a defining feature of multipartite entanglement and has consequences for quantum error correction and quantum networking.

Let's verify this with Qiskit. We'll measure qubit 0 by projecting onto $|0\rangle$ and looking at what's left:

```{code-cell} python
import numpy as np

# Build GHZ state
qc = QuantumCircuit(3)
qc.h(0)
qc.cx(0, 1)
qc.cx(0, 2)
ghz = Statevector.from_instruction(qc)

# Get probabilities — only |000⟩ and |111⟩ should appear
probs = ghz.probabilities_dict()
print("GHZ state probabilities:", probs)
print("\nMeasuring qubit 0 → |0⟩ collapses the state to |000⟩ (product state)")
print("Measuring qubit 0 → |1⟩ collapses the state to |111⟩ (product state)")
print("Either way, the remaining two qubits are NOT entangled.")
```

### Generalizing: $n$-Qubit GHZ

The pattern extends to any number of qubits: Hadamard on the first, then CNOT from the first to every other qubit.

$$
|\text{GHZ}_n\rangle = \frac{1}{\sqrt{2}}(|00\cdots 0\rangle + |11\cdots 1\rangle)
$$

```{code-cell} python
def ghz_circuit(n):
    """Build an n-qubit GHZ state circuit."""
    qc = QuantumCircuit(n)
    qc.h(0)
    for i in range(1, n):
        qc.cx(0, i)
    return qc

# Build and verify a 5-qubit GHZ state
qc5 = ghz_circuit(5)
psi5 = Statevector.from_instruction(qc5)
probs5 = psi5.probabilities_dict()
print("5-qubit GHZ probabilities:", probs5)
```

Only $|00000\rangle$ and $|11111\rangle$ survive — five qubits maximally correlated.

------------------------------------------------------------------------

## Part 3: The CHSH Game

### A Game of Correlation

In Lecture 3.4 we proved Bell's theorem: no local hidden variable model can reproduce all quantum predictions. Now we turn that result into a **game** — a precise, quantitative demonstration of quantum advantage.

The CHSH game (Clauser, Horne, Shimony, Holt, 1969) is played by two cooperating players, Alice and Bob, against a referee.

**Setup:**

- A referee sends Alice a random bit $x \in \{0, 1\}$ and Bob a random bit $y \in \{0, 1\}$.
- Alice and Bob **cannot communicate** after receiving their bits.
- Each responds with an answer: Alice sends back $a \in \{0, 1\}$, Bob sends back $b \in \{0, 1\}$.

**Winning condition:**

$$
a \oplus b = x \wedge y
$$

In words: Alice and Bob should give the **same** answer ($a = b$) unless both questions are 1 — in that case they should give **different** answers ($a \neq b$).

| Questions $(x, y)$ | Win if... |
|---------------------|-----------|
| $(0, 0)$ | $a = b$ |
| $(0, 1)$ | $a = b$ |
| $(1, 0)$ | $a = b$ |
| $(1, 1)$ | $a \neq b$ |

### The Classical Bound: 75%

What's the best strategy if Alice and Bob can only use classical resources (shared randomness, pre-agreed strategy, but no communication)?

Think about it: three of the four cases require $a = b$, but one requires $a \neq b$. If Alice and Bob always answer the same (say both answer 0), they win the first three cases but lose $(1,1)$. Any deterministic strategy loses at least one case out of four.

```{admonition} Theorem (Classical CHSH Bound)
:class: important

No classical strategy — deterministic or probabilistic — can win the CHSH game with probability greater than $3/4 = 75\%$.
```

**Proof sketch:** Any deterministic strategy is a pair of functions $a(x)$ and $b(y)$. If the strategy wins all three "match" cases, then $a(0) = b(0)$, $a(0) = b(1)$, and $a(1) = b(0)$, which forces $a(1) = b(1)$. But the fourth case requires $a(1) \neq b(1)$. Contradiction. So at most 3 out of 4 cases can be won. Since any probabilistic strategy is a mixture of deterministic ones, the bound extends.

### The Quantum Strategy: ~85%

Now Alice and Bob share an entangled Bell state $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$. They each measure their qubit in a basis that depends on their question.

**The key formula:** When Alice and Bob measure their halves of $|\Phi^+\rangle$ in bases rotated by angles $\alpha$ and $\beta$ from the $Z$-axis:

$$
\Pr(a = b) = \cos^2(\alpha - \beta), \qquad \Pr(a \neq b) = \sin^2(\alpha - \beta)
$$

This is the correlation structure of entanglement — the same physics behind Bell's theorem, now expressed as measurement-basis-dependent probabilities.

**Optimal angle choices:**

- Alice: $\alpha = 0$ if $x = 0$, and $\alpha = \pi/4$ if $x = 1$
- Bob: $\beta = \pi/8$ if $y = 0$, and $\beta = -\pi/8$ if $y = 1$

The measurement rotation is:

$$
R(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}
$$

Applied to a qubit before measuring in the $Z$-basis, this is equivalent to measuring in a basis rotated by angle $\theta$.

**Calculating the winning probability for each case:**

| $(x, y)$ | Need | $\alpha - \beta$ | Winning probability |
|-----------|------|-------------------|---------------------|
| $(0, 0)$ | $a = b$ | $0 - \pi/8 = -\pi/8$ | $\cos^2(\pi/8)$ |
| $(0, 1)$ | $a = b$ | $0 + \pi/8 = \pi/8$ | $\cos^2(\pi/8)$ |
| $(1, 0)$ | $a = b$ | $\pi/4 - \pi/8 = \pi/8$ | $\cos^2(\pi/8)$ |
| $(1, 1)$ | $a \neq b$ | $\pi/4 + \pi/8 = 3\pi/8$ | $\sin^2(3\pi/8) = \cos^2(\pi/8)$ |

Every case gives the same probability! The overall winning probability is:

$$
\Pr(\text{win}) = \cos^2(\pi/8) = \frac{2 + \sqrt{2}}{4} \approx 85.4\%
$$

This exceeds the classical limit of 75%. The advantage comes entirely from entanglement.

### Tsirelson's Bound

This quantum strategy is **optimal** — no quantum strategy can do better. The upper bound $\cos^2(\pi/8)$ is called **Tsirelson's bound** (1980), the quantum analog of the classical CHSH bound.

| Strategy | Max win probability |
|----------|---------------------|
| Classical (shared randomness) | $75\%$ |
| Quantum (shared entanglement) | $\approx 85.4\%$ |
| No-signaling (theoretical maximum) | $100\%$ |

The gap between 75% and 85.4% is a direct, quantitative measure of quantum advantage. It's not just "quantum is different" — it's "quantum wins 10% more often, and we can measure that."

### Building the CHSH Circuit in Qiskit

Let's implement the quantum strategy. For each pair of questions $(x, y)$, we:

1. Prepare $|\Phi^+\rangle$
2. Rotate Alice's qubit by angle $\alpha(x)$
3. Rotate Bob's qubit by angle $\beta(y)$
4. Measure both qubits

```{code-cell} python
from qiskit import QuantumCircuit
import numpy as np

def chsh_circuit(x, y):
    """Build the CHSH game circuit for questions x, y."""
    qc = QuantumCircuit(2, 2)

    # Step 1: Create Bell state |Φ⁺⟩
    qc.h(0)
    qc.cx(0, 1)
    qc.barrier()

    # Step 2: Alice's rotation (on qubit 0)
    alpha = 0 if x == 0 else np.pi/4
    qc.ry(-2 * alpha, 0)

    # Step 3: Bob's rotation (on qubit 1)
    beta = np.pi/8 if y == 0 else -np.pi/8
    qc.ry(-2 * beta, 1)
    qc.barrier()

    # Step 4: Measure
    qc.measure([0, 1], [0, 1])

    return qc

# Show the circuit for (x=1, y=1) — the "hard" case
chsh_circuit(1, 1).draw("mpl")
```

```{admonition} Why Ry(-2θ)?
:class: tip

The `ry(θ)` gate in Qiskit rotates by angle $\theta/2$ around the $Y$-axis of the Bloch sphere (it applies $e^{-i\theta Y/2}$). To rotate the measurement basis by angle $\alpha$, we need `ry(-2*alpha)`. The minus sign is because we're rotating the state in the opposite direction to rotating the measurement basis.
```

### Running the Game

Now let's play the game many times and tally the results:

```{code-cell} python
from qiskit_aer import AerSimulator

simulator = AerSimulator()
shots = 10000

total_wins = 0
total_games = 0

print("CHSH Game Results (quantum strategy)")
print("=" * 55)

for x in [0, 1]:
    for y in [0, 1]:
        qc = chsh_circuit(x, y)
        counts = simulator.run(qc, shots=shots).result().get_counts()

        wins = 0
        for outcome, count in counts.items():
            # outcome is "ba" in Qiskit ordering (q1 q0)
            a = int(outcome[1])  # Alice (qubit 0)
            b = int(outcome[0])  # Bob (qubit 1)
            # Win condition: a ⊕ b = x ∧ y
            if (a ^ b) == (x & y):
                wins += count

        win_rate = wins / shots
        total_wins += wins
        total_games += shots

        need = "a=b" if (x & y) == 0 else "a≠b"
        print(f"  (x={x}, y={y})  need {need:4s}  →  win rate = {win_rate:.3f}")

print(f"\nOverall win rate: {total_wins/total_games:.3f}")
print(f"Classical bound:  0.750")
print(f"Tsirelson bound:  {np.cos(np.pi/8)**2:.3f}")
```

You should see an overall win rate around 85%, well above the classical limit of 75%.

### Classical Comparison

For comparison, let's run the best classical strategy: Alice and Bob both always answer 0.

```{code-cell} python
print("CHSH Game Results (best classical strategy: always answer 0)")
print("=" * 55)

classical_wins = 0
for x in [0, 1]:
    for y in [0, 1]:
        a, b = 0, 0  # Always answer 0
        win = (a ^ b) == (x & y)
        result = "WIN " if win else "LOSE"
        print(f"  (x={x}, y={y})  a={a}, b={b}  →  {result}")
        if win:
            classical_wins += 1

print(f"\nClassical win rate: {classical_wins}/4 = {classical_wins/4:.3f}")
```

Three wins out of four — exactly 75%. No classical strategy can do better.

### What the CHSH Game Tells Us

The CHSH game distills the content of Bell's theorem into a single number. In Lecture 3.4, we showed that quantum correlations violate Bell inequalities. The CHSH game makes this operational: if you can win more than 75% of the time, you must be using a non-classical resource.

This is the template for quantum advantage: define a task, prove a classical upper bound, then show that quantum mechanics beats it. We'll see this pattern again with the Deutsch-Jozsa and Grover algorithms.

------------------------------------------------------------------------

## Summary

- **Quantum half-adder:** CNOT computes XOR, Toffoli computes AND. Quantum circuits can implement any classical function reversibly by writing results into target qubits.

- **GHZ states:** $\frac{1}{\sqrt{2}}(|000\rangle + |111\rangle)$ extends Bell-state entanglement to three (or more) qubits. The entanglement is fragile — measuring any single qubit destroys it entirely.

- **CHSH game:** A cooperative game where entanglement gives a measurable advantage. Classical strategies win at most 75%; quantum strategies using $|\Phi^+\rangle$ and optimal rotations win $\cos^2(\pi/8) \approx 85.4\%$ — the Tsirelson bound.

- **The pattern of quantum advantage:** define a task → prove a classical bound → beat it with entanglement. This is the framework for all quantum algorithms to come.

------------------------------------------------------------------------

## What's Next

Next lecture we use no-cloning and entanglement to build our first quantum protocol: **quantum key distribution (BB84)** — provably secure communication using the laws of physics.


# HOMEWORK

[Homework — Qiskit Circuits](../homework/hw_qiskit.ipynb)

**Due: Wednesday at midnight.**