# Lecture 7: Spin, Pauli Matrices, and Time Evolution

## From Polarization to Spin

Last lecture, we saw that a photon's polarization is a qubit — a two-state quantum system described by complex amplitudes. The Jones vector from classical optics became the quantum state vector.

Today we turn to another qubit that nature provides: the **spin** of an electron. This will lead us to the Pauli matrices, which are the fundamental building blocks for describing any qubit. And we'll finally see how quantum states evolve in time.

---

## The Stern-Gerlach Experiment: Nature Hands Us a Qubit

In 1922, Otto Stern and Walther Gerlach performed an experiment that revealed something profound about electrons.

### The Setup

They sent a beam of silver atoms through an inhomogeneous magnetic field — a field that's stronger on one side than the other. Classical physics makes a clear prediction: if the atoms have magnetic moments oriented randomly, the beam should spread out into a continuous band.

```{figure} ./02_03_lecture_files/stern_gerlach.svg
:width: 450px
:name: stern-gerlach

The Stern-Gerlach experiment: atoms pass through an inhomogeneous magnetic field.
```

### The Result

The beam didn't spread continuously. It split into exactly **two discrete spots**.

This was shocking. It meant:
1. The magnetic moment (and therefore angular momentum) is **quantized** — it can only take discrete values
2. For silver atoms (and electrons), there are exactly **two** possible values

This is the experimental discovery of **spin-1/2**.

```{admonition} Why Two?
:class: note

The electron has spin quantum number $s = 1/2$. The rules of quantum mechanics say the $z$-component of angular momentum can take values $m_s = -s, -s+1, ..., s$. For $s = 1/2$, that's just two values: $m_s = +1/2$ ("spin up") and $m_s = -1/2$ ("spin down").

This is the smallest non-trivial representation of rotations in quantum mechanics — the fundamental representation of SU(2).
```

### The Qubit

The two outcomes of a Stern-Gerlach measurement define our basis:

$$
|\uparrow\rangle \equiv |0\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}, \qquad |\downarrow\rangle \equiv |1\rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}
$$

A general spin state is a superposition:

$$
|\psi\rangle = \alpha|\uparrow\rangle + \beta|\downarrow\rangle = \begin{pmatrix} \alpha \\ \beta \end{pmatrix}
$$

This is mathematically identical to polarization. The Bloch sphere we developed for polarization works exactly the same way for spin.

---

## Rotating the Apparatus: Different Measurement Axes

Here's where spin gets interesting. What if we rotate the Stern-Gerlach apparatus?

### Measuring Along Different Axes

If the magnetic field gradient points along $z$, we measure the $z$-component of spin. The eigenstates are $|\uparrow_z\rangle$ and $|\downarrow_z\rangle$.

If we rotate the apparatus to point along $x$, we measure the $x$-component of spin. The eigenstates are different:

$$
|\uparrow_x\rangle = \frac{1}{\sqrt{2}}(|\uparrow_z\rangle + |\downarrow_z\rangle) = |+\rangle
$$

$$
|\downarrow_x\rangle = \frac{1}{\sqrt{2}}(|\uparrow_z\rangle - |\downarrow_z\rangle) = |-\rangle
$$

Similarly for the $y$-axis:

$$
|\uparrow_y\rangle = \frac{1}{\sqrt{2}}(|\uparrow_z\rangle + i|\downarrow_z\rangle)
$$

$$
|\downarrow_y\rangle = \frac{1}{\sqrt{2}}(|\uparrow_z\rangle - i|\downarrow_z\rangle)
$$

### The Connection to Polarization

This is exactly what we saw with polarization!

| Polarization | Spin |
|--------------|------|
| H/V basis | $z$-measurement |
| D/A basis | $x$-measurement |
| R/L basis | $y$-measurement |

