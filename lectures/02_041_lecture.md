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



# Lecture 8: Ramsey Interferometry and Quantum Sensing

## The Payoff: From Abstract to Technology

Over the past three lectures, we've built up a complete toolkit:
- Complex amplitudes and interference (Lecture 5)
- Measurement and state collapse (Lecture 6)
- Time evolution and rotations on the Bloch sphere (Lecture 7)

Today, we put it all together. We'll see how the abstract one-qubit interferometer becomes a real device: the **Ramsey interferometer**. This is the heart of atomic clocks, the most precise instruments ever built.

And we'll see the broader picture: a qubit is a **sensor**. Anything that affects its energy levels can be measured through interference.

---

## Review: The One-Qubit Interferometer

In Lecture 5, we analyzed the Mach-Zehnder interferometer for light:
- Beam splitter (split)
- Phase accumulation in one arm
- Beam splitter (recombine)
- Detection

The quantum circuit version is:

$$
|0\rangle \xrightarrow{H} \xrightarrow{R_z(\phi)} \xrightarrow{H} \xrightarrow{\text{measure}}
$$

We derived:
$$
P(0) = \cos^2(\phi/2), \qquad P(1) = \sin^2(\phi/2)
$$

The phase $\phi$ becomes encoded in the measurement probability. But where does this phase come from physically?

---

## The Ramsey Sequence

In atomic physics, the same interferometer appears with a clear physical interpretation.

### The Setup

1. **Atom starts in ground state:** $|0\rangle$ (or $|\downarrow\rangle$)

2. **First π/2-pulse:** Apply a resonant driving field for time $t_{\pi/2} = \pi/(2\Omega)$

   This rotates around x by 90°, creating superposition:
   $$|0\rangle \to \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$$
   
   On the Bloch sphere: north pole → equator

3. **Free evolution for time $T$:** The atom evolves under its natural Hamiltonian $H = \frac{\hbar\omega_0}{2}\sigma_z$

   The state precesses around z, accumulating phase:
   $$\frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle) \to \frac{1}{\sqrt{2}}(e^{-i\omega_0 T/2}|0\rangle - i e^{i\omega_0 T/2}|1\rangle)$$
   
   The relative phase is $\phi = \omega_0 T$

4. **Second π/2-pulse:** Another rotation around x by 90°

   The two "paths" (being in $|0\rangle$ vs $|1\rangle$) recombine and interfere

5. **Measure:** The interference determines $P(0)$ and $P(1)$

```{figure} ./02_04_lecture_files/ramsey_sequence.svg
:width: 500px
:name: ramsey-sequence

The Ramsey sequence: π/2 pulse - free evolution - π/2 pulse - measure.
```

### The Calculation

Let's work through this carefully.

**After first π/2-pulse** ($R_x(\pi/2)$ on $|0\rangle$):

Using $R_x(\pi/2) = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & -i \\ -i & 1 \end{pmatrix}$:

$$
|\psi_1\rangle = R_x(\pi/2)|0\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)
$$

**After free evolution for time $T$** (under $H = \frac{\hbar\omega_0}{2}\sigma_z$):

$$
|\psi_2\rangle = R_z(\omega_0 T)|\psi_1\rangle = \frac{1}{\sqrt{2}}(e^{-i\omega_0 T/2}|0\rangle - i e^{i\omega_0 T/2}|1\rangle)
$$

Pull out a global phase $e^{-i\omega_0 T/2}$:

$$
|\psi_2\rangle = \frac{e^{-i\omega_0 T/2}}{\sqrt{2}}(|0\rangle - i e^{i\omega_0 T}|1\rangle)
$$

**After second π/2-pulse:**

$$
|\psi_3\rangle = R_x(\pi/2)|\psi_2\rangle
$$

After some algebra (or using the general rotation formula):

$$
|\psi_3\rangle = \frac{e^{-i\omega_0 T/2}}{\sqrt{2}}\left[\frac{1-e^{i\omega_0 T}}{2}|0\rangle + \frac{-i(1+e^{i\omega_0 T})}{2}|1\rangle\right]
$$

The probabilities are:

$$
P(0) = \sin^2\left(\frac{\omega_0 T}{2}\right), \qquad P(1) = \cos^2\left(\frac{\omega_0 T}{2}\right)
$$

