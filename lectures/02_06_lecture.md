# Lecture 2.6: Time Evolution, Rabi Oscillations, and Ramsey Interferometry

{interesitng idea - we could simmulate decoherence as a random phase gate}. then we could explicitley see decoherence!!! i acutally love that in quiskit. }

## Review: Where We Are

In Lecture 2.4, we discovered that: - The Pauli matrices $\sigma_x$, $\sigma_y$, $\sigma_z$ are the generators of qubit rotations - Any rotation on the Bloch sphere: $R_{\hat{n}}(\theta) = e^{-i\theta(\hat{n}\cdot\vec{\sigma})/2}$ - The eigenstates of the Paulis are the six cardinal points on the Bloch sphere

Today we answer the question: **how do quantum states evolve in time?**

The answer will connect everything we've learned: - Time evolution = rotation on the Bloch sphere - The Ramsey sequence = the MZI from Lecture 2.2, realized with spin - Atomic clocks = qubits as sensors

------------------------------------------------------------------------

## Part 1: The Schrödinger Equation

### The Fundamental Law of Quantum Dynamics

Quantum states evolve according to the **Schrödinger equation**:

$$\boxed{i\hbar\frac{d}{dt}|\psi(t)\rangle = H|\psi(t)\rangle}$$

Here $H$ is the **Hamiltonian** — the operator representing the total energy of the system.

Key features: - **First-order in time** (unlike classical wave equation) - **Linear** (superpositions evolve independently) - **Deterministic** (given initial state, evolution is fixed)

### The Solution: Unitary Evolution

For a time-independent Hamiltonian, we can solve this directly.

Guess: $|\psi(t)\rangle = U(t)|\psi(0)\rangle$ for some operator $U(t)$.

Substituting: $$i\hbar\frac{d}{dt}U(t)|\psi(0)\rangle = HU(t)|\psi(0)\rangle$$

This must hold for any initial state, so: $$i\hbar\frac{dU}{dt} = HU$$

The solution is: $$\boxed{U(t) = e^{-iHt/\hbar}}$$

This is the **time evolution operator**.

### Why Unitary?

A unitary operator satisfies $U^\dagger U = I$. Let's verify:

$$U^\dagger = \left(e^{-iHt/\hbar}\right)^\dagger = e^{+iH^\dagger t/\hbar} = e^{+iHt/\hbar}$$

(using $H^\dagger = H$ since the Hamiltonian is Hermitian)

$$U^\dagger U = e^{+iHt/\hbar}e^{-iHt/\hbar} = e^0 = I \quad \checkmark$$

**Why does this matter?** Unitarity guarantees **probability conservation**:

$$\langle\psi(t)|\psi(t)\rangle = \langle\psi(0)|U^\dagger U|\psi(0)\rangle = \langle\psi(0)|\psi(0)\rangle = 1$$

The total probability stays 1 for all time. Probability can slosh around between outcomes, but it can't be created or destroyed.

\`\`\`{admonition} The Deep Connection :class: note

**Hermitian operators** (observables) ↔ **Unitary operators** (evolution)

If $H$ is Hermitian, then $e^{-iHt/\hbar}$ is unitary.

Observables generate evolution: - Energy generates time evolution - Momentum generates spatial translation - Angular momentum generates rotations

```         

### Energy Eigenstates Are Stationary

What if the initial state is an eigenstate of $H$?

Suppose $H|E\rangle = E|E\rangle$. Then:

$$|\psi(t)\rangle = e^{-iHt/\hbar}|E\rangle = e^{-iEt/\hbar}|E\rangle$$

The state just acquires a phase factor $e^{-iEt/\hbar}$.

But global phase doesn't matter! So energy eigenstates are **stationary** — they don't change physically with time.

### Superpositions DO Evolve

What if the initial state is a superposition of energy eigenstates?

$$|\psi(0)\rangle = c_1|E_1\rangle + c_2|E_2\rangle$$

Then:
$$|\psi(t)\rangle = c_1 e^{-iE_1t/\hbar}|E_1\rangle + c_2 e^{-iE_2t/\hbar}|E_2\rangle$$

The **relative phase** between components changes:

$$|\psi(t)\rangle = e^{-iE_1t/\hbar}\left(c_1|E_1\rangle + c_2 e^{-i(E_2-E_1)t/\hbar}|E_2\rangle\right)$$

The relative phase oscillates at frequency:
$$\omega = \frac{E_2 - E_1}{\hbar}$$

This is real, physical evolution — the state moves on the Bloch sphere!

---

## Part 2: Qubit Hamiltonians

### General Form

Any qubit Hamiltonian can be written as:

$$H = \frac{\hbar\omega}{2}\hat{n}\cdot\vec{\sigma} = \frac{\hbar\omega}{2}(n_x\sigma_x + n_y\sigma_y + n_z\sigma_z)$$

where $\hat{n} = (n_x, n_y, n_z)$ is a unit vector.

(We ignore the $h_0 I$ term since it only contributes a global phase.)

