---
kernelspec:
  name: python3
  display_name: Python 3
---

# Lecture 4.6 — Grover's Algorithm: Searching with Quantum Amplitude Amplification

## Where We Left Off

In Lecture 4.5, we built our first quantum algorithms. Deutsch-Jozsa determines whether a function is constant or balanced with a single query — an exponential speedup over classical. Bernstein-Vazirani finds a secret string in one shot. Both use the same skeleton: $H^{\otimes n}$ → oracle → $H^{\otimes n}$ → measure. And both rely on the same trick: **phase kickback** turns function evaluations into phase differences, and the final Hadamards convert those phases into measurable amplitudes through interference.

But here's an honest question: who actually needs to distinguish constant from balanced? Deutsch-Jozsa solves a clean mathematical problem, but it's somewhat artificial. Today we tackle something universal — finding a needle in a haystack — and discover that quantum mechanics gives us a quadratic speedup. Not exponential this time, but applicable to *any* search problem.

The new ingredient is **iteration**. Deutsch-Jozsa works in a single pass. Grover's algorithm applies the same two operations over and over, steadily pumping probability into the answer. This idea — amplitude amplification — is one of the most powerful tools in quantum computing.

------------------------------------------------------------------------

## Part 1: The Unstructured Search Problem

### Finding a Needle in a Haystack

Imagine you have a phone book with $N$ entries, but the names aren't sorted alphabetically — they're in random order. You're looking for one specific person. How do you find them?

Classically, there's no shortcut. You check entries one by one. On average, you'll need to look through about $N/2$ entries before finding the right one, and in the worst case, all $N$. The query complexity is $O(N)$.

This is the **unstructured search problem**: given $N$ items and a black-box function $f(x)$ that returns 1 for the "marked" item and 0 for everything else, find the marked item using as few queries as possible.

```{admonition} Why "Unstructured"?
:class: note
The word "unstructured" is key. If the data has structure — like a sorted list — you can use binary search and find the answer in $O(\log N)$ queries. Grover's algorithm applies when there's *no* structure to exploit. The only thing you can do is query the oracle and check.
```

### Why This Matters

Unstructured search shows up everywhere: searching an unsorted database, checking whether a solution to a constraint satisfaction problem exists, finding a preimage of a hash function, optimizing over a landscape with no gradient information. Any problem where you're essentially trying every possibility can potentially benefit from Grover's speedup.

### Grover's Promise

Lov Grover showed in 1996 that a quantum computer can solve this problem with only $O(\sqrt{N})$ queries. For a database with $N = 10^6$ entries, that's roughly 1,000 quantum queries instead of 500,000 classical ones. For $N = 10^{12}$, it's $10^6$ instead of $10^{12}$ — a million-fold improvement.


This is a **quadratic** speedup, not exponential like Deutsch-Jozsa. But it applies to a much broader class of problems, and it's provably optimal — no quantum algorithm can do better than $O(\sqrt{N})$ for unstructured search (this is the **BBBV theorem**, proven by Bennett, Bernstein, Brassard, and Vazirani in 1997).

------------------------------------------------------------------------

## Part 2: The Oracle — Marking with a Phase

### From Bit Flip to Phase Flip

In Lecture 4.5, we used two forms of the oracle:

- **Standard oracle:** $U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$ (flips the target qubit)
- **Phase oracle:** $Z_f|x\rangle = (-1)^{f(x)}|x\rangle$ (flips the phase of marked states)

We showed that feeding $|-\rangle$ into the target register of the standard oracle automatically gives us the phase oracle through phase kickback.

For Grover's algorithm, we work directly with the **phase oracle** $Z_f$. Its action is simple:

$$
Z_f|x\rangle = \begin{cases} -|x\rangle & \text{if } f(x) = 1 \text{ (marked)} \\ +|x\rangle & \text{if } f(x) = 0 \text{ (unmarked)} \end{cases}
$$

Think of it as a mirror that reflects only the marked state. Everything else passes through unchanged.

### Building the Oracle with a Circuit

For a specific marked state, say $|\omega\rangle = |110\rangle$ on 3 qubits, the phase oracle applies a multi-controlled Z gate. The idea:

