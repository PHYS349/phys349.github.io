# Lecture 2.4: Complex Numbers, Rotations, and Group Theory

## Why Is Quantum Mechanics Complex?

### Review: The Classical Wave Equation

The wave equation for light is:

$$\frac{\partial^2 E}{\partial x^2} = \frac{1}{c^2}\frac{\partial^2 E}{\partial t^2}$$

This is a real equation with real solutions:

$$E(x,t) = A\cos(kx - \omega t), \qquad E(x,t) = B\sin(kx - \omega t)$$

We can also write solutions using complex exponentials:

$$E(x,t) = e^{\pm i(kx \pm \omega t)}$$

All four sign combinations — $e^{i(kx - \omega t)}$, $e^{-i(kx - \omega t)}$, $e^{i(kx + \omega t)}$, $e^{-i(kx + \omega t)}$ — are valid solutions. They correspond to right- and left-traveling waves. When we use these complex exponentials for light, it's a **trick**. The physical field is real; we take the real part at the end.

The dispersion relation is linear:

$$\omega = ck$$

### The Schrödinger Equation

Now consider the equation governing matter waves — the Schrödinger equation for a free particle:

$$i\hbar\frac{\partial \psi}{\partial t} = -\frac{\hbar^2}{2m}\frac{\partial^2 \psi}{\partial x^2}$$

Notice the $i$ on the left side. Let's see what it does.

Try a plane wave solution $\psi(x,t) = e^{i(kx - \omega t)}$:

**Left side:**

$$i\hbar\frac{\partial}{\partial t}e^{i(kx - \omega t)} = i\hbar(-i\omega)e^{i(kx-\omega t)} = \hbar\omega\, e^{i(kx - \omega t)}$$

**Right side:**

$$-\frac{\hbar^2}{2m}\frac{\partial^2}{\partial x^2}e^{i(kx-\omega t)} = -\frac{\hbar^2}{2m}(ik)^2 e^{i(kx-\omega t)} = \frac{\hbar^2 k^2}{2m}e^{i(kx-\omega t)}$$

Setting them equal gives the quantum dispersion relation:

$$\hbar\omega = \frac{\hbar^2 k^2}{2m} \qquad \Longrightarrow \qquad \omega = \frac{\hbar k^2}{2m}$$

This is **quadratic** in $k$, unlike the classical $\omega = ck$.

### iClicker: Which Sign Combinations Solve the Schrödinger Equation?

For the classical wave equation, all four sign combinations $e^{\pm i(kx \pm \omega t)}$ are valid solutions. **Do all four also solve the Schrödinger equation?**

-   

    (A) Yes

-   

    (B) No ✓

Let's check. Write a general plane wave as $\psi = e^{i(\alpha kx - \beta\omega t)}$ where $\alpha, \beta = \pm 1$. Substituting into the Schrödinger equation:

$$i\hbar \frac{\partial\psi}{\partial t} = i\hbar(-i\beta\omega)\psi = \beta\hbar\omega\,\psi$$

$$-\frac{\hbar^2}{2m}\frac{\partial^2\psi}{\partial x^2} = -\frac{\hbar^2}{2m}(i\alpha k)^2\psi = \frac{\hbar^2 k^2}{2m}\psi$$

For these to be equal we need $\beta\hbar\omega = \frac{\hbar^2 k^2}{2m}$. Since $\omega$ and $k$ are positive, we need $\beta = +1$. The sign of $\alpha$ doesn't matter (it only appears as $\alpha^2 = 1$).

So the solutions are:

$$\psi(x,t) = e^{-i\omega t} \cdot e^{\pm ikx}$$

The time dependence **must** be $e^{-i\omega t}$. The Schrödinger equation forces a specific sign on the time evolution. This is fundamentally different from the classical wave equation, where both $e^{+i\omega t}$ and $e^{-i\omega t}$ are valid.

### Why Complex Numbers Are Required

Now consider: can we use a real function? Try $\psi(x,t) = \cos(kx - \omega t)$:

$$\frac{\partial\psi}{\partial t} = \omega\sin(kx - \omega t)$$

$$\frac{\partial^2\psi}{\partial x^2} = -k^2\cos(kx - \omega t)$$

The left side of the Schrödinger equation gives something proportional to $\sin(kx - \omega t)$, while the right side gives something proportional to $\cos(kx - \omega t)$. These are different functions — they cannot be equal for all $x$ and $t$.

**A real cosine does not solve the Schrödinger equation.** Neither does a real sine. The complex exponential $e^{-i(\omega t \pm kx)}$ is the only option.

