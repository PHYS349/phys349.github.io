# Lecture 2.5: Generators, Pauli Matrices, and SU(2)

## Review: From SO(2) to SO(3)

Last lecture we established:

-   **SO(2)** describes 2D rotations. One axis, one generator, commutative. The rotation matrix $R(\theta)$ satisfies $R(\theta_1)R(\theta_2) = R(\theta_2)R(\theta_1)$ — order doesn't matter.

-   **SO(3)** describes 3D rotations. Three axes, three generators, **non-commutative**. $R_x(\theta_1)R_z(\theta_2) \neq R_z(\theta_2)R_x(\theta_1)$ — order matters.

We also saw (in the homework) that an infinitesimal rotation can be written as:

$$R(\delta\theta) \approx I + \delta\theta\, G$$

where $G$ is called the **generator**. A finite rotation is the exponential:

$$R(\theta) = e^{\theta G}$$

For SO(2), there was one generator $G$ satisfying $G^2 = -I$, and the exponential gave us:

$$e^{\theta G} = \cos\theta\, I + \sin\theta\, G$$

Today we extend this to 3D, find the generators of SO(3), then move to the quantum version — SU(2) and the Pauli matrices.

------------------------------------------------------------------------

## Generators of SO(3)

### Infinitesimal Rotations

The 3D rotation matrices around the three axes are:

$$R_x(\theta) = \begin{pmatrix} 1 & 0 & 0 \\ 0 & \cos\theta & -\sin\theta \\ 0 & \sin\theta & \cos\theta \end{pmatrix}, \quad R_y(\theta) = \begin{pmatrix} \cos\theta & 0 & \sin\theta \\ 0 & 1 & 0 \\ -\sin\theta & 0 & \cos\theta \end{pmatrix}, \quad R_z(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{pmatrix}$$

For a small angle $\delta\theta$, we Taylor expand: $\cos\delta\theta \approx 1$ and $\sin\delta\theta \approx \delta\theta$:

$$R_x(\delta\theta) \approx \begin{pmatrix} 1 & 0 & 0 \\ 0 & 1 & -\delta\theta \\ 0 & \delta\theta & 1 \end{pmatrix} = I + \delta\theta \begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & -1 \\ 0 & 1 & 0 \end{pmatrix}$$

$$R_y(\delta\theta) \approx \begin{pmatrix} 1 & 0 & \delta\theta \\ 0 & 1 & 0 \\ -\delta\theta & 0 & 1 \end{pmatrix} = I + \delta\theta \begin{pmatrix} 0 & 0 & 1 \\ 0 & 0 & 0 \\ -1 & 0 & 0 \end{pmatrix}$$

$$R_z(\delta\theta) \approx \begin{pmatrix} 1 & -\delta\theta & 0 \\ \delta\theta & 1 & 0 \\ 0 & 0 & 1 \end{pmatrix} = I + \delta\theta \begin{pmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix}$$

### The Three Generators

Reading off the matrices that multiply $\delta\theta$, the three generators of SO(3) are:

$$J_x = \begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & -1 \\ 0 & 1 & 0 \end{pmatrix}, \qquad J_y = \begin{pmatrix} 0 & 0 & 1 \\ 0 & 0 & 0 \\ -1 & 0 & 0 \end{pmatrix}, \qquad J_z = \begin{pmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix}$$

Each generator is an antisymmetric matrix ($J^T = -J$), which follows from the orthogonality condition $R^T R = I$.

Notice the pattern: each generator has the SO(2) generator $\begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}$ embedded in the $2\times 2$ block corresponding to the plane of rotation, with zeros along the rotation axis. For example, $J_z$ rotates in the $x$-$y$ plane and has the familiar $2\times 2$ block in the upper left.

Finite rotations are exponentials of the generators:

$$R_x(\theta) = e^{\theta J_x}, \qquad R_y(\theta) = e^{\theta J_y}, \qquad R_z(\theta) = e^{\theta J_z}$$

### The Generators Don't Commute

The non-commutativity of 3D rotations shows up at the generator level. Let's compute:

$$J_x J_y = \begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & -1 \\ 0 & 1 & 0 \end{pmatrix}\begin{pmatrix} 0 & 0 & 1 \\ 0 & 0 & 0 \\ -1 & 0 & 0 \end{pmatrix} = \begin{pmatrix} 0 & 0 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix}$$

$$J_y J_x = \begin{pmatrix} 0 & 0 & 1 \\ 0 & 0 & 0 \\ -1 & 0 & 0 \end{pmatrix}\begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & -1 \\ 0 & 1 & 0 \end{pmatrix} = \begin{pmatrix} 0 & 1 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix}$$

The commutator is:

$$[J_x, J_y] = J_x J_y - J_y J_x = \begin{pmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix} = J_z$$

The full set of commutation relations (verify the others as an exercise):

$$\boxed{[J_x, J_y] = J_z, \qquad [J_y, J_z] = J_x, \qquad [J_z, J_x] = J_y}$$

Or compactly: $[J_i, J_j] = \epsilon_{ijk} J_k$, where $\epsilon_{ijk}$ is $+1$ for cyclic permutations (xyz, yzx, zxy), $-1$ for anti-cyclic, and $0$ if any indices repeat.

These commutation relations are the **defining structure** of rotations. Any set of three matrices satisfying these relations generates a rotation group. This is the key idea that connects SO(3) to SU(2).

------------------------------------------------------------------------

## The Stern-Gerlach Experiment

We've been developing abstract mathematics. Now let's see how nature hands us a quantum system that needs all of this machinery.

### The Setup (1922)

Otto Stern and Walther Gerlach sent a beam of silver atoms through an **inhomogeneous magnetic field** — a field that is stronger at the top than at the bottom. A magnetic dipole in a field gradient experiences a force proportional to its component along the field direction. Atoms with their magnetic moment pointing up get deflected up; atoms pointing down get deflected down.

### Classical vs. Quantum

**Classical prediction:** If the silver atoms have randomly oriented magnetic moments, each atom should be deflected by a different amount depending on its orientation. The beam should spread out into a **continuous band**.

**Quantum result:** The beam split into exactly **two discrete spots**.

Angular momentum is quantized. For the outermost electron in silver, it can only take two values. This is **spin-1/2**: the smallest nontrivial quantum angular momentum.

### Spin as a Qubit

The two spin states define a qubit:

$$|\uparrow\rangle \equiv |0\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}, \qquad |\downarrow\rangle \equiv |1\rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$$

The spin angular momentum along the $z$-axis can only be:

$$S_z = +\frac{\hbar}{2} \quad (\text{spin up}) \qquad \text{or} \qquad S_z = -\frac{\hbar}{2} \quad (\text{spin down})$$

### Rotating the Apparatus

If we orient the Stern-Gerlach apparatus along different axes, we measure different components of spin:

**Z-oriented:** Eigenstates $|\uparrow_z\rangle = |0\rangle$ and $|\downarrow_z\rangle = |1\rangle$.

**X-oriented:** Eigenstates $|\uparrow_x\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) = |+\rangle$ and $|\downarrow_x\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle) = |-\rangle$.

**Y-oriented:** Eigenstates $|\uparrow_y\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle) = |+i\rangle$ and $|\downarrow_y\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle) = |-i\rangle$.

This is exactly our polarization story — the same Bloch sphere, the same three bases.

### iClicker: Measuring Spin Along a Different Axis

**An electron is prepared in state** $|\uparrow_z\rangle = |0\rangle$ (spin-up along z). You measure its spin along the x-axis. What do you get?

-   

    (A) Always spin-up along x

-   

    (B) Always spin-down along x

-   

    (C) 50% up, 50% down ✓

-   

    (D) The measurement is undefined

**Solution:** Express $|0\rangle$ in the x-basis:

$$|0\rangle = \frac{1}{\sqrt{2}}|+\rangle + \frac{1}{\sqrt{2}}|-\rangle$$

$$P(\uparrow_x) = |\langle +|0\rangle|^2 = \frac{1}{2}, \qquad P(\downarrow_x) = |\langle -|0\rangle|^2 = \frac{1}{2}$$

