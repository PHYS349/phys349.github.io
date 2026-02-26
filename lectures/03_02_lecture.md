# Lecture 3.2 — Entanglement and the Bell States

## Where We Left Off

Last lecture we built the machinery for two-qubit states: the tensor product $\otimes$, the amplitude table $C_{ij}$, and the $6 = 4 + 2$ DOF counting that told us correlations must exist.

Today we make that concrete. We'll learn to *test* whether a state is entangled, meet the four Bell states — the maximally entangled two-qubit states that form the backbone of quantum information — and review the measurement formalism we'll need to analyze them.

---

## Part 1: Review and Practice

### The General Two-Qubit State

The most general state of two qubits is:

$$|\Psi\rangle = C_{00}|00\rangle + C_{01}|01\rangle + C_{10}|10\rangle + C_{11}|11\rangle$$

Four complex amplitudes, satisfying $\sum|C_{ij}|^2 = 1$.

### Independent Qubits: The Tensor Product

If qubit A is in state $|\psi\rangle = a|0\rangle + b|1\rangle$ and qubit B is in state $|\phi\rangle = c|0\rangle + d|1\rangle$, and they are **independent**, the joint state is:

$$|\psi\rangle \otimes |\phi\rangle = ac|00\rangle + ad|01\rangle + bc|10\rangle + bd|11\rangle$$

The amplitudes are products: $\Psi_{ij} = u_i v_j$. This is the quantum version of $P_{AB}(i,j) = P_A(i) \cdot P_B(j)$ — independence means the joint state factors.

```{admonition} iClicker
:class: tip

Compute $(|0\rangle - |1\rangle) \otimes (|0\rangle + |1\rangle)$.

$|\Psi\rangle = \_\_ |00\rangle + \_\_ |01\rangle + \_\_ |10\rangle + \_\_ |11\rangle$

| | $|00\rangle$ | $|01\rangle$ | $|10\rangle$ | $|11\rangle$ |
|---|---|---|---|---|
| **(A)** | 1 | 1 | 1 | 1 |
| **(B)** | 1 | −1 | −1 | 1 |
| **(C)** | 1 | 1 | −1 | −1 |
```

The answer is **(C)**. With $a = 1$, $b = -1$, $c = 1$, $d = 1$:

$$ac|00\rangle + ad|01\rangle + bc|10\rangle + bd|11\rangle = |00\rangle + |01\rangle - |10\rangle - |11\rangle$$

The pattern makes sense: qubit A contributes a minus sign whenever it's in state $|1\rangle$, because $b = -1$.

<!-- ---

## Part 2: Entanglement

### Can Every State Be Factored?

Consider the state:

$$|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$$

Can we write this as $|\psi\rangle \otimes |\phi\rangle = (a|0\rangle + b|1\rangle) \otimes (c|0\rangle + d|1\rangle)$?

Let's try. Matching coefficients to $ac|00\rangle + ad|01\rangle + bc|10\rangle + bd|11\rangle$:

$$ac = \frac{1}{\sqrt{2}}, \qquad bd = \frac{1}{\sqrt{2}}$$
$$ad = 0, \qquad bc = 0$$

From $ad = 0$: either $a = 0$ or $d = 0$.
From $bc = 0$: either $b = 0$ or $c = 0$.

But if $a = 0$, then $ac = 0 \neq \frac{1}{\sqrt{2}}$. If $d = 0$, then $bd = 0 \neq \frac{1}{\sqrt{2}}$. Same problem with $b = 0$ or $c = 0$.

**No assignment of $a, b, c, d$ works.** This state cannot be written as a tensor product.

This means three things — all equivalent ways of saying the same thing:

1. The state **cannot be written as a tensor product** of single-qubit states.
2. The two qubits are **not independent** — they are correlated.
3. The state is **entangled**.

```{admonition} Entanglement
:class: important

A two-qubit state is **entangled** if it cannot be written as $|\psi\rangle_A \otimes |\phi\rangle_B$ for any choice of single-qubit states. Entangled states have correlations that go beyond anything independent qubits can produce.
```

---

### The Determinant Test

Trying to factor states by hand is tedious. We need a systematic test.

Recall from last lecture that we can arrange the four amplitudes of any two-qubit state into a $2 \times 2$ **amplitude table**:

$$C = \begin{pmatrix} C_{00} & C_{01} \\ C_{10} & C_{11} \end{pmatrix}$$

For a product state, this matrix is an outer product of two vectors:

$$C = \vec{u}\,\vec{v}^T = \begin{pmatrix} a \\ b \end{pmatrix}\begin{pmatrix} c & d \end{pmatrix} = \begin{pmatrix} ac & ad \\ bc & bd \end{pmatrix}$$

An outer product of two vectors is a **rank-1 matrix** — its rows (or columns) are proportional to each other. Rank-1 matrices have a simple property: **their determinant is zero**.

$$\det(\vec{u}\,\vec{v}^T) = (ac)(bd) - (ad)(bc) = abcd - abcd = 0$$

This gives us a clean test:

```{admonition} Determinant Test for Entanglement
:class: important

Given a two-qubit state with amplitude table $C$:

$$\det(C) = C_{00}C_{11} - C_{01}C_{10}$$

- $\det(C) = 0$ → **product state** (not entangled)
- $\det(C) \neq 0$ → **entangled**
```

**Why does this work?** A $2 \times 2$ matrix can be written as an outer product of two vectors if and only if it has rank 1, which for a $2\times 2$ matrix is equivalent to having determinant zero. The determinant measures exactly the "entanglement content" — how far the amplitude table is from being factorable.

---

### Practice: Entangled or Not?

```{admonition} iClicker
:class: tip

Is this state entangled?

$$|00\rangle + |01\rangle + |10\rangle + |11\rangle$$

**(A)** Yes $\qquad$ **(B)** No $\qquad$ **(C)** Can't tell
```

Build the amplitude table and compute the determinant:

$$C = \begin{pmatrix} 1 & 1 \\ 1 & 1 \end{pmatrix}, \qquad \det(C) = (1)(1) - (1)(1) = 0$$

**Not entangled.** We can factor it:

$$|00\rangle + |01\rangle + |10\rangle + |11\rangle = (|0\rangle + |1\rangle) \otimes (|0\rangle + |1\rangle)$$

(up to normalization). Both qubits are independently in the $|+\rangle$ state.

---

```{admonition} iClicker
:class: tip

Is this state entangled?

$$|00\rangle - |01\rangle - |10\rangle + |11\rangle$$

**(A)** Yes $\qquad$ **(B)** No $\qquad$ **(C)** Can't tell
```

$$C = \begin{pmatrix} 1 & -1 \\ -1 & 1 \end{pmatrix}, \qquad \det(C) = (1)(1) - (-1)(-1) = 1 - 1 = 0$$

**Not entangled.** The factorization is:

$$|00\rangle - |01\rangle - |10\rangle + |11\rangle = (|0\rangle - |1\rangle) \otimes (|0\rangle - |1\rangle)$$

Both qubits are in the $|-\rangle$ state.

---

Now try $|00\rangle + |11\rangle$:

$$C = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}, \qquad \det(C) = (1)(1) - (0)(0) = 1 \neq 0$$

**Entangled.** The amplitude table is the identity matrix — it has rank 2, not rank 1. No choice of single-qubit states can produce it. This is the state we tried (and failed) to factor by hand earlier.

---

```{admonition} Try It
:class: tip

Is $|00\rangle - |01\rangle + |10\rangle + |11\rangle$ entangled? Compute the determinant and check.

$$C = \begin{pmatrix} 1 & -1 \\ 1 & 1 \end{pmatrix}, \qquad \det(C) = (1)(1) - (-1)(1) = 1 + 1 = 2 \neq 0$$

**Yes, entangled.** There's no way to write this as a tensor product.
```

The pattern is becoming clear: states where both diagonal entries are nonzero but the off-diagonal entries are zero (or vice versa) tend to be entangled — the two qubits are "doing different things together" in a way that can't be separated.

---

## Part 3: The Bell States

We've seen that $\frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$ is entangled. In fact, it's one of four special entangled states that form a complete orthonormal basis for the two-qubit Hilbert space.

### The Four Bell States

$$|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}} \qquad |\Phi^-\rangle = \frac{|00\rangle - |11\rangle}{\sqrt{2}}$$

$$|\Psi^+\rangle = \frac{|01\rangle + |10\rangle}{\sqrt{2}} \qquad |\Psi^-\rangle = \frac{|01\rangle - |10\rangle}{\sqrt{2}}$$

The naming convention: $\Phi$ states have same-outcome correlations ($|00\rangle$ and $|11\rangle$), $\Psi$ states have opposite-outcome correlations ($|01\rangle$ and $|10\rangle$). The $+/-$ superscript is the relative phase between the two terms.

$|\Psi^-\rangle$ is called the **singlet** state — it will play a starring role when we get to Bell's theorem.

