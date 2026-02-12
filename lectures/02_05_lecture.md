# Lecture 2.5: Generators, Pauli Matrices, and SU(2)

## What Is a Generator?

### Infinitesimal Rotations

We've been working with rotation matrices — $R(\theta)$ rotates a vector by angle $\theta$. But rotations can be understood more deeply by asking: what happens when the angle is very, very small?

Start with the 2D rotation matrix:

$$R(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}$$

For a tiny angle $\delta\theta$, Taylor expand: $\cos\delta\theta \approx 1$ and $\sin\delta\theta \approx \delta\theta$:

$$R(\delta\theta) \approx \begin{pmatrix} 1 & -\delta\theta \\ \delta\theta & 1 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} + \delta\theta\begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}$$

$$R(\delta\theta) \approx I + \delta\theta\, G$$

where:

$$G = \begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}$$

$G$ is called the **generator** of rotations. It tells you *which direction you move* when you begin to rotate. The identity $I$ says "stay where you are," and $\delta\theta\, G$ is the tiny nudge that starts the rotation.

### From Generator to Finite Rotation

How do you build a large rotation from the generator? The same way you walk a mile — one step at a time.

To rotate by a finite angle $\theta$, break it into $N$ tiny steps, each of size $\theta/N$:

$$R(\theta) = R(\theta/N) \cdot R(\theta/N) \cdots R(\theta/N) = \left[R(\theta/N)\right]^N$$

For large $N$, each step is infinitesimal, so $R(\theta/N) \approx I + \frac{\theta}{N}G$:

$$R(\theta) = \lim_{N \to \infty}\left(I + \frac{\theta}{N}G\right)^N$$

This limit is the definition of the matrix exponential:

$$\boxed{R(\theta) = e^{\theta G}}$$

A finite rotation is the exponential of the generator. This is one of the most important ideas in physics: **the generator encodes all rotations**. You don't need to memorize the full rotation matrix — just the generator, and exponentiation does the rest.

### Evaluating the Exponential

Let's verify this actually works. We need to compute $e^{\theta G}$ using the Taylor series:

$$e^{\theta G} = I + \theta G + \frac{\theta^2}{2!}G^2 + \frac{\theta^3}{3!}G^3 + \frac{\theta^4}{4!}G^4 + \cdots$$

First, what is $G^2$?

$$G^2 = \begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}\begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix} = \begin{pmatrix} -1 & 0 \\ 0 & -1 \end{pmatrix} = -I$$

So $G^2 = -I$. This is the key property of the generator. The higher powers follow immediately:

$$G^3 = G^2 \cdot G = (-I)G = -G$, \; G^4 = G^2 \cdot G^2 = (-I)(-I) = +I$$ $$G^5 = G^4 \cdot G = G$$

The pattern repeats with period 4: $I, G, -I, -G, I, G, \ldots$

Substituting into the Taylor series:

$$e^{\theta G} = I + \theta G - \frac{\theta^2}{2!}I - \frac{\theta^3}{3!}G + \frac{\theta^4}{4!}I + \frac{\theta^5}{5!}G - \cdots$$

Group the terms with $I$ and the terms with $G$:

$$= \left(1 - \frac{\theta^2}{2!} + \frac{\theta^4}{4!} - \cdots\right)I + \left(\theta - \frac{\theta^3}{3!} + \frac{\theta^5}{5!} - \cdots\right)G$$

These are the Taylor series for cosine and sine:

$$\boxed{e^{\theta G} = \cos\theta\, I + \sin\theta\, G}$$

Let's check:

$$e^{\theta G} = \cos\theta\begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} + \sin\theta\begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix} = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix} = R(\theta) \quad \checkmark$$

It works. The generator $G$ contains everything we need.

------------------------------------------------------------------------

## The Generator and the Imaginary Unit

### $G^2 = -I$ and $i^2 = -1$

We just found that the rotation generator satisfies $G^2 = -I$. Look at this property carefully — it's the matrix version of $i^2 = -1$.

This is not a coincidence. Recall Euler's formula for complex number rotations:

$$e^{i\theta} = \cos\theta + i\sin\theta$$

Now compare our matrix rotation formula:

$$e^{\theta G} = \cos\theta\ I + \sin\theta\ G$$

The structures are identical. Everywhere that $i$ appears in Euler's formula, $G$ appears in the matrix formula. Everywhere that 1 appears, $I$ appears.

**The generator** $G$ plays the role of $i$. Both satisfy the same algebraic property ($G^2 = -I$ and $i^2 = -1$), and both generate rotations through exponentiation.

### Two Representations of the Same Rotation

We saw last lecture that complex multiplication by $e^{i\theta}$ and matrix multiplication by $R(\theta)$ do the same thing to a vector. Now we see why: they are two representations of the same object, built from generators that obey the same algebra.

The complex number $i$ and the matrix $G$ are different representations of the same mathematical structure — the generator of 2D rotations.

|   | Complex numbers | Matrices |
|---------------|-------------------------------------|--------------------|
| Element | $e^{i\theta}$ | $R(\theta) = e^{\theta G}$ |
| Generator | $i$ | $G = \begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}$ |
| Key property | $i^2 = -1$ | $G^2 = -I$ |
| Rotation formula | $e^{i\theta} = \cos\theta + i\sin\theta$ | $e^{\theta G} = \cos\theta\, I + \sin\theta\, G$ |
| Acts on | $z \in \mathbb{C}$ | $(x, y)^T \in \mathbb{R}^2$ |

------------------------------------------------------------------------

## Generators of SO(3)

### From 2D to 3D: Things Get Complicated

In 2D there is one rotation axis and one generator. In 3D there are three rotation axes — $x$, $y$, and $z$ — so we expect three generators.

The full $3\times 3$ rotation matrices are:

$$R_x(\theta) = \begin{pmatrix} 1 & 0 & 0 \\ 0 & \cos\theta & -\sin\theta \\ 0 & \sin\theta & \cos\theta \end{pmatrix}, \quad R_y(\theta) = \begin{pmatrix} \cos\theta & 0 & \sin\theta \\ 0 & 1 & 0 \\ -\sin\theta & 0 & \cos\theta \end{pmatrix}, \quad R_z(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{pmatrix}$$

These are already getting unwieldy. Multiplying two of them together — say to check if they commute — would be a mess of trig identities. And we already established last lecture that 3D rotations *don't* commute.

The generator approach cuts through this complexity. Instead of working with the full rotation matrices, we extract the generators — much simpler objects that encode all the essential information.

### Extracting the Generators

Taylor expand each rotation matrix for small $\delta\theta$:

$$R_x(\delta\theta) \approx I + \delta\theta \begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & -1 \\ 0 & 1 & 0 \end{pmatrix}, \quad R_y(\delta\theta) \approx I + \delta\theta \begin{pmatrix} 0 & 0 & 1 \\ 0 & 0 & 0 \\ -1 & 0 & 0 \end{pmatrix}, \quad R_z(\delta\theta) \approx I + \delta\theta \begin{pmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix}$$

The three generators are:

$$J_x = \begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & -1 \\ 0 & 1 & 0 \end{pmatrix}, \qquad J_y = \begin{pmatrix} 0 & 0 & 1 \\ 0 & 0 & 0 \\ -1 & 0 & 0 \end{pmatrix}, \qquad J_z = \begin{pmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix}$$

Notice the pattern: each generator has the familiar SO(2) block $\begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}$ embedded in the $2\times 2$ subspace corresponding to its plane of rotation, with zeros along the rotation axis. For example, $J_z$ rotates in the $x$-$y$ plane, so the block sits in the upper-left corner. $J_x$ rotates in the $y$-$z$ plane, so the block sits in the lower-right corner.

Finite rotations are exponentials:

$$R_x(\theta) = e^{\theta J_x}, \qquad R_y(\theta) = e^{\theta J_y}, \qquad R_z(\theta) = e^{\theta J_z}$$

------------------------------------------------------------------------

## SO(3) Commutation Relations

The non-commutativity of 3D rotations — which was messy to show with the full rotation matrices — becomes clean and elegant at the generator level. Let's compute the commutator $[J_x, J_y] = J_x J_y - J_y J_x$:

$$J_x J_y = \begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & -1 \\ 0 & 1 & 0 \end{pmatrix}\begin{pmatrix} 0 & 0 & 1 \\ 0 & 0 & 0 \\ -1 & 0 & 0 \end{pmatrix} = \begin{pmatrix} 0 & 0 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix}$$

$$J_y J_x = \begin{pmatrix} 0 & 0 & 1 \\ 0 & 0 & 0 \\ -1 & 0 & 0 \end{pmatrix}\begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & -1 \\ 0 & 1 & 0 \end{pmatrix} = \begin{pmatrix} 0 & 1 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix}$$

$$[J_x, J_y] = \begin{pmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix} = J_z$$

The commutator of two generators gives the third! The full set of commutation relations:

$$\boxed{[J_x, J_y] = J_z, \qquad [J_y, J_z] = J_x, \qquad [J_z, J_x] = J_y}$$

Compactly: $[J_i, J_j] = \epsilon_{ijk}J_k$, where $\epsilon_{ijk} = +1$ for cyclic permutations (xyz, yzx, zxy), $-1$ for anti-cyclic, and $0$ if any indices repeat.

These commutation relations are profound. They capture the **entire structure** of 3D rotations in a simple algebraic statement. Any set of three objects satisfying these relations generates a rotation group — even if those objects are $2\times 2$ matrices instead of $3\times 3$ matrices. This is the key insight that will connect SO(3) to SU(2).

The physical consequence of non-commuting generators is the **uncertainty principle**: if two physical quantities are described by non-commuting operators, they cannot both be known simultaneously. We'll make this precise shortly.

------------------------------------------------------------------------

## From SO(3) to SU(2)

### A Different Kind of Space

SO(3) describes rotations of real 3D vectors — positions, velocities, electric field directions. These are vectors in $\mathbb{R}^3$.

But throughout this course, we've been working with a different kind of object: a qubit.

$$|\psi\rangle = c_0|0\rangle + c_1|1\rangle$$

This is a two-configuration system with **complex** amplitudes. The state is a vector in $\mathbb{C}^2$, and we've been visualizing it on the Bloch sphere. When we apply gates — Hadamard, phase gates, Pauli matrices — we rotate the state on the Bloch sphere.

We need a group that describes these rotations: transformations of $\mathbb{C}^2$ that preserve the normalization $|c_0|^2 + |c_1|^2 = 1$.

### Defining SU(2)

This group is **SU(2)**: the group of $2\times 2$ unitary matrices with determinant 1.

-   **S** = Special: $\det(U) = 1$
-   **U** = Unitary: $U^\dagger U = I$
-   **2** = $2\times 2$ complex matrices

Unitarity is the complex generalization of orthogonality:

|   | SO(n) | SU(n) |
|-----------------|----------------------------|----------------------------|
| Acts on | Real vectors ($\mathbb{R}^n$) | Complex vectors ($\mathbb{C}^n$) |
| Preserves | $R^T R = I$ (transpose) | $U^\dagger U = I$ (conjugate transpose) |
| Entries | Real | Complex |
| Determinant | 1 | 1 |

Both preserve lengths. Orthogonal matrices preserve $\vec{v}^T\vec{v}$. Unitary matrices preserve $\vec{v}^\dagger\vec{v} = |c_0|^2 + |c_1|^2$ — exactly the normalization condition we need for quantum states.

### What We Expect

SU(2) should behave like SO(3) — after all, both describe rotations on a sphere. The Bloch sphere has three axes, so SU(2) should have **three generators**, and those generators should satisfy the same commutation relations as the SO(3) generators $J_x, J_y, J_z$.

