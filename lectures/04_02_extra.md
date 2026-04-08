# Lecture 3.6: The Deutsch-Jozsa Algorithm

## Our First Quantum Algorithm

We've built up the machinery: qubits, gates, entanglement, measurement. Now we use it for computation.

The **Deutsch-Jozsa algorithm** was the first example of a problem where a quantum computer provably outperforms any classical computer. It's not a practically important problem — but it demonstrates the key principles that make quantum algorithms work.

### Historical Context

The story begins with David Deutsch in 1985, asking: "Can quantum computers do anything classical computers cannot?" He constructed a simple 1-bit problem where a quantum computer needs only 1 query while any classical computer needs 2.

In 1992, Deutsch and Richard Jozsa generalized this to $n$ bits, achieving an **exponential** separation: 1 quantum query versus $O(2^n)$ classical queries.

This was revolutionary. Before Deutsch-Jozsa, it wasn't clear that quantum computers offered any fundamental advantage. The algorithm proved they did — at least for some problems.

The techniques introduced here — superposition, phase kickback, interference — became the foundation for all subsequent quantum algorithms:
- **Simon's algorithm** (1994): Exponential speedup for finding hidden structure
- **Shor's algorithm** (1994): Exponential speedup for factoring (breaks RSA!)
- **Grover's algorithm** (1996): Quadratic speedup for search

Deutsch-Jozsa is the "Hello, World!" of quantum algorithms. Master it, and you'll understand the core of quantum computation.

By the end of this lecture, you'll understand:
- The **oracle model** (black-box functions)
- **Quantum parallelism** (evaluating a function on all inputs at once)
- **Interference** (making the answer emerge from cancellation patterns)
- Why quantum computers can sometimes do exponentially better than classical ones

---

## The Oracle Model

### What's an Oracle?

In algorithm analysis, an **oracle** is a black box that computes some function $f(x)$.

You can query the oracle:
- Input: $x$
- Output: $f(x)$

You don't know how the oracle works internally. You only learn about $f$ by querying it.

### Why Oracles?

The oracle model lets us isolate the "hard part" of a problem. Instead of asking "how many operations does this algorithm take?" we ask "how many queries to the oracle does this algorithm need?"

This is useful when:
- The function $f$ is expensive to compute
- We want to compare quantum vs classical query complexity
- We're studying the fundamental power of quantum computation

### Classical vs Quantum Oracles

**Classical oracle:** Given input $x$, returns $f(x)$.

**Quantum oracle:** Must be reversible (all quantum operations are unitary). We use the standard construction:

$$
U_f: |x\rangle|y\rangle \mapsto |x\rangle|y \oplus f(x)\rangle
$$

where $\oplus$ is XOR (addition mod 2).

The input $x$ is preserved, and the output $f(x)$ is XORed into an ancilla register.

```{figure} ./03_06_lecture_files/quantum_oracle.svg
:width: 300px
:name: quantum-oracle

Quantum oracle: $|x\rangle|y\rangle \to |x\rangle|y \oplus f(x)\rangle$
```

### Phase Kickback

Here's a crucial trick. What if we set the ancilla to $|{-}\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$?

$$
U_f|x\rangle|{-}\rangle = |x\rangle \cdot \frac{|0 \oplus f(x)\rangle - |1 \oplus f(x)\rangle}{\sqrt{2}}
$$

If $f(x) = 0$:
$$
|x\rangle \cdot \frac{|0\rangle - |1\rangle}{\sqrt{2}} = |x\rangle|{-}\rangle
$$

If $f(x) = 1$:
$$
|x\rangle \cdot \frac{|1\rangle - |0\rangle}{\sqrt{2}} = -|x\rangle|{-}\rangle
$$

In both cases, the ancilla stays as $|{-}\rangle$, but the input state picks up a phase!

$$
U_f|x\rangle|{-}\rangle = (-1)^{f(x)}|x\rangle|{-}\rangle
$$

```{admonition} Phase Kickback
:class: important

When the ancilla is in state $|{-}\rangle$, the quantum oracle acts as:

$$
U_f|x\rangle|{-}\rangle = (-1)^{f(x)}|x\rangle|{-}\rangle
$$

The function value $f(x)$ becomes a **phase** on the input state. This is called **phase kickback** and is essential for quantum algorithms.
```

---

## Deutsch's Problem (1 Qubit)

Let's start with the simplest case: a function on a single bit.

### The Problem

You're given an oracle for a function $f: \{0, 1\} \to \{0, 1\}$.

There are four possible functions:

| $x$ | $f_1(x)$ | $f_2(x)$ | $f_3(x)$ | $f_4(x)$ |
|-----|----------|----------|----------|----------|
| 0 | 0 | 1 | 0 | 1 |
| 1 | 0 | 1 | 1 | 0 |
| Type | constant | constant | balanced | balanced |

- **Constant:** $f(0) = f(1)$ (same output for both inputs)
- **Balanced:** $f(0) \neq f(1)$ (different outputs)

**The question:** Is $f$ constant or balanced?

### Classical Solution

Classically, you must query both inputs:
1. Query $f(0)$
2. Query $f(1)$
3. Compare: if equal, constant; if different, balanced

**Classical query complexity: 2**

There's no way to do better — with only one query, you can't distinguish constant from balanced.

### Quantum Solution: Deutsch's Algorithm

Quantum mechanics lets us answer with **just 1 query**!

**Circuit:**

```{figure} ./03_06_lecture_files/deutsch_circuit.svg
:width: 400px
:name: deutsch-circuit

Deutsch's algorithm: H gates, oracle query, H gate, measure.
```

**Step 0: Initialize**
$$
|0\rangle|1\rangle
$$

**Step 1: Apply H to both qubits**
$$
H|0\rangle \otimes H|1\rangle = |{+}\rangle|{-}\rangle = \frac{1}{2}(|0\rangle + |1\rangle)(|0\rangle - |1\rangle)
$$

**Step 2: Apply the oracle**

