---
kernelspec:
  name: python3
  display_name: Python 3
---

# Lecture 4.5 — The Deutsch-Jozsa Algorithm

## Where We Left Off

In Lectures 4.3 and 4.4, we used entanglement as a **communication** resource: BB84 for secure key distribution, teleportation for transmitting quantum states, and superdense coding for doubling classical capacity. These are powerful protocols, but they don't compute anything — they move information around.

Now we shift to **computation**. Back in Lecture 4.2, the CHSH game gave us a template for quantum advantage: define a task, prove a classical bound, and beat it with quantum mechanics. Today we apply that template to an algorithmic problem — and the speedup will be exponential.

The tool that makes it work is **interference**. Since Lecture 1.3, we've been saying that quantum computing is really about engineering constructive and destructive interference. Today that idea becomes a concrete algorithm.

------------------------------------------------------------------------

## Part 1: The Oracle Model

### Black-Box Functions

Many real-world computational tasks involve querying a function you can't inspect. Think of a database lookup, an API call, or a subroutine written by someone else — you can ask "what is $f(x)$?" but you can't open the box and look at the code.

The **query model** (also called the oracle model) formalizes this: you're given access to a function $f$ only through a black box. Each time you ask the box for $f(x)$, that costs one **query**. The goal is to learn some property of $f$ using as few queries as possible.

This isn't just a toy model. The query framework is the standard way to prove quantum speedups rigorously — we count queries, not total gate operations, which gives clean separation between quantum and classical.

### The Quantum Oracle

A classical oracle takes input $x$ and returns $f(x)$. But quantum gates must be **unitary** — reversible — and simply outputting $f(x)$ would overwrite the input. We saw this same issue with the quantum half-adder in Lecture 4.2: the solution is to write the answer into a separate target register.

The standard quantum oracle acts on two registers:

$$
U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle
$$

The input $|x\rangle$ passes through unchanged. The function value is XOR'd into the target qubit $|y\rangle$. This is reversible because applying $U_f$ twice gives back the original: $(y \oplus f(x)) \oplus f(x) = y$.

When the target starts in $|0\rangle$, the oracle simply writes $f(x)$ into it:

$$
U_f|x\rangle|0\rangle = |x\rangle|f(x)\rangle
$$

This is exactly the CNOT pattern from Lecture 4.1 — if $f(x) = x$, then $U_f$ is literally a CNOT gate. For more complex functions, we build $U_f$ from Toffoli gates and other reversible primitives.

```{admonition} iClicker
:class: question
The oracle $U_f$ acts on $|x\rangle|0\rangle$. What is the output?

A) $|f(x)\rangle|0\rangle$ &emsp; B) $|x\rangle|f(x)\rangle$ &emsp; C) $|x \oplus f(x)\rangle|0\rangle$ &emsp; D) $|x\rangle|x\rangle$
```

### Quantum Parallelism — and Its Limits

Here's a tempting idea. If we prepare the input register in an equal superposition, the oracle evaluates $f$ on **all inputs at once**:

$$
U_f \left(\frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1} |x\rangle\right)|0\rangle = \frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1} |x\rangle|f(x)\rangle
$$

All $2^n$ function values are now encoded in the quantum state simultaneously. This is called **quantum parallelism**.

But there's a catch. If we measure, we get a single random pair $(x, f(x))$ — just one function value, no better than a classical query. The information is there, but measurement collapses it.

The trick is to **not** read out individual function values. Instead, we use interference to extract a *global property* of $f$ — something that depends on all the function values collectively. That's what Deutsch-Jozsa does.

```{admonition} iClicker
:class: question
You prepare a uniform superposition over all $2^n$ inputs, apply the oracle, then measure. How many values of $f$ do you learn?

A) $2^n$ &emsp; B) $\sqrt{2^n}$ &emsp; C) 1 &emsp; D) 0
```

------------------------------------------------------------------------

## Part 2: Phase Kickback — The Key Trick

### The Setup

What if we initialize the target qubit to $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$ instead of $|0\rangle$?

### The Derivation

Apply the oracle to $|x\rangle|-\rangle$:

$$
U_f|x\rangle|-\rangle = |x\rangle \otimes \frac{|0 \oplus f(x)\rangle - |1 \oplus f(x)\rangle}{\sqrt{2}}
$$

Consider the two cases for $f(x)$:

**If** $f(x) = 0$: the target becomes $\frac{|0\rangle - |1\rangle}{\sqrt{2}} = |-\rangle$. No change.

**If** $f(x) = 1$: the target becomes $\frac{|1\rangle - |0\rangle}{\sqrt{2}} = -|-\rangle$. A minus sign appears.

In both cases, the target qubit returns to $|-\rangle$ (up to sign). The sign $(-1)^{f(x)}$ has been **kicked back** onto the input register:

$$
\boxed{U_f|x\rangle|-\rangle = (-1)^{f(x)}|x\rangle|-\rangle}
$$

```{admonition} Phase Kickback
:class: important
The function value $f(x)$ appears as a **phase** on the input register. The target qubit is completely unchanged. This is phase kickback — the most important trick in quantum algorithms.
```

### Why This Matters

At first glance, a global phase $(-1)^{f(x)}$ seems useless — we can't measure phase directly. But if the input register is in a **superposition**, different terms pick up different phases, and those become **relative** phases:

$$
U_f\left(\sum_x c_x |x\rangle\right)|-\rangle = \left(\sum_x (-1)^{f(x)} c_x |x\rangle\right)|-\rangle
$$

The oracle has imprinted the function as a pattern of signs across the superposition. Now interference — in the form of a final Hadamard — can convert those phase differences into measurable amplitude differences.

### The Interferometer Analogy

Think back to the Mach-Zehnder interferometer from Lecture 2.2. A beam splitter creates two paths, a phase shifter imprints a relative phase, and a second beam splitter recombines the paths so the phase difference shows up in the detector probabilities.

Phase kickback is the same idea scaled up: the first Hadamard wall creates $2^n$ "paths" (superposition over all inputs), the oracle imprints $(-1)^{f(x)}$ on each path, and the final Hadamard wall recombines them. Constructive or destructive interference determines what we measure.

**The oracle is a phase plate inside a $2^n$-path interferometer.**

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
The oracle has $f(0) = 0$ and $f(1) = 1$. We apply $U_f$ to $|+\rangle|-\rangle$. What is the state of the input qubit afterward?

A) $|+\rangle$ &emsp; B) $|-\rangle$ &emsp; C) $|0\rangle$ &emsp; D) $|1\rangle$
```

------------------------------------------------------------------------

## Part 3: Deutsch's Algorithm ($n = 1$)

### The Problem

Consider a function $f: \{0, 1\} \to \{0, 1\}$. There are four possible functions:

| | $f(0)$ | $f(1)$ | Type |
|---|:---:|:---:|:---:|
| $f_1$ | 0 | 0 | Constant |
| $f_2$ | 1 | 1 | Constant |
| $f_3$ | 0 | 1 | Balanced |
| $f_4$ | 1 | 0 | Balanced |

**Question:** Is $f$ constant (same output for both inputs) or balanced (different outputs)?

**Classical cost:** You must query $f$ **twice** — once for $f(0)$ and once for $f(1)$ — then compare. One query isn't enough, because seeing $f(0) = 0$ is consistent with both $f_1$ (constant) and $f_3$ (balanced).

### The Circuit

Deutsch's algorithm answers this with a **single query**:

```{code-cell} python
:tags: [hide-input]
from qiskit import QuantumCircuit

# Build a placeholder oracle as a labeled gate
oracle_placeholder = QuantumCircuit(2, name="U_f")
oracle_placeholder.id(0)
oracle_placeholder.id(1)
oracle_gate = oracle_placeholder.to_gate()