But SU(2) acts on complex space, so the generators will be $2\times 2$ complex matrices instead of $3\times 3$ real matrices.

------------------------------------------------------------------------

## The Pauli Matrices: Generators of SU(2)

The three generators of SU(2) are the **Pauli matrices**:

$$\boxed{\sigma_x = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, \qquad \sigma_y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}, \qquad \sigma_z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}}$$

The spin angular momentum operators are related by $S_i = \frac{\hbar}{2}\sigma_i$.

### Key Properties

**Hermitian:** $\sigma_i^\dagger = \sigma_i$. Hermitian matrices represent observables — quantities we can measure. Their eigenvalues are guaranteed to be real.

**Traceless:** $\text{Tr}(\sigma_x) = \text{Tr}(\sigma_y) = \text{Tr}(\sigma_z) = 0$.

**Square to identity:** $\sigma_x^2 = \sigma_y^2 = \sigma_z^2 = I$. Let's verify one:

$$\sigma_x^2 = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = I$$

Since $\sigma_i^2 = I$, the eigenvalues satisfy $\lambda^2 = 1$, giving $\lambda = \pm 1$ for every Pauli matrix.

### Eigenstates: The Six Cardinal States

Each Pauli matrix has eigenvalues $+1$ and $-1$. The eigenstates are:

$\sigma_z$:

$$\sigma_z|0\rangle = +1\cdot|0\rangle, \qquad \sigma_z|1\rangle = -1\cdot|1\rangle$$

$\sigma_x$:

$$\sigma_x|+\rangle = +1\cdot|+\rangle, \qquad \sigma_x|-\rangle = -1\cdot|-\rangle$$

where $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ and $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$.

$\sigma_y$:

$$\sigma_y|{+i}\rangle = +1\cdot|{+i}\rangle, \qquad \sigma_y|{-i}\rangle = -1\cdot|{-i}\rangle$$

where $|{+i}\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ and $|{-i}\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$.

These are exactly the six cardinal points on the Bloch sphere:

| Pauli      | $+1$ eigenstate           | $-1$ eigenstate           | Bloch axis |
|----------------|--------------------|--------------------|----------------|
| $\sigma_z$ | $\|0\rangle$ (north pole) | $\|1\rangle$ (south pole) | z          |
| $\sigma_x$ | $\|+\rangle$              | $\|-\rangle$              | x          |
| $\sigma_y$ | $\|{+i}\rangle$           | $\|{-i}\rangle$           | y          |

### Pauli Commutation Relations

Now the crucial check: do the Pauli matrices satisfy the same commutation relations as the SO(3) generators?

Compute $\sigma_x\sigma_y$:

$$\sigma_x\sigma_y = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}\begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix} = \begin{pmatrix} i & 0 \\ 0 & -i \end{pmatrix} = i\sigma_z$$

Compute $\sigma_y\sigma_x$:

$$\sigma_y\sigma_x = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} = \begin{pmatrix} -i & 0 \\ 0 & i \end{pmatrix} = -i\sigma_z$$

### iClicker: Pauli Products

**What is** $\sigma_x\sigma_y$?

-   

    (A) $\sigma_z$

-   

    (B) $-\sigma_z$

-   

    (C) $i\sigma_z$ ✓

-   

    (D) $-i\sigma_z$

### Commutators

$$[\sigma_x, \sigma_y] = \sigma_x\sigma_y - \sigma_y\sigma_x = i\sigma_z - (-i\sigma_z) = 2i\sigma_z$$

The full set:

$$\boxed{[\sigma_x, \sigma_y] = 2i\sigma_z, \qquad [\sigma_y, \sigma_z] = 2i\sigma_x, \qquad [\sigma_z, \sigma_x] = 2i\sigma_y}$$

Compactly: $[\sigma_i, \sigma_j] = 2i\epsilon_{ijk}\sigma_k$.

Compare to SO(3): $[J_i, J_j] = \epsilon_{ijk}J_k$. **The same structure** — just with a factor of $2i$. This factor is a convention: the Pauli matrices are Hermitian (so they can be observables), while the SO(3) generators are antisymmetric. The underlying pattern — which commutator gives which generator — is identical.

SU(2) and SO(3) share the same algebraic DNA.

### Anticommutators

The anticommutator $\{A, B\} = AB + BA$ captures the symmetric part:

$$\{\sigma_x, \sigma_y\} = i\sigma_z + (-i\sigma_z) = 0$$

Different Pauli matrices anticommute to zero. Same Pauli matrices give $\{\sigma_i, \sigma_i\} = 2I$. In general:

$$\boxed{\{\sigma_i, \sigma_j\} = 2\delta_{ij}\,I}$$

### The Master Formula

Adding the commutator and anticommutator:

$$\boxed{\sigma_i\sigma_j = \delta_{ij}\,I + i\epsilon_{ijk}\sigma_k}$$

If $i = j$: you get $I$ (squaring gives the identity). If $i \neq j$: you get $\pm i$ times the third Pauli matrix, with the sign given by cyclic ordering.

### Physical Consequence: The Uncertainty Principle

Non-commuting generators mean that the corresponding physical quantities cannot be simultaneously known.

Since $[\sigma_x, \sigma_z] = -2i\sigma_y \neq 0$:

-   No state can be an eigenstate of both $\sigma_x$ and $\sigma_z$
-   If $S_z$ is definite ($|0\rangle$ or $|1\rangle$), then $S_x$ is completely uncertain
-   If $S_x$ is definite ($|+\rangle$ or $|-\rangle$), then $S_z$ is completely uncertain

**You cannot simultaneously know the spin along two perpendicular axes.** This is fundamentally different from classical physics, where all components of angular momentum can be known at once. The non-commutativity of the Pauli matrices — the same non-commutativity we saw for 3D rotations — is the mathematical origin of quantum uncertainty.

------------------------------------------------------------------------

