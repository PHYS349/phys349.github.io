# Lecture 2.6: Time Evolution and Rabi Oscillations

## Review: The Bloch Sphere and Rotations

### Qubit States on the Bloch Sphere

Any qubit state can be written as:

$$|\psi\rangle = \cos\frac{\theta}{2}|0\rangle + e^{i\phi}\sin\frac{\theta}{2}|1\rangle$$

where $\theta$ and $\phi$ are the polar and azimuthal angles on the Bloch sphere.

The six cardinal states are:
- **z-axis:** $|0\rangle$ (north pole), $|1\rangle$ (south pole)
- **x-axis:** $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$, $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$
- **y-axis:** $|+i\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$, $|-i\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$

### Rotations: SU(2)

Last lecture we learned that **SU(2)** is the group of rotations on the Bloch sphere.

**Rotation about the z-axis:**

$$R_z(\theta) = e^{-i\sigma_z\theta/2} = \cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,\sigma_z$$

where $\sigma_z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}$.

**Rotation about an arbitrary axis $\hat{n}$:**

$$R_{\hat{n}}(\theta) = e^{-i\theta(\hat{n}\cdot\vec{\sigma})/2} = \cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,(\hat{n}\cdot\vec{\sigma})$$

where $\hat{n}\cdot\vec{\sigma} = n_x\sigma_x + n_y\sigma_y + n_z\sigma_z$.

---

## iClicker Question 1

Consider the rotation $R_z(\theta) = e^{-i\sigma_z\theta/2}$.

**Which pair of states are unchanged by this rotation (up to a global phase)?**

- (A) $|+\rangle$ and $|-\rangle$ (the ±x states)
- (B) $|+i\rangle$ and $|-i\rangle$ (the ±y states)
- (C) $|0\rangle$ and $|1\rangle$ (the ±z states) ✓
- (D) All six cardinal states

**Answer:** (C). The states $|0\rangle$ and $|1\rangle$ are eigenstates of $\sigma_z$, so they only pick up a phase under $R_z(\theta)$:

$$R_z(\theta)|0\rangle = e^{-i\theta/2}|0\rangle, \qquad R_z(\theta)|1\rangle = e^{+i\theta/2}|1\rangle$$

States on the equator ($|+\rangle$, $|-\rangle$, $|+i\rangle$, $|-i\rangle$) are *not* eigenstates of $\sigma_z$ — they precess around the z-axis.

**General principle:** Eigenstates of a generator are invariant under the transformation it generates.

---

## The Stern-Gerlach Experiment: Discovering Spin

### The 1922 Experiment

Before we write down the spin Hamiltonian, let's see where it comes from experimentally.

In 1922, Otto Stern and Walther Gerlach performed one of the most important experiments in quantum mechanics. They sent a beam of **silver atoms** through an **inhomogeneous magnetic field** and observed where the atoms landed on a detector screen.

**Classical expectation:** A magnetic dipole in a non-uniform field feels a force proportional to its component along the field gradient. If the atomic magnetic moments were oriented randomly (like tiny compass needles pointing in all directions), the beam should spread into a **continuous smear**.

**What they observed:** The beam split into **exactly two discrete spots**.

### Why Deflection Measures Spin

The key physics is straightforward:

1. **Energy of a dipole in a field:** A magnetic moment $\vec{\mu}$ in a magnetic field has energy
$$U = -\vec{\mu}\cdot\vec{B}$$

2. **Force from a gradient:** Force is the negative gradient of energy. For a field pointing in the z-direction with a gradient:
$$F_z = -\frac{\partial U}{\partial z} = \mu_z\frac{\partial B_z}{\partial z}$$

3. **Magnetic moment comes from spin:** For an electron, $\mu_z = \gamma S_z$ where $\gamma$ is the gyromagnetic ratio.

4. **Putting it together:**
$$F_z = \gamma S_z \frac{\partial B_z}{\partial z}$$

