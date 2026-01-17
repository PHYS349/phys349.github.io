# Lecture 6: Polarization and the Qubit

## A Qubit You Can Hold in Your Hands

Last lecture, we saw that interference emerges from adding complex amplitudes. The key formula was:

$$
P = |c_1 + c_2|^2 = |c_1|^2 + |c_2|^2 + 2\text{Re}(c_1^* c_2)
$$

The cross term is where the physics happens.

Today we make this concrete. We'll study **polarization** — the simplest quantum system, and one you can experiment with using cheap plastic filters.

Polarization is not just an analogy for a qubit. It *is* a qubit. The mathematics is identical. And polarization makes measurement tangible: a polarizer is a measurement device you can rotate with your fingers.

---

## Classical Polarization: The Electric Field

Light is an electromagnetic wave. For light traveling along the $z$-axis, Maxwell's equations tell us the electric field oscillates in the transverse $x$-$y$ plane:

$$
\vec{E}(z,t) = \begin{pmatrix} E_x \\ E_y \end{pmatrix} e^{i(kz - \omega t)}
$$

The complex amplitudes $E_x$ and $E_y$ encode both the magnitude and phase of oscillation in each direction.

```{figure} ./02_02_lecture_files/transverse_wave.svg
:width: 400px
:name: transverse-wave

Light traveling along z has its electric field oscillating in the x-y plane.
```

### The Jones Vector

We can strip away the propagation factor $e^{i(kz - \omega t)}$ and focus on what makes one polarization different from another. What remains is the **Jones vector**:

$$
\vec{J} = \begin{pmatrix} E_x \\ E_y \end{pmatrix}
$$

This two-component complex vector completely characterizes the polarization state.

**Examples:**

| Polarization | Jones Vector |
|--------------|--------------|
| Horizontal | $\begin{pmatrix} 1 \\ 0 \end{pmatrix}$ |
| Vertical | $\begin{pmatrix} 0 \\ 1 \end{pmatrix}$ |
| Diagonal (+45°) | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix}$ |
| Right circular | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ i \end{pmatrix}$ |

The Jones vector is already a 2D complex vector — exactly the mathematical structure we need for a qubit. But so far, this is classical electromagnetism. The electric field is a smooth, continuous wave.

---

## The Quantum Leap: Single Photons

Now we make a conceptual leap that took physics decades to understand.

Remember Planck and Einstein's revolutionary insight from Lecture 1: light comes in discrete packets called **photons**, each carrying energy $E = \hbar\omega$. This was the resolution to the photoelectric effect — light below a threshold frequency can't eject electrons, no matter how intense, because each photon must have enough energy individually.

What does this mean for polarization?

When we turn down the intensity of a polarized laser beam, we don't get a weaker continuous wave. We get fewer photons, each with the same polarization state.

At the single-photon level, something remarkable happens:

```{admonition} The Fundamental Discreteness
:class: warning

**You don't get part of a photon.**

When a single photon encounters a polarizer:
- It either passes through entirely, OR
- It is absorbed entirely

There is no "50% of a photon" that passes through. The photon is indivisible.
```

This is where the classical wave picture breaks down. A classical wave would have 50% of its intensity transmitted through a polarizer at 45°. But a single photon must make a choice: all or nothing.

### From Wave to Particle... and Back?

Here's the deep mystery. Before measurement:
- The photon has a polarization state described by a Jones vector
- This state can be a superposition, like $\frac{1}{\sqrt{2}}(|H\rangle + |V\rangle)$
- The photon behaves like a wave, capable of interference

During measurement (hitting a polarizer):
- The photon "collapses" to a definite outcome
- It's either transmitted OR absorbed — particle-like behavior
- The outcome is probabilistic, governed by the Born rule

After measurement:
- If transmitted, the photon emerges with a definite polarization
- It can interfere again in subsequent experiments

This wave-particle duality is at the heart of quantum mechanics. We'll explore it more deeply later, but for now, accept that something strange is happening: the act of measurement changes the nature of reality from "superposition of possibilities" to "definite outcome."

---

## The Polarization Qubit

With this in mind, let's write the quantum state of a single photon's polarization.

The two basis states are:

- **Horizontal (H):** Electric field along $x$
- **Vertical (V):** Electric field along $y$

In Dirac notation:

$$
|H\rangle \equiv |0\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}, \qquad |V\rangle \equiv |1\rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}
$$