### Time Evolution Is Rotation

The time evolution operator is:

$$U(t) = e^{-iHt/\hbar} = e^{-i\omega t(\hat{n}\cdot\vec{\sigma})/2}$$

But we know from Lecture 2.4 that this is a rotation!

$$\boxed{U(t) = R_{\hat{n}}(\omega t) = \cos\frac{\omega t}{2}I - i\sin\frac{\omega t}{2}(\hat{n}\cdot\vec{\sigma})}$$

**Time evolution under Hamiltonian $H \propto \hat{n}\cdot\vec{\sigma}$ is rotation around axis $\hat{n}$ at angular frequency $\omega$.**

### Physical Example: Spin in a Magnetic Field

An electron spin in a magnetic field $\vec{B}$ has Hamiltonian:

$$H = -\gamma\vec{S}\cdot\vec{B} = -\frac{\gamma\hbar}{2}\vec{\sigma}\cdot\vec{B}$$

where $\gamma$ is the gyromagnetic ratio.

For a field along z, $\vec{B} = B_0\hat{z}$:

$$H = -\frac{\gamma\hbar B_0}{2}\sigma_z = \frac{\hbar\omega_0}{2}\sigma_z$$

where $\omega_0 = -\gamma B_0$ is the **Larmor frequency**.

The spin precesses around the magnetic field direction at the Larmor frequency!

---

## Part 3: Precession — $H \propto \sigma_z$

Let's work out the simplest case in detail.

### The Setup

$$H = \frac{\hbar\omega_0}{2}\sigma_z$$

This describes a qubit with energy splitting $\hbar\omega_0$ (like a spin in a magnetic field along z).

### The Evolution Operator

$$U(t) = e^{-i\omega_0 t\sigma_z/2} = R_z(\omega_0 t)$$

Using $\sigma_z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}$:

$$U(t) = \begin{pmatrix} e^{-i\omega_0 t/2} & 0 \\ 0 & e^{i\omega_0 t/2} \end{pmatrix}$$

### Evolution of $|0\rangle$ (Energy Eigenstate)

$$U(t)|0\rangle = \begin{pmatrix} e^{-i\omega_0 t/2} & 0 \\ 0 & e^{i\omega_0 t/2} \end{pmatrix}\begin{pmatrix} 1 \\ 0 \end{pmatrix} = e^{-i\omega_0 t/2}\begin{pmatrix} 1 \\ 0 \end{pmatrix} = e^{-i\omega_0 t/2}|0\rangle$$

Just a global phase — $|0\rangle$ is stationary. (It's an eigenstate of $H$.)

Similarly, $|1\rangle \to e^{+i\omega_0 t/2}|1\rangle$ — also stationary.

### Evolution of $|+\rangle$ (Superposition)

Now the interesting case:

$$|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$$

$$U(t)|+\rangle = \frac{1}{\sqrt{2}}(e^{-i\omega_0 t/2}|0\rangle + e^{i\omega_0 t/2}|1\rangle)$$

Factor out the global phase:

$$= \frac{e^{-i\omega_0 t/2}}{\sqrt{2}}(|0\rangle + e^{i\omega_0 t}|1\rangle)$$

Ignoring the global phase:

$$|+\rangle \to \frac{1}{\sqrt{2}}(|0\rangle + e^{i\omega_0 t}|1\rangle)$$

This is a state on the equator with azimuthal angle $\phi = \omega_0 t$!

**The state rotates around the z-axis at angular frequency $\omega_0$.**

### The Trajectory

| Time | Phase $\phi = \omega_0 t$ | State | Bloch position |
|------|---------------------------|-------|----------------|
| $0$ | $0$ | $\|+\rangle$ | +x |
| $\pi/(2\omega_0)$ | $\pi/2$ | $\|+i\rangle$ | +y |
| $\pi/\omega_0$ | $\pi$ | $\|-\rangle$ | −x |
| $3\pi/(2\omega_0)$ | $3\pi/2$ | $\|-i\rangle$ | −y |
| $2\pi/\omega_0$ | $2\pi$ | $\|+\rangle$ | +x (back to start) |

This is **precession** — the quantum analog of a gyroscope.

```{figure} precession_placeholder.svg
:name: precession
:width: 60%

Precession: Under $H \propto \sigma_z$, states on the equator rotate around the z-axis. States at the poles (eigenstates of $H$) are stationary.
```

------------------------------------------------------------------------

## iClicker Question 1

**Under the Hamiltonian** $H = \frac{\hbar\omega}{2}\sigma_z$, the state $|+\rangle$ evolves to $|-\rangle$ after time:

-   

    (A) $t = \pi/\omega$ ✓

-   

    (B) $t = 2\pi/\omega$

-   

    (C) $t = \pi/(2\omega)$

-   

    (D) Never

**Solution:** We need the phase to advance by $\pi$ (half a rotation around the equator):

$$\omega_0 t = \pi \implies t = \pi/\omega_0$$

At this time: $$|+\rangle \to \frac{1}{\sqrt{2}}(|0\rangle + e^{i\pi}|1\rangle) = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle) = |-\rangle$$