A state with definite $S_z$ has completely uncertain $S_x$.

------------------------------------------------------------------------

## From SO(3) to SU(2)

### The Problem

SO(3) acts on real 3D vectors — positions in space, classical angular momenta. But qubits live in $\mathbb{C}^2$: a 2D complex vector space. We need a group that:

-   Acts on 2D complex vectors (the qubit state space)
-   Preserves the norm: $|c_0|^2 + |c_1|^2 = 1$
-   Has three generators with the same commutation relations as SO(3)

### SU(2): The Quantum Rotation Group

The group we need is **SU(2)**: the Special Unitary group in 2 dimensions.

-   **S** = Special: $\det(U) = 1$
-   **U** = Unitary: $U^\dagger U = I$
-   **2** = acts on 2D complex space ($\mathbb{C}^2$)

Compare to SO(2):

| Property    | SO(n)                         | SU(n)                            |
|------------------------------|---------------------|---------------------|
| Acts on     | Real vectors ($\mathbb{R}^n$) | Complex vectors ($\mathbb{C}^n$) |
| Preserves   | $R^T R = I$ (orthogonal)      | $U^\dagger U = I$ (unitary)      |
| Determinant | $\det R = 1$                  | $\det U = 1$                     |
| Entries     | Real                          | Complex                          |

Unitary is the complex generalization of orthogonal. Where orthogonality uses the transpose ($R^T$), unitarity uses the conjugate transpose ($U^\dagger$). Both preserve the length of vectors in their respective spaces.

### The Key Relationship

SU(2) and SO(3) are almost the same group. They have:

-   The same number of generators (three)
-   The same commutation relations
-   The same local structure

But they differ globally: SU(2) is a **double cover** of SO(3). A $2\pi$ rotation in SO(3) gives $R(2\pi) = I$ (identity), but in SU(2) it gives $R(2\pi) = -I$ (minus the identity). You need $4\pi$ to get back to $+I$ in SU(2).

Nature cares about this distinction deeply. Fermions (electrons, quarks) transform under SU(2) and pick up a $-1$ under $2\pi$ rotation. Bosons (photons) transform under SO(3) and return to $+1$. This is connected to the spin-statistics theorem.

------------------------------------------------------------------------

## The Pauli Matrices

The generators of SU(2) are the **Pauli matrices**:

$$\boxed{\sigma_x = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, \qquad \sigma_y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}, \qquad \sigma_z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}}$$

The spin angular momentum operators are $S_i = \frac{\hbar}{2}\sigma_i$.

### Property 1: Hermitian

$$\sigma_x^\dagger = \sigma_x, \qquad \sigma_y^\dagger = \sigma_y, \qquad \sigma_z^\dagger = \sigma_z$$

Hermitian matrices represent **observables** — quantities we can measure. Their eigenvalues are guaranteed to be real.

### Property 2: Traceless

$$\text{Tr}(\sigma_x) = 0 + 0 = 0, \qquad \text{Tr}(\sigma_y) = 0 + 0 = 0, \qquad \text{Tr}(\sigma_z) = 1 + (-1) = 0$$

### Property 3: Square to Identity

$$\sigma_x^2 = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = I$$

Similarly $\sigma_y^2 = I$ and $\sigma_z^2 = I$.

Since $\sigma_i^2 = I$, the eigenvalues must satisfy $\lambda^2 = 1$, giving $\lambda = \pm 1$.

### Eigenstates: The Six Cardinal Points

$\sigma_z$ eigenstates (z-axis):

$$\sigma_z|0\rangle = +1 \cdot |0\rangle, \qquad \sigma_z|1\rangle = -1 \cdot |1\rangle$$

$\sigma_x$ eigenstates (x-axis):

$$\sigma_x|+\rangle = +1 \cdot |+\rangle, \qquad \sigma_x|-\rangle = -1 \cdot |-\rangle$$

Let's verify the first one:

$$\sigma_x|+\rangle = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}\frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix} = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix} = +1 \cdot |+\rangle \quad \checkmark$$