Any polarization state is a superposition:

$$
|\psi\rangle = \alpha |H\rangle + \beta |V\rangle = \begin{pmatrix} \alpha \\ \beta \end{pmatrix}
$$

where $\alpha$ and $\beta$ are complex numbers satisfying $|\alpha|^2 + |\beta|^2 = 1$.

This looks exactly like the Jones vector — and it is! The mathematics carries over directly. But the interpretation is profoundly different:

| Classical (Jones vector) | Quantum (qubit state) |
|--------------------------|----------------------|
| $E_x$, $E_y$ are field amplitudes | $\alpha$, $\beta$ are probability amplitudes |
| $\|E_x\|^2 + \|E_y\|^2$ = intensity | $\|\alpha\|^2 + \|\beta\|^2 = 1$ (normalization) |
| Continuous wave | Single indivisible photon |
| Measurement reads intensity | Measurement gives discrete outcome |

```{admonition} The Polarization Qubit
:class: important

A single photon's polarization is a qubit:

$$
|\psi\rangle = \alpha |H\rangle + \beta |V\rangle
$$

- $|\alpha|^2$ = probability of measuring H
- $|\beta|^2$ = probability of measuring V
- The relative phase between $\alpha$ and $\beta$ determines interference

The math is identical to spin-1/2, to a two-level atom, to a superconducting qubit.
```

---

## Relative Phase Creates Different Polarizations

Here's where it gets interesting. Consider states with equal amplitudes $|\alpha| = |\beta| = 1/\sqrt{2}$, but different relative phases.

### Diagonal and Anti-Diagonal

$$
|D\rangle = \frac{1}{\sqrt{2}}\left(|H\rangle + |V\rangle\right) \qquad \text{(diagonal, +45°)}
$$

$$
|A\rangle = \frac{1}{\sqrt{2}}\left(|H\rangle - |V\rangle\right) \qquad \text{(anti-diagonal, -45°)}
$$

The only difference is a minus sign — a relative phase of $\pi$. But these are physically different polarizations! $|D\rangle$ is linear polarization at +45°, while $|A\rangle$ is at -45°.

### Circular Polarization

$$
|R\rangle = \frac{1}{\sqrt{2}}\left(|H\rangle + i|V\rangle\right) \qquad \text{(right circular)}
$$

$$
|L\rangle = \frac{1}{\sqrt{2}}\left(|H\rangle - i|V\rangle\right) \qquad \text{(left circular)}
$$

Here the relative phase is $\pm \pi/2$. The electric field doesn't oscillate back and forth — it rotates in a circle.

```{figure} ./02_02_lecture_files/polarization_states.svg
:width: 500px
:name: polarization-states

Different polarization states: linear (H, V, D, A) and circular (R, L).
```

### The Pattern

| State | $\alpha$ | $\beta$ | Relative Phase | Physical Polarization |
|-------|----------|---------|----------------|----------------------|
| $\|H\rangle$ | 1 | 0 | — | Horizontal |
| $\|V\rangle$ | 0 | 1 | — | Vertical |
| $\|D\rangle$ | $1/\sqrt{2}$ | $1/\sqrt{2}$ | 0 | Diagonal (+45°) |
| $\|A\rangle$ | $1/\sqrt{2}$ | $-1/\sqrt{2}$ | $\pi$ | Anti-diagonal (-45°) |
| $\|R\rangle$ | $1/\sqrt{2}$ | $i/\sqrt{2}$ | $\pi/2$ | Right circular |
| $\|L\rangle$ | $1/\sqrt{2}$ | $-i/\sqrt{2}$ | $-\pi/2$ | Left circular |

Same magnitudes, different phases, completely different physical states.

This is the first concrete demonstration that **relative phase is real physics**.

---

## Measurement: The Polarizer

A polarizer is a filter that transmits light polarized along one axis and blocks the orthogonal polarization.

```{figure} ./02_02_lecture_files/polarizer.svg
:width: 350px
:name: polarizer

A polarizer transmits one polarization component and blocks the orthogonal one.
```

### Classical vs Quantum: The Critical Difference

**For a classical wave:** If a wave with intensity $I_0$ hits a polarizer at angle $\theta$ to the polarization direction, the transmitted intensity is:

$$
I = I_0 \cos^2\theta
$$

