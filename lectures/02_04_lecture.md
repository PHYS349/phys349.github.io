# Lecture 2.4: Groups, Spin, and the Pauli Matrices

## Review: Where We Are

We've now seen two physical realizations of qubits:
- **Interferometer paths:** Light in arm 1 vs arm 2 (Lecture 2.2)
- **Polarization:** Horizontal vs vertical (Lecture 2.3)

Both are described by 2D complex vectors on the Bloch sphere. But we haven't yet asked: **why complex numbers?**

Today we answer this question. We'll see that:
1. Complex numbers are *fundamental* to quantum mechanics — not just a convenience
2. Complex numbers encode rotations (the group SO(2))
3. Qubit transformations form a larger group (SU(2))
4. Nature hands us a qubit: electron spin (Stern-Gerlach experiment)
5. The Pauli matrices are the generators of SU(2) — the fundamental building blocks

---

## Part 1: Why Is Quantum Mechanics Complex?

### The Classical Wave Equation

For light (or sound, or water waves), the wave equation is:

$$\frac{\partial^2 E}{\partial x^2} = \frac{1}{c^2}\frac{\partial^2 E}{\partial t^2}$$

This is a **real** equation. The physical solutions are real:

$$E(x,t) = A\cos(kx - \omega t + \phi)$$

When we write $E = A e^{i(kx - \omega t)}$, this is a **trick**. We always take the real part at the end to get the physical field. The complex numbers are bookkeeping — they make the math easier, but the physics is real.

The dispersion relation is:

$$\omega = ck$$

Linear in $k$ — frequency proportional to wavenumber.

### The Schrödinger Equation

Now look at the Schrödinger equation for a free particle:

$$i\hbar\frac{\partial \psi}{\partial t} = -\frac{\hbar^2}{2m}\frac{\partial^2 \psi}{\partial x^2}$$

Notice the $i$ on the left side. This is **not** a trick — it's built into the fundamental equation. The wavefunction $\psi$ is irreducibly complex.

Let's see why. Try a plane wave solution $\psi = e^{i(kx - \omega t)}$:

**Left side:**
$$i\hbar \frac{\partial}{\partial t}e^{i(kx-\omega t)} = i\hbar \cdot (-i\omega) e^{i(kx-\omega t)} = \hbar\omega \cdot e^{i(kx-\omega t)}$$

**Right side:**
$$-\frac{\hbar^2}{2m}\frac{\partial^2}{\partial x^2}e^{i(kx-\omega t)} = -\frac{\hbar^2}{2m}(ik)^2 e^{i(kx-\omega t)} = \frac{\hbar^2 k^2}{2m}e^{i(kx-\omega t)}$$

Setting them equal:
$$\hbar\omega = \frac{\hbar^2 k^2}{2m} \implies \omega = \frac{\hbar k^2}{2m}$$

This is the quantum dispersion relation — **quadratic** in $k$.

### Why Complex Is Required

Here's the key point. Let's rewrite the Schrödinger equation using $E = \hbar\omega$ and $p = \hbar k$:

$$\frac{1}{k^2}\frac{\partial^2 \psi}{\partial x^2} = \frac{i}{\omega}\frac{\partial \psi}{\partial t}$$

The left side has $\partial^2/\partial x^2$ (second derivative in space).
The right side has $\partial/\partial t$ (first derivative in time).

For a real equation, we'd need both sides to have the same "parity" of derivatives. But they don't match — one is second-order, one is first-order.

The $i$ fixes this! It provides the "missing" derivative's worth of sign changes.

If you try to use real numbers only, $\psi = A\cos(kx - \omega t)$ does **not** satisfy the Schrödinger equation. Try it:

$$\frac{\partial}{\partial t}\cos(kx - \omega t) = \omega\sin(kx - \omega t)$$

$$\frac{\partial^2}{\partial x^2}\cos(kx - \omega t) = -k^2\cos(kx - \omega t)$$

These are different functions (sin vs cos) — they can't be proportional for all $x$ and $t$.

---

## iClicker Question 1

**The equation $\frac{1}{k^2}\frac{\partial^2 \psi}{\partial x^2} = \frac{i}{\omega}\frac{\partial \psi}{\partial t}$ has which traveling wave solution?**

