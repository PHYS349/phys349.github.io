---
kernelspec:
  name: python3
  display_name: Python 3
---

# Lecture 4.5 — First Quantum Algorithms: Deutsch-Jozsa and Bernstein-Vazirani

## Where We Left Off

In Lectures 4.3 and 4.4, we used entanglement as a communication resource: BB84 for secure key distribution, teleportation for transmitting quantum states, and superdense coding for doubling classical capacity. These are powerful protocols, but they don't compute anything — they move information around.

Now we shift to computation. Back in Lecture 4.2, the CHSH game gave us a template for quantum advantage: define a task, prove a classical bound, and beat it with quantum mechanics. Today we apply that template to an algorithmic problem — and the speedup will be exponential.

The tool that makes it work is interference. Since Lecture 1.3, we've been saying that quantum computing is really about engineering constructive and destructive interference. Today that idea becomes a concrete algorithm.

------------------------------------------------------------------------

## Part 1: The Problem and the Oracle Model

### The Classical Problem: Constant or Balanced?

Imagine you're handed a black box — a function $f$ that takes an $n$-bit input and returns a single bit: 0 or 1. You can't look inside the box; you can only feed it inputs and read the output. Each time you query the box costs you one call.

You're told — promised — that $f$ is one of two types:

- **Constant:** $f$ returns the same value (all 0's or all 1's) for every possible input.
- **Balanced:** $f$ returns 0 for exactly half the inputs and 1 for the other half.

Your job: figure out which type it is.

Let's make this concrete. Suppose $n = 3$, so the input is a 3-bit string and there are $2^3 = 8$ possible inputs. Here are two example functions:

| Input | $f_{\text{const}}$ | $f_{\text{bal}}$ |
|:-----:|:-------------------:|:-----------------:|
| 000 | 1 | 0 |
| 001 | 1 | 1 |
| 010 | 1 | 0 |
| 011 | 1 | 1 |
| 100 | 1 | 0 |
| 101 | 1 | 1 |
| 110 | 1 | 0 |
| 111 | 1 | 1 |

The constant function returns 1 for every input. The balanced function returns 0 for four inputs and 1 for four inputs — exactly half and half.

How many queries do you need classically? Think about the worst case. Suppose your first 4 queries all return 0. Is the function constant? Maybe — or maybe it's balanced and you've just been unlucky, hitting all the 0's first. You don't know until you see a 1 or exhaust more than half the inputs. In the worst case, you need $2^{n-1} + 1$ queries — just over half — before you can be certain.

```{admonition} iClicker
:class: question
You have a black-box function on $n$-bit strings (so $2^n$ possible inputs). You're promised it's either constant or balanced. In the worst case, how many times must you call $f$ classically to be certain?

A) $n$ &emsp; B) $2^n$ &emsp; C) $2^{n-1}$ &emsp; D) $2^{n-1} + 1$
```

The answer is $2^{n-1} + 1$ — just over half the inputs. For $n = 10$, that's 513. For $n = 50$, that's over $10^{15}$. A quantum computer will do it in one call.

### Multi-Qubit Registers and Notation

Before we build the quantum version, a note on notation. In Chapters 2 and 3, $|0\rangle$ and $|1\rangle$ were states of a single qubit. Now our function takes $n$-bit inputs, so we need $n$ qubits to represent the input.

We'll use $|x\rangle$ as shorthand for an $n$-qubit computational basis state — the state of the entire input register. For example, with $n = 3$ qubits:

$$
|x\rangle = |x_1 x_2 x_3\rangle \quad \text{where each } x_i \in \{0, 1\}
$$

So $|x\rangle$ could be $|000\rangle$, $|001\rangle$, $|010\rangle$, ..., $|111\rangle$ — eight states total, representing the integers 0 through 7 in binary. When you see $|x\rangle$ in this lecture, think "$n$ qubits encoding a binary number," not a single qubit.

### The Oracle as a Black Box

Classically, the oracle takes an $n$-bit string $x$ and returns one bit $f(x)$. Quantumly, we need the oracle to be unitary — reversible. The standard construction uses an extra "target" qubit (just like the quantum half-adder in Lecture 4.2):

$$
U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle
$$

Here $\oplus$ denotes addition mod 2 (XOR): $0 \oplus 0 = 0$, $0 \oplus 1 = 1$, $1 \oplus 0 = 1$, $1 \oplus 1 = 0$. The register $|x\rangle$ is the $n$-qubit input and $|y\rangle$ is a single target qubit. The input passes through unchanged; the function value is XOR'd into the target. This is reversible because applying $U_f$ twice undoes itself: $(y \oplus f(x)) \oplus f(x) = y$.

When the target starts in $|0\rangle$, the oracle simply writes $f(x)$ into it:

$$
U_f|x\rangle|0\rangle = |x\rangle|f(x)\rangle
$$

For a single input qubit ($n = 1$) and $f(x) = x$, this is literally a CNOT — a gate you already know. For multi-qubit inputs, we build $U_f$ from CNOTs and Toffoli gates. In oracle problems, we treat the black box as given and count oracle calls, not the internal cost of building the oracle — just like complexity theory counts the number of times you query a database, not the cost of building the database.

### Quantum Parallelism — and Its Limits