Using phase kickback:
$$
U_f|{+}\rangle|{-}\rangle = \frac{1}{\sqrt{2}}\big((-1)^{f(0)}|0\rangle + (-1)^{f(1)}|1\rangle\big)|{-}\rangle
$$

Let's factor out and simplify:
$$
= \frac{(-1)^{f(0)}}{\sqrt{2}}\big(|0\rangle + (-1)^{f(0) \oplus f(1)}|1\rangle\big)|{-}\rangle
$$

The global phase $(-1)^{f(0)}$ doesn't matter. What matters is the relative phase $(-1)^{f(0) \oplus f(1)}$:

- If $f$ is **constant**: $f(0) \oplus f(1) = 0$, so we have $\frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) = |{+}\rangle$
- If $f$ is **balanced**: $f(0) \oplus f(1) = 1$, so we have $\frac{1}{\sqrt{2}}(|0\rangle - |1\rangle) = |{-}\rangle$

**Step 3: Apply H to the first qubit**

$$
H|{+}\rangle = |0\rangle, \quad H|{-}\rangle = |1\rangle
$$

**Step 4: Measure the first qubit**

- Result $|0\rangle$: $f$ is constant
- Result $|1\rangle$: $f$ is balanced

### Summary: Deutsch's Algorithm

| Step | State (ignoring ancilla) |
|------|--------------------------|
| Initialize | $\|0\rangle$ |
| After H | $\|{+}\rangle = \frac{1}{\sqrt{2}}(\|0\rangle + \|1\rangle)$ |
| After oracle | $\frac{1}{\sqrt{2}}(\|0\rangle + (-1)^{f(0) \oplus f(1)}\|1\rangle)$ |
| After H | $\|0\rangle$ if constant, $\|1\rangle$ if balanced |

**Quantum query complexity: 1**

We've achieved a 2× speedup: 1 query instead of 2.

Not very impressive yet. But the real power comes when we scale up.

---

## The Deutsch-Jozsa Problem (n Qubits)

Now let's generalize to $n$-bit inputs.

### The Problem

You're given an oracle for a function $f: \{0, 1\}^n \to \{0, 1\}$.

You're **promised** that $f$ is either:
- **Constant:** $f(x) = 0$ for all $x$, or $f(x) = 1$ for all $x$
- **Balanced:** $f(x) = 0$ for exactly half the inputs, and $f(x) = 1$ for the other half

**The question:** Is $f$ constant or balanced?

```{admonition} The Promise is Essential
:class: warning

The algorithm **only works** when the promise is satisfied. If $f$ is neither constant nor balanced (say, 60% zeros and 40% ones), the algorithm gives unreliable results.

This is a **promise problem**: we're guaranteed the input has a special structure, and we exploit that structure. Without the promise, the problem becomes much harder.
```

### Classical Solution

In the worst case, you might need to query just over half the inputs:
- Query $f(x)$ for $2^{n-1} + 1$ different values of $x$
- If all outputs are the same, $f$ must be constant
- If you see both 0 and 1, $f$ is balanced

**Classical query complexity: $O(2^n)$** (exponential in $n$)

Actually, with high probability, you can detect balanced functions much faster (after ~$\sqrt{2^n}$ queries, you're very likely to see both outputs). But in the **worst case**, you need exponentially many queries.

### The Hadamard Transform on n Qubits

Before diving into the algorithm, let's understand the key mathematical tool: the $n$-qubit Hadamard transform.

**Single qubit Hadamard:**
$$
H|0\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) = |+\rangle
$$
$$
H|1\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle) = |-\rangle
$$

We can write this compactly as:
$$
H|x\rangle = \frac{1}{\sqrt{2}}\sum_{z \in \{0,1\}} (-1)^{xz}|z\rangle
$$

where $x, z \in \{0, 1\}$ and $xz$ is ordinary multiplication.

**Two qubit Hadamard (worked example):**
$$
H^{\otimes 2}|00\rangle = H|0\rangle \otimes H|0\rangle = |+\rangle|+\rangle = \frac{1}{2}(|00\rangle + |01\rangle + |10\rangle + |11\rangle)
$$

$$
H^{\otimes 2}|01\rangle = |+\rangle|-\rangle = \frac{1}{2}(|00\rangle - |01\rangle + |10\rangle - |11\rangle)
$$

$$
H^{\otimes 2}|10\rangle = |-\rangle|+\rangle = \frac{1}{2}(|00\rangle + |01\rangle - |10\rangle - |11\rangle)
$$

$$
H^{\otimes 2}|11\rangle = |-\rangle|-\rangle = \frac{1}{2}(|00\rangle - |01\rangle - |10\rangle + |11\rangle)
$$

Notice the pattern of signs! For $H^{\otimes 2}|xy\rangle$, the coefficient of $|ab\rangle$ is $\frac{1}{2}(-1)^{xa + yb}$.

**General n-qubit Hadamard:**
$$
H^{\otimes n}|x\rangle = \frac{1}{\sqrt{2^n}}\sum_{z=0}^{2^n-1} (-1)^{x \cdot z}|z\rangle
$$

where $x \cdot z = x_1 z_1 \oplus x_2 z_2 \oplus \cdots \oplus x_n z_n$ is the **bitwise inner product** (XOR of the AND of corresponding bits).

```{admonition} The Hadamard Transform as Quantum Fourier Transform
:class: note

The $n$-qubit Hadamard transform is the **Quantum Fourier Transform over $\mathbb{Z}_2^n$** — the group of $n$-bit strings under XOR.

In this context:
- The basis states $|x\rangle$ correspond to group elements
- The phases $(-1)^{x \cdot z}$ are the **characters** of the group
- The transform converts between "position" and "frequency" representations

This is why the Hadamard transform is so powerful: it's a Fourier transform, and Fourier transforms are nature's way of revealing hidden structure.

The more general Quantum Fourier Transform over $\mathbb{Z}_N$ (used in Shor's algorithm) is a direct generalization of this idea.
```