1. Apply X gates to any qubit that should be $|0\rangle$ in the target (here, qubit 0).
2. Apply a multi-controlled Z gate (all qubits controlling a phase flip).
3. Apply the X gates again to undo step 1.

This flips the phase *only* when the input matches the target state. In Qiskit, the multi-controlled Z is built using `MCMTGate(ZGate(), n-1, 1)`:

```{code-cell} python
:tags: [hide-input]
from qiskit import QuantumCircuit
from qiskit.circuit.library import MCMTGate, ZGate
from qiskit.quantum_info import Statevector, Operator
import numpy as np
import matplotlib.pyplot as plt
```

```{code-cell} python
# Oracle for marking |110⟩ on 3 qubits
oracle = QuantumCircuit(3)

# Flip qubit 0 so that |110⟩ → |111⟩
oracle.x(0)

# Multi-controlled Z: phase flip when all qubits are |1⟩
oracle.compose(MCMTGate(ZGate(), 2, 1), inplace=True)

# Undo the X gate
oracle.x(0)

oracle.draw(output="mpl", style="iqp")
```

Let's verify this does what we expect — it should put a $-1$ phase on $|110\rangle$ and leave everything else alone:

```{code-cell} python
# Check the oracle's action on all basis states
op = Operator(oracle)
print("Oracle diagonal elements (phases on each basis state):")
for i in range(2**3):
    bits = format(i, '03b')
    phase = np.real(op.data[i, i])
    marker = " ← MARKED" if phase < 0 else ""
    print(f"  |{bits}⟩: phase = {phase:+.0f}{marker}")
```

```{admonition} Qiskit Qubit Ordering
:class: warning
Remember: Qiskit labels qubit 0 as the *rightmost* bit in the ket. So $|110\rangle$ in our notation means qubit 2 = 1, qubit 1 = 1, qubit 0 = 0. When building oracles, keep track of which qubit index corresponds to which bit position.
```

------------------------------------------------------------------------

## Part 3: The Diffusion Operator and Inversion About the Mean

### The Missing Ingredient

The oracle marks the target by flipping its phase. But a phase flip alone doesn't help. If we measure right after the oracle, every state still has the same probability $1/N$. We can't *see* a phase difference in the measurement probabilities.

We need a second operation that converts the phase difference into an *amplitude* difference. This is the **diffusion operator**, and it's the key new idea in Grover's algorithm.

### Set Up the Notation

Write a general state on $N = 2^n$ basis states as

$$
|\psi\rangle = \sum_{x=0}^{N-1} a_x\, |x\rangle,
$$

so $a_x$ is the (complex) amplitude of basis state $|x\rangle$. Immediately after the oracle acts on the uniform superposition, we have

$$
a_x \;=\; \begin{cases} +\,\dfrac{1}{\sqrt{N}} & x \text{ is unmarked}, \\[6pt] -\,\dfrac{1}{\sqrt{N}} & x \text{ is marked}. \end{cases}
$$

All $N-1$ unmarked amplitudes sit at $+1/\sqrt{N}$; a single marked amplitude sits at $-1/\sqrt{N}$.

### The Mean Amplitude

Compute the mean of all $N$ amplitudes:

$$
\bar{a} \;=\; \frac{1}{N}\sum_x a_x \;=\; \frac{1}{N}\!\left[\,(N-1)\cdot\tfrac{1}{\sqrt{N}} + 1\cdot\!\left(-\tfrac{1}{\sqrt{N}}\right)\right] \;=\; \frac{1}{\sqrt{N}} - \frac{2}{N\sqrt{N}}.
$$

In words: the mean is *almost* the uniform amplitude $1/\sqrt{N}$, minus a tiny correction of order $1/(N\sqrt{N})$ from the one flipped amplitude. For large $N$, we have $\bar{a} \approx 1/\sqrt{N}$.

### The Reflection Idea

Now the key step. Imagine we could apply an operation that **reflects each amplitude about this mean**: take whatever $a_x$ is, and flip it to the opposite side of $\bar{a}$.

Geometrically, the distance from $a_x$ to $\bar{a}$ is $a_x - \bar{a}$. Reflecting across $\bar{a}$ sends this distance to its negative:

$$
a_x \;\longrightarrow\; \bar{a} - (a_x - \bar{a}) \;=\; 2\bar{a} - a_x.
$$