Now here's the quantum advantage: if we prepare the input register in a superposition of all possible inputs, one oracle call acts coherently on the entire superposition.

Start with the simplest case: $n = 1$ (one input qubit). Preparing the input in $|+\rangle$ gives:

$$
U_f|+\rangle|0\rangle = U_f\left(\frac{|0\rangle + |1\rangle}{\sqrt{2}}\right)|0\rangle = \frac{|0\rangle|f(0)\rangle + |1\rangle|f(1)\rangle}{\sqrt{2}}
$$

Both $f(0)$ and $f(1)$ are now encoded in the state. The oracle is a unitary operator, so it acts on the entire superposition at once.

For $n$ qubits, applying Hadamard to every input qubit creates an equal superposition over all $2^n$ computational basis states:

$$
H^{\otimes n}|00\cdots 0\rangle = \frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1} |x\rangle
$$

where $|x\rangle$ is the $n$-qubit state encoding the binary number $x$ (for example, with $n = 3$: $|000\rangle + |001\rangle + |010\rangle + \cdots + |111\rangle$, all with equal amplitude). One oracle call on this superposition gives:

$$
\frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1} |x\rangle|f(x)\rangle
$$

All $2^n$ function values are encoded simultaneously. This is **quantum parallelism**.

But there's a catch. If we measure, the superposition collapses and we get a single random pair $(x, f(x))$ — just one function value, no better than one classical query. The information was all there, but measurement destroys it.

So quantum parallelism alone doesn't help. The algorithm is not trying to learn all values of $f(x)$ — it is designed to learn only one global property. The trick is to *not* read out individual function values. Instead, we use interference to extract a global property of $f$ — like whether it's constant or balanced — something that depends on all the values collectively. To do that, we need one more tool: phase kickback.

------------------------------------------------------------------------

## Part 2: Phase Kickback — The Key Trick

### The Derivation

What if we initialize the target qubit to $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$ instead of $|0\rangle$? Apply the oracle to $|x\rangle|-\rangle$:

$$
U_f|x\rangle|-\rangle = |x\rangle \otimes \frac{|0 \oplus f(x)\rangle - |1 \oplus f(x)\rangle}{\sqrt{2}}
$$

Consider the two cases for $f(x)$:

**If** $f(x) = 0$: the target becomes $\frac{|0\rangle - |1\rangle}{\sqrt{2}} = |-\rangle$. No change.

**If** $f(x) = 1$: the target becomes $\frac{|1\rangle - |0\rangle}{\sqrt{2}} = -|-\rangle$. A minus sign appears.

In both cases, the target qubit returns to $|-\rangle$ (up to sign). The sign $(-1)^{f(x)}$ has been kicked back onto the input register:

$$
\boxed{U_f|x\rangle|-\rangle = (-1)^{f(x)}|x\rangle|-\rangle}
$$

So the oracle does not leave the answer in the target qubit — it leaves a sign on the branch labeled by $x$.

```{admonition} Phase Kickback
:class: important
The function value $f(x)$ appears as a phase on the input register. The target qubit is completely unchanged. This is phase kickback — the most important trick in quantum algorithms.
```

### Why This Matters

At first glance, a global phase $(-1)^{f(x)}$ seems useless — we can't measure phase directly. But if the input register is in a superposition, different terms pick up different phases, and those become relative phases:

$$
U_f\left(\sum_x c_x |x\rangle\right)|-\rangle = \left(\sum_x (-1)^{f(x)} c_x |x\rangle\right)|-\rangle
$$

The oracle has imprinted the function as a pattern of signs across the superposition. Now interference — in the form of a final Hadamard — can convert those phase differences into measurable amplitude differences.

### The Interferometer Analogy

Think back to the Mach-Zehnder interferometer from Lecture 2.2. A beam splitter creates two paths, a phase shifter imprints a relative phase, and a second beam splitter recombines the paths so the phase difference shows up in the detector probabilities.

Phase kickback is the same idea scaled up: the first Hadamard wall creates $2^n$ "paths" (superposition over all inputs), the oracle imprints $(-1)^{f(x)}$ on each path, and the final Hadamard wall recombines them. Constructive or destructive interference determines what we measure.

The oracle is a phase plate inside a $2^n$-path interferometer.

### Verification in Qiskit

Let's see phase kickback in action. We'll use $f(x) = x$ (the identity function), so $U_f$ is just a CNOT. The oracle should flip $|+\rangle$ to $|-\rangle$:

```{code-cell} python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
import numpy as np

# Phase kickback demo: f(x) = x, so U_f = CNOT

# --- Before the oracle ---
qc_before = QuantumCircuit(2)
qc_before.h(0)       # qubit 0 → |+⟩
qc_before.x(1)       # qubit 1 → |1⟩
qc_before.h(1)       # qubit 1 → |−⟩

sv_before = Statevector.from_instruction(qc_before)
print("Before oracle (|+⟩|−⟩):")
print(sv_before.draw("text"))

# --- After the oracle (CNOT) ---
qc_after = qc_before.copy()
qc_after.cx(0, 1)

sv_after = Statevector.from_instruction(qc_after)
print("\nAfter oracle (CNOT):")
print(sv_after.draw("text"))

# --- Verify: qubit 0 flipped from |+⟩ to |−⟩ ---
# Measure qubit 0 in the X-basis by applying H first
qc_xbasis = qc_after.copy()
qc_xbasis.h(0)
sv_x = Statevector.from_instruction(qc_xbasis)
probs = sv_x.probabilities([0])  # probabilities for qubit 0 only
print(f"\nQubit 0 measured in X-basis: P(|+⟩) = {probs[0]:.3f}, P(|−⟩) = {probs[1]:.3f}")
print("Qubit 0 flipped from |+⟩ to |−⟩ — the phase was kicked back!")
print("The target qubit (qubit 1) is unchanged.")
```