The same Bloch/Poincaré sphere describes both systems. Choosing a measurement basis in polarization (rotating a polarizer) is equivalent to choosing a measurement axis in spin (rotating the Stern-Gerlach apparatus).

---

## The Uncertainty Principle: You Can't Know Everything

Here's a fundamental consequence of what we've seen.

Suppose an electron is in state $|\uparrow_z\rangle$ — definitely spin-up along $z$.

**Question:** What is its spin along $x$?

**Answer:** It doesn't have a definite value! If we measure $S_x$, we get:
- $+\hbar/2$ (spin-up along $x$) with probability $1/2$
- $-\hbar/2$ (spin-down along $x$) with probability $1/2$

This isn't because we don't know — the electron genuinely doesn't have a definite $x$-component of spin when it has a definite $z$-component.

```{admonition} The Uncertainty Principle for Spin
:class: warning

You cannot simultaneously know the spin along two perpendicular axes.

If you know $S_z$ precisely, then $S_x$ and $S_y$ are completely uncertain (and vice versa).

Mathematically, this is because the operators $\hat{S}_x$, $\hat{S}_y$, $\hat{S}_z$ don't commute:

$$
[\hat{S}_x, \hat{S}_z] = \hat{S}_x \hat{S}_z - \hat{S}_z \hat{S}_x \neq 0
$$
```

This is visualized on the Bloch sphere: a state that's at the north pole (definite $S_z = +\hbar/2$) has no definite position on the equator (uncertain $S_x$ and $S_y$).

---

## The Pauli Matrices

We need mathematical objects to represent "measure spin along $x$" vs "measure spin along $z$."

These are the **Pauli matrices**:

$$
\sigma_x = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, \qquad
\sigma_y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}, \qquad
\sigma_z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}
$$

### What They Represent

The spin operators are:

$$
\hat{S}_x = \frac{\hbar}{2}\sigma_x, \qquad \hat{S}_y = \frac{\hbar}{2}\sigma_y, \qquad \hat{S}_z = \frac{\hbar}{2}\sigma_z
$$

Each Pauli matrix has eigenvalues $\pm 1$, so each spin operator has eigenvalues $\pm \hbar/2$.

### Eigenstates

The eigenstates of each Pauli matrix are the basis states for measuring along that axis:

**$\sigma_z$ eigenstates:**
$$
\sigma_z |0\rangle = +|0\rangle, \qquad \sigma_z |1\rangle = -|1\rangle
$$

**$\sigma_x$ eigenstates:**
$$
\sigma_x |+\rangle = +|+\rangle, \qquad \sigma_x |-\rangle = -|-\rangle
$$

where $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ and $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$.

### Key Properties

The Pauli matrices have beautiful algebraic properties:

1. **Hermitian:** $\sigma_i^\dagger = \sigma_i$ (they represent observables)

2. **Traceless:** $\text{Tr}(\sigma_i) = 0$

3. **Square to identity:** $\sigma_i^2 = I$

4. **Anti-commute:** $\sigma_i \sigma_j = -\sigma_j \sigma_i$ for $i \neq j$

5. **Commutators:** $[\sigma_x, \sigma_y] = 2i\sigma_z$ (and cyclic permutations)

### Any 2×2 Matrix Can Be Written Using Paulis

Here's a powerful fact. Any $2 \times 2$ Hermitian matrix can be written as:

$$
H = a_0 I + a_x \sigma_x + a_y \sigma_y + a_z \sigma_z = a_0 I + \vec{a} \cdot \vec{\sigma}
$$

where $a_0, a_x, a_y, a_z$ are real numbers and $\vec{\sigma} = (\sigma_x, \sigma_y, \sigma_z)$.

This means the Pauli matrices (plus identity) form a **basis** for all qubit operators.

---

## Time Evolution: The Schrödinger Equation

So far, we've discussed static states. But quantum states evolve in time. How?

### The Fundamental Law

The Schrödinger equation tells us:

$$
i\hbar \frac{d}{dt}|\psi(t)\rangle = H|\psi(t)\rangle
$$

Here $H$ is the **Hamiltonian** — the operator representing the total energy of the system.

### The Solution

For a time-independent Hamiltonian, the solution is:

$$
|\psi(t)\rangle = e^{-iHt/\hbar}|\psi(0)\rangle = U(t)|\psi(0)\rangle
$$

The operator $U(t) = e^{-iHt/\hbar}$ is called the **time evolution operator**.

### Unitary Evolution

A crucial property: $U(t)$ is **unitary**, meaning $U^\dagger U = I$.

Why does this matter?

$$
\langle\psi(t)|\psi(t)\rangle = \langle\psi(0)|U^\dagger U|\psi(0)\rangle = \langle\psi(0)|\psi(0)\rangle = 1
$$

Unitarity guarantees that **probability is conserved**. The total probability stays equal to 1 for all time.

```{admonition} Why Hermitian Hamiltonians?
:class: note

For $U = e^{-iHt/\hbar}$ to be unitary, we need $H^\dagger = H$ — the Hamiltonian must be **Hermitian**.

This also guarantees that energy eigenvalues are real numbers, which makes physical sense.
```

---

## Example 1: Energy Eigenstates (Phase Evolution)

The simplest case: what if $|\psi(0)\rangle$ is an eigenstate of $H$?

Suppose $H|\psi(0)\rangle = E|\psi(0)\rangle$. Then:

$$
|\psi(t)\rangle = e^{-iHt/\hbar}|\psi(0)\rangle = e^{-iEt/\hbar}|\psi(0)\rangle
$$

The state just picks up a phase factor $e^{-iEt/\hbar}$. The physical state doesn't change — probabilities are unaffected since $|e^{-iEt/\hbar}|^2 = 1$.

**Energy eigenstates are stationary states.** They don't evolve (except for an overall phase).

---

## Example 2: A Spin in a Magnetic Field (Precession)

Now let's do a real physical example.

A spin in a magnetic field $\vec{B} = B\hat{z}$ (pointing along $z$) has Hamiltonian:

$$
H = -\vec{\mu} \cdot \vec{B} = -\gamma \vec{S} \cdot \vec{B} = -\gamma B S_z = -\frac{\gamma B \hbar}{2}\sigma_z
$$

where $\gamma$ is the gyromagnetic ratio.

Define $\omega_0 = \gamma B$ (the Larmor frequency). Then:

$$
H = -\frac{\hbar\omega_0}{2}\sigma_z = \frac{\hbar\omega_0}{2}\begin{pmatrix} -1 & 0 \\ 0 & 1 \end{pmatrix}
$$

(We can flip the sign convention; the physics is the same.)

### Time Evolution

For a simple diagonal Hamiltonian $H = \frac{\hbar\omega_0}{2}\sigma_z$:

$$
U(t) = e^{-iHt/\hbar} = e^{-i\omega_0 t \sigma_z/2} = \begin{pmatrix} e^{-i\omega_0 t/2} & 0 \\ 0 & e^{i\omega_0 t/2} \end{pmatrix}
$$

If we start in a superposition $|\psi(0)\rangle = \alpha|0\rangle + \beta|1\rangle$:

$$
|\psi(t)\rangle = \alpha e^{-i\omega_0 t/2}|0\rangle + \beta e^{i\omega_0 t/2}|1\rangle
$$

### What's Happening?

The probabilities $|\alpha|^2$ and $|\beta|^2$ don't change — if you measure in the $z$-basis, you get the same statistics at all times.

But the **relative phase** between $|0\rangle$ and $|1\rangle$ is changing:

$$
\frac{\beta(t)}{\alpha(t)} = \frac{\beta}{\alpha}e^{i\omega_0 t}
$$

On the Bloch sphere, this is **precession around the $z$-axis**. The state vector rotates around $z$ at angular frequency $\omega_0$.