That is why the formula $a_x \to 2\bar{a} - a_x$ shows up. It is what "reflect across $\bar{a}$ on the number line" looks like in algebra.

Let's see what this reflection does to our post-oracle amplitudes:

- **Unmarked** amplitude ($a_x = +1/\sqrt{N}$). This sits a tiny bit *above* $\bar{a}$. After reflection it lands a tiny bit *below* $\bar{a}$: its value shrinks to $2\bar{a} - 1/\sqrt{N}$.
- **Marked** amplitude ($a_x = -1/\sqrt{N}$). This sits *far below* $\bar{a}$, by roughly $2/\sqrt{N}$. After reflection it jumps *far above*: its new value is $2\bar{a} + 1/\sqrt{N} \;\approx\; 3/\sqrt{N}$ for large $N$, which is about **three times** the uniform amplitude.

Each iteration of oracle + diffusion pumps more amplitude into the marked state and drains it from the rest.

```{admonition} Physical Intuition
:class: tip
This is interference, just like the Mach-Zehnder interferometer from Lecture 2.2. The oracle creates a phase difference (like a phase shifter in one arm). The diffusion operator acts like the second beam splitter — it mixes the paths so that the phase difference shows up as an amplitude (intensity) difference. Grover's algorithm is an interferometer that you walk through multiple times, each pass increasing the contrast.
```

### The Diffusion Operator as a Matrix

We just derived the inversion about the mean component by component. Now let's package it into a single operator. Define $|s\rangle$ as the uniform superposition:

$$
|s\rangle = H^{\otimes n}|0^n\rangle = \frac{1}{\sqrt{N}} \sum_{x=0}^{N-1} |x\rangle.
$$

Then the diffusion operator is:

$$
D \;=\; 2|s\rangle\langle s| - I.
$$

At first glance this expression looks nothing like "flip each amplitude about the mean." Let's unpack why it *is* a reflection, and then why that reflection is the same as inversion about the mean.

#### Why $D = 2|s\rangle\langle s| - I$ is a reflection

Pick any state $|\psi\rangle$ and split it into a piece along $|s\rangle$ and a piece orthogonal to $|s\rangle$:

$$
|\psi\rangle \;=\; \alpha\,|s\rangle \;+\; \beta\,|s^\perp\rangle, \qquad \alpha = \langle s|\psi\rangle.
$$

Apply $D$ to each piece separately. Using $\langle s|s\rangle = 1$ and $\langle s|s^\perp\rangle = 0$:

$$
D|s\rangle \;=\; 2|s\rangle\langle s|s\rangle - |s\rangle \;=\; 2|s\rangle - |s\rangle \;=\; +|s\rangle,
$$

$$
D|s^\perp\rangle \;=\; 2|s\rangle\langle s|s^\perp\rangle - |s^\perp\rangle \;=\; 0 - |s^\perp\rangle \;=\; -|s^\perp\rangle.
$$

So $D$ leaves the component *along* $|s\rangle$ alone and *flips the sign* of everything perpendicular to $|s\rangle$:

$$
D|\psi\rangle \;=\; \alpha\,|s\rangle \;-\; \beta\,|s^\perp\rangle.
$$

That is exactly what "reflection about the axis $|s\rangle$" means. Compare it to reflecting a point $(x,y)$ in the plane across the $x$-axis: $x$ stays, $y$ flips sign. Here $|s\rangle$ plays the role of the $x$-axis.

#### Why this reflection equals inversion about the mean

Now the punchline: the component of $|\psi\rangle$ along $|s\rangle$ is proportional to the **mean amplitude**. If $|\psi\rangle = \sum_x a_x |x\rangle$, then

$$
\alpha \;=\; \langle s|\psi\rangle \;=\; \frac{1}{\sqrt{N}}\sum_x a_x \;=\; \sqrt{N}\,\bar{a}.
$$

So "reflecting about $|s\rangle$" and "reflecting each amplitude about its mean $\bar{a}$" are the same operation. Writing out the components of $D|\psi\rangle$:

$$
D|\psi\rangle \;=\; 2\langle s|\psi\rangle\,|s\rangle - |\psi\rangle \;=\; 2\sqrt{N}\,\bar{a}\cdot\tfrac{1}{\sqrt{N}}\sum_x |x\rangle - \sum_x a_x|x\rangle \;=\; \sum_x (2\bar{a} - a_x)\,|x\rangle.
$$

The $x$-th component is $2\bar{a} - a_x$, exactly the inversion-about-the-mean formula from the previous section.

### Circuit for the Diffusion Operator

Since $|s\rangle = H^{\otimes n}|0^n\rangle$, we can rewrite:

$$
D = H^{\otimes n}\left(2|0^n\rangle\langle 0^n| - I\right)H^{\otimes n}
$$

The middle piece, $2|0^n\rangle\langle 0^n| - I$, flips the phase of every state *except* $|0\cdots 0\rangle$. We call this $Z_{\text{OR}}$ (it applies a phase flip if any qubit is $|1\rangle$). The full diffusion circuit is:

$$
\boxed{D = H^{\otimes n} \cdot Z_{\text{OR}} \cdot H^{\otimes n}}
$$

```{code-cell} python
def diffusion_operator(n):
    """Build the n-qubit diffusion operator D = H⊗n · Z_OR · H⊗n"""
    qc = QuantumCircuit(n)
    
    # H on all qubits
    qc.h(range(n))
    
    # Z_OR: phase flip on everything except |00...0⟩
    # This is equivalent to: X on all, multi-controlled Z, X on all
    qc.x(range(n))
    qc.compose(MCMTGate(ZGate(), n - 1, 1), inplace=True)
    qc.x(range(n))
    
    # H on all qubits
    qc.h(range(n))
    
    return qc

# Show the 3-qubit diffusion operator
diffusion_operator(3).draw(output="mpl", style="iqp")
```

```{admonition} iClicker
:class: question
After one oracle call on 2 qubits ($N = 4$, one marked state), the marked state's amplitude goes from $+1/2$ to $-1/2$. The mean of all four amplitudes is now $\bar{a} = 1/4$. After inversion about the mean, what is the marked state's amplitude?

A) $1/4$ &emsp; B) $1/2$ &emsp; C) $3/4$ &emsp; D) $1$
```

```{dropdown} Answer
**D) $1$.** The formula gives $2\bar{a} - a_{\text{marked}} = 2(1/4) - (-1/2) = 1/2 + 1/2 = 1$. The marked state gets *all* the amplitude in one iteration! (This only works so cleanly for $N = 4$.)
```

------------------------------------------------------------------------

## Part 4: The Full Algorithm — 2-Qubit Worked Example

### The Algorithm

Grover's algorithm has three steps:

1. **Initialize:** Start in $|0^n\rangle$, apply $H^{\otimes n}$ to create uniform superposition $|s\rangle$.
2. **Iterate:** Repeat $t$ times: apply oracle $Z_f$, then diffusion $D$.
3. **Measure:** Measure all qubits. The marked state appears with high probability.

The number of iterations is:

$$
t = \left\lfloor \frac{\pi}{4}\sqrt{\frac{N}{M}} \right\rfloor
$$

where $M$ is the number of marked states. For $M = 1$ and $N = 4$: $t = \lfloor \frac{\pi}{4}\sqrt{4} \rfloor = \lfloor \frac{\pi}{2} \rfloor = 1$. Just one iteration!

### Complete State Evolution for $N = 4$

Let's work through Grover's algorithm on 2 qubits with marked state $|\omega\rangle = |11\rangle$. This is small enough to track every amplitude by hand.

**Step 0: Initialize**

$$
|\psi_0\rangle = |00\rangle
$$

**Step 1: Apply $H^{\otimes 2}$**

$$
|\psi_1\rangle = \frac{1}{2}\left(|00\rangle + |01\rangle + |10\rangle + |11\rangle\right)
$$

All four amplitudes are $+1/2$. Probability of measuring $|11\rangle$ is $(1/2)^2 = 25\%$ — no better than random guessing.

**Step 2: Apply the oracle $Z_f$**

The oracle flips the sign of $|11\rangle$:

$$
|\psi_2\rangle = \frac{1}{2}\left(|00\rangle + |01\rangle + |10\rangle - |11\rangle\right)
$$

The probabilities haven't changed — still 25% each! The oracle only changed a phase, which is invisible to measurement. But the diffusion operator will convert this phase into amplitude.