## The Double Cover: What Does $R(2\pi) = -I$ Mean?

### SU(2) $\neq$ SO(3)

SU(2) and SO(3) have the same commutation relations, but they are not the same group. They differ in a fundamental way.

In SO(3), a $2\pi$ rotation around any axis brings you back to the identity:

$$R_{SO(3)}(2\pi) = +I$$

This makes intuitive sense — spin a ball $360°$ and it's back where it started.

In SU(2), a $2\pi$ rotation gives **minus** the identity:

$$R_{SU(2)}(2\pi) = -I$$

You need to rotate by $4\pi$ — two full turns — to get back to $+I$. SU(2) is the **double cover** of SO(3): it wraps around twice for every single winding of SO(3).

### What Does the $-1$ Mean Physically?

In quantum mechanics, the state of a system is described by a complex amplitude:

$$|\psi\rangle = c_0|0\rangle + c_1|1\rangle$$

If $|\psi\rangle \to -|\psi\rangle$, every amplitude picks up a minus sign: $c_0 \to -c_0$, $c_1 \to -c_1$. That $-1$ is a **phase** — specifically, it's the phase $e^{i\pi} = -1$.

For a single isolated state, this phase is unobservable. The probabilities $|c_0|^2$ and $|c_1|^2$ don't change when you multiply by $-1$.

**But in a superposition, the phase matters.** Consider a state that is a superposition of a rotated part and an unrotated part. The rotated part picks up $-1$, the unrotated part doesn't. Where the two parts used to add constructively (interference maximum), they now add destructively (interference minimum). Where they used to cancel (minimum), they now reinforce (maximum).

**The interference pattern shifts.** The $-1$ is physically real.

This has been confirmed experimentally. In neutron interferometry experiments, a neutron beam is split, one path is rotated by $2\pi$ using a magnetic field, and the beams are recombined. The interference pattern shifts — exactly as predicted by the $-1$ phase from SU(2). The neutron must be rotated by $4\pi$ to restore the original interference pattern.

### Fermions vs. Bosons

Nature has two kinds of particles:

**Fermions** (electrons, quarks, neutrinos — the matter particles) have half-integer spin and transform under SU(2): $R(2\pi) = -1$.

**Bosons** (photons, gluons, the Higgs — the force carriers) have integer spin and transform under SO(3): $R(2\pi) = +1$.

This distinction is connected to the spin-statistics theorem: fermions obey the Pauli exclusion principle (no two can occupy the same state), while bosons don't. The $-1$ under $2\pi$ rotation and the exclusion principle are two manifestations of the same deep mathematical structure. More on this later in the course.

------------------------------------------------------------------------

## Rotations on the Bloch Sphere

### The General Rotation

A rotation by angle $\theta$ around a unit axis $\hat{n} = (n_x, n_y, n_z)$ on the Bloch sphere is:

$$R_{\hat{n}}(\theta) = e^{-i\theta(\hat{n}\cdot\vec{\sigma})/2}$$

where $\hat{n}\cdot\vec{\sigma} = n_x\sigma_x + n_y\sigma_y + n_z\sigma_z$.

The factor of $1/2$ is because of the double cover — SU(2) matrices make half-angle rotations on the Bloch sphere.

### Deriving the Closed Form

We can evaluate this exponential using the same Taylor series technique that worked for SO(2). Define $A = \hat{n}\cdot\vec{\sigma}$.

**Step 1:** Compute $A^2$ using the master formula $\sigma_i\sigma_j = \delta_{ij}I + i\epsilon_{ijk}\sigma_k$:

$$A^2 = \sum_{i,j}n_in_j\,\sigma_i\sigma_j = \sum_{i,j}n_in_j(\delta_{ij}I + i\epsilon_{ijk}\sigma_k)$$

The symmetric piece gives $\sum_i n_i^2\,I = |\hat{n}|^2\,I = I$. The antisymmetric piece $\sum_{ij}n_in_j\epsilon_{ijk}$ vanishes because $n_in_j$ is symmetric while $\epsilon_{ijk}$ is antisymmetric.

$$(\hat{n}\cdot\vec{\sigma})^2 = I$$

Just like $\sigma_i^2 = I$ for each individual Pauli, the combination $(\hat{n}\cdot\vec\sigma)^2 = I$ for any unit vector $\hat{n}$.

**Step 2:** Higher powers cycle: $A^2 = I$, $A^3 = A$, $A^4 = I$, ... — exactly the same pattern as $G$ in SO(2).

**Step 3:** Taylor expand with $\alpha = \theta/2$:

$$e^{-i\alpha A} = I + (-i\alpha)A + \frac{(-i\alpha)^2}{2!}I + \frac{(-i\alpha)^3}{3!}A + \frac{(-i\alpha)^4}{4!}I + \cdots$$

**Step 4:** Group even powers ($I$) and odd powers ($A$):

$$= \left(1 - \frac{\alpha^2}{2!} + \frac{\alpha^4}{4!} - \cdots\right)I + (-i)\left(\alpha - \frac{\alpha^3}{3!} + \frac{\alpha^5}{5!} - \cdots\right)A$$

$$= \cos\alpha\,I - i\sin\alpha\,A$$

**Step 5:** Substitute back $\alpha = \theta/2$:

$$\boxed{R_{\hat{n}}(\theta) = \cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,(\hat{n}\cdot\vec{\sigma})}$$

Compare to our SO(2) result: $e^{\theta G} = \cos\theta\,I + \sin\theta\,G$. The same structure — the generator multiplied by sine, the identity multiplied by cosine. The $-i$ and $\theta/2$ reflect the Hermitian convention and the double cover.

**Verification:** At $\theta = 2\pi$: $R_{\hat{n}}(2\pi) = \cos\pi\,I - i\sin\pi\,(\hat{n}\cdot\vec\sigma) = -I$. ✓

------------------------------------------------------------------------

## Explicit Rotations and Gate Connections

### Rotation Around the $z$-Axis