```{figure} ./02_03_lecture_files/precession.svg
:width: 350px
:name: precession

Precession: a state rotates around the z-axis under $H \propto \sigma_z$.
```

This is the quantum version of a classical gyroscope precessing in a gravitational field.

---

## Example 3: Rabi Oscillations (Population Transfer)

Now let's add a driving field that can flip the spin.

### The Setup

A spin in a static field $B_z$ plus an oscillating transverse field. In the rotating frame (a standard trick we'll justify later), the effective Hamiltonian becomes:

$$
H = \frac{\hbar\Omega}{2}\sigma_x = \frac{\hbar\Omega}{2}\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}
$$

where $\Omega$ is called the **Rabi frequency**.

### Time Evolution

We need to compute $e^{-i\Omega t \sigma_x/2}$.

Using the identity $e^{i\theta \hat{n}\cdot\vec{\sigma}/2} = \cos(\theta/2)I + i\sin(\theta/2)\hat{n}\cdot\vec{\sigma}$:

$$
U(t) = e^{-i\Omega t \sigma_x/2} = \cos\frac{\Omega t}{2}I - i\sin\frac{\Omega t}{2}\sigma_x
$$

$$
= \begin{pmatrix} \cos\frac{\Omega t}{2} & -i\sin\frac{\Omega t}{2} \\ -i\sin\frac{\Omega t}{2} & \cos\frac{\Omega t}{2} \end{pmatrix}
$$

### Starting in $|0\rangle$

If $|\psi(0)\rangle = |0\rangle$:

$$
|\psi(t)\rangle = \cos\frac{\Omega t}{2}|0\rangle - i\sin\frac{\Omega t}{2}|1\rangle
$$

The probabilities are:

$$
P_0(t) = \cos^2\frac{\Omega t}{2}, \qquad P_1(t) = \sin^2\frac{\Omega t}{2}
$$

```{figure} ./02_03_lecture_files/rabi_oscillations.svg
:width: 450px
:name: rabi-oscillations

Rabi oscillations: population oscillates between $|0\rangle$ and $|1\rangle$.
```

### The Physics

This is dramatically different from precession:
- The **populations** are changing
- The spin oscillates between "up" and "down"
- On the Bloch sphere, this is rotation around the **x-axis**

At time $t = \pi/\Omega$ (a "**π-pulse**"):

$$
|\psi\rangle = -i|1\rangle
$$

The spin has completely flipped from $|0\rangle$ to $|1\rangle$. (The $-i$ is a global phase and doesn't affect measurements.)

At time $t = \pi/(2\Omega)$ (a "**π/2-pulse**"):

$$
|\psi\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)
$$

We've created an equal superposition — the spin is now on the equator of the Bloch sphere.

```{admonition} Rabi Oscillations Are Everywhere
:class: important

This same physics appears whenever a two-level system is driven by a resonant field:
- Atoms driven by light ($H_{int} = -\vec{d} \cdot \vec{E} \propto \sigma_x$)
- Superconducting qubits driven by microwaves
- NMR (nuclear spins driven by RF fields)

The Rabi frequency $\Omega$ depends on the strength of the driving field.
```

---

## The General Rule: Hamiltonians Are Rotation Axes

We can now state the general principle.

Any qubit Hamiltonian can be written as:

$$
H = \frac{\hbar\omega}{2}\hat{n}\cdot\vec{\sigma} = \frac{\hbar\omega}{2}(n_x\sigma_x + n_y\sigma_y + n_z\sigma_z)
$$

where $\hat{n}$ is a unit vector.

The time evolution is:

$$
U(t) = e^{-iHt/\hbar} = e^{-i\omega t \hat{n}\cdot\vec{\sigma}/2}
$$

This represents a **rotation around the axis $\hat{n}$ by angle $\theta = \omega t$** on the Bloch sphere.