$\sigma_y$ eigenstates (y-axis):

$$\sigma_y|{+i}\rangle = +1 \cdot |{+i}\rangle, \qquad \sigma_y|{-i}\rangle = -1 \cdot |{-i}\rangle$$

The eigenstates of the three Pauli matrices are exactly the six cardinal points on the Bloch sphere.

### The Complete Dictionary

| Pauli | $+1$ eigenstate | $-1$ eigenstate | Bloch axis | Spin | Polarization |
|------------|------------|------------|------------|------------|------------|
| $\sigma_z$ | $\|0\rangle$ | $\|1\rangle$ | z (poles) | $\uparrow_z / \downarrow_z$ | H / V |
| $\sigma_x$ | $\|+\rangle$ | $\|-\rangle$ | x | $\uparrow_x / \downarrow_x$ | D / A |
| $\sigma_y$ | $\|{+i}\rangle$ | $\|{-i}\rangle$ | y | $\uparrow_y / \downarrow_y$ | R / L |

------------------------------------------------------------------------

## Pauli Algebra

### Products of Pauli Matrices

Let's compute $\sigma_x \sigma_y$:

$$\sigma_x \sigma_y = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}\begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix} = \begin{pmatrix} i & 0 \\ 0 & -i \end{pmatrix} = i\sigma_z$$

And $\sigma_y \sigma_x$:

$$\sigma_y \sigma_x = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} = \begin{pmatrix} -i & 0 \\ 0 & i \end{pmatrix} = -i\sigma_z$$

These are **not equal**. The Pauli matrices don't commute.

### iClicker: Pauli Product

**What is** $\sigma_x \sigma_y$?

-   

    (A) $\sigma_z$

-   

    (B) $-\sigma_z$

-   

    (C) $i\sigma_z$ ✓

-   

    (D) $-i\sigma_z$

### Commutation Relations

The commutator $[A, B] = AB - BA$ captures the failure to commute:

$$[\sigma_x, \sigma_y] = \sigma_x\sigma_y - \sigma_y\sigma_x = i\sigma_z - (-i\sigma_z) = 2i\sigma_z$$

The full set of commutation relations:

$$\boxed{[\sigma_x, \sigma_y] = 2i\sigma_z, \qquad [\sigma_y, \sigma_z] = 2i\sigma_x, \qquad [\sigma_z, \sigma_x] = 2i\sigma_y}$$

Compactly: $[\sigma_i, \sigma_j] = 2i\epsilon_{ijk}\sigma_k$.