### Properties

**All four are maximally entangled.** You can verify: each has an amplitude table with $|\det(C)| = 1/2$ (the maximum possible for a normalized state).

For example:

$$|\Phi^+\rangle: \quad C = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}, \quad \det(C) = \frac{1}{2}$$

$$|\Psi^-\rangle: \quad C = \frac{1}{\sqrt{2}}\begin{pmatrix} 0 & 1 \\ -1 & 0 \end{pmatrix}, \quad \det(C) = -\frac{1}{2}$$

**They form an orthonormal basis.** Any two-qubit state can be expanded in the Bell basis instead of the computational basis $\{|00\rangle, |01\rangle, |10\rangle, |11\rangle\}$. The four Bell states are mutually orthogonal:

$$\langle\Phi^+|\Phi^-\rangle = 0, \quad \langle\Phi^+|\Psi^+\rangle = 0, \quad \langle\Phi^+|\Psi^-\rangle = 0, \quad \text{etc.}$$

You can verify the first: $\langle\Phi^+|\Phi^-\rangle = \frac{1}{2}(\langle 00| + \langle 11|)(|00\rangle - |11\rangle) = \frac{1}{2}(1 - 1) = 0$.

**Two complete bases for the same space:**

| Computational basis | Bell basis |
|---|---|
| $\|00\rangle$ | $\|\Phi^+\rangle$ |
| $\|01\rangle$ | $\|\Phi^-\rangle$ |
| $\|10\rangle$ | $\|\Psi^+\rangle$ |
| $\|11\rangle$ | $\|\Psi^-\rangle$ |

The computational basis is made of product states — no correlations. The Bell basis is made entirely of maximally entangled states. Both are perfectly valid bases for the 4-dimensional two-qubit Hilbert space.

---

## Part 4: Measurement Formalism

Before we can analyze what happens when you measure entangled states, we need to sharpen our tools. Let's review the measurement formalism, first for a single qubit.

### A Note on Labels and Eigenvalues

The computational basis labels $|0\rangle$ and $|1\rangle$ are **names**, not eigenvalues. The Pauli $Z$ operator has eigenvalues $+1$ and $-1$:

$$Z|0\rangle = +|0\rangle, \qquad Z|1\rangle = -|1\rangle$$

So $|0\rangle$ means "spin up" (eigenvalue $+1$) and $|1\rangle$ means "spin down" (eigenvalue $-1$). The X and Y eigenstates follow the same convention:

$$X|+\rangle = +|+\rangle, \quad X|-\rangle = -|-\rangle \qquad Y|{+i}\rangle = +|{+i}\rangle, \quad Y|{-i}\rangle = -|{-i}\rangle$$

Throughout this course, **measurement outcomes are always $\pm 1$**, and basis states are labeled by their eigenvalue sign. When we get to Bell's theorem, Alice measuring Z and getting $|0\rangle$ will correspond to outcome $A = +1$.

---

### Projective Measurement: Single Qubit

Suppose we have a qubit in state $|\Psi\rangle = \alpha|0\rangle + \beta|1\rangle$ and we measure in the Z-basis.

**Step 1: Build the projector.** For the outcome $|0\rangle$ (eigenvalue $+1$), the projector is $|0\rangle\langle 0|$. As a matrix:

$$|0\rangle\langle 0| = \begin{pmatrix} 1 \\ 0 \end{pmatrix}\begin{pmatrix} 1 & 0 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}$$

**Step 2: Apply the projector.** The (unnormalized) post-measurement state is:

$$|0\rangle\langle 0|\Psi\rangle = |0\rangle\langle 0|(\alpha|0\rangle + \beta|1\rangle) = \alpha|0\rangle$$

The projector "kills" the $|1\rangle$ component and keeps the $|0\rangle$ component.

**Step 3: Find the probability.** The probability of this outcome is the squared norm of the projected state:

$$p(+1) = \langle\Psi|0\rangle\langle 0|\Psi\rangle = |\alpha|^2$$

This is just Born's rule: the probability equals the squared magnitude of the amplitude for that outcome. (We used the projector properties $(|0\rangle\langle 0|)^\dagger = |0\rangle\langle 0|$ and $(|0\rangle\langle 0|)^2 = |0\rangle\langle 0|$.)

**Step 4: Normalize.** The state after measurement is:

$$|\Psi_{\text{after}}\rangle = \frac{|0\rangle\langle 0|\Psi\rangle}{\sqrt{p(+1)}} = \frac{\alpha|0\rangle}{|\alpha|} = e^{i\phi}|0\rangle$$