```{admonition} iClicker
:class: question
The oracle has $f(0) = 1$ and $f(1) = 0$. We apply $U_f$ to $|+\rangle|-\rangle$. What is the state of the input qubit afterward?

A) $|+\rangle$ &emsp; B) $|-\rangle$ &emsp; C) $-|+\rangle$ &emsp; D) $-|-\rangle$
```

------------------------------------------------------------------------

## Part 3: Deutsch's Algorithm — The Simplest Case ($n = 1$)

We now have the two ingredients we need: the oracle model (Part 1) and phase kickback (Part 2). Let's put them together to solve the constant-vs.-balanced problem.

We'll start with $n = 1$ — a function on a single bit. This is **Deutsch's algorithm** (1985), historically the first quantum algorithm to beat a classical one. It's the warm-up. In Part 4 we'll generalize to $n$ qubits — that's the full **Deutsch-Jozsa algorithm** (1992). The idea is identical; only the number of qubits changes.

### The $n = 1$ Problem

With one input bit, $f: \{0, 1\} \to \{0, 1\}$, there are only four possible functions:

| | $f(0)$ | $f(1)$ | Type |
|---|:---:|:---:|:---:|
| $f_1$ | 0 | 0 | Constant |
| $f_2$ | 1 | 1 | Constant |
| $f_3$ | 0 | 1 | Balanced |
| $f_4$ | 1 | 0 | Balanced |

Is $f$ constant or balanced? Classically, you must query $f$ twice — once for $f(0)$ and once for $f(1)$ — then compare. One query isn't enough, because seeing $f(0) = 0$ is consistent with both $f_1$ (constant) and $f_3$ (balanced).

### The Circuit

Deutsch's algorithm answers this with a single query, using phase kickback to convert function values into measurable phases:

The circuit uses two qubits:

- $q_0$ (top wire): the input qubit, initialized to $|0\rangle$
- $q_1$ (bottom wire): the target qubit, initialized to $|1\rangle$ (via the X gate)

The $U_f$ box is the oracle — the unknown function we're trying to classify. We don't know what's inside it. All we know is the promise: $f$ is either constant or balanced.

```{code-cell} python
:tags: [hide-input]
from qiskit import QuantumCircuit
import matplotlib.pyplot as plt

# Build the oracle as a labeled 2-qubit gate
oracle_placeholder = QuantumCircuit(2, name="  ?  ")
oracle_placeholder.id(0)
oracle_placeholder.id(1)
oracle_gate = oracle_placeholder.to_gate()
oracle_gate.label = "$U_f$"

qc_deutsch = QuantumCircuit(2, 1)

# Label the qubits
qc_deutsch.x(1)
qc_deutsch.barrier()
qc_deutsch.h(0)
qc_deutsch.h(1)
qc_deutsch.barrier()
qc_deutsch.append(oracle_gate, [0, 1])
qc_deutsch.barrier()
qc_deutsch.h(0)
qc_deutsch.measure(0, 0)

fig, ax = plt.subplots(figsize=(10, 2.5))
qc_deutsch.draw("mpl", ax=ax)
ax.set_title("Deutsch's Algorithm:  q₀ = input (starts |0⟩),  q₁ = target (starts |1⟩)", fontsize=11)
plt.tight_layout()
plt.show()
```

Reading the circuit left to right: the X gate prepares $q_1$ in $|1\rangle$. The two Hadamards create $|+\rangle$ on $q_0$ and $|-\rangle$ on $q_1$. Then the oracle $U_f$ acts on both qubits — we don't know what it does internally, only that it implements one of the four functions in the table above. A final Hadamard on $q_0$ converts phase information into a measurable bit.

### Step-by-Step State Evolution

**Step 0 — Initialization.** $q_0$ starts in $|0\rangle$, $q_1$ starts in $|1\rangle$ (after the X gate):

$$
|\psi_0\rangle = |0\rangle_{q_0}|1\rangle_{q_1}
$$

**Step 1 — Hadamard on both qubits.** $q_0$ becomes $|+\rangle$, $q_1$ becomes $|-\rangle$:

$$
|\psi_1\rangle = |+\rangle_{q_0}|-\rangle_{q_1} = \frac{1}{2}(|0\rangle + |1\rangle)_{q_0}(|0\rangle - |1\rangle)_{q_1}
$$

$q_0$ is in a superposition of both possible inputs. $q_1$ is in $|-\rangle$, ready for phase kickback.

**Step 2 — The oracle $U_f$.** Remember, we don't know which function is inside the box — but phase kickback (Part 2) tells us what happens regardless:

$$
U_f|+\rangle_{q_0}|-\rangle_{q_1} = \frac{1}{\sqrt{2}}\Big((-1)^{f(0)}|0\rangle + (-1)^{f(1)}|1\rangle\Big)_{q_0} \otimes |-\rangle_{q_1}
$$

The relative phase between $|0\rangle$ and $|1\rangle$ on $q_0$ now depends on whether $f(0)$ and $f(1)$ agree:

- If $f$ is constant: $(-1)^{f(0)} = (-1)^{f(1)}$, so $q_0$ is in $|+\rangle$ (up to global phase).
- If $f$ is balanced: $(-1)^{f(0)} = -(-1)^{f(1)}$, so $q_0$ is in $|-\rangle$ (up to global phase).

Notice that $q_1$ is still $|-\rangle$ — the target qubit is unchanged, just as phase kickback promised.

**Step 3 — Hadamard on $q_0$.** This converts $|+\rangle \leftrightarrow |0\rangle$ and $|-\rangle \leftrightarrow |1\rangle$:

$$
|\psi_3\rangle = (-1)^{f(0)}|f(0) \oplus f(1)\rangle_{q_0} \otimes |-\rangle_{q_1}
$$

**Step 4 — Measure $q_0$.**

$$
\text{Measure } |0\rangle \Rightarrow f \text{ is constant}, \qquad \text{Measure } |1\rangle \Rightarrow f \text{ is balanced}
$$

One query to the unknown oracle. Deterministic answer. The global phase $(-1)^{f(0)}$ is unobservable.

```{admonition} Check Yourself
:class: tip
Trace through the state evolution for $f_3$ (the function $f(0) = 0, f(1) = 1$) explicitly. After the oracle, the input qubit should be $|-\rangle$, and after the final Hadamard, you should measure $|1\rangle$ with certainty.
```

```{admonition} iClicker
:class: question
Suppose you accidentally initialize $q_1$ to $|0\rangle$ instead of $|1\rangle$ (i.e., you forget the X gate). What happens when you run Deutsch's algorithm?

A) It still works &emsp; B) You always measure 0 &emsp; C) You get a random result &emsp; D) You always measure 1
```

```{admonition} iClicker
:class: question
You run Deutsch's algorithm and measure $q_0 = 1$. Your friend says "the function must be $f_3$, where $f(0) = 0$ and $f(1) = 1$." Is your friend right?

A) Yes &emsp; B) No — it could be $f_3$ or $f_4$ &emsp; C) No — the algorithm failed &emsp; D) No — you need to also measure $q_1$
```

### Building and Running All Four Oracles

Let's implement the four possible oracles and verify that Deutsch's algorithm classifies each correctly:

```{code-cell} python
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator

def deutsch_oracle(f0, f1):
    """Build the oracle for f(0)=f0, f(1)=f1.
    Acts on 2 qubits: qubit 0 = input, qubit 1 = target."""
    oracle = QuantumCircuit(2, name=f"f({f0},{f1})")
    if f0 == 1:
        # f(0)=1: flip target unconditionally for |0⟩ input
        # We need to flip when input is 0, so: X on input, CNOT, X on input
        oracle.x(0)
        oracle.cx(0, 1)
        oracle.x(0)
    if f1 == 1:
        # f(1)=1: flip target when input is |1⟩ → CNOT
        oracle.cx(0, 1)
    return oracle

def run_deutsch(f0, f1):
    """Run Deutsch's algorithm for the given function."""
    qc = QuantumCircuit(2, 1)

    # Prepare |0⟩|1⟩ → H⊗H → |+⟩|−⟩
    qc.x(1)
    qc.h(0)
    qc.h(1)
    qc.barrier()

    # Oracle
    qc.compose(deutsch_oracle(f0, f1), inplace=True)
    qc.barrier()

    # Final Hadamard + measure input qubit
    qc.h(0)
    qc.measure(0, 0)

    sim = AerSimulator()
    result = sim.run(qc, shots=1000).result()
    return result.get_counts()

# Test all four functions
print("Deutsch's Algorithm Results")
print("=" * 50)
for f0, f1 in [(0,0), (1,1), (0,1), (1,0)]:
    ftype = "constant" if f0 == f1 else "balanced"
    counts = run_deutsch(f0, f1)
    measured = list(counts.keys())[0]
    result = "constant" if measured == "0" else "balanced"
    print(f"  f(0)={f0}, f(1)={f1}  ({ftype:8s})  →  measured {measured}  →  {result}")
```

Every function is correctly classified with a single query.

------------------------------------------------------------------------

## Part 4: The Deutsch-Jozsa Algorithm ($n$ Qubits)

Deutsch's algorithm solved the $n = 1$ case: one input qubit, four possible functions, one query. But we started this lecture with a much bigger problem — $n$-bit inputs, $2^n$ possible inputs, and a classical cost of $2^{n-1} + 1$ queries. The full Deutsch-Jozsa algorithm (1992) handles this general case. The circuit is the same idea as Deutsch's — Hadamards, oracle, Hadamards, measure — just with $n$ input qubits instead of one. If you understood Part 3, the generalization is straightforward.

### The Generalized Problem

Consider $f: \{0, 1\}^n \to \{0, 1\}$ — a Boolean function on $n$-bit strings. We are promised that $f$ is one of two types:

- **Constant:** $f(x)$ is the same for all $2^n$ inputs (all 0's or all 1's).
- **Balanced:** $f(x) = 0$ for exactly half the inputs and $f(x) = 1$ for the other half.

**Question:** Which type is $f$?

```{admonition} The Promise Matters
:class: warning
The problem assumes $f$ is **promised** to be either constant or balanced — nothing in between. If $f$ outputs 0 for 60% of inputs and 1 for 40%, neither label applies and the algorithm's output is meaningless. This "promise" structure is common in quantum algorithms: the speedup is only valid when the input satisfies the stated guarantee.
```

### The Classical Cost

Classically, you need to query $f$ on enough inputs to be sure. In the worst case, the first $2^{n-1}$ queries could all return the same value — that's consistent with both constant and balanced. You need one more query to decide:

$$
\text{Classical worst-case queries} = 2^{n-1} + 1
$$

For $n = 10$, that's 513 queries. For $n = 50$, that's over $10^{15}$.

### The Quantum Circuit: One Query

The circuit uses $n + 1$ qubits:

- $q_0$ through $q_{n-1}$ (top $n$ wires): the input register, all initialized to $|0\rangle$. These encode the $n$-bit input $x$.
- $q_n$ (bottom wire): the target qubit, initialized to $|1\rangle$ (via the X gate). This is the phase-kickback ancilla — it will stay in $|-\rangle$ throughout, just like $q_1$ in Deutsch's algorithm.

The $U_f$ box is the oracle — the unknown function we're trying to classify. We don't know what's inside it; we only know the promise that $f$ is either constant or balanced.

```{code-cell} python
:tags: [hide-input]
import matplotlib.pyplot as plt
from qiskit import QuantumCircuit

# Show the general Deutsch-Jozsa circuit structure for n = 4
n_show = 4

# Build the oracle as a labeled gate
oracle_placeholder = QuantumCircuit(n_show + 1, name="  ?  ")
for i in range(n_show + 1):
    oracle_placeholder.id(i)
oracle_gate = oracle_placeholder.to_gate()
oracle_gate.label = "$U_f$"

qc_show = QuantumCircuit(n_show + 1, n_show)
qc_show.x(n_show)
qc_show.barrier()
qc_show.h(range(n_show + 1))
qc_show.barrier()
qc_show.append(oracle_gate, range(n_show + 1))
qc_show.barrier()
qc_show.h(range(n_show))
qc_show.measure(range(n_show), range(n_show))

fig, ax = plt.subplots(figsize=(12, 4))
qc_show.draw("mpl", ax=ax)
ax.set_title(f"Deutsch-Jozsa Algorithm (n = {n_show}):  q₀–q₃ = input register (start |0⟩),  q₄ = target (starts |1⟩)", fontsize=11)
plt.tight_layout()
plt.show()
```

Reading the circuit left to right: the X gate prepares $q_n$ in $|1\rangle$. The first wall of Hadamards puts the input register into an equal superposition of all $2^n$ computational basis states and puts the target into $|-\rangle$. The oracle $U_f$ acts on all $n + 1$ qubits — via phase kickback, it imprints $(-1)^{f(x)}$ on each branch of the input superposition without changing the target. A second wall of Hadamards on the input register only (not the target) converts the phase pattern into measurable amplitudes. Finally, we measure the $n$ input qubits.

### The Derivation

We now walk through the math step by step, following the circuit above from left to right.

**Step 1 — Prepare the input superposition.**

Apply $H^{\otimes n}$ to the $n$ input qubits (starting from $|0\rangle^{\otimes n}$) and $H$ to the target qubit (starting from $|1\rangle$):

$$
H^{\otimes n}|0\rangle^{\otimes n} = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n - 1} |x\rangle, \qquad H|1\rangle = |-\rangle
$$

The full state is:

$$
|\pi_1\rangle = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n - 1} |x\rangle \otimes |-\rangle
$$

Every possible $n$-bit input is present with equal amplitude. The target is ready for phase kickback.

**Step 2 — Apply the oracle.**

Phase kickback gives us:

$$
|\pi_2\rangle = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n - 1} (-1)^{f(x)} |x\rangle \otimes |-\rangle
$$

The oracle has imprinted $f$ as a pattern of signs across the superposition. If $f$ is constant, all signs are the same ($+$ or $-$). If $f$ is balanced, half are $+$ and half are $-$.

**Step 3 — Apply $H^{\otimes n}$ to the input register.**

This is where interference happens. The second Hadamard wall recombines all $2^n$ branches, just like the second beam splitter in an interferometer. We don't need the full output state — we only need to know the amplitude of one particular outcome: $|0\rangle^{\otimes n}$.

The key identity is that $H^{\otimes n}$ maps $|0\rangle^{\otimes n}$ to the equal superposition and vice versa. So the amplitude of $|0\rangle^{\otimes n}$ in the output is the overlap of the input with the equal superposition — which is just the average of all the phases:

$$
\langle 0^n | H^{\otimes n} \left(\frac{1}{\sqrt{2^n}} \sum_x (-1)^{f(x)} |x\rangle\right) = \frac{1}{2^n} \sum_{x=0}^{2^n-1} (-1)^{f(x)}
$$

So the amplitude of measuring all zeros is:

$$
\boxed{a_{0\cdots 0} = \frac{1}{2^n} \sum_{x=0}^{2^n - 1} (-1)^{f(x)}}
$$

```{dropdown} Full derivation: how $H^{\otimes n}$ acts on a general state
For the Bernstein-Vazirani algorithm (Part 5), we'll need the full action of $H^{\otimes n}$ on a computational basis state. Start with the single-qubit case:

$$
H|x\rangle = \frac{1}{\sqrt{2}}\sum_{z \in \{0,1\}} (-1)^{xz} |z\rangle
$$

Check: $H|0\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ and $H|1\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$. Correct — the $(-1)^{xz}$ picks up a minus sign only when both $x = 1$ and $z = 1$.

For $n$ qubits, $H^{\otimes n}$ acts on each qubit independently, so the phases multiply:

$$
H^{\otimes n}|x\rangle = \frac{1}{\sqrt{2^n}} \sum_{z=0}^{2^n - 1} (-1)^{x \cdot z} |z\rangle
$$

where $x \cdot z = x_1 z_1 \oplus x_2 z_2 \oplus \cdots \oplus x_n z_n$ is the bitwise inner product mod 2 (recall $\oplus$ is XOR — addition mod 2). Setting $z = 0\cdots 0$ gives $x \cdot z = 0$ for all $x$, which is how we obtained the amplitude formula above.
```

Now the two cases:

**If $f$ is constant:** All $(-1)^{f(x)}$ terms are the same — either all $+1$ or all $-1$. The sum is $\pm 2^n$. So $a_{0\cdots 0} = \pm 1$ and we measure $|0\cdots 0\rangle$ with **certainty**.

**If $f$ is balanced:** Half the terms are $+1$ and half are $-1$. They cancel exactly: the sum is 0. So $a_{0\cdots 0} = 0$ and we **never** measure $|0\cdots 0\rangle$.

```{admonition} Deutsch-Jozsa Decision Rule
:class: important
Measure all $n$ input qubits. If the result is $|0\rangle^{\otimes n}$, the function is **constant**. If the result is **anything else**, the function is **balanced**. One query. Deterministic. Exponential speedup.
```

```{admonition} iClicker
:class: question
You run Deutsch-Jozsa and measure $|0\rangle^{\otimes n}$, concluding the function is constant. Do you know whether $f(x) = 0$ for all $x$ or $f(x) = 1$ for all $x$?

A) Yes — $f(x) = 0$ for all $x$ &emsp; B) Yes — $f(x) = 1$ for all $x$ &emsp; C) No — both constant functions produce the same measurement &emsp; D) You need one more query
```

```{admonition} iClicker
:class: question
Consider a balanced oracle where $f(x) = x_1$ (the value of the first bit of $x$). After running the $n$-qubit Deutsch-Jozsa algorithm, what do you measure?

A) $|0\cdots 0\rangle$ &emsp; B) $|10\cdots 0\rangle$ &emsp; C) A random nonzero string &emsp; D) Cannot determine without knowing $n$
```

### The Interference Picture

Let's be explicit about what just happened, because this is the heart of quantum algorithms.

The first Hadamard wall splits the computation into $2^n$ "paths" — one for each possible input $x$. The oracle imprints a phase $(-1)^{f(x)}$ on each path. The second Hadamard wall recombines all $2^n$ paths.

For a constant function, all paths carry the same phase. They constructively interfere at $|0\cdots 0\rangle$, and the probability piles up there.

For a balanced function, half the paths carry $+1$ and half carry $-1$. At $|0\cdots 0\rangle$, they destructively interfere — perfect cancellation, zero probability.

This is the Mach-Zehnder interferometer from Lecture 2.2, scaled to $2^n$ paths. The quantum speedup comes entirely from interference.

```{admonition} What Deutsch-Jozsa Does NOT Do
:class: warning
The algorithm does *not* tell you the value of $f(x)$ for any specific input — it only reveals a global property (constant vs. balanced). Quantum parallelism puts all $2^n$ function values into the state, but interference extracts only collective information. This is a recurring theme: quantum speedups come from asking the right *global* question, not from reading out exponentially many individual answers.
```

### Qiskit Implementation

```{code-cell} python
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator
import numpy as np

def dj_constant_oracle(n, output_value=0):
    """Constant oracle: f(x) = output_value for all x."""
    oracle = QuantumCircuit(n + 1, name=f"constant({output_value})")
    if output_value == 1:
        # X on |−⟩ gives −|−⟩, which is just a global phase.
        # So f(x)=1 (constant) acts the same as f(x)=0 up to
        # an unobservable sign — both yield |0...0⟩ at the output.
        oracle.x(n)
    return oracle

def dj_balanced_oracle(n, seed=None):
    """Balanced oracle: apply CNOT from random subset of input qubits to target,
    with optional NOT on target to ensure balance."""
    rng = np.random.default_rng(seed)
    oracle = QuantumCircuit(n + 1, name="balanced")

    # Pick a random non-empty subset of input qubits to CNOT onto target
    # This creates f(x) = x_{i1} ⊕ x_{i2} ⊕ ... which is balanced
    subset = []
    while len(subset) == 0:
        subset = [i for i in range(n) if rng.random() > 0.5]
    for i in subset:
        oracle.cx(i, n)

    # Optionally flip the function (still balanced)
    if rng.random() > 0.5:
        oracle.x(n)

    return oracle

def run_deutsch_jozsa(n, oracle):
    """Run the Deutsch-Jozsa algorithm on n input qubits with the given oracle."""
    qc = QuantumCircuit(n + 1, n)

    # Prepare |0...0⟩|1⟩
    qc.x(n)

    # Hadamard all qubits
    qc.h(range(n + 1))
    qc.barrier()

    # Apply oracle
    qc.compose(oracle, inplace=True)
    qc.barrier()

    # Hadamard on input qubits only
    qc.h(range(n))

    # Measure input qubits
    qc.measure(range(n), range(n))

    sim = AerSimulator()
    result = sim.run(qc, shots=1024).result()
    return result.get_counts()
```