For classical waves, complex numbers are a convenient bookkeeping device. For quantum mechanics, complex numbers are **mandatory**. The wavefunction is irreducibly complex.

### Nature Is Fundamentally Complex

This has deep consequences for how we think about quantum states. When we write:

$$|\psi\rangle = c_0|0\rangle + c_1|1\rangle$$

the coefficients $c_0$ and $c_1$ are complex numbers. Each carries two pieces of information: an **amplitude** and a **phase**. Both are physical — the amplitude determines probabilities, and the relative phase determines interference. We've seen this on the Bloch sphere: the six cardinal states all have the same amplitudes ($1/\sqrt{2}$ each) for some basis, but differ by their relative phases, and they represent completely different physical states.

------------------------------------------------------------------------

## Why Group Theory?

Before we dive into the mathematics, let's ask: why should physicists care about group theory?

A **group** describes a collection of actions — things you can do to something. Rotations are the classic example: you can rotate a sphere around the x-axis, then the z-axis, or do them in the opposite order. The collection of all possible rotations, together with the rule for combining them, forms a group.

We've already been using groups without naming them. Every time we apply a Hadamard gate or a phase gate to a qubit, we're rotating a state on the Bloch sphere. The set of all single-qubit gates forms a group — specifically, the group SU(2).

Nature seems to know group theory deeply. The mathematical structure of rotations is built into the fabric of physics. Here's a preview of where we're headed:

**SO(3)** is the group of rotations in 3D space — the kind you do when you rotate a ball in your hands. A full $2\pi$ rotation brings everything back to where it started: $R(2\pi) = +1$.

**SU(2)** is the group of qubit transformations — rotations on the Bloch sphere. It is almost the same as SO(3), but with one crucial difference: a full $2\pi$ rotation gives $R(2\pi) = -1$. You need to rotate by $4\pi$ to get back to the identity.

Nature cares about this distinction. Particles called **fermions** (electrons, quarks — the matter particles) transform under SU(2) and pick up a minus sign under $2\pi$ rotation. Particles called **bosons** (photons, gluons — the force carriers) transform under SO(3) and return to $+1$. This is connected to the spin-statistics theorem, one of the deepest results in physics. We'll say more about this later.

For now, let's build the mathematics from the ground up.

------------------------------------------------------------------------

## Complex Numbers as Rotations

A complex number can be written in Cartesian or polar form:

$$z = x + iy = re^{i\theta}$$

What happens when we multiply two complex numbers in polar form?

$$e^{i\theta_1} \cdot e^{i\theta_2} = e^{i(\theta_1 + \theta_2)}$$

Geometrically, multiplying by $e^{i\theta}$ **rotates** a point in the complex plane by angle $\theta$. The magnitudes multiply and the angles add. For numbers on the unit circle ($r = 1$), multiplication is pure rotation.

------------------------------------------------------------------------

## What Is a Group?

A **group** is a set together with an operation that satisfies four rules.

Take our example: the set is $\{e^{i\theta} : \theta \in (-\pi, \pi]\}$ (all complex numbers on the unit circle), and the operation is multiplication.

### Rule 1: Closure

Combining two elements gives another element in the set.

$$e^{i\theta_1} \cdot e^{i\theta_2} = e^{i(\theta_1 + \theta_2)}$$

The product of two unit-magnitude complex numbers is another unit-magnitude complex number. The result stays in the set. ✓

### Rule 2: Associativity

$$(a \cdot b) \cdot c = a \cdot (b \cdot c)$$

Multiplication of complex numbers is associative. ✓

### Rule 3: Identity

There exists an element $\mathbb{1}$ such that $\mathbb{1} \cdot a = a \cdot \mathbb{1} = a$ for all $a$.

The identity is $e^{i \cdot 0} = 1$. Multiplying any element by 1 leaves it unchanged. ✓

### Rule 4: Inverses

Every element $a$ has an inverse $a^{-1}$ such that $a \cdot a^{-1} = \mathbb{1}$.

$$(e^{i\theta})^{-1} = e^{-i\theta} = \overline{e^{i\theta}}$$

The inverse is the complex conjugate. Multiplying gives $e^{i\theta} \cdot e^{-i\theta} = e^{0} = 1$. ✓

All four rules are satisfied. The unit complex numbers under multiplication form a group. This group is called **U(1)** — the unitary group in 1 dimension.

------------------------------------------------------------------------

## Matrix Representation of SO(2)

There's another way to describe 2D rotations: as matrices acting on vectors.

A rotation by angle $\theta$ in the $x$-$y$ plane is:

$$R(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}$$

It acts on a position vector:

$$\begin{pmatrix} x' \\ y' \end{pmatrix} = R(\theta)\begin{pmatrix} x \\ y \end{pmatrix}$$

turning it by angle $\theta$ counterclockwise.

### Connection to Complex Multiplication

The matrix rotation and complex multiplication are doing the same thing. A complex number $z = x + iy$ corresponds to the vector $(x, y)^T$. Multiplying by $e^{i\theta} = \cos\theta + i\sin\theta$:

$$e^{i\theta}z = (\cos\theta + i\sin\theta)(x + iy) = (\cos\theta\cdot x - \sin\theta\cdot y) + i(\sin\theta\cdot x + \cos\theta\cdot y)$$

Reading off the real and imaginary parts:

$$x' = \cos\theta\cdot x - \sin\theta\cdot y, \qquad y' = \sin\theta\cdot x + \cos\theta\cdot y$$

This is exactly:

$$\begin{pmatrix} x' \\ y' \end{pmatrix} = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}\begin{pmatrix} x \\ y \end{pmatrix}$$

**Complex multiplication by** $e^{i\theta}$ and matrix multiplication by $R(\theta)$ are two representations of the same rotation.

### Verifying the Group Axioms for $R(\theta)$

The set of all rotation matrices $\{R(\theta)\}$ also forms a group:

**Closure:** $R(\theta_1 + \theta_2) = R(\theta_1)R(\theta_2)$. The product of two rotations is another rotation. ✓

**Associativity:** Matrix multiplication is associative. ✓

**Identity:** $R(0) = I = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}$. ✓

**Inverses:** $R(\theta)^{-1} = R(-\theta) = R(\theta)^T$. The inverse is rotation by the opposite angle, which is the transpose. You can verify: $R(\theta)^T R(\theta) = I$. ✓

### Naming: SO(2)

This group is called **SO(2)**:

-   **S** = Special: $\det R(\theta) = \cos^2\theta + \sin^2\theta = 1$
-   **O** = Orthogonal: $R^T R = I$ (the inverse is the transpose; this preserves lengths)
-   **2** = $2 \times 2$ real matrices

### iClicker: Does Order Matter in SO(2)?

**Is** $R(\theta_2)R(\theta_1) = R(\theta_1)R(\theta_2)$? Does the order of two rotations matter?

-   

    (A) Yes, order matters

-   

    (B) No, order doesn't matter ✓

Since $R(\theta_1)R(\theta_2) = R(\theta_1 + \theta_2) = R(\theta_2 + \theta_1) = R(\theta_2)R(\theta_1)$, the order doesn't matter. SO(2) is a **commutative** (abelian) group. Rotating by 30° then 45° is the same as 45° then 30° — it's all rotations in a single plane, and angles just add.

------------------------------------------------------------------------

## From 2D to 3D: Non-Commutativity

Now let's move to three dimensions. In 3D, we can rotate around three independent axes: $x$, $y$, and $z$.

### The Key Question

Consider two sequences of rotations applied to an object:

1.  Rotate 90° around the x-axis, then 90° around the z-axis
2.  Rotate 90° around the z-axis, then 90° around the x-axis

$$R_x(90°)\,R_z(90°) \qquad \text{vs.} \qquad R_z(90°)\,R_x(90°)$$

### iClicker: Do 3D Rotations Commute?

**Do these two sequences give the same result?**

-   

    (A) Yes, they commute

-   

    (B) No, they don't commute ✓

**Try it with a book!** Hold a book in front of you:

*Sequence 1:* Rotate 90° around the x-axis (tip the top edge away from you), then rotate 90° around the z-axis (turn the book counterclockwise as seen from above).

*Sequence 2:* Rotate 90° around the z-axis first, then 90° around the x-axis.

The book ends up in a different orientation. **3D rotations do not commute.**

$$R_x(\theta_1)\,R_z(\theta_2) \neq R_z(\theta_2)\,R_x(\theta_1)$$

This is the essential new feature of 3D. In 2D, all rotations are around the same axis (perpendicular to the plane), and angles simply add — order doesn't matter. In 3D, there are multiple rotation axes, and the order in which you compose rotations changes the outcome.

The group of 3D rotations is called **SO(3)**: special orthogonal $3 \times 3$ real matrices. Unlike SO(2), it is **non-commutative** (non-abelian).

This non-commutativity will turn out to be the mathematical origin of the uncertainty principle: if two measurements don't commute, you can't know both simultaneously. But that's for next lecture.

------------------------------------------------------------------------

## Summary

1.  **Quantum mechanics is fundamentally complex.** The $i$ in the Schrödinger equation is not a bookkeeping trick — it's essential. Only complex exponentials $e^{-i(\omega t \pm kx)}$ solve the Schrödinger equation; real cosines and sines do not.