- (A) $e^{i(kx - \omega t)}$
- (B) $e^{-i(kx - \omega t)}$ ✓
- (C) $\cos(kx - \omega t)$
- (D) $\sin(kx - \omega t)$

**Solution:** Let's check (B): $\psi = e^{-i(kx - \omega t)}$

$$\frac{\partial \psi}{\partial t} = i\omega e^{-i(kx-\omega t)}$$

$$\frac{\partial^2 \psi}{\partial x^2} = -k^2 e^{-i(kx-\omega t)}$$

Substituting:
$$\frac{-k^2}{k^2}e^{-i(kx-\omega t)} = \frac{i \cdot i\omega}{\omega}e^{-i(kx-\omega t)}$$
$$-e^{-i(kx-\omega t)} = -e^{-i(kx-\omega t)} \checkmark$$

Note the sign! Option (A) gives $+1 = -1$, which fails.

```{admonition} The Key Difference
:class: important

**Classical waves:** Complex numbers are a convenient trick. The real part is physical.

**Quantum mechanics:** Complex numbers are fundamental. The $i$ in Schrödinger's equation is essential — it connects first-order time evolution to second-order spatial structure.
```

---

## Part 2: Complex Numbers as Rotations — The Group SO(2)

So complex numbers are fundamental to quantum mechanics. But what *are* complex numbers, really? Here's a deep answer: **they encode rotations**.

### Multiplication Is Rotation

Consider multiplying a complex number $z$ by $e^{i\theta}$:

$$z \mapsto e^{i\theta} z$$

What does this do geometrically?

Write $z = re^{i\phi}$ (polar form). Then:

$$e^{i\theta} z = e^{i\theta} \cdot re^{i\phi} = re^{i(\phi + \theta)}$$

The magnitude $r$ is unchanged. The angle increases by $\theta$.

**Multiplication by $e^{i\theta}$ rotates by angle $\theta$ in the complex plane!**

```python
import numpy as np
import matplotlib.pyplot as plt

# Visualize rotation in complex plane
z = 1 + 0.5j  # Original complex number
theta = np.pi/3  # Rotation angle

z_rotated = np.exp(1j * theta) * z

plt.figure(figsize=(8, 8))
plt.arrow(0, 0, z.real, z.imag, head_width=0.1, color='blue', label='z')
plt.arrow(0, 0, z_rotated.real, z_rotated.imag, head_width=0.1, color='red', label=f'e^(iθ)z, θ={theta:.2f}')
plt.xlim(-2, 2)
plt.ylim(-2, 2)
plt.axhline(y=0, color='k', linewidth=0.5)
plt.axvline(x=0, color='k', linewidth=0.5)
plt.grid(True, alpha=0.3)
plt.legend()
plt.title('Multiplication by e^(iθ) = Rotation')
plt.axis('equal')
plt.show()
```

### The Group Structure

The set of all rotations in 2D forms a **group** called SO(2) (Special Orthogonal group in 2 dimensions).

**What is a group?** A set with an operation satisfying four properties:

1. **Closure:** Combining two elements gives another element in the set
2. **Associativity:** $(a \cdot b) \cdot c = a \cdot (b \cdot c)$
3. **Identity:** There's an element $e$ such that $e \cdot a = a \cdot e = a$
4. **Inverses:** Every element $a$ has an inverse $a^{-1}$ such that $a \cdot a^{-1} = e$

Let's verify SO(2) satisfies these:

**Closure:** Rotating by $\theta_1$ then $\theta_2$ is the same as rotating by $\theta_1 + \theta_2$:
$$e^{i\theta_1} \cdot e^{i\theta_2} = e^{i(\theta_1 + \theta_2)}$$
This is still a rotation. ✓

**Associativity:** Addition of angles is associative. ✓

**Identity:** $e^{i \cdot 0} = 1$ (rotation by zero). ✓

**Inverses:** $(e^{i\theta})^{-1} = e^{-i\theta}$ (rotate backward). ✓

So rotations form a group, and complex numbers of magnitude 1 (the unit circle) represent this group.

### Matrix Representation

We can also represent 2D rotations as matrices acting on vectors $\begin{pmatrix} x \\ y \end{pmatrix}$:

$$R(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}$$

Let's verify this is the same as complex multiplication.

A complex number $z = x + iy$ corresponds to the vector $\begin{pmatrix} x \\ y \end{pmatrix}$.

Multiplying by $e^{i\theta} = \cos\theta + i\sin\theta$:

$$e^{i\theta} z = (\cos\theta + i\sin\theta)(x + iy)$$
$$= (\cos\theta \cdot x - \sin\theta \cdot y) + i(\sin\theta \cdot x + \cos\theta \cdot y)$$
$$= x' + iy'$$

where:
$$x' = \cos\theta \cdot x - \sin\theta \cdot y$$
$$y' = \sin\theta \cdot x + \cos\theta \cdot y$$

In matrix form:
$$\begin{pmatrix} x' \\ y' \end{pmatrix} = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix} = R(\theta) \begin{pmatrix} x \\ y \end{pmatrix}$$

**Complex multiplication and matrix rotation are the same thing!**

### The Generator of Rotations

Here's a powerful idea. Instead of thinking about finite rotations $R(\theta)$, think about infinitesimal rotations.

For small $\delta\theta$:

$$R(\delta\theta) = \begin{pmatrix} \cos\delta\theta & -\sin\delta\theta \\ \sin\delta\theta & \cos\delta\theta \end{pmatrix} \approx \begin{pmatrix} 1 & -\delta\theta \\ \delta\theta & 1 \end{pmatrix} = I + \delta\theta \begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}$$

Define the **generator**:

$$G = \begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}$$

Then:
$$R(\delta\theta) \approx I + \delta\theta \cdot G$$

An infinitesimal rotation is "the identity plus a little bit of $G$."

### From Generator to Finite Rotation

How do we get a finite rotation from the generator? 

Think of a finite rotation as many infinitesimal rotations:

$$R(\theta) = \lim_{N \to \infty} \left(I + \frac{\theta}{N} G\right)^N$$

This limit is the definition of the matrix exponential:

$$\boxed{R(\theta) = e^{\theta G}}$$

**Finite rotations are exponentials of the generator!**

### Verifying the Exponential

Let's verify this works. First, compute powers of $G$:

$$G^2 = \begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}\begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix} = \begin{pmatrix} -1 & 0 \\ 0 & -1 \end{pmatrix} = -I$$

So $G^2 = -I$. This is just like $i^2 = -1$!

Continuing:
- $G^3 = G^2 \cdot G = -G$
- $G^4 = G^2 \cdot G^2 = (-I)(-I) = I$
- $G^5 = G$, and so on...

Now expand $e^{\theta G}$ as a Taylor series:

$$e^{\theta G} = I + \theta G + \frac{(\theta G)^2}{2!} + \frac{(\theta G)^3}{3!} + \frac{(\theta G)^4}{4!} + \cdots$$

$$= I + \theta G + \frac{\theta^2 G^2}{2!} + \frac{\theta^3 G^3}{3!} + \frac{\theta^4 G^4}{4!} + \cdots$$

Using $G^2 = -I$, $G^3 = -G$, $G^4 = I$, etc.:

$$= I + \theta G - \frac{\theta^2}{2!}I - \frac{\theta^3}{3!}G + \frac{\theta^4}{4!}I + \cdots$$

Group the terms with $I$ and with $G$:

$$= \left(1 - \frac{\theta^2}{2!} + \frac{\theta^4}{4!} - \cdots\right)I + \left(\theta - \frac{\theta^3}{3!} + \frac{\theta^5}{5!} - \cdots\right)G$$

$$= \cos\theta \cdot I + \sin\theta \cdot G$$

$$= \begin{pmatrix} \cos\theta & 0 \\ 0 & \cos\theta \end{pmatrix} + \begin{pmatrix} 0 & -\sin\theta \\ \sin\theta & 0 \end{pmatrix} = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix} = R(\theta)$$

**It works!**

```{admonition} The Generator-Exponential Relationship
:class: important

$$e^{\theta G} = \cos\theta \cdot I + \sin\theta \cdot G$$

Compare to Euler's formula:
$$e^{i\theta} = \cos\theta + i\sin\theta$$

The matrix $G$ plays the role of $i$! Both satisfy:
- $G^2 = -I$ (like $i^2 = -1$)
- Exponentiating gives rotations
```