$$R_z(\theta) = \cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,\sigma_z = \begin{pmatrix} e^{-i\theta/2} & 0 \\ 0 & e^{i\theta/2} \end{pmatrix}$$

**Connection to the phase gate:**

$$P(\phi) = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\phi} \end{pmatrix} = e^{i\phi/2}\begin{pmatrix} e^{-i\phi/2} & 0 \\ 0 & e^{i\phi/2} \end{pmatrix} = e^{i\phi/2}\,R_z(\phi)$$

Up to a global phase (which is unobservable), the phase gate IS a z-rotation. This is why applying $P(\phi)$ to $|+\rangle$ traces the equator of the Bloch sphere — it rotates around the z-axis.

**Connection to the Z gate:**

$$Z = \sigma_z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix} = iR_z(\pi)$$

The Z gate is a $\pi$ rotation around the z-axis. It maps $|+\rangle \to |-\rangle$ and $|-\rangle \to |+\rangle$ — it flips the equatorial states.

### Rotation Around the $x$-Axis

$$R_x(\theta) = \cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,\sigma_x = \begin{pmatrix} \cos\frac{\theta}{2} & -i\sin\frac{\theta}{2} \\ -i\sin\frac{\theta}{2} & \cos\frac{\theta}{2} \end{pmatrix}$$

**Connection to the X gate:**

$$X = \sigma_x = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} = iR_x(\pi)$$

The X gate is a $\pi$ rotation around the x-axis. It swaps $|0\rangle \leftrightarrow |1\rangle$ — the quantum NOT gate, flipping the north and south poles.

### Rotation Around the $y$-Axis

$$R_y(\theta) = \cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,\sigma_y = \begin{pmatrix} \cos\frac{\theta}{2} & -\sin\frac{\theta}{2} \\ \sin\frac{\theta}{2} & \cos\frac{\theta}{2} \end{pmatrix}$$

Note: $R_y(\theta)$ is a real matrix. This is because $-i\sigma_y = \begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}$, which is the SO(2) generator $G$. So y-rotations on the Bloch sphere look exactly like classical 2D rotations.

**Connection to the Y gate:**

$$Y = \sigma_y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix} = iR_y(\pi)$$

The Y gate is a $\pi$ rotation around the y-axis.

### The Hadamard Gate

The Hadamard doesn't correspond to a rotation around a coordinate axis. Instead:

$$H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix} = \frac{1}{\sqrt{2}}(\sigma_x + \sigma_z)$$

This is a $\pi$ rotation around the axis $\hat{n} = (\hat{x} + \hat{z})/\sqrt{2}$, halfway between $x$ and $z$ on the Bloch sphere. Geometrically, it swaps the z-axis and the x-axis:

$$H|0\rangle = |+\rangle, \quad H|1\rangle = |-\rangle, \quad H|+\rangle = |0\rangle, \quad H|-\rangle = |1\rangle$$

### The Fundamental Result

$$\boxed{\text{Every single-qubit unitary is a rotation on the Bloch sphere.}}$$

Any $U \in$ SU(2) can be written as $R_{\hat{n}}(\theta) = e^{-i\theta(\hat{n}\cdot\vec\sigma)/2}$ for some axis $\hat{n}$ and angle $\theta$. There are no other single-qubit gates. The three Pauli matrices generate all possible qubit operations through rotation.

------------------------------------------------------------------------

## Summary

| Concept | SO(2) | SO(3) | SU(2) |
|---------------------|-----------------|-----------------|-----------------|
| Dimension | 1 axis | 3 axes | 3 axes |
| Acts on | $\mathbb{R}^2$ | $\mathbb{R}^3$ | $\mathbb{C}^2$ |
| Generators | $G$ | $J_x, J_y, J_z$ | $\sigma_x, \sigma_y, \sigma_z$ |
| Key property | $G^2 = -I$ | $[J_i,J_j] = \epsilon_{ijk}J_k$ | $[\sigma_i,\sigma_j] = 2i\epsilon_{ijk}\sigma_k$ |
| Commutative? | Yes | No | No |
| $R(2\pi)$ | $+I$ | $+I$ | $-I$ |

### Key Results

1.  **Generators** encode infinitesimal rotations. Exponentiating gives finite rotations: $R(\theta) = e^{\theta G}$.

2.  $G$ and $i$ are the same thing in different representations. $G^2 = -I$ is the matrix version of $i^2 = -1$. Both generate rotations.

3.  **SO(3)** has three generators $J_x, J_y, J_z$ satisfying $[J_i, J_j] = \epsilon_{ijk}J_k$. Non-commutativity of generators → uncertainty principle.

4.  **SU(2)** is the quantum rotation group. Its generators — the **Pauli matrices** — satisfy the same commutation relations as SO(3). They are Hermitian (observables), traceless, and square to the identity (eigenvalues $\pm 1$). Their eigenstates are the six Bloch sphere cardinal states.

5.  **The double cover:** $R_{SU(2)}(2\pi) = -I$. The minus sign is a physical phase that shifts interference patterns. Fermions transform under SU(2); bosons under SO(3).

6.  **Every qubit gate is a Bloch sphere rotation:**

$$R_{\hat{n}}(\theta) = \cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,(\hat{n}\cdot\vec\sigma)$$

Phase gate $\sim R_z$. X gate $\sim R_x(\pi)$. Hadamard $\sim R_{(\hat{x}+\hat{z})/\sqrt{2}}(\pi)$.

### Next Lecture

The Stern-Gerlach experiment: nature hands us a real physical qubit. Electron spin, measurement in different bases, and the birth of the qubit.

------------------------------------------------------------------------

## Homework 2.5

### Problem 1: A Different Kind of Generator — Lorentz Boosts

Not all generators produce rotations. In special relativity, a **Lorentz boost** along the x-direction by rapidity $\beta$ is described by:

$$B(\beta) = \begin{pmatrix} \cosh\beta & \sinh\beta \\ \sinh\beta & \cosh\beta \end{pmatrix}$$

This matrix mixes space and time coordinates, just as a rotation matrix mixes $x$ and $y$.

**(a)** Verify that the set of boost matrices $\{B(\beta)\}$ forms a group: show $B(\beta_1)B(\beta_2) = B(\beta_1 + \beta_2)$, identify the identity element and the inverse of $B(\beta)$.

*Hint:* You will need the hyperbolic addition formulas: $\cosh(\alpha+\beta) = \cosh\alpha\cosh\beta + \sinh\alpha\sinh\beta$ and $\sinh(\alpha+\beta) = \sinh\alpha\cosh\beta + \cosh\alpha\sinh\beta$.

**(b)** Find the generator $K$ by expanding $B(\delta\beta)$ for small $\delta\beta$ (use $\cosh\delta\beta \approx 1$, $\sinh\delta\beta \approx \delta\beta$):

$$B(\delta\beta) \approx I + \delta\beta\, K$$

Write out $K$.

**(c)** Compute $K^2$. Compare to the rotation generator where $G^2 = -I$. What is different?

**(d)** Using $K^2 = +I$, expand the exponential $e^{\beta K}$ as a Taylor series. Group even and odd powers (as we did in lecture for $e^{\theta G}$). What functions appear instead of cosine and sine?

**(e)** Verify that your result matches $B(\beta)$.

**(f)** Compute $\det B(\beta)$. Compare to $\det R(\theta) = 1$. Is a Lorentz boost a rotation?

**(g)** Reflect on the difference: $G^2 = -I$ gives oscillatory behavior ($\cos, \sin$) — rotations go around in circles. $K^2 = +I$ gives hyperbolic behavior ($\cosh, \sinh$) — boosts grow without bound. What physical difference does this correspond to? (Can you rotate forever? Can you boost forever?)

------------------------------------------------------------------------

### Problem 2: The Pauli Vector Identity

**(a)** Using the master formula $\sigma_i\sigma_j = \delta_{ij}I + i\epsilon_{ijk}\sigma_k$, prove the identity:

$$(\vec{a}\cdot\vec\sigma)(\vec{b}\cdot\vec\sigma) = (\vec{a}\cdot\vec{b})\,I + i(\vec{a}\times\vec{b})\cdot\vec\sigma$$

where $\vec{a}$ and $\vec{b}$ are arbitrary real 3-vectors.

*Hint:* Write $\vec{a}\cdot\vec\sigma = \sum_i a_i\sigma_i$ and $\vec{b}\cdot\vec\sigma = \sum_j b_j\sigma_j$, multiply, and use the master formula on $\sigma_i\sigma_j$. You will need to recognize $\sum_i a_i b_i = \vec{a}\cdot\vec{b}$ and $\sum_{ijk}\epsilon_{ijk}a_i b_j = (\vec{a}\times\vec{b})_k$.

**(b)** As a special case, set $\vec{a} = \vec{b} = \hat{n}$ (a unit vector). Show that $(\hat{n}\cdot\vec\sigma)^2 = I$.

**(c)** Set $\vec{a} = \hat{x}$ and $\vec{b} = \hat{y}$. Evaluate both sides of the identity and verify they match.

**(d)** Set $\vec{a} = \vec{b} = \hat{x} + \hat{z}$ (not a unit vector). What is $[(\hat{x}+\hat{z})\cdot\vec\sigma]^2$? How does this relate to the Hadamard gate?

------------------------------------------------------------------------

### Problem 3: Mystery Matrix

Consider the unitary matrix:

$$U = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & -i \\ -i & 1 \end{pmatrix}$$

**(a)** Verify that $U$ is unitary: show $U^\dagger U = I$.

**(b)** Verify that $\det U = 1$, confirming $U \in$ SU(2).

**(c)** Every element of SU(2) can be written as:

$$U = \cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,(\hat{n}\cdot\vec\sigma)$$

By comparing $U$ to this formula, determine the rotation angle $\theta$ and axis $\hat{n}$.

*Hint:* The coefficient of $I$ gives $\cos(\theta/2)$. The coefficients of $\sigma_x$, $\sigma_y$, $\sigma_z$ in the remainder give $\sin(\theta/2)\, n_x$, $\sin(\theta/2)\, n_y$, $\sin(\theta/2)\, n_z$.

**(d)** Describe geometrically what this rotation does on the Bloch sphere. Where does it send $|0\rangle$? Where does it send $|+\rangle$?

**(e)** Compute $U^2$. What rotation is this? What about $U^4$?

------------------------------------------------------------------------

### Problem 4: Visualizing Rotations on the Bloch Sphere (Qiskit)

In this problem you'll use Qiskit to see rotations in action.

**Setup:** You will need the following imports:

``` python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_bloch_multivector, plot_bloch_vector
import numpy as np
import matplotlib.pyplot as plt
```

**(a)** Start with $|0\rangle$ and apply $R_y(\theta)$ for $\theta = 0, \pi/6, \pi/3, \pi/2, 2\pi/3, 5\pi/6, \pi$. For each value of $\theta$: - Create a circuit with `qc.ry(theta, 0)` - Extract the statevector - Visualize on the Bloch sphere (or record the Bloch coordinates)

Describe the path traced out. What curve on the Bloch sphere is this?

**(b)** Now start with $|0\rangle$, apply $R_y(\pi/2)$ to move to the equator, and then apply $R_z(\phi)$ for $\phi = 0, \pi/4, \pi/2, 3\pi/4, \pi, 5\pi/4, 3\pi/2, 7\pi/4$. Describe the path.

**(c)** Starting from $|0\rangle$, apply the following sequences and plot each final state on the Bloch sphere: 1. $R_y(\pi/2)$ 2. $R_y(\pi/2)$ then $R_z(\pi/2)$ 3. $R_z(\pi/2)$ then $R_y(\pi/2)$