```{admonition} The Ramsey Result
:class: important

For a Ramsey sequence with free evolution time $T$:

$$
P(0) = \sin^2\left(\frac{\omega_0 T}{2}\right) = \frac{1 - \cos(\omega_0 T)}{2}
$$

The atomic transition frequency $\omega_0$ is directly encoded in the oscillation of the measurement probability.
```

---

## Ramsey Fringes

If we vary the free evolution time $T$ (or equivalently, the detuning), the probability oscillates. These oscillations are called **Ramsey fringes**.

```{figure} ./02_04_lecture_files/ramsey_fringes.svg
:width: 500px
:name: ramsey-fringes

Ramsey fringes: probability oscillates with free evolution time or detuning.
```

### With Detuning

In practice, we often work in a frame rotating at some reference frequency $\omega_{\text{ref}}$. The free evolution phase then depends on the **detuning**:

$$
\delta = \omega_0 - \omega_{\text{ref}}
$$

The accumulated phase is $\phi = \delta \cdot T$, and:

$$
P(0) = \sin^2\left(\frac{\delta T}{2}\right)
$$

Scanning $\delta$ (by changing the reference frequency) produces fringes with period:

$$
\Delta\delta = \frac{2\pi}{T}
$$

**Key insight:** Longer free evolution time $T$ means narrower fringes — higher frequency resolution.

---

## Why Ramsey Beats Direct Measurement

Why use this complicated sequence? Why not just measure the atomic frequency directly?

### The Linewidth Problem

If you try to measure $\omega_0$ by driving the transition and looking for maximum absorption, the spectral line has a **width** determined by how long you probe:

$$
\Delta\omega \sim \frac{1}{t_{\text{probe}}}
$$

This is the time-energy uncertainty relation.

### Ramsey's Trick

Ramsey's insight: **separate the "probing" (π/2-pulses) from the "measuring" (free evolution).**

The π/2-pulses can be short (microseconds). The free evolution can be long (seconds). The resolution is set by the free evolution time:

$$
\Delta\omega \sim \frac{1}{T}
$$

You get the resolution of a long measurement with the control of short pulses.

```{admonition} Norman Ramsey's Nobel Prize
:class: note

Norman Ramsey received the 1989 Nobel Prize in Physics for developing this technique. His "separated oscillatory fields" method is the basis of all modern precision spectroscopy and atomic clocks.
```

---

## Atomic Clocks: The Most Precise Instruments

An atomic clock is a Ramsey interferometer where the phase comes from the energy difference between two atomic states.

### The Principle

Atoms have precise, universal energy levels. The transition frequency between two levels is a constant of nature:

**Cesium-133:** The ground state hyperfine transition defines the second.

$$
\omega_{\text{Cs}} = 2\pi \times 9,192,631,770 \text{ Hz (exactly)}
$$

This isn't an approximate measurement — it's the **definition** of the second since 1967.

### How It Works

1. **Prepare atoms in state $|0\rangle$** (ground hyperfine level)

2. **Apply π/2-pulse** from a local oscillator at frequency $\omega_{\text{LO}}$

3. **Free evolution for time $T$**

4. **Apply second π/2-pulse**

5. **Measure population in $|0\rangle$**

6. **Feedback:** Adjust $\omega_{\text{LO}}$ to lock to the atomic frequency

```{figure} ./02_04_lecture_files/atomic_clock_schematic.svg
:width: 450px
:name: atomic-clock

Schematic of an atomic clock: Ramsey sequence + feedback loop.
```

The feedback loop keeps the local oscillator exactly on resonance with the atomic transition. The oscillator's ticks become "atomic ticks."

### Performance

Modern cesium clocks achieve:
- **Fractional accuracy:** $\sim 10^{-16}$
- **Would gain or lose 1 second in:** 300 million years

Optical atomic clocks (using visible light transitions) do even better:
- **Fractional accuracy:** $\sim 10^{-18}$
- **Would gain or lose 1 second in:** longer than the age of the universe