### Why This Matters for Quantum Mechanics

We've shown that:
- Complex numbers encode rotations (SO(2))
- Phase factors $e^{i\theta}$ are rotations by angle $\theta$
- The generator $G$ (or equivalently, $i$) is what makes rotation possible

In quantum mechanics, **phase is rotation**. The complex structure of quantum mechanics isn't arbitrary — it's because quantum amplitudes can rotate, interfere, and accumulate phase.

---

## Part 3: From SO(2) to SU(2)

### SO(2): One-Dimensional Rotations

SO(2) describes rotations in a plane — rotations around a single axis. Key features:

- **One generator** ($G$, or equivalently $i$)
- **Commutative:** $R(\theta_1)R(\theta_2) = R(\theta_2)R(\theta_1)$
  - Rotating by $\theta_1$ then $\theta_2$ is the same as $\theta_2$ then $\theta_1$
  - Addition of angles commutes

### SO(3): Three-Dimensional Rotations

In 3D, we can rotate around three axes: x, y, and z.

**Crucially, 3D rotations do NOT commute!**

Try this with a book:
1. Rotate 90° around x-axis, then 90° around z-axis
2. Rotate 90° around z-axis, then 90° around x-axis

You get different final orientations!

This means SO(3) has:
- **Three generators** ($J_x, J_y, J_z$)
- **Non-commutative:** $R_x R_z \neq R_z R_x$

The non-commutativity is captured by the commutation relations:
$$[J_i, J_j] = i\epsilon_{ijk}J_k$$

where $\epsilon_{ijk}$ is +1 for cyclic permutations (xyz, yzx, zxy), -1 for anti-cyclic, and 0 if any indices repeat.

### SU(2): Quantum Rotations

Now here's the key for qubits.

A qubit state is a point on the Bloch sphere. Transformations of qubits are **rotations of the Bloch sphere**.

But qubits live in a 2D *complex* vector space ($\mathbb{C}^2$). The group that acts on this space while preserving:
- The norm: $|\alpha|^2 + |\beta|^2 = 1$
- The determinant: $\det(U) = 1$

is called **SU(2)** (Special Unitary group in 2 dimensions).

**SU(2) properties:**
- **S** = Special (determinant 1)
- **U** = Unitary ($U^\dagger U = I$, preserves norm)
- **2** = Acts on 2-dimensional complex space

Like SO(3), SU(2) has **three generators**. Unlike SO(2), it's **non-commutative**.

The generators of SU(2) are the **Pauli matrices** — which we'll meet shortly.

```{admonition} SU(2) and SO(3)
:class: note

SU(2) and SO(3) are closely related but not identical:
- Both describe 3D rotations
- Both have three generators with the same commutation relations
- But SU(2) is a "double cover" of SO(3): a 360° rotation in SO(3) corresponds to a 720° rotation in SU(2)

For qubits, this means rotating by $2\pi$ gives a *minus sign* (not the identity). This is related to spin being half-integer.
```

---

## Part 4: The Stern-Gerlach Experiment

We've been doing abstract math. Now let's see how nature hands us a qubit.

### The Setup (1922)

Otto Stern and Walther Gerlach sent a beam of silver atoms through an **inhomogeneous magnetic field** — a field that's stronger at the top than at the bottom.

```{figure} stern_gerlach_placeholder.svg
:name: stern-gerlach
:width: 80%

The Stern-Gerlach experiment. A beam of atoms passes through a magnetic field gradient. Classical physics predicts a continuous spread; quantum mechanics predicts discrete spots.
```

**Classical prediction:** If atoms have randomly oriented magnetic moments, the force on each atom depends on its orientation. The beam should spread out into a **continuous band**.

**Quantum result:** The beam split into exactly **two discrete spots**.

### What This Means

1. **Angular momentum is quantized** — it can only take discrete values

2. **For silver atoms (and electrons), there are exactly two values** — "spin up" and "spin down"

3. **This is spin-1/2** — the smallest non-trivial quantum angular momentum