After measuring and getting $|0\rangle$, the qubit *is* in state $|0\rangle$ (up to global phase). The superposition has **collapsed**.

```{admonition} Measurement Recipe
:class: important

For a measurement with possible outcome $|m\rangle$:

1. **Projector:** $|m\rangle\langle m|$
2. **Probability:** $p_m = \langle\Psi|m\rangle\langle m|\Psi\rangle = |\langle m|\Psi\rangle|^2$
3. **Post-measurement state:** $\displaystyle|\Psi_{\text{after}}\rangle = \frac{|m\rangle\langle m|\Psi\rangle}{\sqrt{p_m}}$
```

---

### Measuring in a Different Basis

Now measure the same qubit in the **X-basis** instead. The eigenstates of $X$ are:

$$|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle), \qquad |-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$$

The projector onto $|+\rangle$ is:

$$|+\rangle\langle+| = \frac{1}{2}(|0\rangle + |1\rangle)(\langle 0| + \langle 1|) = \frac{1}{2}\begin{pmatrix} 1 & 1 \\ 1 & 1 \end{pmatrix}$$

**Example:** Suppose our qubit is already in $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$. What happens when we measure X?

$$|+\rangle\langle+|\,|+\rangle = |+\rangle\underbrace{\langle+|+\rangle}_{1} = |+\rangle$$

Probability: $|\langle+|+\rangle|^2 = 1$. The measurement gives $|+\rangle$ (eigenvalue $+1$) with certainty, and the state is unchanged. This makes sense: $|+\rangle$ is an eigenstate of X, so measuring X reveals its eigenvalue without disturbing it.

**Contrast with Z-measurement:** If we instead measured Z on the same state $|+\rangle$:

$$p(+1) = |\langle 0|+\rangle|^2 = \frac{1}{2}, \qquad p(-1) = |\langle 1|+\rangle|^2 = \frac{1}{2}$$

Now the outcome is random — 50/50. Measuring in a **compatible basis** (X) gives a definite result; measuring in an **incompatible basis** (Z) gives a random one. The state $|+\rangle$ has a definite X-value but no definite Z-value.

This isn't an accident — it's enforced by the mathematics. Let's see why.



---

### Why Incompatible Bases Give Random Outcomes

Why does $|0\rangle$ (definite Z) give completely random results when measured in X? Let's see it two ways.

**The quick way: expand in the other basis.** Write $|0\rangle$ in the X-eigenbasis:

$$|0\rangle = \frac{1}{\sqrt{2}}\big(|+\rangle + |-\rangle\big)$$

It's an equal-weight superposition of $|+\rangle$ and $|-\rangle$. So a measurement of X gives $+1$ or $-1$ with equal probability $1/2$. The outcome is maximally uncertain — as random as a coin flip.

The same holds for Y. In the Y-eigenbasis:

$$|0\rangle = \frac{1}{\sqrt{2}}\big(|{+i}\rangle + |{-i}\rangle\big)$$

