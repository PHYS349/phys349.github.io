# Lecture 4.5 — The Deutsch-Jozsa Algorithm (OUTLINE)

## Overview

**Goal:** Introduce the oracle/query model and derive the first quantum algorithm with an exponential speedup over classical. Students build oracles, see phase kickback in action, and run Deutsch-Jozsa in Qiskit.

**Prerequisites from prior lectures:**
- Hadamard gate, CNOT, circuit model (4.1)
- Quantum half-adder, Toffoli gate (4.2)
- No-cloning, measurement collapse (4.1)
- Interference as the engine of quantum advantage (recurring theme since 1.3)
- CHSH as the template: define task → prove classical bound → beat it with QM (4.2)

**Homework:** Deutsch-Jozsa Qiskit notebook (from IBM tutorials, in `lecture_resources/`)

---

## Where We Left Off

Connect back to 4.4 (teleportation/superdense coding). We've seen entanglement as a *communication* resource. Now we shift to *computation*. The CHSH game (4.2) showed quantum advantage in a game; today we show it in an algorithm. Same template: define a problem, prove a classical lower bound, beat it with interference.

---

## Part 1: The Oracle Model (~10 min)

### Why black boxes?

- In the real world, we often interact with functions we can't inspect — APIs, database lookups, subroutines written by someone else.
- The **query model**: we're given access to a function $f$ only through a black box (oracle). We can query it, but each query costs us. Goal: minimize the number of queries.
- This is a *rigorous* framework for proving quantum speedups — we count queries, not gate operations.

### The quantum oracle

- Classical oracle: input $x$, output $f(x)$.
- Quantum oracle must be **reversible** (unitary). Standard construction:

$$U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$$

- The input register $|x\rangle$ passes through unchanged; the result is XOR'd into the target qubit $|y\rangle$.
- Connect to lecture 4.1/4.2: this is exactly the CNOT/Toffoli pattern — writing the answer into a target qubit to keep things reversible.

### iClicker

What does the oracle $U_f$ do to $|x\rangle|0\rangle$?

A) $|x\rangle|f(x)\rangle$ — yes, because $0 \oplus f(x) = f(x)$
B) $|f(x)\rangle|0\rangle$
C) $|x \oplus f(x)\rangle|0\rangle$
D) It depends on $f$

---

## Part 2: Phase Kickback — The Key Trick (~15 min)

### Setup

- What happens if the target qubit is $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$ instead of $|0\rangle$?

### Derivation

$$U_f|x\rangle|-\rangle = |x\rangle \otimes \frac{|0 \oplus f(x)\rangle - |1 \oplus f(x)\rangle}{\sqrt{2}}$$

- If $f(x) = 0$: target stays $|-\rangle$, overall state is $+|x\rangle|-\rangle$
- If $f(x) = 1$: target becomes $\frac{|1\rangle - |0\rangle}{\sqrt{2}} = -|-\rangle$, so overall state is $-|x\rangle|-\rangle$

$$\boxed{U_f|x\rangle|-\rangle = (-1)^{f(x)}|x\rangle|-\rangle}$$

- The function value has been **kicked back** into the phase of the input register. The target qubit is unchanged!
- This is remarkable: we learned something about $f(x)$ without changing the output register. The information is now encoded in a **global phase** on $|x\rangle$ — but since $|x\rangle$ can be in superposition, it becomes a *relative* phase between terms.

### Physical intuition

- Connect to interferometers (Chapter 2): a phase shift in one arm of a Mach-Zehnder interferometer changes the output probabilities. Phase kickback is the multi-qubit version of the same idea.
- The oracle is like a "phase plate" inside an interferometer — it imprints $f(x)$ as a phase, and the final Hadamard layer converts phase differences into measurable amplitude differences.

### iClicker

$U_f|+\rangle|-\rangle$ where $f(0)=0, f(1)=1$. What is the state of the first qubit (ignoring the target)?

A) $|+\rangle$
B) $|-\rangle$ ← correct: $\frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$
C) $|0\rangle$
D) $|1\rangle$