The electron has intrinsic angular momentum called **spin** with quantum number $s = 1/2$. The z-component can only be:

$$S_z = m_s \hbar, \quad \text{where } m_s = +\frac{1}{2} \text{ or } -\frac{1}{2}$$

### The Qubit

The two spin states define a qubit:

$$|\uparrow\rangle \equiv |0\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}, \qquad |\downarrow\rangle \equiv |1\rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$$

A general spin state is:

$$|\psi\rangle = \alpha|\uparrow\rangle + \beta|\downarrow\rangle$$

This is *exactly* the same structure as polarization. The Bloch sphere works identically.

### Rotating the Apparatus

Here's where spin gets interesting. What if we rotate the Stern-Gerlach apparatus?

**Z-oriented apparatus:** Measures $S_z$. Eigenstates are $|\uparrow_z\rangle = |0\rangle$ and $|\downarrow_z\rangle = |1\rangle$.

**X-oriented apparatus:** Measures $S_x$. Eigenstates are:

$$|\uparrow_x\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) = |+\rangle$$
$$|\downarrow_x\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle) = |-\rangle$$

**Y-oriented apparatus:** Measures $S_y$. Eigenstates are:

$$|\uparrow_y\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle) = |+i\rangle$$
$$|\downarrow_y\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle) = |-i\rangle$$

**This is exactly our polarization story!**

| Spin measurement | Polarization measurement |
|------------------|-------------------------|
| z-axis (up/down) | H/V basis |
| x-axis | D/A basis |
| y-axis | R/L basis |

---

## iClicker Question 2

**An electron is prepared in state $|\uparrow_z\rangle = |0\rangle$ (spin-up along z). You measure its spin along the x-axis. What do you get?**

- (A) Always spin-up along x
- (B) Always spin-down along x
- (C) 50% up, 50% down ✓
- (D) The measurement is undefined

**Solution:** We need to express $|0\rangle$ in the x-basis:

$$|0\rangle = \frac{1}{\sqrt{2}}|+\rangle + \frac{1}{\sqrt{2}}|-\rangle$$

So:
$$P(\uparrow_x) = |\langle + | 0 \rangle|^2 = \frac{1}{2}$$
$$P(\downarrow_x) = |\langle - | 0 \rangle|^2 = \frac{1}{2}$$

A state with definite $S_z$ has completely uncertain $S_x$!

---

## Part 5: The Pauli Matrices

We need operators to represent "measure spin along x" vs "measure spin along z."

These are the **Pauli matrices**:

$$\boxed{\sigma_x = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, \quad \sigma_y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}, \quad \sigma_z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}}$$

The spin operators are:
$$S_x = \frac{\hbar}{2}\sigma_x, \quad S_y = \frac{\hbar}{2}\sigma_y, \quad S_z = \frac{\hbar}{2}\sigma_z$$

### Property 1: Hermitian

$$\sigma_x^\dagger = \sigma_x, \quad \sigma_y^\dagger = \sigma_y, \quad \sigma_z^\dagger = \sigma_z$$

Hermitian matrices represent **observables** — things we can measure. Their eigenvalues are real.

### Property 2: Traceless

$$\text{Tr}(\sigma_x) = 0 + 0 = 0$$
$$\text{Tr}(\sigma_y) = 0 + 0 = 0$$
$$\text{Tr}(\sigma_z) = 1 + (-1) = 0$$

### Property 3: Square to Identity

$$\sigma_x^2 = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = I$$

Similarly, $\sigma_y^2 = I$ and $\sigma_z^2 = I$.

Since $\sigma_i^2 = I$, the eigenvalues must satisfy $\lambda^2 = 1$, so $\lambda = \pm 1$.

### Property 4: Eigenvalues ±1

Each Pauli matrix has eigenvalues **+1 and -1**.

For spin, this means $S_i = \frac{\hbar}{2}\sigma_i$ has eigenvalues $\pm\frac{\hbar}{2}$.

### Eigenstates of Each Pauli Matrix

**$\sigma_z$ eigenstates:**

$$\sigma_z |0\rangle = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}\begin{pmatrix} 1 \\ 0 \end{pmatrix} = \begin{pmatrix} 1 \\ 0 \end{pmatrix} = +1 \cdot |0\rangle$$