This is Malus's Law. At 45°, exactly half the intensity gets through. The wave is smoothly attenuated.

**For a single photon:** Something fundamentally different happens. The photon either:
- Passes through entirely (with probability $\cos^2\theta$), OR
- Is absorbed entirely (with probability $\sin^2\theta$)

There is no half-transmission. The photon is indivisible.

```{admonition} The Born Rule
:class: important

For a photon in state $|\psi\rangle = \alpha|H\rangle + \beta|V\rangle$ hitting a horizontal polarizer:

$$
P(\text{transmitted}) = |\langle H | \psi \rangle|^2 = |\alpha|^2
$$

$$
P(\text{absorbed}) = |\langle V | \psi \rangle|^2 = |\beta|^2
$$

The probabilities are set by the squared amplitudes — but each individual photon makes a definite choice.
```

### Measurement Changes the State

Here's the crucial point: measurement doesn't just reveal information, it *changes* the quantum state.

Before the polarizer: $|\psi\rangle = \alpha|H\rangle + \beta|V\rangle$ (superposition)

After the polarizer (if photon passes): $|\psi\rangle = |H\rangle$ (definite state)

The superposition is gone. The act of measurement has **collapsed** the state onto one of the measurement basis states.

This isn't just "we learned something we didn't know before." The photon wasn't secretly in $|H\rangle$ all along. Before measurement, it was genuinely in a superposition. The measurement forced it to "choose."

```{admonition} Measurement Postulate
:class: important

1. Measurement outcomes are the basis states of the measurement
2. Probabilities are given by $|\langle \text{outcome} | \psi \rangle|^2$
3. After measurement, the state becomes the measured outcome

This is one of the deepest mysteries of quantum mechanics. We'll return to it throughout the course.
```

---

## The Three-Polarizer Paradox

Here's an experiment that reveals something deep about quantum measurement.

### Setup 1: Crossed Polarizers

Place two polarizers at 90° to each other:
- First polarizer: Horizontal (H)
- Second polarizer: Vertical (V)

Send in unpolarized light. What comes out?

1. After the H polarizer: light is in state $|H\rangle$
2. $|H\rangle$ hits the V polarizer: $P(\text{pass}) = |\langle V|H\rangle|^2 = 0$

**Nothing gets through.** The polarizations are orthogonal.

```{figure} ./02_02_lecture_files/crossed_polarizers.svg
:width: 400px
:name: crossed-polarizers

Crossed polarizers: no light gets through.
```

### Setup 2: Insert a Third Polarizer

Now insert a diagonal polarizer (D, at 45°) *between* the H and V polarizers:

H → D → V

What happens?

1. After H polarizer: state is $|H\rangle$

2. $|H\rangle$ hits D polarizer. We need to rewrite $|H\rangle$ in the D/A basis:
   $$|H\rangle = \frac{1}{\sqrt{2}}(|D\rangle + |A\rangle)$$
   
   Probability to pass: $|\langle D|H\rangle|^2 = \frac{1}{2}$
   
   If it passes, state becomes $|D\rangle$

3. $|D\rangle$ hits V polarizer. Rewrite in H/V basis:
   $$|D\rangle = \frac{1}{\sqrt{2}}(|H\rangle + |V\rangle)$$
   
   Probability to pass: $|\langle V|D\rangle|^2 = \frac{1}{2}$

**Total probability:** $\frac{1}{2} \times \frac{1}{2} = \frac{1}{4}$

```{figure} ./02_02_lecture_files/three_polarizers.svg
:width: 450px
:name: three-polarizers

Three polarizers: inserting a diagonal polarizer allows light through!
```

```{admonition} The Paradox
:class: warning

By *adding* a polarizer, we went from 0% transmission to 25% transmission.

How can *more* filtering let *more* light through?
```

### The Resolution

The key insight: **each polarizer is a measurement, and measurement changes the state.**

- The H polarizer doesn't just filter — it *prepares* $|H\rangle$
- The D polarizer doesn't just filter — it *prepares* $|D\rangle$  
- $|D\rangle$ has a non-zero overlap with $|V\rangle$

The middle measurement changed the state from $|H\rangle$ (orthogonal to V) to $|D\rangle$ (not orthogonal to V).

This is not a trick of classical waves. It's a fundamental feature of quantum measurement.

---

## Orthogonal Bases

We've now seen three natural bases for polarization:

| Basis | States | Physical Meaning |
|-------|--------|------------------|
| H/V | $\{|H\rangle, |V\rangle\}$ | Horizontal/Vertical |
| D/A | $\{|D\rangle, |A\rangle\}$ | Diagonal/Anti-diagonal |
| R/L | $\{|R\rangle, |L\rangle\}$ | Right/Left circular |

Each pair is **orthonormal**:
- $\langle H|V\rangle = 0$, $\langle D|A\rangle = 0$, $\langle R|L\rangle = 0$
- $\langle H|H\rangle = 1$, etc.

And each pair is **complete** — any state can be written as a superposition of either pair.

### Changing Basis

We can express any basis in terms of any other:

$$
|D\rangle = \frac{1}{\sqrt{2}}(|H\rangle + |V\rangle), \qquad |A\rangle = \frac{1}{\sqrt{2}}(|H\rangle - |V\rangle)
$$

Inverting:

$$
|H\rangle = \frac{1}{\sqrt{2}}(|D\rangle + |A\rangle), \qquad |V\rangle = \frac{1}{\sqrt{2}}(|D\rangle - |A\rangle)
$$

This is just linear algebra — a change of basis in a 2D complex vector space.

### Choosing a Basis = Choosing a Question

Here's a profound point:

**Measuring in a particular basis means asking a particular question.**

- H/V basis: "Is the photon horizontally or vertically polarized?"
- D/A basis: "Is the photon diagonally or anti-diagonally polarized?"
- R/L basis: "Is the photon right or left circularly polarized?"

A photon in state $|D\rangle$:
- Definite answer to the D/A question: "D, with certainty"
- Random answer to the H/V question: "H or V, 50/50"
- Random answer to the R/L question: "R or L, 50/50"

You can't simultaneously know the answers to all questions. This is a hint of the uncertainty principle.

---

## The Poincaré Sphere (= Bloch Sphere)

We now have a lot of states to keep track of. There's a beautiful way to visualize them all: the **Poincaré sphere** (called the **Bloch sphere** in quantum computing).

Every pure polarization state corresponds to a point on the surface of a sphere:

```{figure} ./02_02_lecture_files/poincare_sphere.svg
:width: 400px
:name: poincare-sphere

The Poincaré sphere: every polarization state is a point on the sphere.
```

### The Mapping

A general qubit state can be written as:

$$
|\psi\rangle = \cos\frac{\theta}{2}|H\rangle + e^{i\phi}\sin\frac{\theta}{2}|V\rangle
$$

The angles $(\theta, \phi)$ are spherical coordinates:
- $\theta$: polar angle (0 at north pole, $\pi$ at south pole)
- $\phi$: azimuthal angle (around the equator)

### Landmarks on the Sphere

| Location | State | Coordinates |
|----------|-------|-------------|
| North pole | $\|H\rangle$ | $\theta = 0$ |
| South pole | $\|V\rangle$ | $\theta = \pi$ |
| +x axis | $\|D\rangle$ | $\theta = \pi/2$, $\phi = 0$ |
| -x axis | $\|A\rangle$ | $\theta = \pi/2$, $\phi = \pi$ |
| +y axis | $\|R\rangle$ | $\theta = \pi/2$, $\phi = \pi/2$ |
| -y axis | $\|L\rangle$ | $\theta = \pi/2$, $\phi = -\pi/2$ |

### What the Sphere Tells Us

1. **Orthogonal states are antipodal.** $|H\rangle$ and $|V\rangle$ are opposite poles. $|D\rangle$ and $|A\rangle$ are opposite points on the equator.

2. **The equator contains equal superpositions.** All points on the equator have $|α|² = |β|² = 1/2$.

3. **Longitude is relative phase.** Moving around the equator changes the phase $\phi$ while keeping amplitudes equal.

4. **Measurement axes correspond to sphere axes.** Measuring in H/V is "measuring along z." Measuring in D/A is "measuring along x."

---

## From Polarization to Qubits: The Translation Dictionary

Everything we've learned about polarization translates directly to any qubit:

| Polarization | General Qubit | Qubit Notation |
|--------------|---------------|----------------|
| $\|H\rangle$ | Ground state | $\|0\rangle$ |
| $\|V\rangle$ | Excited state | $\|1\rangle$ |
| $\|D\rangle$ | $(\|0\rangle + \|1\rangle)/\sqrt{2}$ | $\|+\rangle$ |
| $\|A\rangle$ | $(\|0\rangle - \|1\rangle)/\sqrt{2}$ | $\|-\rangle$ |
| H/V polarizer | Z-basis measurement | Computational basis |
| D/A polarizer | X-basis measurement | Hadamard basis |
| Waveplate | Single-qubit gate | Rotation on Bloch sphere |