**Key property:** The Hadamard transform is its own inverse: $H^{\otimes n} H^{\otimes n} = I$.

This means:
- $H^{\otimes n}|0\rangle^{\otimes n}$ creates a uniform superposition
- Applying $H^{\otimes n}$ again returns to $|0\rangle^{\otimes n}$

But if we modify the phases in between (via the oracle), the final state changes!

### Quantum Solution: The Deutsch-Jozsa Algorithm

The quantum algorithm solves this with **just 1 query**, regardless of $n$!

**Circuit:**

```{figure} ./03_06_lecture_files/deutsch_jozsa_circuit.svg
:width: 500px
:name: dj-circuit

Deutsch-Jozsa algorithm for $n$ qubits.
```

**Step 0: Initialize**
$$
|0\rangle^{\otimes n}|1\rangle
$$

That's $n$ qubits in $|0\rangle$ (the input register) and 1 qubit in $|1\rangle$ (the ancilla).

**Step 1: Apply H to all qubits**
$$
H^{\otimes n}|0\rangle^{\otimes n} \otimes H|1\rangle = \frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1}|x\rangle \otimes |{-}\rangle
$$

The input register is now in an **equal superposition** of all $2^n$ possible inputs!

**Step 2: Apply the oracle**

Using phase kickback on each term:
$$
U_f \left(\frac{1}{\sqrt{2^n}}\sum_x |x\rangle\right)|{-}\rangle = \frac{1}{\sqrt{2^n}}\sum_x (-1)^{f(x)}|x\rangle \otimes |{-}\rangle
$$

The function value $f(x)$ is now encoded as a **phase** on each basis state.

**Step 3: Apply H to the input register**

This is the key step. We apply $H^{\otimes n}$ to the input register.

Recall that:
$$
H|x\rangle = \frac{1}{\sqrt{2}}\sum_{z \in \{0,1\}} (-1)^{xz}|z\rangle
$$

For $n$ qubits:
$$
H^{\otimes n}|x\rangle = \frac{1}{\sqrt{2^n}}\sum_{z=0}^{2^n-1} (-1)^{x \cdot z}|z\rangle
$$

where $x \cdot z = x_1 z_1 \oplus x_2 z_2 \oplus \cdots \oplus x_n z_n$ is the bitwise inner product.

Applying this to our state:
$$
H^{\otimes n}\left(\frac{1}{\sqrt{2^n}}\sum_x (-1)^{f(x)}|x\rangle\right) = \frac{1}{2^n}\sum_x \sum_z (-1)^{f(x) + x \cdot z}|z\rangle
$$

Rearranging:
$$
= \sum_z \left(\frac{1}{2^n}\sum_x (-1)^{f(x) + x \cdot z}\right)|z\rangle
$$

The amplitude of $|z\rangle$ is:
$$
a_z = \frac{1}{2^n}\sum_x (-1)^{f(x) + x \cdot z}
$$

**Step 4: Measure**

What's the amplitude of $|0\rangle^{\otimes n}$ (all zeros)?

$$
a_0 = \frac{1}{2^n}\sum_x (-1)^{f(x)}
$$

**If $f$ is constant:**
- All $(-1)^{f(x)}$ terms are the same (either all $+1$ or all $-1$)
- $a_0 = \frac{1}{2^n} \cdot 2^n \cdot (\pm 1) = \pm 1$
- Probability of measuring $|0\rangle^{\otimes n}$ is $|a_0|^2 = 1$

**If $f$ is balanced:**
- Half the terms are $+1$, half are $-1$
- $a_0 = \frac{1}{2^n}(2^{n-1} - 2^{n-1}) = 0$
- Probability of measuring $|0\rangle^{\otimes n}$ is $0$

### The Result

| Measurement | Conclusion |
|-------------|------------|
| All zeros ($\|0\rangle^{\otimes n}$) | $f$ is constant |
| Anything else | $f$ is balanced |

**Quantum query complexity: 1**

We've achieved an **exponential speedup**: 1 query instead of $O(2^n)$!

### What Do You Measure When f is Balanced?

If $f$ is constant, you always measure $|0\rangle^{\otimes n}$. But if $f$ is balanced, what do you actually measure?

The answer depends on the **structure** of $f$. Let's work out a specific example.

**Example: The parity function**

Suppose $f(x) = x_1 \oplus x_2 \oplus \cdots \oplus x_n$ (XOR of all bits). This is balanced: half the $n$-bit strings have even parity, half have odd.

After the oracle, the state is:
$$
\frac{1}{\sqrt{2^n}}\sum_x (-1)^{x_1 \oplus x_2 \oplus \cdots \oplus x_n}|x\rangle
$$

After the final Hadamard, the amplitude of $|z\rangle$ is:
$$
a_z = \frac{1}{2^n}\sum_x (-1)^{f(x) + x \cdot z} = \frac{1}{2^n}\sum_x (-1)^{(x_1 \oplus \cdots \oplus x_n) + (x_1 z_1 \oplus \cdots \oplus x_n z_n)}
$$

This simplifies (after some algebra) to:
- $a_z = \pm 1$ if $z = 11\ldots1$ (all ones)
- $a_z = 0$ otherwise

So we measure $|11\ldots1\rangle$ with certainty!

**The general pattern:** For functions of the form $f(x) = s \cdot x$ (inner product with a secret string $s$), we measure exactly $|s\rangle$. This is the **Bernstein-Vazirani algorithm**.

For other balanced functions, we measure some nonzero string that reflects the structure of $f$. The key point is: **we never measure all zeros**, which is how we know $f$ is balanced.

```{admonition} Deutsch-Jozsa: The Bottom Line
:class: important

The Deutsch-Jozsa algorithm determines whether an $n$-bit function is constant or balanced using **1 quantum query**.

Classical algorithms need up to $2^{n-1}+1$ queries in the worst case.

This is an **exponential speedup** — the first provable quantum advantage!
```

---

## How Does It Work? The Key Ideas

Let's understand the algorithm at a deeper level.

### Idea 1: Quantum Parallelism