(up to a relative phase that doesn't affect probabilities). Again, 50/50.

**The systematic way: use $\sigma_i^2 = I$.** For any Pauli matrix, $\sigma_i^2 = I$, so $\langle\sigma_i^2\rangle = 1$ in any state. The variance is:

$$(\Delta\sigma_i)^2 = \langle\sigma_i^2\rangle - \langle\sigma_i\rangle^2 = 1 - \langle\sigma_i\rangle^2$$

Maximum uncertainty is $\Delta\sigma_i = 1$, which happens when $\langle\sigma_i\rangle = 0$ — outcomes equally likely, completely random. In a Z-eigenstate:

$$\langle\sigma_x\rangle = \langle 0|\sigma_x|0\rangle = 0, \qquad \langle\sigma_y\rangle = \langle 0|\sigma_y|0\rangle = 0$$

$$\Longrightarrow \quad \Delta\sigma_x = \Delta\sigma_y = 1 \quad \text{(maximal)}$$

Both orthogonal components are individually maximally uncertain. And the same argument holds cyclically: an eigenstate of X is maximally uncertain in Y and Z, and so on.

---

### The Uncertainty Principle

The deeper reason is structural. The spin operators satisfy the **commutation relations**:

$$[S_x, S_y] = i\hbar S_z, \qquad [S_y, S_z] = i\hbar S_x, \qquad [S_z, S_x] = i\hbar S_y$$

where $S_i = \frac{\hbar}{2}\sigma_i$. (In terms of the dimensionless Pauli matrices: $[\sigma_x, \sigma_y] = 2i\sigma_z$, etc.)

The **generalized uncertainty relation** says that for any two observables $A$ and $B$:

$$\boxed{\Delta A \cdot \Delta B \geq \frac{1}{2}\big|\langle [A, B] \rangle\big|}$$

where $\Delta A = \sqrt{\langle A^2 \rangle - \langle A \rangle^2}$ is the standard deviation.

Applied to spin in state $|0\rangle$ (an eigenstate of $S_z$ with $\langle S_z\rangle = +\hbar/2$):

$$\Delta S_x \cdot \Delta S_y \geq \frac{1}{2}|\langle [S_x, S_y] \rangle| = \frac{1}{2}\hbar \cdot \frac{\hbar}{2} = \frac{\hbar^2}{4}$$

We already computed $\Delta S_x = \Delta S_y = \hbar/2$, so $\Delta S_x \cdot \Delta S_y = \hbar^2/4$. The bound is **saturated**. The uncertainty isn't just bounded from below — it's as large as the algebra allows.

```{admonition} Spin and the Uncertainty Principle
:class: important

For spin-$\frac{1}{2}$, being in an eigenstate of $S_x$, $S_y$, or $S_z$ means the spin is definite along one axis and **maximally uncertain** along the other two: measurements along either orthogonal axis give $\pm\hbar/2$ with equal probability $1/2$.

This is not a limitation of our measurement apparatus. It follows from the non-commuting algebra $[S_x, S_y] = i\hbar S_z$. Angular momentum in quantum mechanics cannot be simultaneously sharp in different directions: making one component definite forces the orthogonal components to become maximally indeterminate.
```

This also explains the "$6 = 4 + 2$" DOF counting from last lecture in a new light. A single qubit on the Bloch sphere has a definite value along *one* axis, but that forces uncertainty along the other two. You can't have definite values for all three components simultaneously — the information budget is 2 real numbers (one axis direction), not 6.

Next lecture, we'll extend the measurement formalism to **two qubits** — measuring just one qubit of an entangled pair — and discover that the choice of measurement basis for one qubit determines what happens to the other.

---

## Summary

1. **Entanglement** means a two-qubit state cannot be written as $|\psi\rangle_A \otimes |\phi\rangle_B$. Equivalently: the qubits are not independent, and the amplitude table $C$ is not rank 1.

2. **The determinant test:** arrange amplitudes into a $2\times 2$ matrix. If $\det(C) = 0$, the state is a product (not entangled). If $\det(C) \neq 0$, the state is entangled. The determinant measures how far the state is from being factorable.

3. **The four Bell states** $\{|\Phi^\pm\rangle, |\Psi^\pm\rangle\}$ are maximally entangled and form an orthonormal basis — an alternative to the computational basis for two qubits. The singlet $|\Psi^-\rangle$ will be central to Bell's theorem.

4. **Projective measurement** uses projectors $|m\rangle\langle m|$ to compute probabilities ($p_m = |\langle m|\Psi\rangle|^2$) and post-measurement states ($|m\rangle\langle m|\Psi\rangle / \sqrt{p_m}$). Measuring in a compatible basis gives a definite result; measuring in an incompatible basis gives a random one.

5. **The uncertainty principle** for spin-$\frac{1}{2}$: an eigenstate of $S_x$, $S_y$, or $S_z$ is definite along one axis and maximally uncertain along the other two (50/50 outcomes). This follows from $[S_x, S_y] = i\hbar S_z$ and the generalized uncertainty relation $\Delta A \cdot \Delta B \geq \frac{1}{2}|\langle[A,B]\rangle|$. -->

<!-- ---

## Homework

### Problem 1: Tensor Product Practice

Compute the following and write the result as a sum of computational basis states.

**(a)** $(|0\rangle + |1\rangle) \otimes (|0\rangle - |1\rangle)$

**(b)** $(|0\rangle - |1\rangle) \otimes (|0\rangle - |1\rangle)$

**(c)** $(|0\rangle + i|1\rangle) \otimes (|0\rangle - i|1\rangle)$

For each, write the amplitude table $C$ and verify $\det(C) = 0$.

---

### Problem 2: The Determinant Test

For each two-qubit state, compute $\det(C)$ and determine whether the state is entangled. If it's a product state, find the factors.

**(a)** $|00\rangle + |01\rangle + |10\rangle + |11\rangle$

**(b)** $|00\rangle - |01\rangle + |10\rangle + |11\rangle$

**(c)** $|00\rangle + |11\rangle$

**(d)** $|00\rangle + |01\rangle + |10\rangle - |11\rangle$

**(e)** $|01\rangle + |10\rangle$

**(f)** $|00\rangle + i|11\rangle$

---

### Problem 3: Bell State Verification

**(a)** Write the amplitude table for each of the four Bell states and verify that $|\det(C)| = 1/2$ for all of them.

**(b)** Verify orthogonality: compute $\langle\Phi^+|\Phi^-\rangle$, $\langle\Phi^+|\Psi^+\rangle$, and $\langle\Psi^+|\Psi^-\rangle$.

**(c)** Show that $|\Phi^+\rangle$ can be written as: $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|{+}{+}\rangle + |{-}{-}\rangle)$, where $|{+}{+}\rangle = |+\rangle \otimes |+\rangle$ and $|{-}{-}\rangle = |-\rangle \otimes |-\rangle$. What does this say about correlations in the X-basis?