The Poincaré sphere becomes the Bloch sphere. The physics is identical.

---

## Lab: Visualizing Qubit States

Let's see these states on the Bloch sphere using Qiskit.

### Setup

```{code-block} python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_bloch_multivector
```

### The Basis States

```{code-block} python
# |0⟩ state (= |H⟩ in polarization)
state_0 = Statevector.from_label('0')
plot_bloch_multivector(state_0)
```

```{code-block} python
# |1⟩ state (= |V⟩ in polarization)
state_1 = Statevector.from_label('1')
plot_bloch_multivector(state_1)
```

### Creating Superpositions

```{code-block} python
# |+⟩ state (= |D⟩ in polarization)
# Created by applying Hadamard to |0⟩
qc_plus = QuantumCircuit(1)
qc_plus.h(0)
state_plus = Statevector(qc_plus)
plot_bloch_multivector(state_plus)
```

```{code-block} python
# |−⟩ state (= |A⟩ in polarization)
# Created by applying Hadamard to |1⟩
qc_minus = QuantumCircuit(1)
qc_minus.x(0)  # First flip to |1⟩
qc_minus.h(0)  # Then apply H
state_minus = Statevector(qc_minus)
plot_bloch_multivector(state_minus)
```

### Circular States

```{code-block} python
# |+i⟩ state (= |R⟩ in polarization)
# Created by H then S gate
qc_R = QuantumCircuit(1)
qc_R.h(0)
qc_R.s(0)  # S gate adds phase of π/2
state_R = Statevector(qc_R)
plot_bloch_multivector(state_R)
```

---

## Summary

Today we learned:

1. **Classical polarization is a Jones vector** — a 2D complex vector $(E_x, E_y)$ describing the electric field

2. **Single photons are indivisible** — you don't get part of a photon; this was Planck and Einstein's revolutionary insight

3. **Polarization is a qubit** — the same math, but now $|\alpha|^2$ and $|\beta|^2$ are probabilities, not intensities

4. **Relative phase is physical** — same amplitudes, different phases → different polarization (linear, diagonal, circular)

5. **A polarizer is a measurement device** — it asks "which basis state?" and collapses the superposition

6. **The Born rule gives probabilities** — $P = |\langle \text{outcome}|\psi\rangle|^2$

7. **Measurement changes the state** — this explains the three-polarizer paradox

8. **The Poincaré/Bloch sphere visualizes all states** — orthogonal states are antipodal, phase is longitude

---

## Homework

### Problem 1: Polarization States

Write each of the following states in the H/V basis (i.e., find $\alpha$ and $\beta$):

**(a)** $|D\rangle$ (diagonal, +45°)

**(b)** $|A\rangle$ (anti-diagonal, -45°)

**(c)** $|R\rangle$ (right circular)

**(d)** Light polarized at 30° from horizontal

*Hint for (d):* Linear polarization at angle $\theta$ from horizontal is $\cos\theta|H\rangle + \sin\theta|V\rangle$.

---

### Problem 2: Measurement Probabilities

A photon is prepared in state $|D\rangle = \frac{1}{\sqrt{2}}(|H\rangle + |V\rangle)$.

**(a)** What is the probability it passes a horizontal polarizer?

**(b)** What is the probability it passes a vertical polarizer?

**(c)** What is the probability it passes a diagonal (+45°) polarizer?

**(d)** What is the probability it passes a polarizer at 30° from horizontal?

*Hint:* You'll need to compute $|\langle \psi_{30°}|D\rangle|^2$.

---

### Problem 3: Sequential Measurements

A photon starts in state $|H\rangle$ and passes through a series of polarizers.

**(a)** H polarizer, then V polarizer. What is the probability it makes it through both?

**(b)** H polarizer, then D polarizer, then V polarizer. What is the probability it makes it through all three?

**(c)** H polarizer, then polarizer at 30°, then polarizer at 60°, then V polarizer. What is the probability?