The force is **directly proportional to** $S_z$. Classically, $S_z$ could be anything, giving a continuous range of forces and a smeared spot. But quantum mechanics says $S_z$ can only be $+\hbar/2$ or $-\hbar/2$ — nothing in between.

**Two allowed values of spin → two allowed forces → two spots.**

This was shocking. The atoms behaved as if their magnetic moment could only point in two directions — "up" or "down" relative to the field — with nothing in between.

### The Interpretation: Spin-1/2

We now understand that:

1. Silver has **one unpaired electron** in its outer shell (configuration: [Kr] 4d¹⁰ 5s¹)

2. The electron has an intrinsic angular momentum called **spin**, with quantum number $s = 1/2$

3. When measured along any axis, spin-1/2 can only yield two outcomes: $+\hbar/2$ ("spin up") or $-\hbar/2$ ("spin down")

4. The two spots correspond to these two eigenstates

**The Stern-Gerlach apparatus is a qubit measurement device.** It projects the electron spin onto the $|0\rangle$ or $|1\rangle$ state (relative to the field direction) and physically separates them.

### Why This Matters for Us

The Stern-Gerlach experiment shows that:
- Quantum measurements give **discrete outcomes**, not continuous values
- The spin state **collapses** upon measurement
- A spin-1/2 particle is a natural **physical qubit**

When we write $|\psi\rangle = c_0|0\rangle + c_1|1\rangle$ and talk about the Bloch sphere, we're describing something real: the quantum state of an electron spin.

---

## Back to Physics: Spin in a Magnetic Field

### The Physical Setup

An electron (or any spin-1/2 particle) in a magnetic field $\vec{B}$ has Hamiltonian:

$$H = -\gamma\vec{S}\cdot\vec{B} = -\frac{\gamma\hbar}{2}\vec{\sigma}\cdot\vec{B}$$

where $\gamma$ is the gyromagnetic ratio.

The spin precesses around the magnetic field direction — this is the quantum analog of a gyroscope.

### The Question

Given an initial state $|\psi(0)\rangle$, how do we find $|\psi(t)\rangle$?

---

## The Schrödinger Equation

### The Fundamental Law

Quantum states evolve according to:

$$H|\psi(t)\rangle = i\hbar\frac{\partial}{\partial t}|\psi(t)\rangle$$

### Solving with the Time Evolution Operator

Define the **time evolution operator** $U(t)$ by:

$$U(t)|\psi(0)\rangle = |\psi(t)\rangle$$

Substituting into the Schrödinger equation:

$$H\,U(t)|\psi(0)\rangle = i\hbar\frac{\partial}{\partial t}U(t)|\psi(0)\rangle$$

Since this must hold for *any* initial state:

$$H\,U(t) = i\hbar\frac{\partial U}{\partial t}$$

**The solution is:**

$$\boxed{U(t) = e^{-iHt/\hbar}}$$

You can verify this solves the equation: $\frac{\partial}{\partial t}e^{-iHt/\hbar} = \frac{-iH}{\hbar}e^{-iHt/\hbar}$.

### The Hamiltonian as a Generator

In the infinitesimal limit ($\Delta t$ small):

$$U(\Delta t) \approx I - \frac{iH}{\hbar}\Delta t$$

Compare to our rotation formula: $R(\delta\theta) \approx I + \delta\theta\, G$.

**The Hamiltonian is the generator of time evolution.**

Just as the Pauli matrices generate rotations in space, the Hamiltonian generates translations in time.

---

## $U(t)$ is Unitary

### Why Unitarity Matters

We start with a normalized state: $\langle\psi(0)|\psi(0)\rangle = 1$.

We need it to *stay* normalized:

$$\langle\psi(t)|\psi(t)\rangle = \langle\psi(0)|U^\dagger U|\psi(0)\rangle$$

For this to equal 1 for any initial state, we need:

$$\boxed{U^\dagger U = I}$$