```{admonition} Why So Precise?
:class: tip

1. **Universal:** All cesium atoms are identical. The frequency is a constant of nature.

2. **Long coherence:** Atoms can be isolated from perturbations, allowing long free evolution times.

3. **Quantum interference:** The Ramsey fringe pattern allows exquisite frequency discrimination.

4. **Feedback averaging:** Running continuously and averaging reduces statistical noise.
```

---

## GPS: Atomic Clocks in Your Pocket

The Global Positioning System relies on atomic clocks.

### The Principle

Each GPS satellite carries atomic clocks and broadcasts timing signals. Your phone receives signals from multiple satellites, each with a different travel time.

The time delay tells you the distance: $d = c \cdot \Delta t$

From multiple distances, you triangulate your position.

### The Numbers

- Light travels ~30 cm in 1 nanosecond
- To get 1-meter position accuracy, you need ~3 ns timing accuracy
- Over one day, a clock must not drift more than ~1 µs
- This requires fractional accuracy of $\sim 10^{-14}$

Without atomic clocks, GPS would accumulate errors of kilometers per day.

```{admonition} Relativity Correction
:class: note

GPS satellites orbit at high altitude and high speed. General relativity (gravity) and special relativity (velocity) both affect the clock rates:
- Gravitational time dilation: clocks run faster by ~45 µs/day
- Velocity time dilation: clocks run slower by ~7 µs/day
- Net effect: clocks run ~38 µs/day fast

This is corrected for. GPS is a real-world test of Einstein's theories.
```

---

## Quantum Sensing: The Qubit as a Probe

Atomic clocks are the most famous application, but the principle is much broader.

**Core idea:** A qubit is sensitive to anything that shifts its energy levels. The Ramsey sequence converts that energy shift into a measurable phase.

### The General Framework

Any perturbation that causes a Hamiltonian $H = \frac{\hbar\omega}{2}\sigma_z$ leads to phase accumulation:

$$
\phi = \omega \cdot T
$$

If $\omega$ depends on some physical quantity $X$ (magnetic field, electric field, gravity, etc.), then measuring $\phi$ measures $X$:

$$
X \to \omega(X) \to \phi = \omega T \to P(0) = \sin^2(\phi/2)
$$

### Example: Magnetometry

An electron spin in a magnetic field $B$ has:

$$
\omega = \gamma_e B
$$

where $\gamma_e = 2\pi \times 28 \text{ GHz/T}$ is the electron gyromagnetic ratio.

A Ramsey sequence with time $T$ gives phase $\phi = \gamma_e B T$.

**Sensitivity:** The minimum detectable field change corresponds to the minimum detectable phase change:

$$
\delta B = \frac{\delta\phi}{\gamma_e T}
$$

Longer $T$ → better sensitivity (until decoherence sets in).

### Example: Gravimetry

Atom interferometers use the gravitational potential energy:

$$
\phi = \frac{m g \Delta h \cdot T}{\hbar}
$$

where $\Delta h$ is the height difference between paths in a matter-wave interferometer.

These devices can measure:
- Gravitational acceleration $g$ to 9 decimal places
- Gravitational gradients (useful for underground mapping)
- Tests of the equivalence principle

### The Quantum Advantage

Classical sensors measure a field by its direct effect (deflection, voltage, etc.). Quantum sensors measure through **interference** — they're sensitive to phase, which can be accumulated over time.

The precision scales as:
- **Classical averaging:** $\delta X \propto 1/\sqrt{T}$ (central limit theorem)
- **Quantum (Ramsey):** $\delta X \propto 1/T$ (phase accumulation)

This is called the **Standard Quantum Limit**. With entanglement, you can do even better (**Heisenberg Limit**), but that's for a later lecture.

---

## Decoherence: The Enemy of Coherence

There's a catch. We've been assuming the qubit evolves purely under the desired Hamiltonian. In reality, the qubit also interacts with its environment.

### What Goes Wrong

Random interactions with the environment cause:
1. **Dephasing:** Random phase kicks scramble the accumulated phase
2. **Relaxation:** The qubit decays from $|1\rangle$ to $|0\rangle$

Both effects destroy the interference pattern.

```{figure} ./02_04_lecture_files/decoherence.svg
:width: 400px
:name: decoherence

Decoherence: Ramsey fringes decay as free evolution time increases.
```

### Coherence Times