------------------------------------------------------------------------

## Part 4: Rabi Oscillations — $H \propto \sigma_x$

Now let's drive the qubit with a transverse field.

### The Setup

$$H = \frac{\hbar\Omega}{2}\sigma_x$$

This describes a qubit being driven by a resonant field (e.g., microwave pulses on a superconducting qubit, or laser light on an atom).

$\Omega$ is called the **Rabi frequency** — it depends on the strength of the driving field.

### The Evolution Operator

$$U(t) = e^{-i\Omega t\sigma_x/2} = R_x(\Omega t)$$

Using the rotation formula:

$$R_x(\theta) = \cos\frac{\theta}{2}I - i\sin\frac{\theta}{2}\sigma_x = \begin{pmatrix} \cos\frac{\theta}{2} & -i\sin\frac{\theta}{2} \\ -i\sin\frac{\theta}{2} & \cos\frac{\theta}{2} \end{pmatrix}$$

With $\theta = \Omega t$:

$$U(t) = \begin{pmatrix} \cos\frac{\Omega t}{2} & -i\sin\frac{\Omega t}{2} \\ -i\sin\frac{\Omega t}{2} & \cos\frac{\Omega t}{2} \end{pmatrix}$$

### Evolution of $|0\rangle$

$$|\psi(t)\rangle = U(t)|0\rangle = \begin{pmatrix} \cos\frac{\Omega t}{2} \\ -i\sin\frac{\Omega t}{2} \end{pmatrix} = \cos\frac{\Omega t}{2}|0\rangle - i\sin\frac{\Omega t}{2}|1\rangle$$

### The Probabilities Oscillate!

$$P_0(t) = \left|\cos\frac{\Omega t}{2}\right|^2 = \cos^2\frac{\Omega t}{2}$$

$$P_1(t) = \left|-i\sin\frac{\Omega t}{2}\right|^2 = \sin^2\frac{\Omega t}{2}$$

The population **oscillates** between $|0\rangle$ and $|1\rangle$!

This is dramatically different from precession, where $P_0$ and $P_1$ stayed constant.

\`\`\`{figure} rabi_oscillations_placeholder.svg :name: rabi-oscillations :width: 80%

Rabi oscillations: The probabilities $P_0(t)$ and $P_1(t)$ oscillate sinusoidally. The state rotates from the north pole through the equator to the south pole and back.

```         

### Special Pulses

**π-pulse** ($\Omega t = \pi$):

$$|\psi\rangle = \cos\frac{\pi}{2}|0\rangle - i\sin\frac{\pi}{2}|1\rangle = -i|1\rangle$$

Complete population inversion! The global phase $-i$ doesn't matter, so effectively:

$$|0\rangle \xrightarrow{\pi\text{-pulse}} |1\rangle$$

**π/2-pulse** ($\Omega t = \pi/2$):

$$|\psi\rangle = \cos\frac{\pi}{4}|0\rangle - i\sin\frac{\pi}{4}|1\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$$

This creates an equal superposition — the state is now on the equator!

$$|0\rangle \xrightarrow{\pi/2\text{-pulse}} \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$$

```{admonition} Pulse Summary
:class: important

| Pulse | Condition | Effect | Bloch sphere |
|-------|-----------|--------|--------------|
| π-pulse | $\Omega t = \pi$ | $\|0\rangle \to \|1\rangle$ | North → South pole |
| π/2-pulse | $\Omega t = \pi/2$ | $\|0\rangle \to$ superposition | North pole → Equator |
```

### Qiskit Simulation

``` python
import numpy as np
import matplotlib.pyplot as plt
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Simulate Rabi oscillations: Rx(θ) applied to |0⟩
thetas = np.linspace(0, 4*np.pi, 100)
P0 = []
P1 = []

for theta in thetas:
    qc = QuantumCircuit(1)
    qc.rx(theta, 0)  # Rotation around x-axis
    state = Statevector(qc)
    probs = state.probabilities()
    P0.append(probs[0])
    P1.append(probs[1])

# Plot
plt.figure(figsize=(10, 5))
plt.plot(thetas/np.pi, P0, 'b-', linewidth=2, label='$P_0(t)$')
plt.plot(thetas/np.pi, P1, 'r-', linewidth=2, label='$P_1(t)$')
plt.xlabel('$\\Omega t / \\pi$', fontsize=14)
plt.ylabel('Probability', fontsize=14)
plt.title('Rabi Oscillations', fontsize=16)
plt.legend(fontsize=12)
plt.grid(True, alpha=0.3)
plt.axhline(y=0.5, color='gray', linestyle='--', alpha=0.5)
plt.show()
```

------------------------------------------------------------------------

## iClicker Question 2

**A π/2-pulse (rotation by** $\pi/2$ around the x-axis) is applied to $|0\rangle$. The resulting state is located on the Bloch sphere at:

-   

    (A) North pole

-   

    (B) South pole