2.  **Complex numbers encode rotations.** Multiplication by $e^{i\theta}$ rotates by angle $\theta$ in the complex plane. This is identical to matrix multiplication by the rotation matrix $R(\theta)$.

3.  **Rotations form a group.** A group is a set with an operation satisfying closure, associativity, identity, and inverses. The unit complex numbers (U(1)) and the $2\times 2$ rotation matrices (SO(2)) are two representations of the same group.

4.  **2D rotations commute; 3D rotations do not.** SO(2) is commutative — the order of rotations doesn't matter. SO(3) is non-commutative — order matters. This non-commutativity will be central to quantum mechanics.

5.  **Looking ahead.** The qubit lives on the Bloch sphere, which is acted on by the group SU(2). SU(2) is like SO(3) — three-dimensional, non-commutative — but with the twist that $R(2\pi) = -1$. The generators of SU(2) are the Pauli matrices, which we will meet next lecture.

------------------------------------------------------------------------

## Homework 2.4

### Problem 1: Is It a Group?

For each of the following, determine whether the set and operation form a group. If yes, verify all four axioms. If no, identify which axiom fails and explain why.

**(a)** The set of all integers $\{\ldots, -2, -1, 0, 1, 2, \ldots\}$ under addition.

**(b)** The set $\{1, -1, i, -i\}$ under multiplication. If it is a group, construct the complete $4 \times 4$ multiplication table.

**(c)** The set of all $2\times 2$ real matrices under multiplication.

*Hint:* Consider the matrix $\begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}$. Does it have an inverse?

<!-- **(d)** The set of all $2\times 2$ real matrices with $\det \neq 0$, under multiplication. Which axiom from part (c) is now repaired? -->

<!-- **(e)** The set of reflections in 2D. A reflection across an axis at angle $\alpha$ from the horizontal is given by:

$$M(\alpha) = \begin{pmatrix} \cos 2\alpha & \sin 2\alpha \\ \sin 2\alpha & -\cos 2\alpha \end{pmatrix}$$

Compute $M(\alpha_1) M(\alpha_2)$ and determine whether the result is another reflection matrix. Which axiom fails?

*Hint:* A reflection matrix has $\det = -1$ and $M^2 = I$. Does the product have these properties? -->

<!--#
**(f)** The set $\{I, \sigma_x\}$ under matrix multiplication, where $\sigma_x = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$.
**(g)** The set $\{I, \sigma_x, \sigma_y, \sigma_z\}$ under matrix multiplication.
*Hint:* Compute $\sigma_x \sigma_y$. Is the result in the set?
-->


------------------------------------------------------------------------

### Problem 2: SO(2) — Matrix Properties

The 2D rotation matrix is:

$$R(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}$$

**(a)** Compute $R(\theta_1) R(\theta_2)$ by explicit matrix multiplication. Show that the result equals $R(\theta_1 + \theta_2)$.

*Hint:* You will need the angle addition formulas: $\cos(\alpha + \beta) = \cos\alpha\cos\beta - \sin\alpha\sin\beta$ and $\sin(\alpha + \beta) = \sin\alpha\cos\beta + \cos\alpha\sin\beta$.

**(b)** Show that $\det R(\theta) = 1$ for all $\theta$. This is the "S" (Special) in SO(2).

**(c)** Show that $R(\theta)^T R(\theta) = I$ for all $\theta$. This is the "O" (Orthogonal) in SO(2).

**(d)** Using part (c), show that rotations preserve the length of any vector. That is, if $\vec{v}' = R(\theta)\vec{v}$, show that $|\vec{v}'| = |\vec{v}|$.

*Hint:* $|\vec{v}'|^2 = \vec{v}'^T \vec{v}' = (R\vec{v})^T(R\vec{v})$.

------------------------------------------------------------------------

### Problem 3: Non-Commutativity in 3D

The 3D rotation matrices around the $x$, $y$, and $z$ axes by angle $\theta$ are:

$$R_x(\theta) = \begin{pmatrix} 1 & 0 & 0 \\ 0 & \cos\theta & -\sin\theta \\ 0 & \sin\theta & \cos\theta \end{pmatrix}$$

$$R_y(\theta) = \begin{pmatrix} \cos\theta & 0 & \sin\theta \\ 0 & 1 & 0 \\ -\sin\theta & 0 & \cos\theta \end{pmatrix}$$