### Qiskit verification

- Build a simple 2-qubit circuit: prepare $|+\rangle|-\rangle$, apply $U_f$ (CNOT for $f(x)=x$), show the first qubit is now $|-\rangle$.
- Measure in the X-basis (apply H then measure Z) to distinguish $|+\rangle$ from $|-\rangle$.

---

## Part 3: Deutsch's Algorithm (n = 1) (~15 min)

### The problem

- $f: \{0,1\} \to \{0,1\}$. Four possible functions:
  - **Constant:** $f(0) = f(1) = 0$, or $f(0) = f(1) = 1$
  - **Balanced:** $f(0) = 0, f(1) = 1$, or $f(0) = 1, f(1) = 0$
- **Question:** Is $f$ constant or balanced?
- **Classical cost:** Must query $f$ twice (check both inputs). No way around it.

### The circuit

$$|0\rangle|1\rangle \xrightarrow{H \otimes H} |+\rangle|-\rangle \xrightarrow{U_f} (-1)^{f(0)} \left[\frac{|0\rangle + (-1)^{f(0) \oplus f(1)}|1\rangle}{\sqrt{2}}\right]|-\rangle \xrightarrow{H \otimes I} (-1)^{f(0)}|f(0) \oplus f(1)\rangle|-\rangle$$

### Full state evolution (step by step)

Walk through each step with explicit ket notation:
- $|\pi_0\rangle = |01\rangle$
- $|\pi_1\rangle = |+\rangle|-\rangle = \frac{1}{2}(|0\rangle + |1\rangle)(|0\rangle - |1\rangle)$
- $|\pi_2\rangle$ after oracle: use phase kickback result
- $|\pi_3\rangle$ after final Hadamard on first qubit

### Result

- Measure the first qubit:
  - $|0\rangle$ → constant ($f(0) \oplus f(1) = 0$)
  - $|1\rangle$ → balanced ($f(0) \oplus f(1) = 1$)
- **One query.** Classical needs two.

### iClicker

Deutsch's algorithm uses how many queries to the oracle?

A) 0 &emsp; B) 1 &emsp; C) 2 &emsp; D) It depends on $f$

### Qiskit implementation

- Build all four oracles explicitly as circuits.
- Run Deutsch's algorithm on each, verify correct classification.
- Show histograms.

---

## Part 4: Deutsch-Jozsa for n Qubits (~20 min)

### The generalized problem

- $f: \{0,1\}^n \to \{0,1\}$. Promised that $f$ is either:
  - **Constant:** same output for all $2^n$ inputs
  - **Balanced:** outputs 0 for exactly half the inputs and 1 for the other half
- **Classical cost:** In the worst case, must query $2^{n-1} + 1$ times (check just over half the inputs). Even if you see half 0's, you don't know it's balanced until you see one 1.
- **Quantum cost:** 1 query. Exponential speedup.

### The circuit

$$|0\rangle^{\otimes n}|1\rangle \xrightarrow{H^{\otimes (n+1)}} |+\rangle^{\otimes n}|-\rangle \xrightarrow{U_f} \sum_x \frac{(-1)^{f(x)}|x\rangle}{2^{n/2}} \otimes |-\rangle \xrightarrow{H^{\otimes n}} |\text{result}\rangle|-\rangle$$

### Derivation sketch

1. **After first Hadamard wall:** equal superposition over all $2^n$ inputs — the oracle sees *all inputs simultaneously*.
2. **After oracle:** phase kickback turns each $|x\rangle$ into $(-1)^{f(x)}|x\rangle$.
3. **After second Hadamard wall:** interference. Show that the amplitude of $|0\rangle^{\otimes n}$ is:

$$\langle 0^n | H^{\otimes n} \sum_x (-1)^{f(x)} |x\rangle = \frac{1}{2^n} \sum_x (-1)^{f(x)}$$

   - If $f$ is **constant**: all signs are the same → sum is $\pm 1$ → measure $|0\cdots 0\rangle$ with certainty.
   - If $f$ is **balanced**: half are $+1$, half are $-1$ → sum is 0 → probability of $|0\cdots 0\rangle$ is **zero**.