Are sequences 2 and 3 the same? Relate to what we learned about non-commutativity.

**(d)** **The path of a general rotation.** Choose an axis $\hat{n} = (1, 1, 1)/\sqrt{3}$ and apply $R_{\hat{n}}(\theta)$ for $\theta$ from $0$ to $2\pi$ in 20 steps. Start from $|0\rangle$. Plot all 20 states on a single Bloch sphere.

*Hint:* In Qiskit, use `qc.r(theta, phi, 0)` where `phi` is the azimuthal angle of $\hat{n}$, or construct the rotation matrix manually and use `qc.unitary(U, 0)`.

------------------------------------------------------------------------

### Problem 5: Phase Gate = Z-Rotation (Qiskit)

This problem tests the lecture's claim that the phase gate $P(\phi)$ and the rotation $R_z(\phi)$ are the same up to a global phase.

**Setup:**

``` python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit_aer import AerSimulator
import numpy as np
```

**(a)** Create two circuits that act on $|+\rangle = H|0\rangle$:

*Circuit A:* Apply $H$, then $P(\pi/3)$.

*Circuit B:* Apply $H$, then $R_z(\pi/3)$.

Use `Statevector.from_instruction(qc)` to extract the output states. Print both statevectors. Are they the same?

**(b)** Compute the ratio of corresponding amplitudes: $(\text{state A})_0 / (\text{state B})_0$ and $(\text{state A})_1 / (\text{state B})_1$. Show that the ratio is the same for both components — this is the global phase $e^{i\phi/2}$.

**(c)** Add measurements to both circuits. Run each on `AerSimulator()` with 10,000 shots. Compare the measurement statistics. Are they distinguishable?

**(d)** Repeat parts (a)–(c) for $\phi = \pi$ (where $P(\pi) = Z$ and $R_z(\pi) = -iZ$). Verify that the measurement statistics are still identical despite different global phases.

**(e)** Explain in your own words: if two unitaries differ only by a global phase, why can't any measurement distinguish them?

------------------------------------------------------------------------

### Problem 6: The Hadamard Gate as a Rotation

The Hadamard gate is one of the most important single-qubit gates:

$$H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$$

**(a)** Verify that $H = \frac{1}{\sqrt{2}}(\sigma_x + \sigma_z)$ by writing out the right-hand side explicitly.

**(b)** The general SU(2) rotation is $R_{\hat{n}}(\theta) = \cos(\theta/2)\,I - i\sin(\theta/2)\,(\hat{n}\cdot\vec\sigma)$. Find the rotation axis $\hat{n}$ and angle $\theta$ such that $H = e^{i\phi}\,R_{\hat{n}}(\theta)$, where $e^{i\phi}$ is a global phase. Determine $\phi$ as well.

*Hint:* Since $H$ is Hermitian and $H^2 = I$, what does that tell you about the rotation angle? What direction is $\sigma_x + \sigma_z$?

**(c)** Verify your answer by computing $R_{\hat{n}}(\theta)$ explicitly and comparing to $e^{-i\phi}H$.

**(d)** Compute $H^2$ directly. Show it equals $I$. Explain geometrically: why does applying the same rotation twice return to the identity? For what class of rotation angles $\theta$ is $R_{\hat{n}}(\theta)^2 = I$ (up to global phase)?

**(e)** Where does $H$ send each of the six cardinal states? Describe the geometric action of $H$ on the Bloch sphere in one sentence.

------------------------------------------------------------------------
<!-- 
### Problem 7: Composition of Rotations

When two rotations are applied in sequence, the result is another rotation. In this problem you'll find what that rotation is.

**(a)** Write out $R_z(\pi/2)$ and $R_x(\pi/2)$ as explicit $2\times 2$ matrices.

**(b)** Compute the product $U = R_z(\pi/2)\,R_x(\pi/2)$ by matrix multiplication.

**(c)** Every SU(2) matrix can be written as $U = \cos(\theta/2)\,I - i\sin(\theta/2)\,(\hat{n}\cdot\vec\sigma)$. Use the following procedure to find $\theta$ and $\hat{n}$:

1.  Compute $\text{Tr}(U) = 2\cos(\theta/2)$ to find $\theta$.
2.  Compute $U - U^\dagger = -2i\sin(\theta/2)\,(\hat{n}\cdot\vec\sigma)$ to read off $\hat{n}$.

**(d)** Verify: compute $R_{\hat{n}}(\theta)$ with your values and check it matches $U$.

**(e)** Now compute $V = R_x(\pi/2)\,R_z(\pi/2)$ (opposite order). Find its $\theta$ and $\hat{n}$. Is it the same rotation as $U$? How are the two axes related?

------------------------------------------------------------------------ -->

<!-- ### Problem 8: Decomposing a Random Unitary (Qiskit)

Every single-qubit gate is a rotation on the Bloch sphere. In this problem you'll verify this computationally.

**Setup:**

``` python
from qiskit.quantum_info import random_unitary, Operator
import numpy as np
```

**(a)** Generate a random $2\times 2$ unitary matrix:

``` python
U = random_unitary(2)
mat = U.data  # 2x2 numpy array
```

Print the matrix. Verify it is unitary by checking `mat @ mat.conj().T` $\approx I$.

**(b)** Extract the rotation angle $\theta$ and axis $\hat{n}$:

The trace gives the angle: $\text{Tr}(U) = 2e^{i\alpha}\cos(\theta/2)$ where $e^{i\alpha}$ is a global phase. So $\cos(\theta/2) = |\text{Tr}(U)|/2$.

The axis components come from: $U - U^\dagger = -2ie^{i\alpha}\sin(\theta/2)(\hat{n}\cdot\vec\sigma)$. Extract $n_x, n_y, n_z$ by identifying the coefficients of each Pauli matrix.

Implement this extraction numerically.

**(c)** Reconstruct the rotation matrix using your extracted $\theta$ and $\hat{n}$:

``` python
I2 = np.eye(2)
sx = np.array([[0,1],[1,0]])
sy = np.array([[0,-1j],[1j,0]])
sz = np.array([[1,0],[0,-1]])
R = np.cos(theta/2)*I2 - 1j*np.sin(theta/2)*(nx*sx + ny*sy + nz*sz)
```

Compare `R` to the original `mat` (up to a global phase). Verify they agree.

**(d)** Repeat for 5 different random unitaries. Does the decomposition always work?

**(e)** What happens if you generate a random unitary with $\det(U) \neq 1$? How would you handle the global phase?

------------------------------------------------------------------------ -->
<!-- 
### Problem 9: Gate Sequence Trajectory (Qiskit)

In this problem you'll track a qubit's path on the Bloch sphere as it passes through a sequence of gates.

**Setup:**

``` python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
import numpy as np
import matplotlib.pyplot as plt
```

A useful function to extract Bloch coordinates from a statevector:

``` python
def bloch_coords(sv):
    """Extract (x, y, z) Bloch coordinates from a Statevector."""
    rho = np.outer(sv.data, sv.data.conj())
    sx = np.array([[0,1],[1,0]])
    sy = np.array([[0,-1j],[1j,0]])
    sz = np.array([[1,0],[0,-1]])
    x = np.real(np.trace(rho @ sx))
    y = np.real(np.trace(rho @ sy))
    z = np.real(np.trace(rho @ sz))
    return x, y, z
```

**(a)** Starting from $|0\rangle$, apply the gate sequence $H \to S \to H \to T$, where $S = P(\pi/2)$ and $T = P(\pi/4)$. Record the state (and Bloch coordinates) after each gate:

| Step | Gate applied | State        | $(x, y, z)$ |
|------|--------------|--------------|-------------|
| 0    | —            | $\|0\rangle$ | $(0, 0, 1)$ |
| 1    | $H$          | ?            | ?           |
| 2    | $S$          | ?            | ?           |
| 3    | $H$          | ?            | ?           |
| 4    | $T$          | ?            | ?           |

**(b)** Compute the final state analytically (by multiplying the $2\times 2$ matrices). Verify it matches your Qiskit result.

**(c)** Plot the trajectory on the Bloch sphere. You can use Qiskit's `plot_bloch_vector` or plot the 5 points in 3D with `matplotlib`.

**(d)** The full sequence $THSH$ is equivalent to a single rotation $R_{\hat{n}}(\theta)$. Find $\theta$ and $\hat{n}$ using the trace method from Problem 7.

**(e)** Now replace $T$ with $T^\dagger = P(-\pi/4)$ and repeat. How does the trajectory change? How do $\theta$ and $\hat{n}$ change?

------------------------------------------------------------------------

### Problem 10: The Uncertainty Principle — Quantified (Qiskit)

The commutation relation $[\sigma_z, \sigma_x] = -2i\sigma_y$ implies that $S_z$ and $S_x$ cannot both be known precisely. The Robertson uncertainty relation makes this quantitative:

$$\Delta\sigma_z \cdot \Delta\sigma_x \geq \frac{1}{2}|\langle[\sigma_z, \sigma_x]\rangle| = |\langle\sigma_y\rangle|$$

where $\Delta\sigma_i = \sqrt{\langle\sigma_i^2\rangle - \langle\sigma_i\rangle^2}$ is the standard deviation.

In this problem you'll verify this numerically.

**Setup:**

``` python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
import numpy as np
import matplotlib.pyplot as plt

sx = np.array([[0,1],[1,0]])
sy = np.array([[0,-1j],[1j,0]])
sz = np.array([[1,0],[0,-1]])

def expectation(sv, op):
    """Compute <psi|op|psi>."""
    return np.real(sv.data.conj() @ op @ sv.data)

def uncertainty(sv, op):
    """Compute Delta(op) = sqrt(<op^2> - <op>^2)."""
    exp = expectation(sv, op)
    exp2 = expectation(sv, op @ op)
    return np.sqrt(max(exp2 - exp**2, 0))
```

**(a)** Prepare the family of states $|\psi(\theta)\rangle = R_y(\theta)|0\rangle$ for $\theta \in [0, \pi]$ (say, 100 values). For each state, compute: - $\langle\sigma_z\rangle$ and $\Delta\sigma_z$ - $\langle\sigma_x\rangle$ and $\Delta\sigma_x$ - $\langle\sigma_y\rangle$

**(b)** Plot $\Delta\sigma_z$ and $\Delta\sigma_x$ individually as functions of $\theta$. At what $\theta$ is $\Delta\sigma_z = 0$? At what $\theta$ is $\Delta\sigma_x = 0$? Can both be zero simultaneously?

**(c)** Plot the product $\Delta\sigma_z \cdot \Delta\sigma_x$ as a function of $\theta$. On the same plot, show the lower bound $|\langle\sigma_y\rangle|$. Verify that the product is always greater than or equal to the bound.

**(d)** At what value of $\theta$ is $\Delta\sigma_z \cdot \Delta\sigma_x$ minimized? What state is this? Is the uncertainty relation **saturated** (equality achieved) at this point?

**(e)** The states $R_y(\theta)|0\rangle$ all lie in the $xz$-plane of the Bloch sphere, so $\langle\sigma_y\rangle = 0$ for all of them. This means the Robertson bound is zero — not very informative! Repeat the analysis for the family $|\psi(\theta, \phi)\rangle = R_z(\phi)R_y(\theta)|0\rangle$ with fixed $\theta = \pi/3$ and $\phi \in [0, 2\pi)$. Now plot $\Delta\sigma_z \cdot \Delta\sigma_x$ and $|\langle\sigma_y\rangle|$ vs $\phi$. For what $\phi$ is the bound tightest?

**(f)** Explain in your own words: what does the uncertainty principle say, and what does it *not* say? Does it mean measurements are "noisy"? Does it apply to a single measurement or to an ensemble?