**Step 3: Apply the diffusion operator $D$**

The mean amplitude is:

$$
\bar{a} = \frac{1}{4}\left(\frac{1}{2} + \frac{1}{2} + \frac{1}{2} + \left(-\frac{1}{2}\right)\right) = \frac{1}{4}
$$

Reflecting each amplitude about the mean:

- **$|00\rangle$:** $2(1/4) - 1/2 = 0$
- **$|01\rangle$:** $2(1/4) - 1/2 = 0$
- **$|10\rangle$:** $2(1/4) - 1/2 = 0$
- **$|11\rangle$:** $2(1/4) - (-1/2) = 1$

$$
|\psi_3\rangle = 0 \cdot |00\rangle + 0 \cdot |01\rangle + 0 \cdot |10\rangle + 1 \cdot |11\rangle = |11\rangle
$$

**The marked state has amplitude 1. Measurement gives $|11\rangle$ with 100% probability!**

In just one oracle query, Grover's algorithm finds the needle in the haystack — deterministically for $N = 4$.

### Verify with Qiskit

Let's confirm this with a Statevector simulation:

```{code-cell} python
# Build the 2-qubit Grover circuit for marked state |11⟩
n = 2
grover = QuantumCircuit(n)

# Step 1: Create uniform superposition
grover.h(range(n))
grover.barrier(label="H⊗n")

# Step 2: Oracle — phase flip on |11⟩
# For |11⟩, all qubits are already |1⟩, so just use multi-controlled Z
grover.compose(MCMTGate(ZGate(), 1, 1), inplace=True)
grover.barrier(label="Oracle")

# Step 3: Diffusion operator
grover.h(range(n))
grover.x(range(n))
grover.compose(MCMTGate(ZGate(), 1, 1), inplace=True)
grover.x(range(n))
grover.h(range(n))
grover.barrier(label="Diffusion")

grover.draw(output="mpl", style="iqp")
```

```{code-cell} python
# Trace the state at each stage
sv = Statevector.from_label('00')

# After H⊗2
h_layer = QuantumCircuit(2)
h_layer.h(range(2))
sv = sv.evolve(h_layer)

print("After H⊗2:")
for i, amp in enumerate(sv.data):
    print(f"  |{format(i, '02b')}⟩: {amp.real:+.4f}")

# After oracle (CZ = multi-controlled Z for |11⟩)
oracle_qc = QuantumCircuit(2)
oracle_qc.compose(MCMTGate(ZGate(), 1, 1), inplace=True)
sv = sv.evolve(oracle_qc)

print("\nAfter oracle:")
for i, amp in enumerate(sv.data):
    print(f"  |{format(i, '02b')}⟩: {amp.real:+.4f}")

# After diffusion
diff_qc = diffusion_operator(2)
sv = sv.evolve(diff_qc)

print("\nAfter diffusion:")
for i, amp in enumerate(sv.data):
    print(f"  |{format(i, '02b')}⟩: {amp.real:+.4f}")
    
print(f"\nProbability of |11⟩: {abs(sv.data[3])**2:.1%}")
```

```{admonition} Check Yourself
:class: tip
Work through the same calculation with marked state $|01\rangle$ instead of $|11\rangle$. You'll need to modify the oracle (add an X gate on qubit 1 before and after the multi-controlled Z). Do you still get a deterministic result in one iteration?
```

------------------------------------------------------------------------

## Part 5: The Geometric Picture — Why $\sqrt{N}$?

### A Rotation in Two Dimensions

Here's the beautiful insight behind Grover's algorithm. The entire computation takes place in a **2-dimensional subspace** of the $N$-dimensional Hilbert space. We only need two basis vectors:

$$
|\omega\rangle = \text{the marked state}
$$

$$
|\omega^\perp\rangle = \frac{1}{\sqrt{N-1}} \sum_{x \neq \omega} |x\rangle = \text{uniform superposition of all unmarked states}
$$

The initial state $|s\rangle = \frac{1}{\sqrt{N}}\sum_x |x\rangle$ can be written as:

$$
|s\rangle = \sin\theta \, |\omega\rangle + \cos\theta \, |\omega^\perp\rangle
$$