-   

    (C) On the equator ✓

-   

    (D) Depends on the rotation axis

**Solution:** A π/2-pulse rotates the state by 90°. Starting at the north pole and rotating around the x-axis by 90° lands us on the equator (specifically at the −y position due to the $-i$ phase).

------------------------------------------------------------------------

## Part 5: The Ramsey Sequence

Now we combine both types of evolution to create an **interferometer**.

### The Key Insight

-   **Rabi pulses** (π/2-pulses) create and recombine superpositions — like beam splitters
-   **Free precession** accumulates phase — like path length difference

This is exactly the Mach-Zehnder interferometer, but with spin!

### The Ramsey Sequence

1.  **Start in** $|0\rangle$ (ground state)

2.  **First π/2-pulse around x:** Creates superposition $$|0\rangle \xrightarrow{R_x(\pi/2)} \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$$

3.  **Free evolution for time** $T$: Precession around z accumulates phase $$\xrightarrow{R_z(\phi)} \frac{1}{\sqrt{2}}(e^{-i\phi/2}|0\rangle - ie^{i\phi/2}|1\rangle)$$ where $\phi = \omega_0 T$

4.  **Second π/2-pulse around x:** Converts phase to population $$\xrightarrow{R_x(\pi/2)} \text{final state}$$

5.  **Measure** in the z-basis

\`\`\`{figure} ramsey_sequence_placeholder.svg :name: ramsey-sequence :width: 90%

The Ramsey sequence: π/2 pulse — free evolution — π/2 pulse — measure. This is identical to the MZI circuit $H \to P(\phi) \to H$.

```         

### The Full Calculation

Let's work through this step by step.

**Step 1: Initial state**
$$|\psi_0\rangle = |0\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$$

**Step 2: First π/2-pulse** ($R_x(\pi/2)$)

$$R_x(\pi/2) = \begin{pmatrix} \cos(\pi/4) & -i\sin(\pi/4) \\ -i\sin(\pi/4) & \cos(\pi/4) \end{pmatrix} = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & -i \\ -i & 1 \end{pmatrix}$$

$$|\psi_1\rangle = R_x(\pi/2)|0\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ -i \end{pmatrix} = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$$

**Step 3: Free evolution for time T** ($R_z(\phi)$ where $\phi = \omega_0 T$)

$$R_z(\phi) = \begin{pmatrix} e^{-i\phi/2} & 0 \\ 0 & e^{i\phi/2} \end{pmatrix}$$

$$|\psi_2\rangle = R_z(\phi)|\psi_1\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} e^{-i\phi/2} \\ -ie^{i\phi/2} \end{pmatrix}$$

**Step 4: Second π/2-pulse** ($R_x(\pi/2)$)

$$|\psi_3\rangle = R_x(\pi/2)|\psi_2\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & -i \\ -i & 1 \end{pmatrix} \cdot \frac{1}{\sqrt{2}}\begin{pmatrix} e^{-i\phi/2} \\ -ie^{i\phi/2} \end{pmatrix}$$

$$= \frac{1}{2}\begin{pmatrix} e^{-i\phi/2} - i(-ie^{i\phi/2}) \\ -ie^{-i\phi/2} + (-ie^{i\phi/2}) \end{pmatrix} = \frac{1}{2}\begin{pmatrix} e^{-i\phi/2} - e^{i\phi/2} \\ -i(e^{-i\phi/2} + e^{i\phi/2}) \end{pmatrix}$$

Using $e^{i\theta} - e^{-i\theta} = 2i\sin\theta$ and $e^{i\theta} + e^{-i\theta} = 2\cos\theta$:

$$|\psi_3\rangle = \frac{1}{2}\begin{pmatrix} -2i\sin(\phi/2) \\ -2i\cos(\phi/2) \end{pmatrix} = -i\begin{pmatrix} \sin(\phi/2) \\ \cos(\phi/2) \end{pmatrix}$$

**Step 5: Measurement probabilities**

$$P_0 = |\langle 0|\psi_3\rangle|^2 = |-i\sin(\phi/2)|^2 = \sin^2(\phi/2)$$

$$P_1 = |\langle 1|\psi_3\rangle|^2 = |-i\cos(\phi/2)|^2 = \cos^2(\phi/2)$$

### The Ramsey Result

$$\boxed{P_0 = \sin^2\frac{\phi}{2} = \sin^2\frac{\omega_0 T}{2} = \frac{1 - \cos(\omega_0 T)}{2}}$$

The probability oscillates with the accumulated phase $\phi = \omega_0 T$!

### Ramsey Fringes

If we vary $T$ (or equivalently, scan the detuning $\delta = \omega_0 - \omega_{ref}$), the probability oscillates. These oscillations are called **Ramsey fringes**.