The Hadamard transform creates a superposition of all inputs:
$$
H^{\otimes n}|0\rangle^{\otimes n} = \frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1}|x\rangle
$$

When we apply the oracle, it acts on **all $2^n$ inputs simultaneously**:
$$
U_f \to \frac{1}{\sqrt{2^n}}\sum_x (-1)^{f(x)}|x\rangle
$$

In one oracle call, we've "evaluated" $f$ on all inputs!

But wait — we can't just read out all the $f(x)$ values. Measurement would give us just one random $x$ and its $f(x)$. The information is encoded in **phases**, which we can't directly observe.

### Idea 2: Phase Kickback

The oracle doesn't output $f(x)$ in a register we can read. Instead, phase kickback encodes $f(x)$ as a **phase** on $|x\rangle$:
$$
|x\rangle \to (-1)^{f(x)}|x\rangle
$$

Phases are not directly measurable, but they affect **interference patterns**.

### Idea 3: Interference — The Geometric Picture

This is the heart of the algorithm. Let's visualize what happens.

**Think of amplitudes as arrows (vectors in the complex plane):**
- Each basis state $|x\rangle$ has an amplitude (a complex number)
- Before the oracle: all amplitudes are $+\frac{1}{\sqrt{2^n}}$ (arrows pointing right)
- After the oracle: amplitudes are $\pm\frac{1}{\sqrt{2^n}}$ (arrows pointing right or left)

**The final Hadamard causes interference:**

The amplitude of $|0\rangle^{\otimes n}$ after the final Hadamard is:
$$
a_0 = \frac{1}{2^n}\sum_x (-1)^{f(x)}
$$

This is a sum of $2^n$ terms, each equal to $+\frac{1}{2^n}$ or $-\frac{1}{2^n}$.

```{figure} ./03_06_lecture_files/interference_diagram.svg
:width: 500px
:name: interference-diagram

Interference in Deutsch-Jozsa: amplitudes add constructively (constant) or cancel (balanced).
```

**Constant function:** All arrows point the same direction.
$$
a_0 = \frac{1}{2^n} \cdot 2^n \cdot (+1) = +1 \quad \text{or} \quad a_0 = \frac{1}{2^n} \cdot 2^n \cdot (-1) = -1
$$

All $2^n$ arrows add up → **constructive interference** → $|a_0|^2 = 1$.

**Balanced function:** Half the arrows point right, half point left.
$$
a_0 = \frac{1}{2^n}(2^{n-1} \cdot (+1) + 2^{n-1} \cdot (-1)) = 0
$$

The arrows cancel in pairs → **destructive interference** → $|a_0|^2 = 0$.

**This is the quantum magic:** The Hadamard transform "focuses" all the phase information into a single amplitude. The global property (constant vs balanced) determines whether the arrows add up or cancel.

### Idea 4: The Algorithm Extracts Global Information

Here's a key insight: the algorithm doesn't tell us $f(x)$ for any particular $x$. It tells us a **global property** of $f$ (whether all values are the same).

This is typical of quantum algorithms:
- Classical algorithms: compute $f(x)$ for specific inputs
- Quantum algorithms: extract global/structural information about $f$

The art of quantum algorithm design is finding problems where:
1. The desired answer is a global property
2. That property can be made to control interference patterns

```{admonition} The Quantum Algorithm Recipe
:class: important

Most quantum algorithms follow this pattern:

1. **Superposition:** Put the input register in a uniform superposition of all values
2. **Oracle/Phase encoding:** Apply the function, encoding results as phases via kickback
3. **Interference:** Apply a transform (often Hadamard or QFT) so the desired answer constructively interferes
4. **Measurement:** Read out the answer

The art is designing step 3 so that the correct answer has amplitude close to 1.

This recipe appears in Deutsch-Jozsa, Simon, Grover, Shor, and most other quantum algorithms.
```

---

## Why Is This Problem "Easy"?

You might object: the Deutsch-Jozsa problem is artificial. Who cares about constant vs balanced functions?

You're right! This is a **contrived problem**. It was designed to demonstrate quantum speedup, not to solve a real-world task.

But the principles it demonstrates — superposition, phase kickback, interference — are the same principles that power actually useful algorithms like Grover's (searching) and Shor's (factoring).

Think of Deutsch-Jozsa as the "Hello, World!" of quantum algorithms. It's not useful by itself, but it teaches you the fundamental techniques.

---

## Deterministic vs Probabilistic

One notable feature of Deutsch-Jozsa: it's **deterministic**.

- The answer is always correct (100% probability)
- No need to repeat and take a majority vote

This is unusual! Most quantum algorithms are probabilistic — they give the right answer with high probability, but not certainty.

However, Deutsch-Jozsa's determinism comes from the promise that $f$ is either constant or balanced. If $f$ could be "almost balanced" (say, 51% zeros and 49% ones), the algorithm would fail.

---

## The Bernstein-Vazirani Algorithm

The Bernstein-Vazirani problem is closely related to Deutsch-Jozsa, but instead of asking "constant or balanced?", it asks "what is the hidden structure?"

### The Problem

You're given an oracle for $f(x) = s \cdot x$ where $s$ is an unknown $n$-bit string and $s \cdot x$ is the bitwise inner product mod 2.

**Goal:** Find $s$.

### Classical Solution

Query $f$ on the $n$ standard basis vectors:
- $f(100...0) = s_1$
- $f(010...0) = s_2$
- ...
- $f(000...1) = s_n$

**Classical query complexity: $n$**

### Quantum Solution

Use the exact same circuit as Deutsch-Jozsa!

**Why it works:** After the algorithm, the amplitude of $|z\rangle$ is:
$$
a_z = \frac{1}{2^n}\sum_x (-1)^{s \cdot x + x \cdot z} = \frac{1}{2^n}\sum_x (-1)^{x \cdot (s \oplus z)}
$$