Compare to the SO(3) generators: $[J_i, J_j] = \epsilon_{ijk}J_k$. The same structure, with an extra factor of $2i$. This factor arises because the Pauli matrices are Hermitian (physicists' convention for quantum observables), while the SO(3) generators $J_i$ are antisymmetric. The underlying algebraic structure — the pattern of which commutator gives which generator — is identical.

### Anticommutation Relations

The anticommutator $\{A, B\} = AB + BA$ is:

$$\{\sigma_x, \sigma_y\} = \sigma_x\sigma_y + \sigma_y\sigma_x = i\sigma_z + (-i\sigma_z) = 0$$

In general:

$$\boxed{\{\sigma_i, \sigma_j\} = 2\delta_{ij}I}$$

Same Paulis anticommute to $2I$ (since $\sigma_i^2 = I$). Different Paulis anticommute to zero.

### The Master Formula

Combining the commutator and anticommutator:

$$\sigma_i\sigma_j = \frac{1}{2}\{\sigma_i,\sigma_j\} + \frac{1}{2}[\sigma_i,\sigma_j] = \delta_{ij}I + i\epsilon_{ijk}\sigma_k$$

This single formula captures everything about how Pauli matrices multiply.

### Physical Consequence: The Uncertainty Principle

If two operators don't commute, they can't be simultaneously diagonalized — they can't share eigenstates.

Since $[\sigma_x, \sigma_z] = -2i\sigma_y \neq 0$:

-   A state cannot be an eigenstate of both $\sigma_x$ and $\sigma_z$ simultaneously
-   If $S_z$ is definite (the state is $|0\rangle$ or $|1\rangle$), then $S_x$ is uncertain
-   If $S_x$ is definite (the state is $|+\rangle$ or $|-\rangle$), then $S_z$ is uncertain

We already saw this in the iClicker: $|0\rangle$ has definite $S_z = +\hbar/2$ but gives 50/50 results for $S_x$. The non-commutativity of the Pauli matrices is the mathematical origin of this complementarity.

**You cannot simultaneously know spin along two perpendicular axes.** This is fundamentally different from classical physics, where all components of angular momentum can be known at once.

------------------------------------------------------------------------

## Paulis as Generators of SU(2)

### Any Qubit Operator in Terms of Paulis

The set $\{I, \sigma_x, \sigma_y, \sigma_z\}$ forms a basis for all $2\times 2$ matrices. Any $2\times 2$ Hermitian matrix can be written:

$$H = h_0 I + h_x \sigma_x + h_y \sigma_y + h_z \sigma_z = h_0 I + \vec{h}\cdot\vec{\sigma}$$

where $h_0, h_x, h_y, h_z$ are real numbers and $\vec{\sigma} = (\sigma_x, \sigma_y, \sigma_z)$.

This means any qubit observable — any qubit Hamiltonian — can be decomposed into Pauli components.

### Rotations from the Generator

Following exactly the same logic as SO(2), a rotation by angle $\theta$ around axis $\hat{n} = (n_x, n_y, n_z)$ on the Bloch sphere is:

$$R_{\hat{n}}(\theta) = e^{-i\theta(\hat{n}\cdot\vec{\sigma})/2}$$

The factor of $1/2$ is because of spin-$1/2$ (the double cover). Let's expand this using the Taylor series, just as we did for SO(2).

### Deriving the Rotation Formula

Define $A = \hat{n}\cdot\vec{\sigma} = n_x\sigma_x + n_y\sigma_y + n_z\sigma_z$. First we need $A^2$.

Using the master formula $\sigma_i\sigma_j = \delta_{ij}I + i\epsilon_{ijk}\sigma_k$:

$$A^2 = \sum_{ij} n_i n_j \sigma_i\sigma_j = \sum_{ij} n_i n_j(\delta_{ij}I + i\epsilon_{ijk}\sigma_k)$$

The $\epsilon_{ijk}$ term vanishes: $\sum_{ij} n_i n_j \epsilon_{ijk} = 0$ because $n_i n_j$ is symmetric in $i,j$ while $\epsilon_{ijk}$ is antisymmetric. What remains:

$$A^2 = \sum_i n_i^2 I = |\hat{n}|^2 I = I$$

since $\hat{n}$ is a unit vector. So $(\hat{n}\cdot\vec{\sigma})^2 = I$, just like $\sigma_i^2 = I$ for each individual Pauli.

Now expand the exponential. Let $\alpha = \theta/2$:

$$e^{-i\alpha A} = I + (-i\alpha)A + \frac{(-i\alpha)^2}{2!}A^2 + \frac{(-i\alpha)^3}{3!}A^3 + \frac{(-i\alpha)^4}{4!}A^4 + \cdots$$

Using $A^2 = I$, so $A^3 = A$, $A^4 = I$, etc.:

$$= I - i\alpha A + \frac{(-i\alpha)^2}{2!}I + \frac{(-i\alpha)^3}{3!}A + \frac{(-i\alpha)^4}{4!}I + \cdots$$

$$= \left(1 - \frac{\alpha^2}{2!} + \frac{\alpha^4}{4!} - \cdots\right)I - i\left(\alpha - \frac{\alpha^3}{3!} + \frac{\alpha^5}{5!} - \cdots\right)A$$

$$= \cos\alpha\, I - i\sin\alpha\, A$$

Substituting back $\alpha = \theta/2$ and $A = \hat{n}\cdot\vec{\sigma}$:

$$\boxed{R_{\hat{n}}(\theta) = \cos\frac{\theta}{2}\, I - i\sin\frac{\theta}{2}\,(\hat{n}\cdot\vec{\sigma})}$$

Compare this to the SO(2) result $e^{\theta G} = \cos\theta\, I + \sin\theta\, G$. The structure is identical — the $-i$ and $\theta/2$ reflect the fact that the Pauli matrices are Hermitian (not antisymmetric) and that SU(2) is a double cover.

### Explicit Rotation Matrices

**Rotation around z-axis:**

$$R_z(\theta) = e^{-i\theta\sigma_z/2} = \cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,\sigma_z = \begin{pmatrix} \cos\frac{\theta}{2} - i\sin\frac{\theta}{2} & 0 \\ 0 & \cos\frac{\theta}{2} + i\sin\frac{\theta}{2} \end{pmatrix} = \begin{pmatrix} e^{-i\theta/2} & 0 \\ 0 & e^{i\theta/2} \end{pmatrix}$$

**Rotation around x-axis:**

$$R_x(\theta) = e^{-i\theta\sigma_x/2} = \cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,\sigma_x = \begin{pmatrix} \cos\frac{\theta}{2} & -i\sin\frac{\theta}{2} \\ -i\sin\frac{\theta}{2} & \cos\frac{\theta}{2} \end{pmatrix}$$

**Rotation around y-axis:**

$$R_y(\theta) = e^{-i\theta\sigma_y/2} = \cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,\sigma_y = \begin{pmatrix} \cos\frac{\theta}{2} & -\sin\frac{\theta}{2} \\ \sin\frac{\theta}{2} & \cos\frac{\theta}{2} \end{pmatrix}$$

### The Double Cover

Notice what happens at $\theta = 2\pi$:

$$R_{\hat{n}}(2\pi) = \cos\pi\, I - i\sin\pi\, (\hat{n}\cdot\vec{\sigma}) = -I$$

A full $2\pi$ rotation gives **minus the identity**, not the identity. The state picks up a global minus sign. At $\theta = 4\pi$:

$$R_{\hat{n}}(4\pi) = \cos 2\pi\, I - i\sin 2\pi\, (\hat{n}\cdot\vec{\sigma}) = +I$$

You need to rotate twice around to get back to where you started. This is the double cover of SO(3) by SU(2).

### Connection to Gates We Know

**Phase gate:**

$$P(\phi) = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\phi} \end{pmatrix} = e^{i\phi/2}\begin{pmatrix} e^{-i\phi/2} & 0 \\ 0 & e^{i\phi/2} \end{pmatrix} = e^{i\phi/2}\, R_z(\phi)$$