| Hamiltonian | Rotation Axis | Physical Effect |
|-------------|---------------|-----------------|
| $H \propto \sigma_z$ | z-axis | Precession (phase accumulation) |
| $H \propto \sigma_x$ | x-axis | Rabi oscillations (population transfer) |
| $H \propto \sigma_y$ | y-axis | Rabi oscillations (different phase) |
| $H \propto \hat{n}\cdot\vec{\sigma}$ | $\hat{n}$-axis | Rotation around $\hat{n}$ |

```{figure} ./02_03_lecture_files/bloch_rotations.svg
:width: 400px
:name: bloch-rotations

On the Bloch sphere, Hamiltonians generate rotations around their axis.
```

---

## Quantum Gates Are Controlled Rotations

This brings us to a crucial insight for quantum computing.

A **quantum gate** is just a unitary transformation — a rotation on the Bloch sphere. By controlling the Hamiltonian (choosing the axis and duration), we can implement any gate we want.

| Gate | Matrix | Bloch Sphere |
|------|--------|--------------|
| $X$ | $\sigma_x$ | 180° rotation around x |
| $Y$ | $\sigma_y$ | 180° rotation around y |
| $Z$ | $\sigma_z$ | 180° rotation around z |
| $H$ | $\frac{1}{\sqrt{2}}(\sigma_x + \sigma_z)$ | 180° around (x+z)/√2 |
| $R_x(\theta)$ | $e^{-i\theta\sigma_x/2}$ | $\theta$ rotation around x |
| $R_z(\phi)$ | $e^{-i\phi\sigma_z/2}$ | $\phi$ rotation around z |

The "recipe" for a quantum gate:
1. Apply a Hamiltonian $H = \frac{\hbar\Omega}{2}\hat{n}\cdot\vec{\sigma}$
2. Wait for time $t = \theta/\Omega$
3. Result: rotation by angle $\theta$ around axis $\hat{n}$

**Quantum control is choosing which Hamiltonian to apply and for how long.**

---

## Lab: Rabi Oscillations in Qiskit

Let's simulate Rabi oscillations using Qiskit.

```{code-block} python
import numpy as np
import matplotlib.pyplot as plt
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator
from qiskit.visualization import plot_bloch_multivector
from qiskit.quantum_info import Statevector

simulator = AerSimulator()

# Rabi oscillations: apply Rx(theta) for varying theta
thetas = np.linspace(0, 4*np.pi, 50)
prob_0 = []
prob_1 = []

for theta in thetas:
    qc = QuantumCircuit(1, 1)
    qc.rx(theta, 0)  # Rotation around x-axis
    qc.measure(0, 0)
    
    counts = simulator.run(qc, shots=1000).result().get_counts()
    prob_0.append(counts.get('0', 0) / 1000)
    prob_1.append(counts.get('1', 0) / 1000)

# Plot
plt.figure(figsize=(10, 5))
plt.plot(thetas, prob_0, 'b-', label='P(0)', linewidth=2)
plt.plot(thetas, prob_1, 'r-', label='P(1)', linewidth=2)
plt.xlabel('Rotation angle θ (radians)', fontsize=12)
plt.ylabel('Probability', fontsize=12)
plt.title('Rabi Oscillations: Rx(θ) applied to |0⟩', fontsize=14)
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

### Visualizing on the Bloch Sphere

```{code-block} python
# Watch the state rotate as we increase theta
thetas_to_show = [0, np.pi/4, np.pi/2, 3*np.pi/4, np.pi]

fig, axes = plt.subplots(1, 5, figsize=(15, 3))

for i, theta in enumerate(thetas_to_show):
    qc = QuantumCircuit(1)
    qc.rx(theta, 0)
    state = Statevector(qc)
    # Note: plot_bloch_multivector creates its own figure
    # In practice, you'd display these one at a time
    print(f"θ = {theta:.2f}: state = {state}")