If $z = s$, then $s \oplus z = 0$, so $x \cdot (s \oplus z) = 0$ for all $x$:
$$
a_s = \frac{1}{2^n}\sum_x (-1)^0 = \frac{1}{2^n} \cdot 2^n = 1
$$

If $z \neq s$, the sum has equal positive and negative terms (this is a Fourier orthogonality relation), so $a_z = 0$.

**Result:** We measure $|s\rangle$ with probability 1!

**Quantum query complexity: 1**

An $n$× speedup, though not exponential (since classical is already $O(n)$, not $O(2^n)$).

---

## Simon's Algorithm: Bridge to Shor

Simon's algorithm (1994) represents a crucial stepping stone between Deutsch-Jozsa and Shor's factoring algorithm.

### The Problem

You're given an oracle for a function $f: \{0,1\}^n \to \{0,1\}^n$ with the following promise:

There exists a secret string $s$ such that:
$$
f(x) = f(y) \iff x \oplus y \in \{0^n, s\}
$$

In other words, $f$ is 2-to-1 with a hidden XOR period $s$ (unless $s = 0^n$, in which case $f$ is 1-to-1).

**Goal:** Find $s$.

### Why It Matters

**Classical:** Any algorithm needs $\Omega(2^{n/2})$ queries (birthday paradox argument).

**Quantum:** Simon's algorithm finds $s$ with $O(n)$ queries — an **exponential speedup**!

### How It Works (Sketch)

The algorithm is similar to Deutsch-Jozsa:
1. Prepare superposition: $\frac{1}{\sqrt{2^n}}\sum_x |x\rangle|0\rangle$
2. Apply oracle: $\frac{1}{\sqrt{2^n}}\sum_x |x\rangle|f(x)\rangle$
3. Measure the second register (getting some value $f(x_0)$)
4. The first register collapses to $\frac{1}{\sqrt{2}}(|x_0\rangle + |x_0 \oplus s\rangle)$
5. Apply Hadamard to the first register
6. Measure, getting some $z$ with $z \cdot s = 0$

Each run gives a random $z$ orthogonal to $s$. After $O(n)$ runs, we have enough equations to solve for $s$ via Gaussian elimination.

### Connection to Shor

Simon's algorithm was the direct inspiration for Shor's factoring algorithm:

| Aspect | Simon | Shor |
|--------|-------|------|
| Group | $\mathbb{Z}_2^n$ (XOR) | $\mathbb{Z}_N$ (addition mod N) |
| Hidden structure | XOR period $s$ | Multiplicative period $r$ |
| Transform | Hadamard = QFT over $\mathbb{Z}_2^n$ | QFT over $\mathbb{Z}_N$ |
| Speedup | Exponential | Exponential |

Shor essentially asked: "Can we do Simon's algorithm over a different group?" The answer was yes, and it broke RSA.

```{admonition} The Quantum Algorithms Family Tree
:class: note

```
Deutsch (1985)
    │
    ├── Deutsch-Jozsa (1992): exponential speedup for promise problem
    │       │
    │       └── Bernstein-Vazirani (1993): finding hidden linear structure
    │               │
    │               └── Simon (1994): finding hidden XOR period
    │                       │
    │                       └── Shor (1994): finding hidden multiplicative period → FACTORING
    │
    └── Grover (1996): quadratic speedup for unstructured search
```

Each algorithm builds on the insights of its predecessors!
```

---

## Building Oracles: From Abstract to Concrete

So far, we've treated oracles as black boxes. But in practice, oracles must be built from quantum gates. Let's see how.

### The Standard Oracle Construction

For a function $f: \{0,1\}^n \to \{0,1\}$, the quantum oracle implements:
$$
U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle
$$

This XORs $f(x)$ into the ancilla register, leaving the input unchanged.

### Example: Inner Product Oracle

For $f(x) = s \cdot x = s_1 x_1 \oplus s_2 x_2 \oplus \cdots \oplus s_n x_n$, the oracle is remarkably simple:

**Circuit:** For each bit $i$ where $s_i = 1$, apply CNOT from qubit $i$ to the ancilla.

```{figure} ./03_06_lecture_files/inner_product_oracle.svg
:width: 350px
:name: inner-product-oracle

Oracle for $f(x) = s \cdot x$ with $s = 101$: CNOTs from qubits 0 and 2 to ancilla.
```

**Why it works:** Each CNOT computes $\text{ancilla} \gets \text{ancilla} \oplus x_i$ when $s_i = 1$. The cumulative effect is:
$$
\text{ancilla} \gets \text{ancilla} \oplus (s_1 x_1 \oplus s_2 x_2 \oplus \cdots \oplus s_n x_n) = \text{ancilla} \oplus f(x)
$$

**Gate count:** $O(n)$ CNOTs (at most one per bit of $s$).

### Example: Constant Oracle

For $f(x) = c$ (constant):
- If $c = 0$: do nothing (identity gate)
- If $c = 1$: apply X to the ancilla

**Gate count:** $O(1)$ (just one X gate or nothing).

### Example: Parity Oracle

For $f(x) = x_1 \oplus x_2 \oplus \cdots \oplus x_n$ (parity of all bits):

**Circuit:** CNOT from every input qubit to the ancilla.

**Gate count:** $O(n)$ CNOTs.

### More Complex Oracles

For more complex functions, oracle construction can be tricky:

**AND function:** $f(x_1, x_2) = x_1 \land x_2$

This requires a **Toffoli gate** (controlled-controlled-NOT):
$$
\text{Toffoli}|x_1\rangle|x_2\rangle|y\rangle = |x_1\rangle|x_2\rangle|y \oplus (x_1 \land x_2)\rangle
$$

The Toffoli flips the target only if both controls are 1.

**General Boolean functions:** Any Boolean function can be implemented as a reversible circuit using:
- NOT gates
- CNOT gates  
- Toffoli gates

But the circuit may be large! A function with no special structure might require $O(2^n)$ gates.

---

## Query Complexity vs Gate Complexity

The oracle model focuses on **query complexity**: how many times must we call the oracle?

But there's another measure: **gate complexity**: how many elementary gates in total?