Up to global phase, the phase gate is a rotation around the z-axis. This is why $P(\phi)$ applied to $|+\rangle$ traces around the equator of the Bloch sphere — it's a z-rotation.

**Hadamard gate:**

$$H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix} = \frac{1}{\sqrt{2}}(\sigma_x + \sigma_z)$$

This is a $\pi$ rotation around the axis $\hat{n} = (\hat{x} + \hat{z})/\sqrt{2}$, halfway between $x$ and $z$:

$$H = -i\, R_{(\hat{x}+\hat{z})/\sqrt{2}}(\pi)$$

The Hadamard swaps the z-axis and the x-axis: $|0\rangle \leftrightarrow |+\rangle$ and $|1\rangle \leftrightarrow |-\rangle$.

**The Pauli gates X, Y, Z:**

The Pauli matrices themselves are $\pi$ rotations (up to global phase):

$$X = \sigma_x = -iR_x(\pi), \qquad Y = \sigma_y = -iR_y(\pi), \qquad Z = \sigma_z = -iR_z(\pi)$$

The X gate is a bit flip: $|0\rangle \leftrightarrow |1\rangle$ (rotation by $\pi$ around x, swaps the poles). The Z gate is a phase flip: $|+\rangle \leftrightarrow |-\rangle$ (rotation by $\pi$ around z).

### The Fundamental Result

$$\boxed{\text{Every single-qubit unitary is a rotation on the Bloch sphere.}}$$

Any $U \in SU(2)$ can be written as $U = e^{-i\theta(\hat{n}\cdot\vec{\sigma})/2}$ for some axis $\hat{n}$ and angle $\theta$. There are no other single-qubit gates — rotations are everything.