This is the definition of a **unitary** operator.

### Verification

For $U(t) = e^{-iHt/\hbar}$ with Hermitian $H$ (i.e., $H^\dagger = H$):

$$U^\dagger = \left(e^{-iHt/\hbar}\right)^\dagger = e^{+iH^\dagger t/\hbar} = e^{+iHt/\hbar}$$

$$U^\dagger U = e^{+iHt/\hbar}e^{-iHt/\hbar} = e^0 = I \quad \checkmark$$

**Unitarity guarantees probability conservation.**


---
## iClicker Questions

### iClicker A: The Generator Pattern

The Hamiltonian generates time evolution: $e^{-iHt/\hbar}|\psi(0)\rangle = |\psi(t)\rangle$.

**What does the operator $e^{-i\hat{p}x_0/\hbar}$ do to a wavefunction $\psi(x)$?**

- (A) Translates it forward in time
- (B) Translates it in position by $x_0$ ✓
- (C) Rotates it on the Bloch sphere
- (D) Decreases its amplitude
- (E) Inverts it

**Answer:** (B). Just as the Hamiltonian $H$ generates time translations, the momentum operator $\hat{p}$ generates spatial translations:

$$e^{-i\hat{p}x_0/\hbar}\psi(x) = \psi(x + x_0)$$


This is the same pattern: Hermitian operator in the exponent -> unitary transformation that "moves" the state.

---

## The Generator Pattern

We've now seen the same mathematical structure three times:

| Transformation | Unitary Operator | Hermitian Generator |
|----------------|------------------|---------------------|
| Spin rotation | $R_z(\theta) = e^{-i\sigma_z\theta/2}$ | $\sigma_z$ (and $\sigma_x, \sigma_y$) |
| Time evolution | $U(t) = e^{-iHt/\hbar}$ | $H$ (Hamiltonian) |
| Space translation | $T(x) = e^{-i\hat{p}x/\hbar}$ | $\hat{p} = -i\hbar\frac{\partial}{\partial x}$ |

**The pattern:** Hermitian operators generate unitary transformations through exponentiation.

This is one of the deepest ideas in physics: **symmetries (unitary transformations) are generated by observables (Hermitian operators)**.

---

## Example 1: Magnetic Field Along z — The Phase Gate

### Setup

$$\vec{B} = B_z\hat{z}$$

$$H = -\gamma B_z \frac{\hbar}{2}\sigma_z = \frac{\hbar\omega_0}{2}\sigma_z$$

where $\omega_0 = -\gamma B_z$ is the **Larmor frequency**.

### Time Evolution

$$U(t) = e^{-iHt/\hbar} = e^{-i\omega_0 t\,\sigma_z/2} = R_z(\omega_0 t)$$

This is **rotation around the z-axis** at angular frequency $\omega_0$.

### What Happens to Different States?

**Starting in $|0\rangle$:**

$$U(t)|0\rangle = e^{-i\omega_0 t/2}|0\rangle$$

Just a global phase — the state doesn't move on the Bloch sphere.

**Starting in $|+\rangle$:**

$$U(t)|+\rangle = \frac{1}{\sqrt{2}}\left(e^{-i\omega_0 t/2}|0\rangle + e^{+i\omega_0 t/2}|1\rangle\right)$$

The relative phase changes — the state precesses around the equator.

This is **precession**: the quantum version of a spinning top in a gravitational field.

---

## Example 2: Magnetic Field Along x — Rabi Oscillations

### Setup

$$\vec{B} = B_x\hat{x}$$

$$H = \frac{\hbar\Omega}{2}\sigma_x$$

where $\Omega$ is the **Rabi frequency** (proportional to $B_x$).

### Time Evolution

$$U(t) = e^{-i\Omega t\,\sigma_x/2} = R_x(\Omega t)$$

Using the rotation formula:

$$R_x(\theta) = \cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,\sigma_x = \begin{pmatrix} \cos\frac{\theta}{2} & -i\sin\frac{\theta}{2} \\ -i\sin\frac{\theta}{2} & \cos\frac{\theta}{2} \end{pmatrix}$$

### Evolution of $|0\rangle$

Starting in the ground state $|0\rangle$:

$$|\psi(t)\rangle = U(t)|0\rangle = \begin{pmatrix} \cos\frac{\Omega t}{2} \\ -i\sin\frac{\Omega t}{2} \end{pmatrix} = \cos\frac{\Omega t}{2}|0\rangle - i\sin\frac{\Omega t}{2}|1\rangle$$

### The Probabilities Oscillate!

$$P_0(t) = \cos^2\frac{\Omega t}{2}$$

$$P_1(t) = \sin^2\frac{\Omega t}{2}$$

The population **oscillates** between $|0\rangle$ and $|1\rangle$. This is fundamentally different from precession (Example 1), where the populations stayed constant.

### Special Pulses

**π-pulse** ($\Omega t = \pi$):
$$|0\rangle \xrightarrow{\pi\text{-pulse}} -i|1\rangle \equiv |1\rangle$$

Complete population inversion — the qubit flips from ground to excited state.

**π/2-pulse** ($\Omega t = \pi/2$):
$$|0\rangle \xrightarrow{\pi/2\text{-pulse}} \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$$

Creates an equal superposition — the state moves from the pole to the equator.

---

## Eigenstates: States That Don't Change

### Definition

An **eigenstate** of the Hamiltonian is a state that evolves only by a global phase:

$$H|\psi_n\rangle = E_n|\psi_n\rangle$$

Under time evolution:

$$e^{-iHt/\hbar}|\psi_n\rangle = e^{-iE_n t/\hbar}|\psi_n\rangle$$

The state just picks up a phase factor — it doesn't move on the Bloch sphere.

### Examples

**For $H = \frac{\hbar\omega_0}{2}\sigma_z$:**
- Eigenstates are $|0\rangle$ and $|1\rangle$ (the poles)
- These don't precess — they're aligned with the field

**For $H = \frac{\hbar\Omega}{2}\sigma_x$:**
- Eigenstates are $|+\rangle$ and $|-\rangle$ (the x-axis states)
- Under a $B_x$ field, $|+\rangle$ doesn't undergo Rabi oscillations — it's aligned with the drive

**General rule:** Eigenstates of the Hamiltonian are stationary. Superpositions of different energy eigenstates oscillate.

---

## Qiskit Demonstration: Rabi Oscillations

```python
import numpy as np
import matplotlib.pyplot as plt
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Rabi oscillations: Rx(θ) applied to |0⟩
thetas = np.linspace(0, 4*np.pi, 100)
P0 = []
P1 = []

for theta in thetas:
    qc = QuantumCircuit(1)
    qc.rx(theta, 0)  # Rotation around x-axis by angle theta
    state = Statevector(qc)
    probs = state.probabilities()
    P0.append(probs[0])
    P1.append(probs[1])

# Plot
plt.figure(figsize=(10, 5))
plt.plot(thetas/np.pi, P0, 'b-', linewidth=2, label='$P_0(t) = \\cos^2(\\Omega t/2)$')
plt.plot(thetas/np.pi, P1, 'r-', linewidth=2, label='$P_1(t) = \\sin^2(\\Omega t/2)$')
plt.xlabel('$\\Omega t / \\pi$', fontsize=14)
plt.ylabel('Probability', fontsize=14)
plt.title('Rabi Oscillations: Population Transfer Between $|0\\rangle$ and $|1\\rangle$', fontsize=14)
plt.legend(fontsize=12)
plt.grid(True, alpha=0.3)
plt.axhline(y=0.5, color='gray', linestyle='--', alpha=0.5)
plt.axvline(x=1, color='green', linestyle=':', alpha=0.7, label='π-pulse')
plt.axvline(x=0.5, color='orange', linestyle=':', alpha=0.7, label='π/2-pulse')
plt.show()
```