```{code-cell} python
# Test with n = 4 qubits
n = 4

print("Deutsch-Jozsa Algorithm (n = 4)")
print("=" * 55)

# Constant oracle: f(x) = 0
counts = run_deutsch_jozsa(n, dj_constant_oracle(n, 0))
print(f"\n  Constant f(x) = 0:  {counts}")
print(f"  → All zeros? {'Yes — CONSTANT' if '0'*n in counts else 'No — BALANCED'}")

# Constant oracle: f(x) = 1
counts = run_deutsch_jozsa(n, dj_constant_oracle(n, 1))
print(f"\n  Constant f(x) = 1:  {counts}")
print(f"  → All zeros? {'Yes — CONSTANT' if '0'*n in counts else 'No — BALANCED'}")

# Balanced oracle (random)
oracle_bal = dj_balanced_oracle(n, seed=42)
counts = run_deutsch_jozsa(n, oracle_bal)
print(f"\n  Balanced (random):  {counts}")
print(f"  → All zeros? {'Yes — CONSTANT' if '0'*n in counts else 'No — BALANCED'}")

# Another balanced oracle
oracle_bal2 = dj_balanced_oracle(n, seed=7)
counts = run_deutsch_jozsa(n, oracle_bal2)
print(f"\n  Balanced (random):  {counts}")
print(f"  → All zeros? {'Yes — CONSTANT' if '0'*n in counts else 'No — BALANCED'}")
```

For constant oracles, we always measure $|0000\rangle$. For balanced oracles, we never do. One query, every time.

```{code-cell} python
:tags: [hide-input]
# Visualize: show the histograms side by side
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

fig, axes = plt.subplots(1, 2, figsize=(12, 3.5))

counts_const = run_deutsch_jozsa(4, dj_constant_oracle(4, 0))
counts_bal = run_deutsch_jozsa(4, dj_balanced_oracle(4, seed=42))

plot_histogram(counts_const, ax=axes[0], title="Constant oracle")
plot_histogram(counts_bal, ax=axes[1], title="Balanced oracle")

plt.tight_layout()
plt.show()
```

The contrast is stark: the constant oracle produces a single spike at $|0000\rangle$, while the balanced oracle produces anything *but* $|0000\rangle$.

------------------------------------------------------------------------

## Part 5: Bernstein-Vazirani — Finding a Hidden String

### A More Specific Problem

The Bernstein-Vazirani problem (1993) uses the same circuit as Deutsch-Jozsa but solves a different — and in some ways more satisfying — problem.

Setup: there exists a secret $n$-bit string $s = s_1 s_2 \cdots s_n$ that you don't know. You're given an oracle that computes:

$$
f(x) = s \cdot x = s_1 x_1 \oplus s_2 x_2 \oplus \cdots \oplus s_n x_n \pmod{2}
$$

Here $\oplus$ is addition mod 2 (XOR) — the same operation we saw in the oracle definition $U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$. So $f(x)$ is the bitwise inner product of $s$ with $x$, mod 2. Your goal: find $s$.

### The Classical Approach

Classically, you can learn one bit of $s$ per query by feeding in unit vectors:

- Query $f(100\cdots 0) = s_1$
- Query $f(010\cdots 0) = s_2$
- $\vdots$
- Query $f(000\cdots 1) = s_n$

That takes $n$ queries. You can't do better — each query reveals at most one bit of information.

### The Quantum Solution: One Query

Run the exact same circuit as Deutsch-Jozsa: $H^{\otimes n}$, oracle, $H^{\otimes n}$, measure. The output is $|s\rangle$ directly.

Why? The oracle for $f(x) = s \cdot x$ imprints the phase $(-1)^{s \cdot x}$. After phase kickback, the input register is:

$$
\frac{1}{\sqrt{2^n}} \sum_x (-1)^{s \cdot x} |x\rangle
$$

But recall the $H^{\otimes n}$ formula from Part 4 (see the dropdown): $H^{\otimes n}|s\rangle = \frac{1}{\sqrt{2^n}} \sum_x (-1)^{s \cdot x} |x\rangle$. So the state above is just $H^{\otimes n}|s\rangle$! Since $H^{\otimes n}$ is its own inverse ($H^{\otimes n} H^{\otimes n} = I$), applying the final Hadamard wall gives:

$$
H^{\otimes n} \cdot H^{\otimes n}|s\rangle = |s\rangle
$$

The secret string pops out directly.