------------------------------------------------------------------------

## Summary

1.  **SO(3) generators:** Three $3\times 3$ antisymmetric matrices $J_x, J_y, J_z$ generate rotations around the three axes. They satisfy $[J_i, J_j] = \epsilon_{ijk}J_k$.

2.  **Stern-Gerlach:** Nature provides a qubit — electron spin-$1/2$. Two discrete outcomes, three measurement bases, same Bloch sphere structure as polarization.

3.  **SU(2):** The quantum rotation group. Acts on $\mathbb{C}^2$, preserves norm ($U^\dagger U = I$), three generators, non-commutative. Double cover of SO(3): $R(2\pi) = -I$.

4.  **Pauli matrices:** The generators of SU(2). Hermitian, traceless, $\sigma_i^2 = I$, eigenvalues $\pm 1$. Eigenstates are the six Bloch sphere cardinal points.

5.  **Pauli algebra:** $\sigma_i\sigma_j = \delta_{ij}I + i\epsilon_{ijk}\sigma_k$. Non-commutativity leads to the uncertainty principle: you can't know spin along two perpendicular axes simultaneously.

6.  **Rotations:** $R_{\hat{n}}(\theta) = \cos(\theta/2)I - i\sin(\theta/2)(\hat{n}\cdot\vec{\sigma})$. Every qubit gate is a rotation. Phase gate $\sim R_z$, Hadamard $\sim R_{(x+z)/\sqrt{2}}(\pi)$, Paulis $\sim R_i(\pi)$.

### The Generator of Rotations

Here's a powerful idea. Instead of thinking about finite rotations $R(\theta)$, think about infinitesimal rotations.

For small $\delta\theta$:

$$R(\delta\theta) = \begin{pmatrix} \cos\delta\theta & -\sin\delta\theta \\ \sin\delta\theta & \cos\delta\theta \end{pmatrix} \approx \begin{pmatrix} 1 & -\delta\theta \\ \delta\theta & 1 \end{pmatrix} = I + \delta\theta \begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}$$

Define the **generator**:

$$G = \begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}$$

Then: $$R(\delta\theta) \approx I + \delta\theta \cdot G$$

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

Continuing: - $G^3 = G^2 \cdot G = -G$ - $G^4 = G^2 \cdot G^2 = (-I)(-I) = I$ - $G^5 = G$, and so on...

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

\`\`\`{admonition} The Generator-Exponential Relationship :class: important

$$e^{\theta G} = \cos\theta \cdot I + \sin\theta \cdot G$$

Compare to Euler's formula: $$e^{i\theta} = \cos\theta + i\sin\theta$$

The matrix $G$ plays the role of $i$! Both satisfy: - $G^2 = -I$ (like $i^2 = -1$) - Exponentiating gives rotations

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

------------------------------------------------------------------------

## Part 4: The Stern-Gerlach Experiment

We've been doing abstract math. Now let's see how nature hands us a qubit.

### The Setup (1922)

Otto Stern and Walther Gerlach sent a beam of silver atoms through an **inhomogeneous magnetic field** — a field that's stronger at the top than at the bottom.

\`\`\`{figure} stern_gerlach_placeholder.svg :name: stern-gerlach :width: 80%

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

------------------------------------------------------------------------

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

$$\boxed{[\sigma_x, \sigma_y] = 2i\sigma_z}$$ $$\boxed{[\sigma_y, \sigma_z] = 2i\sigma_x}$$ $$\boxed{[\sigma_z, \sigma_x] = 2i\sigma_y}$$

Or compactly: $$[\sigma_i, \sigma_j] = 2i\epsilon_{ijk}\sigma_k$$

where the sum over $k$ is implied.

### The Anticommutator

We can also compute the anticommutator $\{A, B\} = AB + BA$:

$$\{\sigma_x, \sigma_y\} = \sigma_x\sigma_y + \sigma_y\sigma_x = i\sigma_z + (-i\sigma_z) = 0$$

In general: $$\{\sigma_i, \sigma_j\} = 2\delta_{ij}I$$

Different Paulis anticommute; same Paulis give $2I$.

### Combining Both

$$\sigma_i \sigma_j = \delta_{ij}I + i\epsilon_{ijk}\sigma_k$$

This single formula captures everything about Pauli multiplication!

------------------------------------------------------------------------

## iClicker Question 3

**What is** $\sigma_x \sigma_y$?

-   

    (A) $\sigma_z$

-   

    (B) $-\sigma_z$

-   

    (C) $i\sigma_z$ ✓

-   

    (D) $-i\sigma_z$

------------------------------------------------------------------------

### Why Non-Commuting Matters: The Uncertainty Principle

If two operators don't commute, they can't be simultaneously diagonalized — they can't share eigenstates.

Since $[\sigma_x, \sigma_z] = -2i\sigma_y \neq 0$:

-   A state can't be an eigenstate of both $\sigma_x$ and $\sigma_z$
-   If you know $S_z$ precisely, $S_x$ is uncertain
-   If you know $S_x$ precisely, $S_z$ is uncertain

This is the **uncertainty principle for spin**.

The state $|0\rangle$ is an eigenstate of $\sigma_z$ (eigenvalue +1), so it has definite $S_z = +\hbar/2$. But it's a 50/50 superposition of $|+\rangle$ and $|-\rangle$, so $S_x$ is completely uncertain.

\`\`\`{admonition} Uncertainty Principle :class: warning

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

``` python
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