## Summary

1. **Time evolution** is governed by the Schrödinger equation: $|\psi(t)\rangle = e^{-iHt/\hbar}|\psi(0)\rangle$

2. **The Hamiltonian generates time evolution**, just as Pauli matrices generate spatial rotations.

3. **Unitarity** ($U^\dagger U = I$) guarantees probability conservation.

4. **Precession** ($H \propto \sigma_z$): States rotate around the z-axis. Poles are stationary; equator precesses.

5. **Rabi oscillations** ($H \propto \sigma_x$): Population oscillates between $|0\rangle$ and $|1\rangle$. π-pulse inverts; π/2-pulse creates superposition.

6. **Eigenstates** of the Hamiltonian are stationary — they evolve only by a global phase.




### iClicker E: Energy Eigenstates

A qubit has Hamiltonian $H = \frac{\hbar\omega}{2}\sigma_z$, so the energy eigenstates are $|0\rangle$ (energy $+\hbar\omega/2$) and $|1\rangle$ (energy $-\hbar\omega/2$).

**The state $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ is prepared at $t=0$. At time $t$, the state is:**

- (A) Still $|+\rangle$ (stationary)
- (B) $\frac{1}{\sqrt{2}}(e^{-i\omega t/2}|0\rangle + e^{+i\omega t/2}|1\rangle)$ ✓
- (C) $|0\rangle$
- (D) $|1\rangle$
- (E) A mixed state

**Answer:** (B). Energy eigenstates each pick up their own phase factor $e^{-iE_n t/\hbar}$. Since $|+\rangle$ is a superposition of two energy eigenstates with different energies, the relative phase changes with time. The state precesses around the z-axis.

---

### iClicker F: What Doesn't Change?

Under the Hamiltonian $H = \frac{\hbar\Omega}{2}\sigma_x$, which quantity is CONSTANT in time?

- (A) $\langle\sigma_z\rangle$ (z-component of spin)
- (B) $\langle\sigma_x\rangle$ (x-component of spin) ✓
- (C) $\langle\sigma_y\rangle$ (y-component of spin)
- (D) The state $|\psi\rangle$
- (E) None of these

**Answer:** (B). The Hamiltonian $H \propto \sigma_x$ generates rotations around the x-axis. Under such rotations, the x-component of any vector is unchanged — only the y and z components rotate into each other.

More formally: $[H, \sigma_x] = [\sigma_x, \sigma_x] = 0$, so $\langle\sigma_x\rangle$ is a conserved quantity.




---

## Homework 2.6
<!-- 
### Problem: Eigenvectors of $\sigma_x$

The Pauli matrix $\sigma_x = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$.

**(a)** Find the eigenvalues of $\sigma_x$. (You should get $\pm 1$.)

**(b)** Find the normalized eigenvectors of $\sigma_x$. Express them in terms of $|0\rangle$ and $|1\rangle$.

**(c)** Verify that your eigenvectors are the states $|+\rangle$ and $|-\rangle$ that we defined as the ±x cardinal states on the Bloch sphere.

**(d)** Now consider the rotation $R_x(\theta) = e^{-i\sigma_x\theta/2}$. Apply $R_x(\theta)$ to $|+\rangle$. Show that the result is just $|+\rangle$ multiplied by a phase factor $e^{-i\theta/2}$.

**(e)** Similarly, show that $R_x(\theta)|-\rangle = e^{+i\theta/2}|-\rangle$.

**(f)** Explain in one sentence why eigenstates of $\sigma_x$ don't change (except for a phase) under rotations generated by $\sigma_x$.

---

### Problem: Unitarity and Probability Conservation

A general $2\times 2$ unitary matrix can be written as:
$$U = \begin{pmatrix} a & b \\ c & d \end{pmatrix}$$
where $a, b, c, d$ are complex numbers.