$$\sigma_z |1\rangle = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}\begin{pmatrix} 0 \\ 1 \end{pmatrix} = \begin{pmatrix} 0 \\ -1 \end{pmatrix} = -1 \cdot |1\rangle$$

**$\sigma_x$ eigenstates:**

$$\sigma_x |+\rangle = \sigma_x \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix} = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix} = +1 \cdot |+\rangle$$

$$\sigma_x |-\rangle = \sigma_x \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ -1 \end{pmatrix} = \frac{1}{\sqrt{2}}\begin{pmatrix} -1 \\ 1 \end{pmatrix} = -1 \cdot |-\rangle$$

**$\sigma_y$ eigenstates:**

$$\sigma_y |+i\rangle = +1 \cdot |+i\rangle, \quad \sigma_y |-i\rangle = -1 \cdot |-i\rangle$$

where $|+i\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ and $|-i\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$.

### Summary: Paulis and the Bloch Sphere

| Pauli | +1 eigenstate | −1 eigenstate | Bloch axis |
|-------|---------------|---------------|------------|
| $\sigma_z$ | $\|0\rangle$ | $\|1\rangle$ | z (poles) |
| $\sigma_x$ | $\|+\rangle$ | $\|-\rangle$ | x (equator) |
| $\sigma_y$ | $\|+i\rangle$ | $\|-i\rangle$ | y (equator) |

The eigenstates of the three Pauli matrices are exactly the six cardinal points on the Bloch sphere!

```python
# Verify eigenstates with Qiskit
import numpy as np
from qiskit.quantum_info import Statevector, Operator

# Define Pauli matrices
sigma_x = Operator([[0, 1], [1, 0]])
sigma_y = Operator([[0, -1j], [1j, 0]])
sigma_z = Operator([[1, 0], [0, -1]])

# Define states
zero = Statevector([1, 0])
one = Statevector([0, 1])
plus = Statevector([1/np.sqrt(2), 1/np.sqrt(2)])
minus = Statevector([1/np.sqrt(2), -1/np.sqrt(2)])
plus_i = Statevector([1/np.sqrt(2), 1j/np.sqrt(2)])
minus_i = Statevector([1/np.sqrt(2), -1j/np.sqrt(2)])

# Check σ_z |0⟩ = +|0⟩
result = zero.evolve(sigma_z)
print(f"σ_z|0⟩ = {result.data}")  # Should be [1, 0]

# Check σ_x |+⟩ = +|+⟩
result = plus.evolve(sigma_x)
print(f"σ_x|+⟩ = {result.data}")  # Should be [1/√2, 1/√2]
```

---

## Part 6: Commutation Relations

The Pauli matrices have a crucial property: **they don't commute**.

### Computing $\sigma_x \sigma_y$

$$\sigma_x \sigma_y = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}\begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix} = \begin{pmatrix} i & 0 \\ 0 & -i \end{pmatrix} = i\sigma_z$$

### Computing $\sigma_y \sigma_x$

$$\sigma_y \sigma_x = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} = \begin{pmatrix} -i & 0 \\ 0 & i \end{pmatrix} = -i\sigma_z$$

### The Commutator

$$[\sigma_x, \sigma_y] = \sigma_x \sigma_y - \sigma_y \sigma_x = i\sigma_z - (-i\sigma_z) = 2i\sigma_z$$

### All Commutation Relations

By similar calculations:

$$\boxed{[\sigma_x, \sigma_y] = 2i\sigma_z}$$
$$\boxed{[\sigma_y, \sigma_z] = 2i\sigma_x}$$
$$\boxed{[\sigma_z, \sigma_x] = 2i\sigma_y}$$

Or compactly:
$$[\sigma_i, \sigma_j] = 2i\epsilon_{ijk}\sigma_k$$

where the sum over $k$ is implied.

### The Anticommutator

We can also compute the anticommutator $\{A, B\} = AB + BA$:

$$\{\sigma_x, \sigma_y\} = \sigma_x\sigma_y + \sigma_y\sigma_x = i\sigma_z + (-i\sigma_z) = 0$$

In general:
$$\{\sigma_i, \sigma_j\} = 2\delta_{ij}I$$