where $\sin\theta = \frac{1}{\sqrt{N}}$, so $\theta \approx \frac{1}{\sqrt{N}}$ for large $N$.

### Each Iteration is a Rotation

Now here's the key geometric fact:

- The **oracle** $Z_f$ reflects the state about $|\omega^\perp\rangle$ (it flips the $|\omega\rangle$ component).
- The **diffusion** $D = 2|s\rangle\langle s| - I$ reflects the state about $|s\rangle$.

Two reflections compose to give a **rotation**. Each application of $G = D \cdot Z_f$ rotates the state by an angle $2\theta$ toward $|\omega\rangle$.

```{code-cell} python
:tags: [hide-input]
# Geometric visualization of Grover's algorithm
fig, ax = plt.subplots(1, 1, figsize=(7, 6))

N = 16  # 4 qubits
theta = np.arcsin(1/np.sqrt(N))

# Draw axes
ax.arrow(0, 0, 1.15, 0, head_width=0.03, head_length=0.03, fc='black', ec='black')
ax.arrow(0, 0, 0, 1.15, head_width=0.03, head_length=0.03, fc='black', ec='black')
ax.text(1.22, -0.05, r'$|\omega^\perp\rangle$', fontsize=14)
ax.text(-0.15, 1.18, r'$|\omega\rangle$', fontsize=14)

# Draw states after each iteration
colors = ['#2563eb', '#dc2626', '#16a34a', '#9333ea']
labels = [r'$|s\rangle$: start', r'After 1 iteration', r'After 2 iterations', r'After 3 iterations']

for k in range(4):
    angle = (2*k + 1) * theta
    x = np.cos(angle)
    y = np.sin(angle)
    ax.arrow(0, 0, x*0.95, y*0.95, head_width=0.03, head_length=0.03, 
             fc=colors[k], ec=colors[k], linewidth=2)
    # Label placement
    offset_x = 0.08 if k < 2 else -0.25
    offset_y = 0.05
    ax.text(x*0.95 + offset_x, y*0.95 + offset_y, labels[k], 
            fontsize=11, color=colors[k])

# Draw angle arc for theta
arc_angles = np.linspace(0, theta, 30)
ax.plot(0.4*np.cos(arc_angles), 0.4*np.sin(arc_angles), 'k-', linewidth=1)
ax.text(0.45, 0.06, r'$\theta$', fontsize=13)

# Draw angle arc for 2theta rotation
arc_angles2 = np.linspace(theta, 3*theta, 30)
ax.plot(0.55*np.cos(arc_angles2), 0.55*np.sin(arc_angles2), 'r--', linewidth=1)
ax.text(0.52, 0.2, r'$2\theta$', fontsize=13, color='red')

# Target line at pi/2
ax.plot([0, 0], [0, 1.1], 'k--', alpha=0.2, linewidth=1)

ax.set_xlim(-0.3, 1.4)
ax.set_ylim(-0.15, 1.35)
ax.set_aspect('equal')
ax.axis('off')
ax.set_title(f'Grover iterations for N = {N} (θ ≈ {np.degrees(theta):.1f}°)', fontsize=13)
plt.tight_layout()
plt.show()
```

### The Optimal Number of Iterations

After $t$ iterations, the state has been rotated to angle $(2t+1)\theta$ from the $|\omega^\perp\rangle$ axis. We want this to equal $\pi/2$ (pointing straight at $|\omega\rangle$):

$$
(2t+1)\theta \approx \frac{\pi}{2} \quad \Longrightarrow \quad t \approx \frac{\pi}{4\theta} - \frac{1}{2} \approx \frac{\pi}{4}\sqrt{N}
$$

since $\theta \approx 1/\sqrt{N}$. This is where the $\sqrt{N}$ comes from — it's the number of small rotations needed to reach the target.

### The Overshoot Problem

What happens if you apply *too many* iterations? The state rotates past $|\omega\rangle$ and the success probability *decreases*. Keep going, and it oscillates back and forth:

```{code-cell} python
:tags: [hide-input]
# Success probability vs. number of iterations
fig, axes = plt.subplots(1, 2, figsize=(12, 4))

for idx, n_qubits in enumerate([2, 4]):
    N = 2**n_qubits
    theta = np.arcsin(1/np.sqrt(N))
    max_iter = int(3 * np.pi / (4 * theta))
    
    iterations = range(0, max_iter + 1)
    probs = [np.sin((2*t + 1) * theta)**2 for t in iterations]
    optimal_t = round(np.pi / (4 * theta) - 0.5)
    
    axes[idx].bar(iterations, probs, color=['#dc2626' if t == optimal_t else '#2563eb' 
                                             for t in iterations], alpha=0.8)
    axes[idx].set_xlabel('Number of Grover iterations', fontsize=12)
    axes[idx].set_ylabel('P(marked state)', fontsize=12)
    axes[idx].set_title(f'{n_qubits} qubits (N = {N}), optimal t = {optimal_t}', fontsize=12)
    axes[idx].set_ylim(0, 1.05)
    axes[idx].axhline(y=1/N, color='gray', linestyle='--', alpha=0.5, label=f'Random guess: 1/{N}')
    axes[idx].legend(fontsize=10)

plt.tight_layout()
plt.show()
```

```{admonition} iClicker
:class: question
You're running Grover's algorithm on $N = 256$ items (8 qubits). The optimal number of iterations is about $\frac{\pi}{4}\sqrt{256} \approx 12$. What happens if you accidentally run 25 iterations instead?

A) Success probability stays near 100% &emsp; B) ~50% &emsp; C) Close to 0% &emsp; D) Exactly 0%
```

```{dropdown} Answer
**C) Close to 0%.** After 25 iterations, the state has rotated well past $|\omega\rangle$. With $\theta \approx 1/16$, the angle after 25 iterations is $(2 \cdot 25 + 1)/16 \approx 3.19$ radians — past $\pi/2$ by about 1.6 radians. The probability is $\sin^2(3.19) \approx 0.002$. More iterations made the answer *worse* — a deeply non-classical phenomenon.
```

```{admonition} A Quantum Paradox
:class: warning
In classical computing, more computation never hurts — checking more entries can only increase your chance of finding the answer. In Grover's algorithm, **more iterations can make the answer worse.** This is because the dynamics are *coherent* — amplitudes interfere, and constructive interference at the optimal point becomes destructive interference past it. You must stop at the right time.
```

------------------------------------------------------------------------

## Part 6: Qiskit Implementation

### Building Grover's for Larger Problems

Let's implement Grover's algorithm for $n = 4$ qubits ($N = 16$ states) with a single marked state. We'll use the `grover_operator` helper from Qiskit:

```{code-cell} python
from qiskit.circuit.library import GroverOperator

def grover_oracle(marked_states, n):
    """Build a phase oracle that marks the given states."""
    oracle = QuantumCircuit(n)
    for target in marked_states:
        # Flip qubits that should be |0⟩ in the target
        for i, bit in enumerate(reversed(target)):
            if bit == '0':
                oracle.x(i)
        # Multi-controlled Z
        oracle.compose(MCMTGate(ZGate(), n - 1, 1), inplace=True)
        # Undo the X flips
        for i, bit in enumerate(reversed(target)):
            if bit == '0':
                oracle.x(i)
    return oracle

# Build oracle for marked state |1010⟩
n = 4
marked = ["1010"]
oracle = grover_oracle(marked, n)

# Build the full Grover operator (oracle + diffusion)
grover_op = GroverOperator(oracle)

# Calculate optimal iterations
import math
num_solutions = len(marked)
optimal_iter = math.floor(math.pi / (4 * math.asin(math.sqrt(num_solutions / 2**n))) - 0.5)
print(f"N = {2**n}, marked states = {marked}")
print(f"Optimal iterations: {optimal_iter}")
```

```{code-cell} python
# Build and run the full circuit
qc = QuantumCircuit(n)
qc.h(range(n))  # Initial superposition

for _ in range(optimal_iter):
    qc.compose(grover_op, inplace=True)

qc.measure_all()

# Simulate
from qiskit_aer import AerSimulator
simulator = AerSimulator()
from qiskit import transpile

qc_transpiled = transpile(qc, simulator)
result = simulator.run(qc_transpiled, shots=1024).result()
counts = result.get_counts()

# Plot
from qiskit.visualization import plot_histogram
plot_histogram(counts, title="Grover's Algorithm: 4 qubits, target = |1010⟩")
```