**(a)** The condition for unitarity is $U^\dagger U = I$. Write out $U^\dagger$ explicitly.

**(b)** Compute the product $U^\dagger U$ and set it equal to $I$. This gives you four equations (one for each matrix element). Write them out.

**(c)** Show that one of these equations is $|a|^2 + |c|^2 = 1$. Interpret this: if the input state is $|0\rangle$, what does this equation say about the output probabilities?

**(d)** Show that another equation is $|b|^2 + |d|^2 = 1$. Interpret this for input state $|1\rangle$.

**(e)** The rotation matrix $R_x(\pi/2) = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & -i \\ -i & 1 \end{pmatrix}$. Verify explicitly that $R_x(\pi/2)^\dagger R_x(\pi/2) = I$. -->

---

### Problem: Time Evolution Practice

A qubit has Hamiltonian $H = \frac{\hbar\omega}{2}\sigma_z$.

**(a)** Write the time evolution operator $U(t) = e^{-iHt/\hbar}$ as an explicit $2\times 2$ matrix.

**(b)** If the initial state is $|0\rangle$, find $|\psi(t)\rangle$. Does the state change physically, or just by a global phase?

**(c)** If the initial state is $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$, find $|\psi(t)\rangle$. 

**(d)** For part (c), compute $P_0(t) = |\langle 0|\psi(t)\rangle|^2$ and $P_1(t) = |\langle 1|\psi(t)\rangle|^2$. Do the populations change with time?

**(e)** For part (c), compute $P_+(t) = |\langle +|\psi(t)\rangle|^2$. At what time $t$ does the state first become orthogonal to $|+\rangle$? What state is it at that moment?

---

### Problem: Rabi Oscillations

A qubit has Hamiltonian $H = \frac{\hbar\Omega}{2}\sigma_x$ and starts in state $|0\rangle$.

**(a)** Using $R_x(\theta) = \cos\frac{\theta}{2}I - i\sin\frac{\theta}{2}\sigma_x$, show that:
$$|\psi(t)\rangle = \cos\frac{\Omega t}{2}|0\rangle - i\sin\frac{\Omega t}{2}|1\rangle$$

**(b)** Compute $P_1(t) = |\langle 1|\psi(t)\rangle|^2$. Sketch this function from $t=0$ to $t = 4\pi/\Omega$.

**(c)** At what time does the qubit first have a 50% probability of being in $|1\rangle$? What is this pulse called?

**(d)** At what time does the qubit first reach $|1\rangle$ with certainty? What is this pulse called?

**(e)** If $\Omega = 2\pi \times 10$ MHz, calculate the duration of a π-pulse in nanoseconds.
<!-- 
---

### Problem: The Ramsey Sequence (Qiskit)

The **Ramsey sequence** is: π/2 pulse → free precession → π/2 pulse → measure.

In Qiskit, this corresponds to: $R_x(\pi/2) \to R_z(\phi) \to R_x(\pi/2)$, where $\phi$ represents the phase accumulated during free precession.

**(a)** Write a Qiskit function that implements the Ramsey sequence and returns $P_0$ (the probability of measuring $|0\rangle$) as a function of $\phi$:

```python
import numpy as np
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

def ramsey(phi):
    """
    Implement Ramsey sequence: Rx(π/2) - Rz(φ) - Rx(π/2)
    Return P_0 (probability of measuring |0⟩)
    """
    qc = QuantumCircuit(1)
    # Your code here
    
    state = Statevector(qc)
    return state.probabilities()[0]
```

**(b)** Plot $P_0$ vs $\phi$ for $\phi$ ranging from $0$ to $4\pi$. 

**(c)** From your plot, what is the period of the oscillation (in terms of $\phi$)?

**(d)** Analytically, the Ramsey signal is $P_0 = \sin^2(\phi/2)$. Add this theoretical curve to your plot and verify it matches.