---

### Problem 4: Projective Measurement

A qubit is in state $|\Psi\rangle = \frac{1}{\sqrt{3}}|0\rangle + \sqrt{\frac{2}{3}}|1\rangle$.

**(a)** Measure in the Z-basis. What are the probabilities for each outcome? What is the state after each outcome?

**(b)** Measure in the X-basis. Construct the projectors $|+\rangle\langle+|$ and $|-\rangle\langle-|$. What are the probabilities? What is the state after each outcome?

**(c)** Verify that the probabilities sum to 1 and the projectors satisfy $|+\rangle\langle+| + |-\rangle\langle-| = I$.

---

### Problem 5: Measurement Basis Matters

A qubit is prepared in state $|0\rangle$.

**(a)** If you measure Z, what do you get? With what probability?

**(b)** If you measure X, what do you get? With what probability?

**(c)** If you measure Y (eigenstates $|+i\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ and $|-i\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$), what are the probabilities?

**(d)** A general principle: in how many measurement bases does $|0\rangle$ give a definite (probability 1) outcome?

---

### Problem 6: Projectors as Matrices

**(a)** Write $|0\rangle\langle 0|$, $|1\rangle\langle 1|$, $|+\rangle\langle+|$, and $|-\rangle\langle-|$ as $2 \times 2$ matrices.

**(b)** Verify that each satisfies the projector properties: $P^2 = P$ and $P^\dagger = P$.

**(c)** Verify that complementary projectors sum to the identity: $|0\rangle\langle 0| + |1\rangle\langle 1| = I$ and $|+\rangle\langle+| + |-\rangle\langle-| = I$.

**(d)** Compute $|0\rangle\langle 0| \cdot |+\rangle\langle+|$. Is the result a projector? What does this say about sequential measurements in different bases?

---

### Problem 7: The Uncertainty Principle for Spin

**(a)** Verify the commutation relation $[\sigma_x, \sigma_y] = 2i\sigma_z$ by explicit matrix multiplication, where:

$$\sigma_x = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, \quad \sigma_y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}, \quad \sigma_z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}$$

**(b)** Show that $\sigma_i^2 = I$ for each Pauli matrix. Conclude that $(\Delta\sigma_i)^2 = 1 - \langle\sigma_i\rangle^2$ in any state.

**(c)** For the state $|0\rangle$: compute $\langle\sigma_x\rangle$ and $\langle\sigma_y\rangle$. Use part (b) to find $\Delta\sigma_x$ and $\Delta\sigma_y$. Are they maximal?

**(d)** Verify the same result by expanding $|0\rangle$ in the X-eigenbasis. What are the probabilities for getting $+1$ and $-1$?

**(e)** For the state $|+\rangle$: which component is definite? Compute $\Delta\sigma_y$ and $\Delta\sigma_z$. Verify $\Delta\sigma_y \cdot \Delta\sigma_z \geq |\langle\sigma_x\rangle|$.

**(f)** Can a state have $\Delta\sigma_x = \Delta\sigma_y = \Delta\sigma_z = 0$ (definite values for all three components simultaneously)? Explain using the commutation relations.

---

## Looking Ahead

We now have all the ingredients: entangled Bell states, the determinant test, and the projective measurement formalism. Next lecture, we put them together. What happens when Alice measures *one qubit* of a Bell pair? How does her measurement basis affect what Bob sees? The answers lead directly to EPR, Bell's theorem, and the deepest question in quantum foundations: **are quantum correlations "just" classical correlations in disguise?** -->