qc_deutsch = QuantumCircuit(2, 1)
qc_deutsch.x(1)
qc_deutsch.barrier()
qc_deutsch.h(0)
qc_deutsch.h(1)
qc_deutsch.barrier()
qc_deutsch.append(oracle_gate, [0, 1])
qc_deutsch.barrier()
qc_deutsch.h(0)
qc_deutsch.measure(0, 0)
qc_deutsch.draw("mpl")
```

### Step-by-Step State Evolution

**Step 0 — Initialization:** Start with $|0\rangle|1\rangle$.

$$
|\pi_0\rangle = |01\rangle
$$

**Step 1 — Hadamard both qubits:**

$$
|\pi_1\rangle = (H \otimes H)|01\rangle = |+\rangle|-\rangle = \frac{1}{2}(|0\rangle + |1\rangle)(|0\rangle - |1\rangle)
$$

The input qubit is in an equal superposition. The target qubit is in $|-\rangle$, ready for phase kickback.

**Step 2 — Apply the oracle $U_f$:**

Using phase kickback, the oracle acts as:

$$
U_f|+\rangle|-\rangle = \frac{1}{\sqrt{2}}\Big((-1)^{f(0)}|0\rangle + (-1)^{f(1)}|1\rangle\Big)|-\rangle
$$

We can factor out $(-1)^{f(0)}$:

$$
|\pi_2\rangle = (-1)^{f(0)} \cdot \frac{1}{\sqrt{2}}\Big(|0\rangle + (-1)^{f(0) \oplus f(1)}|1\rangle\Big)|-\rangle
$$

Now the state of the input qubit depends on $f(0) \oplus f(1)$:

- If $f$ is **constant**: $f(0) \oplus f(1) = 0$, so the input qubit is $|+\rangle$.
- If $f$ is **balanced**: $f(0) \oplus f(1) = 1$, so the input qubit is $|-\rangle$.

**Step 3 — Hadamard on the input qubit:**

$$
H|+\rangle = |0\rangle, \qquad H|-\rangle = |1\rangle
$$

So:

$$
|\pi_3\rangle = (-1)^{f(0)}|f(0) \oplus f(1)\rangle \otimes |-\rangle
$$

**Step 4 — Measure the input qubit:**

$$
\text{Measure } |0\rangle \Rightarrow f \text{ is constant}, \qquad \text{Measure } |1\rangle \Rightarrow f \text{ is balanced}
$$

One query. Deterministic. The global phase $(-1)^{f(0)}$ is unobservable.

```{admonition} Check Yourself
:class: tip
Trace through the state evolution for $f_3$ (the function $f(0) = 0, f(1) = 1$) explicitly. After the oracle, the input qubit should be $|-\rangle$, and after the final Hadamard, you should measure $|1\rangle$ with certainty.
```

```{admonition} iClicker
:class: question
Deutsch's algorithm determines whether $f$ is constant or balanced using how many queries?

A) 0 &emsp; B) 1 &emsp; C) 2 &emsp; D) It depends on $f$
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

### The Generalized Problem

Now scale up. Consider $f: \{0, 1\}^n \to \{0, 1\}$ — a Boolean function on $n$-bit strings. We are promised that $f$ is one of two types:

- **Constant:** $f(x)$ is the same for all $2^n$ inputs (all 0's or all 1's).
- **Balanced:** $f(x) = 0$ for exactly half the inputs and $f(x) = 1$ for the other half.

**Question:** Which type is $f$?

```{admonition} The Promise Matters
:class: warning
The problem assumes $f$ is **promised** to be either constant or balanced — nothing in between. If $f$ outputs 0 for 60% of inputs and 1 for 40%, neither label applies and the algorithm's output is meaningless. This "promise" structure is common in quantum algorithms: the speedup is only valid when the input satisfies the stated guarantee.
```

### The Classical Cost

Classically, you need to query $f$ on enough inputs to be sure. In the worst case, the first $2^{n-1}$ queries could all return the same value — that's consistent with both constant and balanced. You need **one more** query to decide:

$$
\text{Classical worst-case queries} = 2^{n-1} + 1
$$

For $n = 10$, that's 513 queries. For $n = 50$, that's over $10^{15}$.

### The Quantum Circuit: One Query

The Deutsch-Jozsa algorithm uses the same skeleton as Deutsch's algorithm, just with more qubits:

$$
|0\rangle^{\otimes n}|1\rangle \xrightarrow{H^{\otimes (n+1)}} \xrightarrow{U_f} \xrightarrow{H^{\otimes n}} \xrightarrow{\text{measure}}
$$