**(e)** What happens if you change the second pulse to $R_x(-\pi/2)$ instead of $R_x(\pi/2)$? Plot this and explain the difference.

---

### Problem: Ramsey with Decoherence (Qiskit)

In real experiments, the qubit interacts with its environment, causing **dephasing**. We can simulate this by adding random phase noise.

**(a)** Modify your Ramsey function to include random phase noise:

```python
def ramsey_noisy(phi, noise_strength):
    """
    Ramsey sequence with dephasing noise.
    noise_strength controls the standard deviation of random phase kicks.
    """
    qc = QuantumCircuit(1)
    qc.rx(np.pi/2, 0)
    
    # Add intended phase plus random noise
    random_phase = np.random.normal(0, noise_strength)
    qc.rz(phi + random_phase, 0)
    
    qc.rx(np.pi/2, 0)
    state = Statevector(qc)
    return state.probabilities()[0]
```

**(b)** For a given $\phi$, the measured $P_0$ will fluctuate due to the noise. To see the average behavior, run many trials and average:

```python
def ramsey_averaged(phi, noise_strength, n_trials=100):
    return np.mean([ramsey_noisy(phi, noise_strength) for _ in range(n_trials)])
```

Plot $P_0$ vs $\phi$ for `noise_strength = 0`, `0.5`, and `1.5` on the same graph.

**(c)** What happens to the **contrast** (the difference between max and min of the fringes) as noise increases?

**(d)** In real experiments, we say the fringes "wash out" due to decoherence. Explain in your own words why random phase kicks cause this.

**(e)** In a real atomic clock, why does this washing out limit how long you can make the free precession time $T$?

--- -->

### Problem: The Hadamard Gate

The Hadamard gate is one of the most important single-qubit gates:

$$H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$$

**(a)** Verify that $H$ is unitary: compute $H^\dagger H$ and show it equals $I$.

**(b)** Verify that $H^2 = I$. What does this tell you geometrically about the rotation?

**(c)** The general rotation formula is:
$$R_{\hat{n}}(\theta) = \cos\frac{\theta}{2}I - i\sin\frac{\theta}{2}(\hat{n}\cdot\vec{\sigma})$$

For $H^2 = I$, we need $\theta = \pi$ (a 180° rotation). So $H$ should equal $-i(\hat{n}\cdot\vec{\sigma})$ for some axis $\hat{n}$, up to a global phase.

Show that:
$$H = \frac{1}{\sqrt{2}}(\sigma_x + \sigma_z)$$

**(d)** From part (c), what is the rotation axis $\hat{n}$? Express it as a unit vector.