```python
import numpy as np
import matplotlib.pyplot as plt
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Ramsey sequence: Rx(π/2) - Rz(φ) - Rx(π/2)
phases = np.linspace(0, 4*np.pi, 100)
P0_ramsey = []

for phi in phases:
    qc = QuantumCircuit(1)
    qc.rx(np.pi/2, 0)   # First π/2 pulse
    qc.rz(phi, 0)        # Free evolution (phase accumulation)
    qc.rx(np.pi/2, 0)   # Second π/2 pulse
    
    state = Statevector(qc)
    probs = state.probabilities()
    P0_ramsey.append(probs[0])

# Compare to theory
P0_theory = np.sin(phases/2)**2

plt.figure(figsize=(10, 5))
plt.plot(phases/np.pi, P0_ramsey, 'b-', linewidth=2, label='Qiskit simulation')
plt.plot(phases/np.pi, P0_theory, 'r--', linewidth=2, label='Theory: $\\sin^2(\\phi/2)$')
plt.xlabel('Phase $\\phi/\\pi$ (proportional to $\\omega_0 T$)', fontsize=14)
plt.ylabel('$P_0$', fontsize=14)
plt.title('Ramsey Fringes', fontsize=16)
plt.legend(fontsize=12)
plt.grid(True, alpha=0.3)
plt.show()
```

### Connection to the MZI

The Ramsey sequence is **mathematically identical** to the Mach-Zehnder interferometer:

| MZI (Lecture 2.2)        | Ramsey (spin)            |
|--------------------------|--------------------------|
| Input light in arm 1     | Spin in $\|0\rangle$     |
| Beam splitter (Hadamard) | π/2-pulse ($R_x(\pi/2)$) |
| Path length → phase      | Free precession → phase  |
| Beam splitter (Hadamard) | π/2-pulse ($R_x(\pi/2)$) |
| Detector intensity       | Measurement probability  |

The physics is different, but the math is the same!

------------------------------------------------------------------------

## iClicker Question 3

**In a Ramsey sequence, what determines the period of the fringes (as a function of free evolution time T)?**

-   

    (A) The π/2-pulse duration

-   

    (B) The energy splitting $\hbar\omega_0$ ✓

-   

    (C) The Rabi frequency $\Omega$

-   

    (D) The measurement time

**Solution:** The phase accumulated during free evolution is $\phi = \omega_0 T$. The fringes complete one cycle when $\phi$ changes by $2\pi$, which happens when $T$ changes by $2\pi/\omega_0$. The fringe period is:

$$\Delta T = \frac{2\pi}{\omega_0}$$

Larger energy splitting → faster oscillation → shorter fringe period.

------------------------------------------------------------------------

## Part 6: Atomic Clocks

The Ramsey sequence isn't just a curiosity — it's the basis of the most precise instruments ever built.

### The Principle

Atoms have precise, universal energy levels. The transition frequency between two levels is a constant of nature.

**Cesium-133:** The ground state hyperfine transition: $$\nu_{Cs} = 9,192,631,770 \text{ Hz (exactly)}$$

This isn't a measurement — it's the **definition of the second** since 1967. One second is exactly 9,192,631,770 oscillations of cesium.

### How an Atomic Clock Works

1.  **Prepare atoms in** $|0\rangle$ (one hyperfine level)

2.  **Apply π/2-pulse** from a local oscillator at frequency $\omega_{LO}$

3.  **Free evolution for time T**

    -   If $\omega_{LO} = \omega_0$: no phase accumulates (on resonance)
    -   If $\omega_{LO} \neq \omega_0$: phase $\phi = (\omega_0 - \omega_{LO})T$ accumulates

4.  **Apply second π/2-pulse**

5.  **Measure population**

6.  **Feedback:** Adjust $\omega_{LO}$ to stay on resonance (minimize phase accumulation)

\`\`\`{figure} atomic_clock_placeholder.svg :name: atomic-clock :width: 80%

Atomic clock schematic: The Ramsey sequence provides an error signal that locks the local oscillator to the atomic frequency.

```         

The feedback loop keeps the local oscillator **exactly on resonance** with the atomic transition. The oscillator's ticks become "atomic ticks."

### Why Ramsey Beats Direct Measurement

Why use this complicated sequence instead of just measuring the atomic frequency directly?

**Direct probing:** If you probe the transition for time $t_{probe}$, the spectral resolution is:
$$\Delta\omega \sim \frac{1}{t_{probe}}$$

This is the time-energy uncertainty relation.

**Ramsey's insight:** Separate the "probing" (π/2-pulses) from the "measuring" (free evolution).

- The π/2-pulses can be **short** (microseconds)
- The free evolution can be **long** (seconds)
- The resolution is set by the **free evolution time**:
$$\Delta\omega \sim \frac{1}{T}$$

You get the resolution of a long measurement with the control of short pulses!

```{admonition} Norman Ramsey's Nobel Prize
:class: note

Norman Ramsey received the 1989 Nobel Prize in Physics for developing this technique. His "separated oscillatory fields" method is the basis of all modern precision spectroscopy and atomic clocks.
```

### Performance

**Cesium fountain clocks:** - Fractional accuracy: $\sim 10^{-16}$ - Would gain or lose 1 second in: 300 million years