Different Paulis anticommute; same Paulis give $2I$.

### Combining Both

$$\sigma_i \sigma_j = \delta_{ij}I + i\epsilon_{ijk}\sigma_k$$

This single formula captures everything about Pauli multiplication!

---

## iClicker Question 3

**What is $\sigma_x \sigma_y$?**

- (A) $\sigma_z$
- (B) $-\sigma_z$
- (C) $i\sigma_z$ ✓
- (D) $-i\sigma_z$

---

### Why Non-Commuting Matters: The Uncertainty Principle

If two operators don't commute, they can't be simultaneously diagonalized — they can't share eigenstates.

Since $[\sigma_x, \sigma_z] = -2i\sigma_y \neq 0$:

- A state can't be an eigenstate of both $\sigma_x$ and $\sigma_z$
- If you know $S_z$ precisely, $S_x$ is uncertain
- If you know $S_x$ precisely, $S_z$ is uncertain

This is the **uncertainty principle for spin**.

The state $|0\rangle$ is an eigenstate of $\sigma_z$ (eigenvalue +1), so it has definite $S_z = +\hbar/2$. But it's a 50/50 superposition of $|+\rangle$ and $|-\rangle$, so $S_x$ is completely uncertain.

```{admonition} Uncertainty Principle
:class: warning

**You cannot simultaneously know spin along two perpendicular axes.**

If $S_z$ is definite, then $S_x$ and $S_y$ are uncertain (and vice versa).

This is fundamentally different from classical physics, where all components of angular momentum can be known simultaneously.
```

---

## Part 7: Paulis as Generators of SU(2)

### Any Qubit Operator Can Be Written Using Paulis

Any $2 \times 2$ Hermitian matrix can be decomposed as:

$$\boxed{H = h_0 I + h_x \sigma_x + h_y \sigma_y + h_z \sigma_z = h_0 I + \vec{h} \cdot \vec{\sigma}}$$

where $h_0, h_x, h_y, h_z$ are real numbers and $\vec{\sigma} = (\sigma_x, \sigma_y, \sigma_z)$.

**The Pauli matrices (plus identity) form a basis for all $2 \times 2$ Hermitian matrices.**

This means any qubit observable, any qubit Hamiltonian, can be written in terms of Paulis.

### Rotations on the Bloch Sphere

Just as $G$ generates SO(2) rotations, the Pauli matrices generate SU(2) rotations.

A rotation by angle $\theta$ around axis $\hat{n} = (n_x, n_y, n_z)$ is:

$$\boxed{R_{\hat{n}}(\theta) = e^{-i\theta(\hat{n} \cdot \vec{\sigma})/2} = \cos\frac{\theta}{2}I - i\sin\frac{\theta}{2}(\hat{n} \cdot \vec{\sigma})}$$

The factor of 2 appears because of spin-1/2 (the "double cover" of SO(3)).

### Explicit Rotation Matrices

**Rotation around z:**
$$R_z(\theta) = e^{-i\theta\sigma_z/2} = \begin{pmatrix} e^{-i\theta/2} & 0 \\ 0 & e^{i\theta/2} \end{pmatrix}$$

**Rotation around x:**
$$R_x(\theta) = e^{-i\theta\sigma_x/2} = \begin{pmatrix} \cos\frac{\theta}{2} & -i\sin\frac{\theta}{2} \\ -i\sin\frac{\theta}{2} & \cos\frac{\theta}{2} \end{pmatrix}$$

**Rotation around y:**
$$R_y(\theta) = e^{-i\theta\sigma_y/2} = \begin{pmatrix} \cos\frac{\theta}{2} & -\sin\frac{\theta}{2} \\ \sin\frac{\theta}{2} & \cos\frac{\theta}{2} \end{pmatrix}$$

### Connection to Gates We Know

**Phase gate:**
$$P(\phi) = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\phi} \end{pmatrix} = e^{i\phi/2} R_z(\phi)$$

Up to global phase, the phase gate IS $R_z$.

**Hadamard gate:**

The Hadamard is a 180° rotation around the axis $(\hat{x} + \hat{z})/\sqrt{2}$:

$$H = \frac{1}{\sqrt{2}}(\sigma_x + \sigma_z)$$

You can verify:
$$H = e^{i\pi/2} \cdot e^{-i\pi(\sigma_x + \sigma_z)/(2\sqrt{2})} = e^{i\pi/2} R_{(\hat{x}+\hat{z})/\sqrt{2}}(\pi)$$

**X, Y, Z gates:**

These are 180° rotations (up to global phase):
- $X = \sigma_x = iR_x(\pi)$
- $Y = \sigma_y = iR_y(\pi)$
- $Z = \sigma_z = iR_z(\pi)$

```{admonition} The Fundamental Result
:class: important

**Every single-qubit unitary is a rotation on the Bloch sphere.**

Any $U \in SU(2)$ can be written as $U = e^{-i\theta(\hat{n} \cdot \vec{\sigma})/2}$ for some axis $\hat{n}$ and angle $\theta$.

The Pauli matrices are the **generators** of these rotations.
```

### Qiskit Verification

```python
import numpy as np
from qiskit.quantum_info import Operator
from qiskit import QuantumCircuit

# Verify R_z(θ) = e^{-iθσ_z/2}
theta = np.pi/3

# Method 1: Direct matrix
sigma_z = np.array([[1, 0], [0, -1]])
Rz_direct = np.cos(theta/2) * np.eye(2) - 1j * np.sin(theta/2) * sigma_z
print("R_z from formula:")
print(Rz_direct)

# Method 2: Qiskit gate
qc = QuantumCircuit(1)
qc.rz(theta, 0)
Rz_qiskit = Operator(qc).data
print("\nR_z from Qiskit:")
print(Rz_qiskit)

# They match up to global phase!
```

---

## Summary

Today we covered the mathematical foundation of qubits:

1. **Quantum mechanics is fundamentally complex** — the $i$ in Schrödinger's equation is essential, not a trick

2. **Complex numbers = SO(2) rotations**
   - $e^{i\theta}$ rotates by angle $\theta$
   - The generator $G$ satisfies $G^2 = -I$ (like $i^2 = -1$)
   - Finite rotations: $R(\theta) = e^{\theta G}$

3. **Qubits transform under SU(2)**
   - Three generators (three axes)
   - Non-commutative (order matters)
   - Rotations of the Bloch sphere

4. **Stern-Gerlach reveals spin**
   - Nature hands us a qubit: spin-1/2
   - Two discrete outcomes, not a continuum
   - Rotating the apparatus = choosing measurement basis

5. **The Pauli matrices $\sigma_x$, $\sigma_y$, $\sigma_z$:**
   - Hermitian, traceless, square to identity
   - Eigenvalues ±1
   - Eigenstates = Bloch sphere axes (z, x, y)
   - Non-commuting: $[\sigma_i, \sigma_j] = 2i\epsilon_{ijk}\sigma_k$

6. **Paulis generate SU(2):**
   - Any qubit Hamiltonian: $H = h_0 I + \vec{h} \cdot \vec{\sigma}$
   - Rotations: $R_{\hat{n}}(\theta) = e^{-i\theta(\hat{n} \cdot \vec{\sigma})/2}$
   - Every single-qubit gate is a rotation!

### Everything Connects

| Polarization | Spin | Bloch sphere | Pauli eigenstate |
|--------------|------|--------------|------------------|
| H | $\uparrow_z$ | +z (north pole) | $\sigma_z = +1$ |
| V | $\downarrow_z$ | −z (south pole) | $\sigma_z = -1$ |
| D | $\uparrow_x$ | +x | $\sigma_x = +1$ |
| A | $\downarrow_x$ | −x | $\sigma_x = -1$ |
| R | $\uparrow_y$ | +y | $\sigma_y = +1$ |
| L | $\downarrow_y$ | −y | $\sigma_y = -1$ |

---

## Looking Ahead

**Next lecture (final of Chapter 2):**
- Time evolution: Schrödinger equation $i\hbar\frac{d}{dt}|\psi\rangle = H|\psi\rangle$
- Hamiltonians generate rotations
- Precession (rotation around z)
- Rabi oscillations (rotation around x)
- The Ramsey interferometer
- Atomic clocks: the qubit as a sensor