$$R_z(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{pmatrix}$$

**(a)** Write out $R_x(\pi/2)$ and $R_z(\pi/2)$ explicitly (plug in $\theta = \pi/2$).

**(b)** Compute $R_x(\pi/2)\, R_z(\pi/2)$.

**(c)** Compute $R_z(\pi/2)\, R_x(\pi/2)$.

**(d)** Are your answers to (b) and (c) the same? What does this tell you about SO(3)?

**(e)** Compute the **commutator** $R_x(\pi/2)\, R_z(\pi/2) - R_z(\pi/2)\, R_x(\pi/2)$. Is it the zero matrix?

<!-- **(f)** Verify that both $R_x(\pi/2)$ and $R_z(\pi/2)$ satisfy $\det R = 1$ and $R^T R = I$. -->

<!-- ------------------------------------------------------------------------

### Problem 4: Permutations — A Finite Non-Abelian Group

Consider three objects in positions (1, 2, 3). A **permutation** rearranges them. We can write permutations in a compact notation, for example:

-   $e = (1)(2)(3)$: identity, everything stays put
-   $r = (123)$: cyclic shift, $1 \to 2 \to 3 \to 1$
-   $s = (12)(3)$: swap positions 1 and 2, leave 3 fixed

**(a)** List all 6 permutations of three objects.

*Hint:* Three are "even" (identity plus two cyclic shifts) and three are "odd" (three different pairwise swaps).

**(b)** Compute the composition $r \circ s$ (first apply $s$, then apply $r$). Start with $(1, 2, 3)$, apply $s$ to get a new arrangement, then apply $r$.

**(c)** Compute $s \circ r$ (first apply $r$, then apply $s$).

**(d)** Is $r \circ s = s \circ r$? This group is called $S_3$ — it is the smallest non-abelian group.

**(e)** Verify the four group axioms for $S_3$. What is the inverse of the cyclic shift $r = (123)$? -->

------------------------------------------------------------------------

### Problem 4: Discovering the Generator

**(a)** Taylor expand $\cos(\delta\theta)$ and $\sin(\delta\theta)$ to first order in $\delta\theta$ (i.e., for small $\delta\theta$). Write the resulting approximate form of $R(\delta\theta)$.

**(b)** Show that for small $\delta\theta$:

$$R(\delta\theta) \approx I + \delta\theta\, G$$

where:

$$G = \begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}$$

$G$ is called the **generator** of SO(2).

**(c)** Compute $G^2$. What familiar property does it share with $i$?

We will learn more about this next week. 

<!-- **(d)** A finite rotation can be built from $N$ infinitesimal rotations: $R(\theta) = \lim_{N\to\infty}\left(I + \frac{\theta}{N}G\right)^N$. Verify this numerically: compute $\left(I + \frac{\pi/4}{N}G\right)^N$ for $N = 1, 10, 100$ and compare to $R(\pi/4)$. -->

<!-- **(e)** Using the Taylor series $e^{\theta G} = I + \theta G + \frac{\theta^2}{2!}G^2 + \frac{\theta^3}{3!}G^3 + \cdots$ and the result from (c), show that:

$$e^{\theta G} = \cos\theta\, I + \sin\theta\, G$$

*Hint:* Compute $G^3$, $G^4$, etc. using $G^2 = -I$. Group the series into terms with $I$ and terms with $G$, and recognize the Taylor series for $\cos\theta$ and $\sin\theta$. -->

<!-- **(f)** Compare your result to Euler's formula $e^{i\theta} = \cos\theta + i\sin\theta$. What role does $G$ play relative to $i$? -->

<!-- ------------------------------------------------------------------------

### Problem 6: Rotations on the Bloch Sphere

The state $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ sits on the +x axis of the Bloch sphere.

**(a)** Apply the phase gate $P(\phi) = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\phi} \end{pmatrix}$ to $|+\rangle$ for $\phi = \pi/2$. Write the resulting state. Which of the six cardinal states is it? Where is it on the Bloch sphere?

**(b)** Repeat for $\phi = \pi$.

**(c)** Repeat for $\phi = 3\pi/2$.

**(d)** The four states — $|+\rangle$ and your three answers — trace out a path on the Bloch sphere. Describe this path geometrically. What kind of motion is $P(\phi)$ performing?

**(e)** Now start with $|0\rangle$ (north pole) and apply $P(\phi)$. What state do you get? Does the north pole move? Explain why, in terms of what $P(\phi)$ does geometrically.

**(f)** Write $P(\phi)$ in the form $P(\phi) = \cos(\phi/2)\,I - i\sin(\phi/2)\,\sigma_z$ (up to a global phase). This shows that the phase gate is a rotation generated by $\sigma_z$ — a preview of next lecture. -->