**Optical atomic clocks** (using visible light transitions): - Fractional accuracy: $\sim 10^{-18}$ - Would gain or lose 1 second in: longer than the age of the universe

### GPS: Atomic Clocks in Your Pocket

The Global Positioning System relies on atomic clocks.

**How it works:** 1. Each GPS satellite carries atomic clocks and broadcasts timing signals 2. Your phone receives signals from multiple satellites 3. The time delay tells you the distance: $d = c \cdot \Delta t$ 4. From multiple distances, you triangulate your position

**The numbers:** - Light travels \~30 cm in 1 nanosecond - 1 meter position accuracy requires \~3 ns timing accuracy - Over one day, a clock must not drift more than \~1 μs - This requires fractional accuracy of $\sim 10^{-14}$

Without atomic clocks, GPS would accumulate errors of **kilometers per day**.

\`\`\`{admonition} Relativity in GPS :class: note

GPS satellites orbit at high altitude and high speed. Both general relativity (gravity) and special relativity (velocity) affect the clock rates: - Gravitational time dilation: clocks run **faster** by \~45 μs/day (weaker gravity) - Velocity time dilation: clocks run **slower** by \~7 μs/day - Net effect: clocks run \~38 μs/day fast

This is corrected for in the GPS system. GPS is a real-world test of Einstein's theories every day! \`\`\`

------------------------------------------------------------------------

## Part 7: Quantum Sensing

Atomic clocks are the most famous application, but the principle is much broader.

### The General Framework

A qubit is sensitive to **anything that shifts its energy levels**.

Any perturbation that causes a Hamiltonian $H = \frac{\hbar\omega}{2}\sigma_z$ leads to phase accumulation: $$\phi = \omega T$$

If $\omega$ depends on some physical quantity $X$ (magnetic field, electric field, gravity, etc.), then measuring $\phi$ measures $X$:

$$X \to \omega(X) \to \phi = \omega T \to P_0 = \sin^2(\phi/2)$$

### Example: Magnetometry

An electron spin in a magnetic field $B$ has: $$\omega = \gamma_e B$$

where $\gamma_e = 2\pi \times 28$ GHz/T is the electron gyromagnetic ratio.

A Ramsey sequence with free evolution time $T$ gives phase $\phi = \gamma_e B T$.

**Sensitivity:** The minimum detectable field change: $$\delta B = \frac{\delta\phi}{\gamma_e T}$$

Longer $T$ → better sensitivity (until decoherence sets in).

### Example: Gravimetry

Atom interferometers use matter waves to sense gravity: $$\phi = \frac{m g \Delta h \cdot T}{\hbar}$$

These devices can measure: - Gravitational acceleration $g$ to 9 decimal places - Gravitational gradients (useful for underground mapping) - Tests of the equivalence principle

### The Quantum Advantage

Classical sensors measure a field by its direct effect (deflection, voltage, etc.).

Quantum sensors measure through **interference** — they're sensitive to phase, which accumulates over time.

The precision scales as: - **Classical:** $\delta X \propto 1/\sqrt{T}$ (averaging helps as square root) - **Quantum (Ramsey):** $\delta X \propto 1/T$ (phase accumulates linearly)

This is the **Standard Quantum Limit**. With entanglement, you can do even better (Heisenberg Limit), but that's for a later chapter.

------------------------------------------------------------------------

## Part 8: Decoherence — The Enemy

There's a catch. We've assumed the qubit evolves purely under our desired Hamiltonian. In reality, it also interacts with its environment.

### What Goes Wrong

Random interactions with the environment cause: 1. **Dephasing:** Random phase kicks scramble the accumulated phase 2. **Relaxation:** The qubit decays from $|1\rangle$ to $|0\rangle$

Both effects destroy the interference pattern.

### Coherence Times

The characteristic timescales are: - $T_1$ (relaxation time): How long until the qubit decays - $T_2$ (dephasing time): How long until phase coherence is lost

For quantum sensing or computing, you need: $$T_{operation} \ll T_2$$

| System                   | Typical $T_2$      |
|--------------------------|--------------------|
| Trapped ions             | seconds to minutes |
| Neutral atoms            | \~1 second         |
| Superconducting qubits   | \~100 μs           |
| NV centers in diamond    | \~ms               |
| Electron spin in silicon | \~μs to ms         |

### Effect on Ramsey Fringes

As the free evolution time $T$ increases, the fringe contrast decays:

$$P_0 = \frac{1}{2} + \frac{e^{-T/T_2}}{2}\sin(\omega_0 T)$$

There's a trade-off: - Longer $T$ → better frequency resolution - Longer $T$ → worse contrast (decoherence)

The optimal $T$ depends on the specific application and system.

------------------------------------------------------------------------

## Summary: The Complete Single-Qubit Picture

We've now built a complete understanding of single qubits:

### 1. States

Points on the Bloch sphere: $$|\psi\rangle = \cos\frac{\theta}{2}|0\rangle + e^{i\phi}\sin\frac{\theta}{2}|1\rangle$$

### 2. Observables

Pauli matrices — the three axes: - $\sigma_z$: eigenstates $|0\rangle$, $|1\rangle$ (poles) - $\sigma_x$: eigenstates $|+\rangle$, $|-\rangle$ (x-axis) - $\sigma_y$: eigenstates $|+i\rangle$, $|-i\rangle$ (y-axis)

### 3. Evolution

Rotations generated by Hamiltonians: $$|\psi(t)\rangle = e^{-iHt/\hbar}|\psi(0)\rangle$$

-   $H \propto \sigma_z$: precession (rotation around z)
-   $H \propto \sigma_x$: Rabi oscillations (rotation around x)

### 4. Measurement

Projection onto an axis: $$P = |\langle\chi|\psi\rangle|^2$$

State collapses to the measurement outcome.

### 5. Interference

The universal pattern: **Split → Phase → Recombine → Measure**

| System        | Split         | Phase           | Recombine     | Measure       |
|---------------|---------------|---------------|---------------|---------------|
| MZI (light)   | Beam splitter | Path length     | Beam splitter | Detector      |
| Polarization  | Waveplate     | Birefringence   | Waveplate     | Polarizer     |
| Ramsey (spin) | π/2-pulse     | Free precession | π/2-pulse     | Stern-Gerlach |

### 6. Physical Realizations

| Qubit               | $\|0\rangle$ | $\|1\rangle$ |
|---------------------|--------------|--------------|
| Photon polarization | H            | V            |
| Electron spin       | ↑            | ↓            |
| Atom hyperfine      | ground       | excited      |
| Superconducting     | ground       | excited      |

------------------------------------------------------------------------

## Looking Ahead: Chapter 3

We now understand single qubits completely.

But one qubit can only do so much. The real power of quantum mechanics — and quantum computing — emerges when we have **multiple qubits**.

**Coming in Chapter 3:** - Two-qubit states: $|00\rangle$, $|01\rangle$, $|10\rangle$, $|11\rangle$ — and superpositions! - **Entanglement:** Correlations that have no classical explanation - The Bell states: maximally entangled qubits - Quantum gates: CNOT and controlled operations - Why quantum computers can outperform classical ones

------------------------------------------------------------------------

## Homework

### Problem 1: Time Evolution Basics

A qubit has Hamiltonian $H = \frac{\hbar\omega}{2}\sigma_z$.

**(a)** Write the time evolution operator $U(t)$ as a $2 \times 2$ matrix.

**(b)** Starting in $|0\rangle$, find $|\psi(t)\rangle$. Does the state change physically?

**(c)** Starting in $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$, find $|\psi(t)\rangle$.

**(d)** For part (c), compute $P_0(t)$ and $P_1(t)$. Do the populations change?

**(e)** For part (c), compute $P_+(t) = |\langle +|\psi(t)\rangle|^2$. Does this change? Interpret physically.

------------------------------------------------------------------------

### Problem 2: Rabi Oscillations

A qubit has Hamiltonian $H = \frac{\hbar\Omega}{2}\sigma_x$, starting in $|0\rangle$.

**(a)** Show that $|\psi(t)\rangle = \cos\frac{\Omega t}{2}|0\rangle - i\sin\frac{\Omega t}{2}|1\rangle$.

**(b)** Compute $P_1(t)$. At what time does the qubit first reach $|1\rangle$ with certainty?

**(c)** At what time does the qubit first reach an equal superposition?

**(d)** If $\Omega = 2\pi \times 10$ MHz, how long is a π-pulse in nanoseconds?

**(e)** Sketch $P_0(t)$ and $P_1(t)$ from $t = 0$ to $t = 4\pi/\Omega$.

------------------------------------------------------------------------

### Problem 3: Bloch Sphere Trajectories

**(a)** Under $H = \frac{\hbar\omega}{2}\sigma_z$, describe the trajectory of a state that starts on the equator. What shape does it trace?

**(b)** Under $H = \frac{\hbar\Omega}{2}\sigma_x$, describe the trajectory of a state that starts at the north pole. What shape does it trace?

**(c)** Under $H = \frac{\hbar\Omega}{2}\sigma_y$, starting in $|0\rangle$, find $|\psi(t)\rangle$. Where is this state on the Bloch sphere at $t = \pi/(2\Omega)$?

**(d)** A Hamiltonian $H = \frac{\hbar\omega}{2}(\sigma_x + \sigma_z)/\sqrt{2}$ corresponds to rotation around which axis? (No calculation needed — just identify the axis.)

------------------------------------------------------------------------

### Problem 4: Ramsey Sequence

**(a)** Verify the Ramsey calculation: Starting from $|0\rangle$, apply $R_x(\pi/2)$, then $R_z(\phi)$, then $R_x(\pi/2)$. Show that $P_0 = \sin^2(\phi/2)$.

**(b)** For $\phi = 0$, what is $P_0$? Where does the state end up on the Bloch sphere?

**(c)** For $\phi = \pi$, what is $P_0$? Where does the state end up?

**(d)** For $\phi = \pi/2$, what is $P_0$? Write out the final state explicitly.

**(e)** If the free evolution time is $T = 1$ ms and you observe 10 complete fringe oscillations, what is the frequency $\omega_0$?

------------------------------------------------------------------------

### Problem 5: Atomic Clock Precision

A cesium atomic clock uses the hyperfine transition at $\nu_0 = 9.192631770$ GHz.

**(a)** If the Ramsey free evolution time is $T = 1$ s, what is the fringe period $\Delta\nu$ (the frequency change needed for one complete fringe)?

**(b)** What fractional frequency precision $\Delta\nu/\nu_0$ does this correspond to?

**(c)** Modern cesium fountain clocks achieve $T \approx 0.5$ s. What is their frequency resolution?

**(d)** An optical clock uses a transition at $\nu_0 = 429$ THz (strontium). If it achieves the same absolute frequency resolution as the cesium clock in part (c), what fractional precision does this give?

------------------------------------------------------------------------

### Problem 6: Magnetometry

An electron spin magnetometer has gyromagnetic ratio $\gamma_e = 2\pi \times 28$ GHz/T.

**(a)** In Earth's magnetic field ($B \approx 50$ μT), what is the Larmor frequency?

**(b)** If the Ramsey free evolution time is $T = 100$ μs, how much phase accumulates?

**(c)** How many complete Ramsey fringes would you see if you scanned from $B = 0$ to $B = 50$ μT?

**(d)** If you can resolve a phase change of $\delta\phi = 0.01$ rad, what is the minimum detectable field change $\delta B$?

------------------------------------------------------------------------

### Problem 7: Decoherence

A qubit has dephasing time $T_2 = 100$ μs.

**(a)** In a Ramsey experiment with $\omega_0 = 2\pi \times 1$ MHz, the signal is: $$P_0(T) = \frac{1}{2} - \frac{1}{2}e^{-T/T_2}\cos(\omega_0 T)$$

Plot this from $T = 0$ to $T = 500$ μs (you can use Python or sketch by hand).

**(b)** At what time $T$ has the fringe contrast dropped to $1/e$ of its initial value?

**(c)** For sensing, you want to maximize the "signal" which goes like (contrast) × (phase sensitivity). Since contrast $\propto e^{-T/T_2}$ and phase sensitivity $\propto T$, the signal goes as $T \cdot e^{-T/T_2}$. Find the optimal $T$ that maximizes this.

**(d)** What fraction of the original contrast remains at this optimal time?

------------------------------------------------------------------------

### Problem 8: Rotation Matrices

**(a)** Verify that $R_x(\pi) = -i\sigma_x$ by computing $e^{-i\pi\sigma_x/2}$ using the formula $e^{-i\theta\sigma_x/2} = \cos(\theta/2)I - i\sin(\theta/2)\sigma_x$.

**(b)** Similarly, show that $R_z(\pi) = -i\sigma_z$.

**(c)** The Hadamard gate can be written as $H = \frac{1}{\sqrt{2}}(\sigma_x + \sigma_z)$. Verify that $H^2 = I$.

**(d)** Show that applying $H$ to $|0\rangle$ gives $|+\rangle$, and applying $H$ to $|+\rangle$ gives $|0\rangle$.

------------------------------------------------------------------------

### Problem 9: Comparing Precession and Rabi

Consider two Hamiltonians: $H_z = \frac{\hbar\omega}{2}\sigma_z$ and $H_x = \frac{\hbar\omega}{2}\sigma_x$ (same frequency $\omega$).

**(a)** For each Hamiltonian, list which states are stationary (energy eigenstates).

**(b)** Starting in $|0\rangle$, which Hamiltonian causes the populations $P_0$ and $P_1$ to change? Explain.

**(c)** Starting in $|+\rangle$, which Hamiltonian causes the populations $P_0$ and $P_1$ to change?

**(d)** Explain in words why precession doesn't change populations but Rabi oscillations do.

------------------------------------------------------------------------

### Problem 10: Qiskit Exploration

**(a)** Use Qiskit to simulate Rabi oscillations. Plot $P_0$ and $P_1$ vs rotation angle $\theta$ for $R_x(\theta)|0\rangle$ with $\theta$ from $0$ to $4\pi$. Verify the oscillation period is $2\pi$.

**(b)** Simulate a Ramsey sequence: $R_x(\pi/2) \to R_z(\phi) \to R_x(\pi/2)$ for $\phi$ from $0$ to $4\pi$. Plot $P_0$ vs $\phi$ and verify it matches $\sin^2(\phi/2)$.

**(c)** Modify the Ramsey sequence to use $R_y(\pi/2)$ pulses instead of $R_x(\pi/2)$. How does the result change?

**(d)** Simulate the effect of a "pulse error": use $R_x(\pi/2 + \epsilon)$ instead of $R_x(\pi/2)$ with $\epsilon = 0.1$. How does this affect the Ramsey fringes?