### The Distinction

| Complexity | Deutsch-Jozsa Quantum | Deutsch-Jozsa Classical |
|------------|----------------------|-------------------------|
| Query complexity | 1 | $O(2^n)$ |
| Total gate complexity | 1 oracle + $O(n)$ Hadamards | $O(2^n)$ oracle calls |

### Why It Matters

The oracle itself may require many gates to implement:
- Simple oracles (inner product): $O(n)$ gates
- Complex oracles (arbitrary function): up to $O(2^n)$ gates

If the oracle requires $O(2^n)$ gates, then even with 1 query, the total gate complexity is $O(2^n)$ — no better than classical!

### When Is the Quantum Speedup Real?

The speedup is meaningful when:
1. The oracle is "cheap" (polynomial gates), OR
2. The oracle is truly a black box (we can't look inside)

**Example where speedup is real:** The function is computed by a small circuit, but we don't know the circuit's structure. We only have query access.

**Example where speedup is illusory:** The oracle for a random function requires $O(2^n)$ gates. The "1 query" speedup is meaningless because implementing that 1 query costs $O(2^n)$ gates.

```{admonition} Query Complexity: A Useful Abstraction
:class: note

Query complexity is still valuable because:
1. It's a clean theoretical model for comparing quantum and classical
2. Many real problems have structured oracles (polynomial-sized circuits)
3. It reveals fundamental limits: if quantum can't beat classical in queries, it often can't beat classical at all

But always remember: **query complexity is a lower bound**, not the full story. Total gate complexity matters too.
```

---

## Lab: Implementing Deutsch-Jozsa

Let's implement the algorithm in Qiskit.

### Setup

```{code-block} python
import numpy as np
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator

simulator = AerSimulator()
```

### Creating Oracles

```{code-block} python
def constant_oracle(n, value=0):
    """
    Create an oracle for a constant function.
    f(x) = value for all x.
    """
    qc = QuantumCircuit(n + 1)
    if value == 1:
        qc.x(n)  # Flip ancilla if f(x) = 1
    # If value == 0, do nothing
    return qc.to_gate(label=f'f=const({value})')

def balanced_oracle(n, pattern=None):
    """
    Create an oracle for a balanced function.
    Uses a pattern to determine which inputs give 1.
    Default: f(x) = x · (111...1) = parity of x
    """
    qc = QuantumCircuit(n + 1)
    if pattern is None:
        pattern = [1] * n  # Default: all 1s (parity function)
    
    # f(x) = x · pattern = XOR of x_i where pattern_i = 1
    # Implementation: CNOT from each x_i (where pattern_i = 1) to ancilla
    for i in range(n):
        if pattern[i] == 1:
            qc.cx(i, n)
    
    return qc.to_gate(label='f=balanced')

def random_balanced_oracle(n):
    """Create a random balanced oracle"""
    # Use inner product with a random non-zero string
    pattern = [np.random.randint(0, 2) for _ in range(n)]
    while sum(pattern) == 0:  # Ensure at least one 1
        pattern = [np.random.randint(0, 2) for _ in range(n)]
    return balanced_oracle(n, pattern), pattern
```

### The Deutsch-Jozsa Circuit

```{code-block} python
def deutsch_jozsa(oracle, n):
    """
    Construct the Deutsch-Jozsa circuit.
    oracle: the quantum oracle gate
    n: number of input qubits
    """
    qc = QuantumCircuit(n + 1, n)
    
    # Initialize ancilla to |1⟩
    qc.x(n)
    
    # Apply H to all qubits
    qc.h(range(n + 1))
    qc.barrier()
    
    # Apply the oracle
    qc.append(oracle, range(n + 1))
    qc.barrier()
    
    # Apply H to input qubits (not ancilla)
    qc.h(range(n))
    
    # Measure input qubits
    qc.measure(range(n), range(n))
    
    return qc
```

### Testing the Algorithm

```{code-block} python
def test_deutsch_jozsa():
    """Test Deutsch-Jozsa on various oracles"""
    print("Deutsch-Jozsa Algorithm Test")
    print("="*50)
    
    for n in [3, 5, 8]:
        print(f"\nn = {n} qubits ({2**n} possible inputs)")
        print("-"*40)
        
        # Test constant oracles
        for value in [0, 1]:
            oracle = constant_oracle(n, value)
            qc = deutsch_jozsa(oracle, n)
            
            result = simulator.run(qc, shots=1000).result()
            counts = result.get_counts()
            
            # Check if we got all zeros
            all_zeros = '0' * n
            is_constant = all_zeros in counts and counts[all_zeros] > 990
            
            print(f"  Constant ({value}): measured {list(counts.keys())[0]} → ", end='')
            print("CONSTANT ✓" if is_constant else "ERROR")
        
        # Test balanced oracle
        oracle, pattern = random_balanced_oracle(n)
        qc = deutsch_jozsa(oracle, n)
        
        result = simulator.run(qc, shots=1000).result()
        counts = result.get_counts()
        
        all_zeros = '0' * n
        is_balanced = all_zeros not in counts or counts.get(all_zeros, 0) < 10
        
        print(f"  Balanced (pattern {pattern}): measured {list(counts.keys())[0]} → ", end='')
        print("BALANCED ✓" if is_balanced else "ERROR")

test_deutsch_jozsa()
```

### Visualizing the State Evolution

```{code-block} python
def visualize_dj_states(n=2):
    """Visualize the state at each step of Deutsch-Jozsa"""
    from qiskit.quantum_info import Statevector
    
    print(f"\nState Evolution for n={n}")
    print("="*50)
    
    # Create a balanced oracle (parity function)
    def get_state_after_steps(steps):
        qc = QuantumCircuit(n + 1)
        qc.x(n)  # Ancilla to |1⟩
        if steps >= 1:
            qc.h(range(n + 1))  # Hadamard all
        if steps >= 2:
            # Oracle: parity function
            for i in range(n):
                qc.cx(i, n)
        if steps >= 3:
            qc.h(range(n))  # Hadamard input qubits
        return Statevector(qc)
    
    step_names = [
        "Initial: |0...0⟩|1⟩",
        "After H⊗(n+1): superposition",
        "After Oracle: phases applied",
        "After H⊗n: interference"
    ]
    
    for step in range(4):
        sv = get_state_after_steps(step)
        print(f"\nStep {step}: {step_names[step]}")
        
        # Show amplitudes for first few basis states
        for i, amp in enumerate(sv[:min(8, len(sv))]):
            if abs(amp) > 1e-10:
                bits = format(i, f'0{n+1}b')
                print(f"  |{bits}⟩: {amp:.4f}")
        if len(sv) > 8:
            print("  ...")

visualize_dj_states(2)
```

### Comparing Query Complexity

```{code-block} python
def compare_complexity():
    """Compare quantum vs classical query complexity"""
    print("\nQuery Complexity Comparison")
    print("="*50)
    print(f"{'n':<6} {'Classical (worst)':<20} {'Quantum':<10} {'Speedup':<15}")
    print("-"*50)
    
    for n in range(1, 11):
        classical = 2**(n-1) + 1
        quantum = 1
        speedup = classical / quantum
        print(f"{n:<6} {classical:<20} {quantum:<10} {speedup:<15.0f}×")
    
    print("\nNote: Classical worst-case. Quantum is always 1 query!")

compare_complexity()
```

---

## Limitations and Perspective

### What Deutsch-Jozsa Demonstrates

1. **Quantum computers can solve some problems exponentially faster** than classical computers (at least in query complexity)
2. **Superposition** allows evaluating functions on many inputs simultaneously
3. **Phase kickback** encodes function values in quantum phases
4. **Interference** extracts global properties from local phases
5. The **Hadamard transform** acts as a quantum Fourier transform, converting phases to amplitudes

### What Deutsch-Jozsa Doesn't Demonstrate

1. **Practical usefulness** — the constant-vs-balanced problem is artificial
2. **Robustness** — the algorithm requires the promise to be exactly satisfied
3. **Typical quantum advantage** — most real speedups are polynomial (like Grover) or rely on number-theoretic structure (like Shor)
4. **Fault tolerance** — we assumed perfect qubits and gates
5. **Gate complexity advantage** — if the oracle is expensive, the speedup may be illusory

### The Promise Problem Caveat

Deutsch-Jozsa is a **promise problem**: we're guaranteed the input satisfies a special condition.

What if the promise is violated?

| Actual Function | DJ Measurement | Conclusion |
|-----------------|----------------|------------|
| Constant | All zeros | Correct! |
| Balanced | Not all zeros | Correct! |
| 75% zeros | ??? | Could be anything |

For "almost balanced" functions, the algorithm gives **unreliable results**. The probability of measuring all zeros is:
$$
p = \left|\frac{1}{2^n}\sum_x (-1)^{f(x)}\right|^2
$$

If 75% of $f(x) = 0$ and 25% give $f(x) = 1$:
$$
p = \left|\frac{0.75 - 0.25}{1}\right|^2 = 0.25
$$

We'd measure all zeros 25% of the time — neither 0% (balanced) nor 100% (constant).

### The Bigger Picture

Deutsch-Jozsa was historically important: it was the first **proof** that quantum computers could outperform classical computers for some problem.

But the real excitement came later:
- **Simon's algorithm** (1994): Exponential speedup for finding hidden XOR structure → inspired Shor
- **Shor's algorithm** (1994): Exponential speedup for factoring → breaks RSA
- **Grover's algorithm** (1996): Quadratic speedup for search → broadly applicable

The journey from Deutsch-Jozsa to Shor took less than 10 years. It transformed quantum computing from a theoretical curiosity to a potential revolution in cryptography.

---

## Summary

1. **Historical significance:** Deutsch-Jozsa (1992) was the first proof of quantum speedup, inspiring Simon (1994) and Shor (1994).

2. **The oracle model** lets us study query complexity: how many times must we evaluate $f$?

3. **Deutsch's algorithm** solves the 1-bit constant-vs-balanced problem with 1 query (classical needs 2).

4. **The Deutsch-Jozsa algorithm** generalizes to $n$ bits: 1 query vs $O(2^n)$ classical — **exponential speedup**.

5. **The n-qubit Hadamard transform** is the QFT over $\mathbb{Z}_2^n$:
$$H^{\otimes n}|x\rangle = \frac{1}{\sqrt{2^n}}\sum_z (-1)^{x \cdot z}|z\rangle$$

6. **The key techniques:**
   - **Superposition:** Evaluate on all inputs at once
   - **Phase kickback:** Encode $f(x)$ as a phase $(-1)^{f(x)}$
   - **Interference:** Constructive (constant) → measure $|0\rangle^{\otimes n}$; destructive (balanced) → measure anything else

7. **Related algorithms:**
   - **Bernstein-Vazirani:** Same circuit, finds hidden string $s$ in $f(x) = s \cdot x$
   - **Simon:** Finds hidden XOR period — direct precursor to Shor

8. **Oracle construction:** Simple functions (inner product) need $O(n)$ gates; complex functions may need $O(2^n)$ gates.

9. **Query vs gate complexity:** A 1-query algorithm is only meaningful if the oracle is efficiently implementable.

10. **The promise matters:** Algorithm fails gracefully when the constant-or-balanced promise is violated.

---

## Homework

### Problem 1: Deutsch's Algorithm by Hand

Work through Deutsch's algorithm for the function $f(0) = 1, f(1) = 0$ (a balanced function).

**(a)** Write the state after each step: initialization, first Hadamards, oracle, second Hadamard.

**(b)** What is the final measurement outcome? Does it correctly identify $f$ as balanced?

**(c)** Repeat for the constant function $f(0) = f(1) = 1$.

---

### Problem 2: Phase Kickback Derivation

Prove the phase kickback formula:

**(a)** Show that $X|{-}\rangle = -|{-}\rangle$ where $|{-}\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$.

**(b)** Show that for the oracle $U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$:
$$
U_f|x\rangle|{-}\rangle = (-1)^{f(x)}|x\rangle|{-}\rangle
$$

**(c)** Why is it called "kickback"? Where does the phase come from?

---

### Problem 3: Deutsch-Jozsa Amplitude Calculation

For a 3-qubit Deutsch-Jozsa instance:

**(a)** Write the state after the first Hadamard layer (before the oracle).

**(b)** If $f$ is the parity function ($f(x) = x_1 \oplus x_2 \oplus x_3$), write the state after the oracle.

**(c)** Calculate the amplitude of $|000\rangle$ after the final Hadamard layer.

**(d)** Calculate the amplitude of $|111\rangle$ after the final Hadamard layer.

---

### Problem 4: Proving Correctness

Prove that the Deutsch-Jozsa algorithm correctly distinguishes constant and balanced functions.

**(a)** Show that for a constant function, the amplitude of $|0\rangle^{\otimes n}$ is $\pm 1$.

**(b)** Show that for a balanced function, the amplitude of $|0\rangle^{\otimes n}$ is $0$.

**(c)** What if $f$ is neither constant nor balanced (the promise is violated)? What can happen?

---

### Problem 5: Bernstein-Vazirani

The Bernstein-Vazirani problem: find the hidden string $s$ given an oracle for $f(x) = s \cdot x$ (inner product mod 2).

**(a)** Explain why the Deutsch-Jozsa circuit also solves this problem.

**(b)** What is the amplitude of $|s\rangle$ after the algorithm?

**(c)** Implement this in Qiskit for $n = 5$ with a random secret string $s$. Verify you recover $s$.

---

### Problem 6: Oracle Construction

**(a)** Design an oracle circuit for the function $f(x_1, x_2) = x_1 \land x_2$ (AND gate). Your circuit should implement $|x_1 x_2\rangle|y\rangle \to |x_1 x_2\rangle|y \oplus (x_1 \land x_2)\rangle$.

*Hint:* You need the **Toffoli gate** (CCNOT), which flips the target qubit only when both control qubits are $|1\rangle$. In Qiskit, use `qc.ccx(control1, control2, target)`.

**(b)** Is this function constant, balanced, or neither?

**(c)** What would Deutsch-Jozsa output for this oracle? (Remember, the promise is violated!)

**(d)** Design an oracle for $f(x_1, x_2, x_3) = (x_1 \land x_2) \oplus x_3$. How many Toffoli and CNOT gates do you need?

---

### Problem 7: Generalizing the Oracle

The standard oracle computes $U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$.

**(a)** Why must quantum oracles be reversible? Why can't we just compute $|x\rangle \to |f(x)\rangle$?

**(b)** Can you construct a reversible oracle for a function that's not reversible (e.g., $f(x) = 0$ for all $x$)?

**(c)** What's the minimum number of ancilla qubits needed for an oracle computing $f: \{0,1\}^n \to \{0,1\}^m$?

---

### Problem 8: Resource Counting

**(a)** How many qubits does the $n$-qubit Deutsch-Jozsa algorithm use?

**(b)** How many Hadamard gates?

**(c)** The oracle is treated as a "black box." In practice, how would the oracle's complexity affect the overall algorithm?

**(d)** If the oracle requires $O(n)$ elementary gates to implement, what is the total gate complexity of Deutsch-Jozsa?

---

### Problem 9: Probabilistic Classical Algorithm

Consider a randomized classical algorithm for the constant-vs-balanced problem:

**(a)** If you query $k$ random inputs and all give the same output, what's the probability that $f$ is actually balanced?

**(b)** How many queries do you need for 99% confidence?

**(c)** Compare to the quantum algorithm. Is the quantum advantage as impressive when we allow classical randomness?

---

### Problem 10: Simon's Algorithm (Challenge)

Simon's algorithm finds a hidden XOR period $s$ where $f(x) = f(y) \iff x \oplus y \in \{0^n, s\}$.

**(a)** For $n = 2$ with $s = 11$, list all pairs $(x, y)$ where $f(x) = f(y)$.

**(b)** After one run of Simon's algorithm, we get a random $z$ with $z \cdot s = 0$. For $s = 11$, what are the possible values of $z$?

**(c)** Why do we need $O(n)$ runs of the algorithm to find $s$, rather than just one?

**(d)** Explain conceptually why Simon's algorithm gives an exponential speedup while Bernstein-Vazirani only gives a polynomial speedup.

---

### Problem 11: Query vs Gate Complexity

**(a)** For the inner product oracle $f(x) = s \cdot x$, how many CNOT gates are needed in terms of the Hamming weight of $s$ (number of 1s)?

**(b)** Suppose we have a "random" function $f: \{0,1\}^n \to \{0,1\}$ with no special structure. Argue that any circuit computing $f$ requires $\Omega(2^n / n)$ gates. (This is a known result from circuit complexity.)

**(c)** Given part (b), is there any quantum speedup for determining whether a random function is constant or balanced? Explain.

**(d)** What does this tell us about when quantum algorithms provide meaningful speedups?

---

### Problem 12: Beyond the Promise

**(a)** Suppose $f: \{0,1\}^3 \to \{0,1\}$ has $f(x) = 1$ for exactly 3 out of 8 inputs. Calculate the probability of measuring $|000\rangle$ in Deutsch-Jozsa.

**(b)** Can you modify Deutsch-Jozsa to estimate the fraction of inputs where $f(x) = 1$? What would you measure?

**(c)** This problem (counting solutions) is related to a variant of Grover's algorithm. Why might amplitude estimation be useful here?

---

## Looking Ahead

Deutsch-Jozsa showed that quantum computers can be exponentially faster — at least for carefully designed problems.

Next lecture, we'll study **Grover's algorithm**: a quantum speedup for the practical problem of searching an unstructured database. The speedup is "only" quadratic ($O(\sqrt{N})$ vs $O(N)$), but it applies to nearly any search problem.