**(e)** Describe in words: where is this axis on the Bloch sphere? (Hint: it's in the xz-plane, halfway between...)

**(f)** The Hadamard swaps the z-basis and x-basis:
- $H|0\rangle = |+\rangle$
- $H|1\rangle = |-\rangle$
- $H|+\rangle = |0\rangle$
- $H|-\rangle = |1\rangle$

Verify one of these by explicit matrix multiplication.

**(g)** Why do you think the Hadamard gate is preferred over, say, $R_y(\pi/2)$ for creating superpositions? (Hint: look at the matrix elements of each.)

<!-- ---

### Problem 8: Visualizing Trajectories (Qiskit)

Use Qiskit to visualize how states move on the Bloch sphere.

**(a)** Create a function that extracts Bloch sphere coordinates $(x, y, z)$ from a statevector:

```python
def bloch_coords(statevector):
    """Return (x, y, z) Bloch coordinates."""
    sv = statevector.data
    rho = np.outer(sv, sv.conj())  # density matrix
    
    sx = np.array([[0, 1], [1, 0]])
    sy = np.array([[0, -1j], [1j, 0]])
    sz = np.array([[1, 0], [0, -1]])
    
    x = np.real(np.trace(rho @ sx))
    y = np.real(np.trace(rho @ sy))
    z = np.real(np.trace(rho @ sz))
    return x, y, z
```

**(b)** Starting from $|0\rangle$, apply $R_x(\theta)$ for $\theta$ from $0$ to $2\pi$ in 20 steps. Record the Bloch coordinates at each step. What shape does the trajectory trace?

**(c)** Starting from $|0\rangle$, apply $R_z(\theta)$ for $\theta$ from $0$ to $2\pi$. What happens? (This should be boring — explain why.)

**(d)** Starting from $|+\rangle$, apply $R_z(\theta)$ for $\theta$ from $0$ to $2\pi$. Now what shape do you get?

**(e)** Create a 3D plot showing all three trajectories from parts (b), (c), and (d) on the same Bloch sphere. Use `matplotlib` with `projection='3d'`.
 -->


---

### Problem: Unitarity Implies Hermitian Generator

This problem proves the deep connection between unitary operators and Hermitian generators.

**(a)** Let $U = e^{iG}$ for some operator $G$. Write an expression for $U^\dagger$ in terms of $G^\dagger$.

**(b)** The condition for $U$ to be unitary is $U^\dagger U = I$. Substitute your expression from (a) and show that this requires:
$$e^{-iG^\dagger}e^{iG} = I$$

**(c)** For the equation in (b) to hold, we need $G^\dagger = G$. Explain why. 

*Hint:* If $A$ and $B$ are matrices, $e^A e^B = e^{A+B}$ only when $[A,B] = 0$. What does $e^{-iG^\dagger}e^{iG} = I = e^0$ tell you?

**(d)** Conclude: If $U = e^{iG}$ is unitary, then $G$ must be Hermitian.

**(e)** We've been writing time evolution as $U(t) = e^{-iHt/\hbar}$. Using your result, explain why the Hamiltonian $H$ must be Hermitian for time evolution to be unitary.

**(f)** The Pauli matrices $\sigma_x$, $\sigma_y$, $\sigma_z$ are Hermitian. Verify that $\sigma_x^\dagger = \sigma_x$ by writing out the matrix and its conjugate transpose.

**(g)** Using your result from (d), explain why $R_z(\theta) = e^{-i\sigma_z\theta/2}$ is guaranteed to be unitary without doing any explicit calculation.

---

### Problem: Space Translation Operator

The momentum operator in one dimension is $\hat{p} = -i\hbar\frac{d}{dx}$.

**(a)** Consider the translation operator $T(a) = e^{-i\hat{p}a/\hbar}$. Using $\hat{p} = -i\hbar\frac{d}{dx}$, show that:
$$T(a) = e^{-a\frac{d}{dx}}$$

**(b)** Taylor expand $e^{-a\frac{d}{dx}}$ acting on a function $\psi(x)$:
$$e^{-a\frac{d}{dx}}\psi(x) = \left(1 - a\frac{d}{dx} + \frac{a^2}{2!}\frac{d^2}{dx^2} - \cdots\right)\psi(x)$$

**(c)** Recognize this Taylor series as the expansion of $\psi(x-a)$ around $x$. Conclude:
$$T(a)\psi(x) = \psi(x - a)$$

(Note: The sign convention varies. With $T(a) = e^{+i\hat{p}a/\hbar}$, you get $\psi(x+a)$.)

**(d)** Verify that $T(a)$ is unitary using the result from the previous problem.

<!-- **(e)** The pattern is now complete:

| Physical quantity | Operator (generator) | Transformation |
|-------------------|---------------------|----------------|
| Energy | $H$ | Time translation: $\psi(t) \to \psi(t + \Delta t)$ |
| Momentum | $\hat{p}$ | Space translation: $\psi(x) \to \psi(x + \Delta x)$ |
| Angular momentum | $\sigma_z$ (for spin) | Rotation: $|\psi\rangle \to R_z(\theta)|\psi\rangle$ |

In your own words, explain the pattern: what is the relationship between a conserved quantity and the transformation it generates? -->