------------------------------------------------------------------------

## Summary

Today we covered the mathematical foundation of qubits:

1.  **Quantum mechanics is fundamentally complex** — the $i$ in Schrödinger's equation is essential, not a trick

2.  **Complex numbers = SO(2) rotations**

    -   $e^{i\theta}$ rotates by angle $\theta$
    -   The generator $G$ satisfies $G^2 = -I$ (like $i^2 = -1$)
    -   Finite rotations: $R(\theta) = e^{\theta G}$

3.  **Qubits transform under SU(2)**

    -   Three generators (three axes)
    -   Non-commutative (order matters)
    -   Rotations of the Bloch sphere

4.  **Stern-Gerlach reveals spin**

    -   Nature hands us a qubit: spin-1/2
    -   Two discrete outcomes, not a continuum
    -   Rotating the apparatus = choosing measurement basis

5.  **The Pauli matrices** $\sigma_x$, $\sigma_y$, $\sigma_z$:

    -   Hermitian, traceless, square to identity
    -   Eigenvalues ±1
    -   Eigenstates = Bloch sphere axes (z, x, y)
    -   Non-commuting: $[\sigma_i, \sigma_j] = 2i\epsilon_{ijk}\sigma_k$

6.  **Paulis generate SU(2):**

    -   Any qubit Hamiltonian: $H = h_0 I + \vec{h} \cdot \vec{\sigma}$
    -   Rotations: $R_{\hat{n}}(\theta) = e^{-i\theta(\hat{n} \cdot \vec{\sigma})/2}$
    -   Every single-qubit gate is a rotation!

### Everything Connects

| Polarization | Spin           | Bloch sphere    | Pauli eigenstate |
|--------------|----------------|-----------------|------------------|
| H            | $\uparrow_z$   | +z (north pole) | $\sigma_z = +1$  |
| V            | $\downarrow_z$ | −z (south pole) | $\sigma_z = -1$  |
| D            | $\uparrow_x$   | +x              | $\sigma_x = +1$  |
| A            | $\downarrow_x$ | −x              | $\sigma_x = -1$  |
| R            | $\uparrow_y$   | +y              | $\sigma_y = +1$  |
| L            | $\downarrow_y$ | −y              | $\sigma_y = -1$  |

------------------------------------------------------------------------

## Looking Ahead

**Next lecture (final of Chapter 2):** - Time evolution: Schrödinger equation $i\hbar\frac{d}{dt}|\psi\rangle = H|\psi\rangle$ - Hamiltonians generate rotations - Precession (rotation around z) - Rabi oscillations (rotation around x) - The Ramsey interferometer - Atomic clocks: the qubit as a sensor