One query reveals all $n$ bits of $s$. Classical needs $n$ queries. The speedup is linear rather than exponential, but many students find Bernstein-Vazirani more satisfying than Deutsch-Jozsa: instead of learning a yes/no property, you recover the actual hidden answer. Deutsch-Jozsa shows the idea; Bernstein-Vazirani gives a more concrete payoff.

### The Oracle Is Just CNOTs

The oracle for $f(x) = s \cdot x$ has a beautifully simple structure: for each bit $s_i = 1$, place a CNOT from input qubit $i$ to the target. That's it.

```{code-cell} python
def bv_oracle(secret_string):
    """Build the Bernstein-Vazirani oracle for a given secret string."""
    n = len(secret_string)
    oracle = QuantumCircuit(n + 1, name=f"BV({secret_string})")
    for i, bit in enumerate(reversed(secret_string)):
        if bit == '1':
            oracle.cx(i, n)
    return oracle

def run_bernstein_vazirani(secret_string):
    """Run BV algorithm and return the measured secret string."""
    n = len(secret_string)
    qc = QuantumCircuit(n + 1, n)

    # Prepare
    qc.x(n)
    qc.h(range(n + 1))
    qc.barrier()

    # Oracle
    qc.compose(bv_oracle(secret_string), inplace=True)
    qc.barrier()

    # Final Hadamards + measure
    qc.h(range(n))
    qc.measure(range(n), range(n))

    sim = AerSimulator()
    result = sim.run(qc, shots=1).result()
    measured = list(result.get_counts().keys())[0]
    return measured

# Test several secret strings
print("Bernstein-Vazirani Algorithm")
print("=" * 40)
for secret in ["101", "1101", "0110", "11111", "10001"]:
    found = run_bernstein_vazirani(secret)
    match = "✓" if found == secret else "✗"
    print(f"  Secret: {secret}  →  Found: {found}  {match}")
```

Every secret string is recovered in a single query.

```{admonition} iClicker
:class: question
Deutsch-Jozsa tells you a global property (constant vs. balanced). Bernstein-Vazirani tells you the specific secret string $s$. Both use the exact same circuit. Why does BV extract more information from a single query?

A) BV uses more qubits &emsp; B) BV's oracle is more structured — $f(x) = s \cdot x$ is linear, not arbitrary &emsp; C) BV uses a different measurement basis &emsp; D) BV measures the target qubit too
```

```{admonition} iClicker
:class: question
Suppose you modify the BV oracle by adding a constant offset: $f(x) = s \cdot x \oplus 1$. You run the BV algorithm. What do you measure?

A) $|s\rangle$ &emsp; B) The bitwise complement $|\bar{s}\rangle$ &emsp; C) A random string &emsp; D) $|0\cdots 0\rangle$
```

```{admonition} In-Class Activity
:class: tip
Pair up with a neighbor. Each of you pick a secret binary string of length 4–6 and build the oracle circuit (just CNOTs from each qubit where $s_i = 1$ to the target). Exchange oracle circuits — but not your secret strings. Run the Bernstein-Vazirani algorithm to find your partner's secret string in one shot.
```

------------------------------------------------------------------------

## Summary

- **Oracle model:** Quantum algorithms access functions through reversible black boxes: $U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$. We count queries, not gates.

- **Quantum parallelism** lets us evaluate $f$ on all inputs simultaneously — but measurement collapses the result to one value. The trick is to extract *global* properties through interference.

- **Phase kickback:** When the target is $|-\rangle$, the oracle imprints $f$ as a phase: $U_f|x\rangle|-\rangle = (-1)^{f(x)}|x\rangle|-\rangle$. This is the most important trick in quantum algorithms.

- **Deutsch's algorithm** ($n = 1$): determines constant vs. balanced with 1 query (classical needs 2).

- **Deutsch-Jozsa** ($n$ qubits): determines constant vs. balanced with 1 query (classical needs $2^{n-1} + 1$). The amplitude of $|0^n\rangle$ is $\frac{1}{2^n}\sum_x (-1)^{f(x)}$ — full constructive interference for constant, perfect cancellation for balanced.

- **Bernstein-Vazirani:** finds a secret string $s$ with 1 query (classical needs $n$). Same circuit, different oracle.

- **The algorithm skeleton:** $H^{\otimes n}$ (superpose all inputs) → oracle (imprint phases) → $H^{\otimes n}$ (interfere) → measure. This pattern recurs in Grover's algorithm and beyond.

------------------------------------------------------------------------

## What's Next

Next lecture: Grover's search algorithm. Deutsch-Jozsa gives an exponential speedup but solves a somewhat artificial problem (who really needs to distinguish constant from balanced?). Grover's algorithm tackles something universal — finding a marked item in an unstructured list — and achieves a quadratic speedup: $O(\sqrt{N})$ queries instead of $O(N)$. The oracle framework and phase kickback carry over directly. The new ingredient is amplitude amplification: a geometric rotation in Hilbert space that pumps probability into the marked state.

{download}`Download Homework: Teleportation, Entanglement Swapping, and Quantum Algorithms <../homework/04_05_homework.ipynb>`

Due: Wednesday at midnight.

<!-- **Reference:** The [IBM Deutsch-Jozsa tutorial](../lecture_resources/deutsch-jozsa.ipynb) covers the same algorithms with additional context — a useful companion if you get stuck. -->