```{code-cell} python
:tags: [hide-input]
# Show the general Deutsch-Jozsa circuit structure
n_show = 4
qc_show = QuantumCircuit(n_show + 1, n_show)
qc_show.x(n_show)
qc_show.h(range(n_show + 1))
qc_show.barrier(label="$U_f$")
qc_show.barrier()
qc_show.h(range(n_show))
qc_show.measure(range(n_show), range(n_show))
qc_show.draw("mpl")
```

### The Derivation

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

The oracle has imprinted $f$ as a **pattern of signs** across the superposition. If $f$ is constant, all signs are the same ($+$ or $-$). If $f$ is balanced, half are $+$ and half are $-$.

**Step 3 — Apply $H^{\otimes n}$ to the input register.**

This is where interference happens. We need to know how $H^{\otimes n}$ acts on a computational basis state. Start with the single-qubit case you already know:

$$
H|x\rangle = \frac{1}{\sqrt{2}}\sum_{z \in \{0,1\}} (-1)^{xz} |z\rangle
$$

Check: $H|0\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ and $H|1\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$. Correct — the $(-1)^{xz}$ picks up a minus sign only when both $x = 1$ and $z = 1$.

For $n$ qubits, $H^{\otimes n} = H \otimes H \otimes \cdots \otimes H$ acts on each qubit independently, so the phases just multiply:

$$
H^{\otimes n}|x\rangle = \frac{1}{\sqrt{2^n}} \sum_{z=0}^{2^n - 1} (-1)^{x \cdot z} |z\rangle
$$

where $x \cdot z = x_1 z_1 \oplus x_2 z_2 \oplus \cdots \oplus x_n z_n$ is the bitwise inner product mod 2. This isn't a new formula — it's just $n$ copies of the single-qubit Hadamard tensored together.

Applying this to the full state:

$$
|\pi_3\rangle = \sum_{z=0}^{2^n - 1} \left[\frac{1}{2^n} \sum_{x=0}^{2^n - 1} (-1)^{f(x)} (-1)^{x \cdot z}\right] |z\rangle \otimes |-\rangle
$$

The amplitude of each output state $|z\rangle$ is:

$$
a_z = \frac{1}{2^n} \sum_{x=0}^{2^n - 1} (-1)^{f(x) + x \cdot z}
$$

**Step 4 — Measure.** We only need one amplitude: the probability of measuring $|0\rangle^{\otimes n}$.

When $z = 0\cdots 0$, we have $x \cdot z = 0$ for all $x$, so:

$$
\boxed{a_{0\cdots 0} = \frac{1}{2^n} \sum_{x=0}^{2^n - 1} (-1)^{f(x)}}
$$

Now the two cases:

**If $f$ is constant:** All $(-1)^{f(x)}$ terms are the same — either all $+1$ or all $-1$. The sum is $\pm 2^n$. So $a_{0\cdots 0} = \pm 1$ and we measure $|0\cdots 0\rangle$ with **certainty**.

**If $f$ is balanced:** Half the terms are $+1$ and half are $-1$. They cancel exactly: the sum is 0. So $a_{0\cdots 0} = 0$ and we **never** measure $|0\cdots 0\rangle$.

```{admonition} Deutsch-Jozsa Decision Rule
:class: important
Measure all $n$ input qubits. If the result is $|0\rangle^{\otimes n}$, the function is **constant**. If the result is **anything else**, the function is **balanced**. One query. Deterministic. Exponential speedup.
```

```{admonition} iClicker
:class: question
For a 10-qubit Deutsch-Jozsa problem, how many queries does a classical algorithm need in the worst case?

A) 10 &emsp; B) 512 &emsp; C) 513 &emsp; D) 1024
```

```{admonition} iClicker
:class: question
You run Deutsch-Jozsa on a 7-qubit problem and measure $|0010100\rangle$ (not all zeros). What do you conclude?

A) $f$ is constant &emsp; B) $f$ is balanced &emsp; C) Need more queries &emsp; D) The algorithm failed
```

### The Interference Picture

Let's be explicit about what just happened, because this is the heart of quantum algorithms.