**(d)** Generalize: if you insert $n$ polarizers evenly spaced between H and V (at angles $90°/n$, $2 \times 90°/n$, ...), what is the transmission probability as a function of $n$? What happens as $n \to \infty$?

---

### Problem 4: Change of Basis

**(a)** Write $|H\rangle$ and $|V\rangle$ in the D/A basis.

**(b)** Write $|D\rangle$ and $|A\rangle$ in the R/L basis.

**(c)** Write $|R\rangle$ and $|L\rangle$ in the H/V basis. Verify that $\langle R|L\rangle = 0$.

---

### Problem 5: Bloch Sphere Coordinates

The Bloch sphere parametrization is:

$$
|\psi\rangle = \cos\frac{\theta}{2}|0\rangle + e^{i\phi}\sin\frac{\theta}{2}|1\rangle
$$

**(a)** What are $(\theta, \phi)$ for $|0\rangle$? For $|1\rangle$?

**(b)** What are $(\theta, \phi)$ for $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$?

**(c)** What are $(\theta, \phi)$ for $|{-i}\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$?

**(d)** For a general point $(\theta, \phi)$ on the Bloch sphere, what is the probability of measuring $|0\rangle$? What is the probability of measuring $|+\rangle$?

---

### Problem 6: Orthogonality on the Bloch Sphere

Two states are orthogonal if $\langle \psi_1 | \psi_2 \rangle = 0$.

**(a)** Show that $|0\rangle$ and $|1\rangle$ are orthogonal. Where are they on the Bloch sphere?

**(b)** Show that $|+\rangle$ and $|-\rangle$ are orthogonal. Where are they on the Bloch sphere?

**(c)** On the Bloch sphere, orthogonal states are always antipodal (opposite points). Prove this: if $|\psi_1\rangle$ has coordinates $(\theta, \phi)$, show that the orthogonal state $|\psi_2\rangle$ has coordinates $(\pi - \theta, \phi + \pi)$.

---

### Problem 7: Qiskit Exploration

Use Qiskit to explore the Bloch sphere.

**(a)** Create the state $\frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ using gates. Visualize it on the Bloch sphere. Where is it?

**(b)** The `rx(θ)` gate rotates around the x-axis. Starting from $|0\rangle$, apply `rx(π/2)` and visualize the result. What state is this?

**(c)** The `ry(θ)` gate rotates around the y-axis. Starting from $|0\rangle$, apply `ry(π/3)` and visualize. Compute the probabilities $P(0)$ and $P(1)$ from the Bloch sphere coordinates and verify with a measurement simulation.

**(d)** Starting from $|0\rangle$, what sequence of gates creates a state at $(\theta, \phi) = (\pi/2, \pi/4)$?

---

### Problem 8: The Three-Polarizer Experiment (Quantitative)

We derived that H → D → V gives 25% transmission.

**(a)** What if the middle polarizer is at angle $\theta$ instead of 45°? Derive the transmission probability as a function of $\theta$.

**(b)** What angle $\theta$ maximizes transmission? What is the maximum?

**(c)** Plot your result from (a) for $\theta$ from 0° to 90°.

**(d)** For what angles $\theta$ is the transmission zero? Explain physically.

---

### Problem 9: Wave vs Particle (Conceptual)

This problem explores the transition from classical waves to quantum photons.

**(a)** A classical polarized wave with intensity $I_0$ hits a polarizer at 45° to its polarization. What fraction of the intensity is transmitted? This is Malus's Law.

**(b)** Now consider single photons, sent one at a time, with the same polarization. If you send 1000 photons, approximately how many pass through? Is this number exact or approximate?

**(c)** Explain why the classical limit emerges: when you have many photons, the discrete quantum behavior "averages out" to give Malus's Law.

**(d)** Here's the key conceptual question: Before a photon hits the polarizer, is it "secretly" either going to pass or be absorbed, and we just don't know which? Or is it genuinely in a superposition, and the outcome is not determined until measurement? 

Write 2-3 sentences explaining what the quantum mechanical answer is, and why this is different from classical uncertainty (like not knowing which side a coin will land on before you flip it).

---

## Looking Ahead

Next lecture, we'll add dynamics. A qubit state can change in time according to the Schrödinger equation. We'll see that:
- Any Hamiltonian can be written using Pauli matrices
- Time evolution is rotation on the Bloch sphere
- Rabi oscillations are rotation around the x-axis

This will connect polarization wave plates to quantum gates.