```

---

## Summary

Today we covered a lot of ground:

1. **Stern-Gerlach reveals spin** — electrons have exactly two spin states along any axis

2. **Rotating the apparatus = choosing measurement basis** — same as rotating a polarizer

3. **The uncertainty principle** — you can't know spin along perpendicular axes simultaneously

4. **Pauli matrices** — $\sigma_x$, $\sigma_y$, $\sigma_z$ represent spin measurements; any qubit operator is built from them

5. **Schrödinger equation** — $i\hbar\frac{d}{dt}|\psi\rangle = H|\psi\rangle$ governs time evolution

6. **Unitary evolution** — $|\psi(t)\rangle = e^{-iHt/\hbar}|\psi(0)\rangle$; probability is conserved

7. **Precession** — $H \propto \sigma_z$ causes rotation around z (phase accumulation)

8. **Rabi oscillations** — $H \propto \sigma_x$ causes rotation around x (population transfer)

9. **The Bloch sphere picture** — Hamiltonians generate rotations around their axis

---

## Homework

### Problem 1: Pauli Matrix Practice

**(a)** Verify that $\sigma_x^2 = \sigma_y^2 = \sigma_z^2 = I$.

**(b)** Compute $\sigma_x \sigma_y$ and $\sigma_y \sigma_x$. Verify that $\sigma_x \sigma_y = i\sigma_z$.

**(c)** Show that $[\sigma_x, \sigma_y] = 2i\sigma_z$.

**(d)** Show that $\{\sigma_x, \sigma_y\} = \sigma_x\sigma_y + \sigma_y\sigma_x = 0$ (anti-commutator).

---

### Problem 2: Eigenstates of Pauli Matrices

**(a)** Find the eigenstates of $\sigma_x$. Verify they have eigenvalues $\pm 1$.

**(b)** Find the eigenstates of $\sigma_y$. Verify they have eigenvalues $\pm 1$.

**(c)** For each eigenstate you found, what is the probability of measuring $|0\rangle$ (spin-up along z)?

---

### Problem 3: Spin in a Magnetic Field

An electron in a magnetic field $\vec{B} = B_0 \hat{z}$ has Hamiltonian:

$$
H = \frac{\hbar\omega_0}{2}\sigma_z
$$

where $\omega_0 = \gamma B_0$ is the Larmor frequency.

**(a)** The electron starts in state $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$. Find $|\psi(t)\rangle$.

**(b)** Calculate $P_0(t)$ and $P_1(t)$, the probabilities of measuring spin-up and spin-down along z.

**(c)** Calculate $\langle \sigma_x \rangle(t)$ and $\langle \sigma_y \rangle(t)$. What motion does this describe?

**(d)** Sketch the trajectory of the state on the Bloch sphere.

---

### Problem 4: Rabi Oscillations

A qubit has Hamiltonian $H = \frac{\hbar\Omega}{2}\sigma_x$.

**(a)** Starting in $|0\rangle$, find $|\psi(t)\rangle$.

**(b)** What is $P_1(t)$, the probability of finding the qubit in $|1\rangle$?

**(c)** At what time $t$ does the qubit first reach $|1\rangle$ with certainty? This is called a π-pulse.

**(d)** At what time does the qubit first reach an equal superposition? This is a π/2-pulse.

**(e)** Sketch $P_0(t)$ and $P_1(t)$ from $t=0$ to $t=4\pi/\Omega$.

---

### Problem 5: Rotation Matrices

The rotation operator around axis $\hat{n}$ by angle $\theta$ is:

$$
R_{\hat{n}}(\theta) = e^{-i\theta \hat{n}\cdot\vec{\sigma}/2} = \cos\frac{\theta}{2}I - i\sin\frac{\theta}{2}(\hat{n}\cdot\vec{\sigma})
$$

**(a)** Derive $R_x(\theta) = e^{-i\theta\sigma_x/2}$ explicitly as a 2×2 matrix.

**(b)** Verify that $R_x(\pi) = -i\sigma_x$ (the X gate, up to a global phase).

**(c)** Verify that $R_z(\phi) = \begin{pmatrix} e^{-i\phi/2} & 0 \\ 0 & e^{i\phi/2} \end{pmatrix}$.

**(d)** Show that $R_z(\phi)|+\rangle$ gives a state on the equator of the Bloch sphere, rotated by angle $\phi$ from $|+\rangle$.

---

### Problem 6: Unitarity and Probability Conservation

**(a)** Show that if $H = H^\dagger$ (Hermitian), then $U = e^{-iHt/\hbar}$ satisfies $U^\dagger U = I$ (unitary).

*Hint:* Use $(e^A)^\dagger = e^{A^\dagger}$.

**(b)** Show that unitary evolution preserves the norm: if $|\psi(t)\rangle = U(t)|\psi(0)\rangle$ and $U$ is unitary, then $\langle\psi(t)|\psi(t)\rangle = \langle\psi(0)|\psi(0)\rangle$.

**(c)** Why is probability conservation important physically?

---

### Problem 7: Decomposing a Hamiltonian

Any 2×2 Hermitian matrix can be written as $H = a_0 I + a_x\sigma_x + a_y\sigma_y + a_z\sigma_z$.

**(a)** Given 
$$H = \begin{pmatrix} 2 & 1-i \\ 1+i & 0 \end{pmatrix}$$
find $a_0$, $a_x$, $a_y$, $a_z$.

*Hint:* Use $a_0 = \frac{1}{2}\text{Tr}(H)$ and $a_i = \frac{1}{2}\text{Tr}(\sigma_i H)$.

**(b)** What is the rotation axis $\hat{n} = (a_x, a_y, a_z)/|\vec{a}|$ for this Hamiltonian?

**(c)** What are the energy eigenvalues of $H$?

---

### Problem 8: Qiskit Exploration

**(a)** Use Qiskit to implement Rabi oscillations. Starting from $|0\rangle$, apply $R_x(\theta)$ for $\theta$ from 0 to $4\pi$. Plot $P_0$ and $P_1$ vs $\theta$. Verify the period is $2\pi$.

**(b)** Now do the same with $R_y(\theta)$. How does the result compare to $R_x$?

**(c)** Apply $R_z(\phi)$ to $|0\rangle$ for various $\phi$. What happens to $P_0$ and $P_1$? Explain using the Bloch sphere.

**(d)** Create a state on the equator using $H|0\rangle = |+\rangle$. Now apply $R_z(\phi)$ for various $\phi$ and measure in the X-basis (apply $H$ before measuring). Plot the probability vs $\phi$. What do you observe?

---

### Problem 9: Physical Units

The electron spin Hamiltonian in a magnetic field is $H = -\gamma \vec{S} \cdot \vec{B}$ where $\gamma = g_e \mu_B/\hbar$ is the gyromagnetic ratio.

For an electron: $g_e \approx 2$, $\mu_B = 9.274 \times 10^{-24}$ J/T.

**(a)** In a magnetic field $B = 1$ T, what is the Larmor frequency $\omega_0 = \gamma B$ in rad/s? In GHz?

**(b)** What is the period of precession?

**(c)** In NMR, protons have $\gamma_p = 2.675 \times 10^8$ rad/(s·T). In a 1 T field, what is the Larmor frequency? Why is this in the radio frequency (RF) range?

---

## Looking Ahead

Next lecture, we'll put everything together. We'll see how:
- A π/2 pulse followed by free evolution followed by another π/2 pulse creates interference
- This is the **Ramsey sequence** — the basis of atomic clocks
- Phase differences become measurable probabilities
- This is quantum sensing: using interference to measure tiny energy shifts

The one-qubit interferometer we studied in Lecture 5 will finally become a real physical device.