The first Hadamard wall splits the computation into $2^n$ "paths" — one for each possible input $x$. The oracle imprints a phase $(-1)^{f(x)}$ on each path. The second Hadamard wall recombines all $2^n$ paths.

For a constant function, all paths carry the same phase. They **constructively interfere** at $|0\cdots 0\rangle$, and the probability piles up there.

For a balanced function, half the paths carry $+1$ and half carry $-1$. At $|0\cdots 0\rangle$, they **destructively interfere** — perfect cancellation, zero probability.

This is the Mach-Zehnder interferometer from Lecture 2.2, scaled to $2^n$ paths. The quantum speedup comes entirely from interference.

### Qiskit Implementation

```{code-cell} python
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator
import numpy as np

def dj_constant_oracle(n, output_value=0):
    """Constant oracle: f(x) = output_value for all x."""
    oracle = QuantumCircuit(n + 1, name=f"constant({output_value})")
    if output_value == 1:
        oracle.x(n)  # Flip target unconditionally
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

The Bernstein-Vazirani problem (1997) uses the same circuit as Deutsch-Jozsa but solves a different — and in some ways more satisfying — problem.

**Setup:** There exists a secret $n$-bit string $s = s_1 s_2 \cdots s_n$ that you don't know. You're given an oracle that computes:

$$
f(x) = s \cdot x = s_1 x_1 \oplus s_2 x_2 \oplus \cdots \oplus s_n x_n \pmod{2}
$$

This is the bitwise inner product of $s$ with $x$, mod 2. **Your goal: find $s$.**

### The Classical Approach

Classically, you can learn one bit of $s$ per query by feeding in unit vectors:

- Query $f(100\cdots 0) = s_1$
- Query $f(010\cdots 0) = s_2$
- $\vdots$
- Query $f(000\cdots 1) = s_n$

That takes $n$ queries. You can't do better — each query reveals at most one bit of information.

### The Quantum Solution: One Query

Run the **exact same circuit** as Deutsch-Jozsa: $H^{\otimes n}$, oracle, $H^{\otimes n}$, measure. The output is $|s\rangle$ directly.

**Why?** The oracle for $f(x) = s \cdot x$ imprints the phase $(-1)^{s \cdot x}$. After phase kickback, the input register is:

$$
\frac{1}{\sqrt{2^n}} \sum_x (-1)^{s \cdot x} |x\rangle
$$

But recall the formula from Part 4: $H^{\otimes n}|s\rangle = \frac{1}{\sqrt{2^n}} \sum_x (-1)^{s \cdot x} |x\rangle$. So the state above is just $H^{\otimes n}|s\rangle$! Since $H^{\otimes n}$ is its own inverse ($H^{\otimes n} H^{\otimes n} = I$), applying the final Hadamard wall gives:

$$
H^{\otimes n} \cdot H^{\otimes n}|s\rangle = |s\rangle
$$

The secret string pops out directly.

One query reveals all $n$ bits of $s$. Classical needs $n$ queries. Linear speedup, cleaner than Deutsch-Jozsa in some ways because you learn the *actual answer*, not just a property.

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
A classical algorithm needs how many queries to find a 20-bit secret string $s$ in the Bernstein-Vazirani problem?

A) 1 &emsp; B) 10 &emsp; C) 20 &emsp; D) $2^{20}$
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

Next lecture: **Grover's search algorithm**. Deutsch-Jozsa gives an exponential speedup but solves a somewhat artificial problem (who really needs to distinguish constant from balanced?). Grover's algorithm tackles something universal — finding a marked item in an unstructured list — and achieves a **quadratic** speedup: $O(\sqrt{N})$ queries instead of $O(N)$. The oracle framework and phase kickback carry over directly. The new ingredient is **amplitude amplification**: a geometric rotation in Hilbert space that pumps probability into the marked state.

**Homework:** [Deutsch-Jozsa Qiskit Notebook](../lecture_resources/deutsch-jozsa.ipynb)

Work through the IBM tutorial notebook. It covers Deutsch's algorithm, Deutsch-Jozsa, and Bernstein-Vazirani with Qiskit — including running on real hardware. **Due: Wednesday at midnight.**