The characteristic timescales are:
- **$T_1$ (relaxation time):** How long until the qubit decays
- **$T_2$ (dephasing time):** How long until phase coherence is lost

For quantum sensing or computing, you need:
$$
T_{\text{operation}} \ll T_2
$$

| System | Typical $T_2$ |
|--------|---------------|
| Trapped ions | seconds to minutes |
| Neutral atoms | ~1 second |
| Superconducting qubits | ~100 µs |
| NV centers | ~ms |
| Electron spin (solid state) | ~µs to ms |

The quest for longer coherence times drives much of modern quantum hardware research.

---

## Lab: Ramsey Fringes in Qiskit

Let's simulate a Ramsey sequence.

```{code-block} python
import numpy as np
import matplotlib.pyplot as plt
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Ramsey sequence: Rx(pi/2) - Rz(phi) - Rx(pi/2) - measure
# The Rz represents free evolution with accumulated phase phi

phases = np.linspace(0, 4*np.pi, 100)
prob_0 = []

for phi in phases:
    qc = QuantumCircuit(1)
    
    # First pi/2 pulse (around x)
    qc.rx(np.pi/2, 0)
    
    # Free evolution (phase accumulation around z)
    qc.rz(phi, 0)
    
    # Second pi/2 pulse (around x)
    qc.rx(np.pi/2, 0)
    
    # Get probabilities
    state = Statevector(qc)
    probs = state.probabilities()
    prob_0.append(probs[0])

# Plot Ramsey fringes
plt.figure(figsize=(10, 5))
plt.plot(phases/np.pi, prob_0, 'b-', linewidth=2)
plt.xlabel('Phase φ/π (proportional to detuning × time)', fontsize=12)
plt.ylabel('P(0)', fontsize=12)
plt.title('Ramsey Fringes', fontsize=14)
plt.grid(True, alpha=0.3)
plt.axhline(y=0.5, color='gray', linestyle='--', alpha=0.5)
plt.show()

# Verify: P(0) should follow sin²(φ/2)
theory = np.sin(phases/2)**2
plt.figure(figsize=(10, 5))
plt.plot(phases/np.pi, prob_0, 'b-', linewidth=2, label='Qiskit simulation')
plt.plot(phases/np.pi, theory, 'r--', linewidth=2, label='Theory: sin²(φ/2)')
plt.xlabel('Phase φ/π', fontsize=12)
plt.ylabel('P(0)', fontsize=12)
plt.title('Ramsey Fringes: Simulation vs Theory', fontsize=14)
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

### Adding Decoherence (Conceptual)

Real decoherence requires simulating open quantum systems, which is beyond basic Qiskit. But we can simulate the effect:

```{code-block} python
# Simulating the effect of decoherence on Ramsey fringes
# As T increases, the fringe contrast decays

T2 = 10  # arbitrary units

evolution_times = np.linspace(0, 30, 100)
prob_0_with_decoherence = []

for T in evolution_times:
    # Phase accumulates
    phi = T  # Assume detuning = 1 in arbitrary units
    
    # Ideal probability
    P0_ideal = np.sin(phi/2)**2
    
    # Decoherence reduces fringe contrast
    contrast = np.exp(-T/T2)
    P0_real = 0.5 + contrast * (P0_ideal - 0.5)
    
    prob_0_with_decoherence.append(P0_real)

plt.figure(figsize=(10, 5))
plt.plot(evolution_times, prob_0_with_decoherence, 'b-', linewidth=2)
plt.xlabel('Free evolution time T (arb. units)', fontsize=12)
plt.ylabel('P(0)', fontsize=12)
plt.title(f'Ramsey Fringes with Decoherence (T₂ = {T2})', fontsize=14)
plt.axhline(y=0.5, color='gray', linestyle='--', alpha=0.5)
plt.grid(True, alpha=0.3)
plt.show()
```

---

## The Big Picture: Interference as Information

Let's step back and see the unified view.

| System | "Split" | Phase Source | "Recombine" | Observable |
|--------|---------|--------------|-------------|------------|
| Double slit | Two slits | Path length difference | Screen | Intensity pattern |
| Mach-Zehnder | Beam splitter | Arm length or medium | Beam splitter | Detector signal |
| Polarization | Waveplate | Birefringence | Waveplate | Polarization |
| Ramsey | π/2 pulse | Energy splitting × time | π/2 pulse | Population |
| Atom interferometer | Light pulse | Gravity × path × time | Light pulse | Momentum state |

Every quantum sensor follows this pattern:
1. **Create superposition** (split into paths/states)
2. **Accumulate phase** from whatever you want to measure
3. **Interfere** (recombine)
4. **Measure** (convert phase to probability)

```{admonition} The Central Idea
:class: important