The marked state $|1010\rangle$ should dominate the histogram — appearing with probability close to 1, while the other 15 states appear rarely (only due to the discrete number of iterations not perfectly reaching $\pi/2$).

### Multiple Marked States

Grover's algorithm also works when there are multiple marked states. The only change is the iteration count: $t \approx \frac{\pi}{4}\sqrt{N/M}$ where $M$ is the number of marked states. More solutions means fewer iterations needed.

```{code-cell} python
# Grover's with 3 marked states
marked_multi = ["1010", "0110", "1100"]
oracle_multi = grover_oracle(marked_multi, n)
grover_op_multi = GroverOperator(oracle_multi)

optimal_iter_multi = math.floor(
    math.pi / (4 * math.asin(math.sqrt(len(marked_multi) / 2**n))) - 0.5)
print(f"Marked states: {marked_multi}")
print(f"Optimal iterations: {optimal_iter_multi}")

# Build and run
qc_multi = QuantumCircuit(n)
qc_multi.h(range(n))
for _ in range(optimal_iter_multi):
    qc_multi.compose(grover_op_multi, inplace=True)
qc_multi.measure_all()

qc_multi_t = transpile(qc_multi, simulator)
result_multi = simulator.run(qc_multi_t, shots=1024).result()
counts_multi = result_multi.get_counts()

plot_histogram(counts_multi, title=f"Grover's: 3 marked states out of 16")
```

```{admonition} The Oracle Problem
:class: note
You might wonder: if we have to *build* the oracle that marks the target state, don't we already know the answer? Good question — the Qiskit notebook in the course resources addresses this. In practice, Grover's algorithm is used in settings where: (1) two parties are involved — one constructs the oracle, the other searches; (2) the oracle encodes a *criterion* (like "find a state with Hamming weight 3") rather than a specific bitstring; or (3) Grover's is used as a subroutine inside a larger algorithm. See the [Grover's tutorial](../lecture_resources/grovers.ipynb) for worked examples of all three cases.
```

------------------------------------------------------------------------

## Summary

- **Unstructured search:** Given $N$ items and one marked item, find it. Classical cost: $O(N)$. Quantum cost: $O(\sqrt{N})$.

- **The Grover operator** $G = D \cdot Z_f$ consists of two parts: the **oracle** $Z_f$ (phase flip on marked states) and the **diffusion operator** $D = 2|s\rangle\langle s| - I$ (inversion about the mean).

- **Algorithm:** $H^{\otimes n}$ → repeat $G$ for $t \approx \frac{\pi}{4}\sqrt{N}$ iterations → measure.

- **Geometry:** The state lives in a 2D subspace. Each iteration rotates by $2\theta$ where $\sin\theta = 1/\sqrt{N}$. After $\sim \frac{\pi}{4}\sqrt{N}$ rotations, the state aligns with the marked state.

- **Overshoot:** Too many iterations rotates past the answer — probability oscillates. More computation can make the answer *worse*. You must stop at the right time.

- **The algorithm skeleton evolves:**
  - Deutsch-Jozsa: $H^{\otimes n}$ → oracle → $H^{\otimes n}$ → measure (single pass)
  - Grover's: $H^{\otimes n}$ → (oracle → diffusion)$^t$ → measure (iterated)

------------------------------------------------------------------------

## What's Next

We've now built three quantum algorithms — Deutsch-Jozsa, Bernstein-Vazirani, and Grover's — all running on abstract qubits and perfect gates. But what physical systems actually implement these operations? How do you build a Hadamard gate with microwave pulses? What does a CNOT look like when your qubits are trapped ions? And why does everything need to be cooled to 15 millikelvin?

Next lecture, we survey the quantum hardware platforms that make all of this real: superconducting qubits, trapped ions, neutral atoms, and photonics. We'll connect the abstract gate model back to the physical systems you've seen throughout this course — Rabi oscillations, Ramsey fringes, and beam splitters — and see how the engineering challenges of $T_1$, $T_2$, and gate fidelity constrain what we can actually compute today.

------------------------------------------------------------------------

## Homework

[📥 Download Homework: Grover's Algorithm (.ipynb)](../homework/download_04_06.html)