4. **Decision rule:** measure. If all zeros → constant. Anything else → balanced.

### The interference picture

- Draw the analogy explicitly: the first H wall is like splitting into $2^n$ paths of an interferometer. The oracle imprints a phase on each path. The second H wall recombines the paths. Constructive/destructive interference determines the output.
- This is the "quantum computing = engineering interference" theme from lecture 1.3, now fully realized in an algorithm.

### iClicker

For a 10-qubit Deutsch-Jozsa problem ($n=10$), how many queries does a classical computer need in the worst case?

A) 10 &emsp; B) 512 &emsp; C) 513 &emsp; D) 1024

(Answer: C — $2^{n-1} + 1 = 513$)

### iClicker

You run Deutsch-Jozsa and measure $|0010100\rangle$ (not all zeros). What do you conclude?

A) $f$ is constant &emsp; B) $f$ is balanced &emsp; C) Need more queries &emsp; D) The algorithm failed

(Answer: B)

### Qiskit implementation

- Build the full n-qubit circuit.
- Construct explicit constant and balanced oracles.
- Run on simulator, show the all-zeros vs. non-all-zeros histograms.
- *Connect to the IBM notebook*: this is what the homework builds on.

---

## Part 5: Bernstein-Vazirani as a Bonus (~10 min, if time)

### The problem

- Promise: $f(x) = s \cdot x = s_1 x_1 \oplus s_2 x_2 \oplus \cdots \oplus s_n x_n$ for some secret string $s$.
- **Classical cost:** $n$ queries (try each unit vector $e_i$ to learn one bit of $s$ at a time).
- **Quantum cost:** 1 query — same circuit as Deutsch-Jozsa!

### Why it works

- The oracle for $f(x) = s \cdot x$ is just CNOTs from each qubit $i$ where $s_i = 1$ to the target.
- Phase kickback gives $(-1)^{s \cdot x}$, and $H^{\otimes n}$ applied to this directly produces $|s\rangle$.
- This is a cleaner speedup story: $n$ queries → 1 query, and you learn the *actual answer*, not just a property.

### Qiskit implementation

- Students pick a secret string, build the oracle (just CNOTs), run the algorithm, read off $s$ from the measurement.
- Great in-class activity: partner picks a secret string, you run the algorithm to find it.

---

## Summary

- **Oracle model:** quantum algorithms access functions through reversible black boxes $U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$.

- **Phase kickback:** when the target is $|-\rangle$, the function value appears as a phase: $U_f|x\rangle|-\rangle = (-1)^{f(x)}|x\rangle|-\rangle$. This is the key trick.

- **Deutsch's algorithm:** determines if a 1-bit function is constant or balanced with 1 query (classical needs 2).

- **Deutsch-Jozsa:** generalizes to $n$ bits. Quantum: 1 query. Classical: $2^{n-1}+1$ queries. Exponential speedup via interference.

- **Bernstein-Vazirani:** finds a secret string $s$ in 1 query (classical needs $n$). Same circuit, different oracle.

- **The pattern:** $H^{\otimes n}$ (superpose) → oracle (imprint phases) → $H^{\otimes n}$ (interfere) → measure. This is the skeleton of all quantum algorithms.

---

## What's Next

Next lecture: **Grover's search algorithm** — the same oracle framework, but now with a *practical* problem: searching an unstructured database. Grover's doesn't give an exponential speedup, but the quadratic speedup $O(\sqrt{N})$ vs. $O(N)$ is achievable for *any* search problem. We'll build the oracle, derive the "inversion about the mean" trick, and see why too many iterations actually *hurts*.

**Homework:** Deutsch-Jozsa Qiskit notebook (IBM tutorial) — implement oracles, run the algorithm, and extend to Bernstein-Vazirani.