**Quantum sensing converts physical quantities into phase differences, which interference converts into measurable probabilities.**

Phase can accumulate continuously, giving sensitivity that improves with time — until decoherence sets in.
```

---

## Summary

Today we completed our single-qubit journey:

1. **The Ramsey sequence** — π/2 pulse, free evolution, π/2 pulse — is a qubit interferometer

2. **Free evolution creates phase** — $\phi = \omega_0 T$ from energy splitting

3. **Ramsey fringes** — probability oscillates with phase, allowing frequency measurement

4. **Atomic clocks** use Ramsey to lock an oscillator to atomic transitions
   - Cesium defines the second: 9,192,631,770 Hz
   - Best clocks: 1 second accuracy over the age of the universe

5. **GPS** relies on atomic clocks for timing signals

6. **Quantum sensing** — any energy shift becomes a measurable phase
   - Magnetometry, gravimetry, electric fields, etc.
   - Sensitivity: $\delta\omega \sim 1/T$

7. **Decoherence** limits the useful evolution time

8. **Universal pattern:** Split → Phase → Recombine → Measure

---

## Homework

### Problem 1: Ramsey Derivation

Work through the Ramsey sequence calculation in detail.

**(a)** Start with $|0\rangle$. Apply $R_x(\pi/2)$. Write the resulting state.

**(b)** Apply $R_z(\phi)$ where $\phi = \omega_0 T$. Write the resulting state.

**(c)** Apply another $R_x(\pi/2)$. Write the final state.

**(d)** Compute $P(0)$ and $P(1)$. Verify they sum to 1.

**(e)** For what values of $\phi$ is $P(0) = 0$? For what values is $P(0) = 1$?

---

### Problem 2: Ramsey Fringes

Consider a Ramsey experiment where the free evolution time is $T$ and the atomic transition frequency is $\omega_0$.

**(a)** If you scan the local oscillator frequency $\omega_{\text{LO}}$ near $\omega_0$, the effective detuning is $\delta = \omega_0 - \omega_{\text{LO}}$. What is the fringe period $\Delta\delta$?

**(b)** If $T = 1$ s, what is the fringe period in Hz?

**(c)** If $T = 1$ ms, what is the fringe period?

**(d)** Why would you want longer $T$? What limits how long $T$ can be?

---

### Problem 3: Atomic Clock Precision

A cesium atomic clock has transition frequency $\omega_{\text{Cs}} = 2\pi \times 9.192631770$ GHz.

**(a)** If the Ramsey time is $T = 10$ ms, what is the linewidth $\Delta\omega$ of the Ramsey fringes?

**(b)** What fractional frequency precision $\Delta\omega/\omega_0$ does this correspond to?

**(c)** If you average for 1 day ($\sim 10^5$ seconds), and the noise averages down as $1/\sqrt{N}$ where $N$ is the number of measurements, by what factor does the precision improve?

**(d)** A modern optical clock uses a transition at $\omega = 2\pi \times 429$ THz (strontium). If it achieves the same absolute frequency uncertainty $\Delta\omega$ as the cesium clock, what fractional precision does this give?

---

### Problem 4: Magnetometry

An electron spin is used as a magnetic field sensor. The Zeeman splitting is $\omega = \gamma_e B$ where $\gamma_e = 2\pi \times 28$ GHz/T.

**(a)** If the Ramsey time is $T = 1$ µs, what is the phase accumulated in a field $B = 1$ µT?

**(b)** If you can resolve phase to $\delta\phi = 0.01$ rad (from counting statistics), what is the minimum detectable field change $\delta B$?

**(c)** If you increase $T$ to 100 µs (still shorter than $T_2$), how does the sensitivity improve?

**(d)** The Earth's magnetic field is about 50 µT. How many Ramsey fringes would you see scanning from 0 to 50 µT with $T = 1$ µs?

---

### Problem 5: Decoherence

A qubit has dephasing time $T_2 = 100$ µs.

**(a)** In a Ramsey experiment, the fringe contrast decays as $C(T) = e^{-T/T_2}$. The probability becomes:

$$
P(0) = \frac{1}{2} + \frac{C(T)}{2}\sin(\omega_0 T)
$$

Plot $P(0)$ vs $T$ for $\omega_0 = 2\pi \times 10$ MHz, from $T = 0$ to $T = 500$ µs.

**(b)** What is the fringe contrast at $T = T_2$? At $T = 2T_2$?

**(c)** For sensing, there's a trade-off: longer $T$ gives better resolution but lower contrast. The signal-to-noise ratio goes as $C(T) \times T$. What value of $T$ maximizes this?

---

### Problem 6: GPS Timing

GPS satellites orbit at altitude $h = 20,200$ km with orbital period $\approx 12$ hours.

**(a)** The speed of light is $c = 3 \times 10^8$ m/s. How long does a signal take to travel from satellite to ground?

**(b)** If you want 1 meter position accuracy, what timing accuracy is needed? (Recall $d = c \cdot \Delta t$.)

**(c)** Over one day, how much can the satellite clock drift while still meeting this requirement?

**(d)** What fractional clock accuracy does this require?

---

### Problem 7: Comparing Classical and Quantum Sensing

Consider measuring a magnetic field using (i) a classical compass, (ii) a Hall sensor, (iii) an atomic magnetometer.

**(a)** A compass needle deflects by an angle proportional to the field. If you can resolve 0.1° deflection, and the sensitivity is 1°/mT, what is the minimum detectable field?

**(b)** A Hall sensor produces voltage $V = k B$ with $k = 1$ mV/mT. If your voltmeter can resolve 1 µV, what is the minimum detectable field?

**(c)** An atomic magnetometer with $\gamma = 2\pi \times 28$ GHz/T and $T = 1$ ms can resolve phase $\delta\phi = 0.01$ rad. What is the minimum detectable field?

**(d)** Compare the three sensitivities. Why is the atomic magnetometer so much better?

---

### Problem 8: The Ramsey Sequence in Qiskit

Implement the Ramsey sequence in Qiskit.

**(a)** Create a function `ramsey(phi)` that returns the probability $P(0)$ for a Ramsey sequence with phase $\phi$. Use the circuit: `rx(π/2)` → `rz(phi)` → `rx(π/2)` → measure.

**(b)** Plot Ramsey fringes: $P(0)$ vs $\phi$ for $\phi$ from 0 to $4\pi$. Verify they match the theoretical curve.

**(c)** Modify the sequence to use `ry(π/2)` pulses instead of `rx(π/2)`. How does the fringe pattern change?

**(d)** Now add a small "error" in the first pulse: use `rx(π/2 + ε)` with $\epsilon = 0.1$. Plot the new fringes. How does the contrast change?

---

### Problem 9: Conceptual Questions

**(a)** In a Ramsey sequence, what physical role does the first π/2-pulse play? What about the second?

**(b)** Why is Ramsey better than a single long pulse for measuring frequency? (Hint: think about the difference between "probe time" and "evolution time.")

**(c)** An atomic clock doesn't directly measure time — it measures frequency. How do you get a "clock" from a frequency measurement?

**(d)** If all cesium atoms have exactly the same transition frequency, why do we need to stabilize a local oscillator to them? Why not just count atomic transitions directly?

**(e)** Decoherence destroys quantum interference. Does this mean a decohered qubit is useless for sensing? Or can you still extract some information?

---

## Looking Ahead

We've now completed our study of single-qubit physics:
- **States:** Points on the Bloch sphere
- **Evolution:** Rotations (from Hamiltonians)
- **Measurement:** Projection onto an axis
- **Interference:** Split-phase-recombine pattern

Next chapter, we enter the truly quantum regime: **two qubits and entanglement**. When you have more than one qubit, something entirely new happens — correlations that have no classical explanation. This is where quantum computing gets its power.
