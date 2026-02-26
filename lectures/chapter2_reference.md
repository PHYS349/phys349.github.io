# PHYS 349 — Chapter 2 Reference: The Qubit

## Course Context

This document summarizes the key equations, concepts, and notation from **Lectures 2.1–2.7** of an undergraduate quantum computing course (PHYS 349). The chapter answers the question: **what is a qubit, physically and mathematically?** It builds from classical wave interference through complex numbers, interferometers, polarization, group theory, Pauli matrices, time evolution, and decoherence. Homework problems involve both analytical derivations and Python/Qiskit simulations.

---

## Lecture 2.1 — Waves, Complex Numbers, and Interference

### The Wave Equation

Spatial wave equation and its two linearly independent solution sets:

$$\frac{d^2 E}{dx^2} = -k^2 E \qquad \Longrightarrow \qquad E(x) = A\cos(kx) + B\sin(kx) \quad \text{or} \quad E(x) = Ce^{ikx} + De^{-ikx}$$

Temporal wave equation: $\ddot{E} = -\omega^2 E$, with solutions $e^{\pm i\omega t}$.

Full traveling wave equation and its solution:

$$\frac{\partial^2 E}{\partial x^2} = \frac{1}{c^2}\frac{\partial^2 E}{\partial t^2} \qquad \Longrightarrow \qquad E(x,t) = E_0 e^{i(kx - \omega t)}$$

### Key Wave Relations

$$k = \frac{2\pi}{\lambda}, \qquad \omega = 2\pi f = \frac{2\pi}{T}, \qquad c = f\lambda = \frac{\omega}{k}$$

### Euler's Formula

$$e^{i\theta} = \cos\theta + i\sin\theta, \qquad \cos\theta = \frac{e^{i\theta}+e^{-i\theta}}{2}, \qquad \sin\theta = \frac{e^{i\theta}-e^{-i\theta}}{2i}$$

### Intensity

Detectors measure intensity $\propto |E|^2$. For the complex representation, $|E_0 e^{i(kx-\omega t)}|^2 = |E_0|^2$ — no time averaging required, because $|e^{i\theta}|^2 = 1$.

### Interference

When two waves combine, we add amplitudes first, then square:

$$|E_1 + E_2|^2 = |E_1|^2 + |E_2|^2 + \underbrace{2\,\text{Re}(E_1^* E_2)}_{\text{interference (cross) term}}$$

For two equal-amplitude sources with path difference $\Delta x$:

$$I = 4\cos^2\!\left(\frac{k\Delta x}{2}\right)$$

- **Constructive:** $\Delta x = n\lambda$ → $I = 4$ (max)
- **Destructive:** $\Delta x = (n+\tfrac{1}{2})\lambda$ → $I = 0$ (min)

### Global Phase Is Unobservable

Multiplying all amplitudes by the same phase $e^{i\gamma}$ does not change any measurement outcome. Only **relative** phases matter.

### Central Message

**Interference comes from adding amplitudes before squaring.** The cross term can be positive (constructive) or negative (destructive). This is the engine of quantum computing: manipulate amplitudes so wrong answers cancel and right answers reinforce.

---

## Lecture 2.2 — Interferometers and the Bloch Sphere

### Michelson Interferometer

Round-trip path gives phases $\phi_j = 2kL_j = 4\pi L_j / \lambda$. Detected intensity:

$$I_{\text{det}} = \frac{I_0}{2}(1 + \cos\Delta\phi), \qquad \Delta\phi = \frac{4\pi}{\lambda}(L_1 - L_2)$$

Historical context: Michelson-Morley null result (1887) → special relativity. Modern: LIGO detects $\Delta L \sim 10^{-18}$ m using 4 km arms with Fabry-Pérot cavities.

### Mach-Zehnder Interferometer (MZI)

Two beam splitters (BS1, BS2), two arms, two output detectors. The beam splitter requires a $\pi$ phase shift on one reflection (energy conservation). Output intensities:

$$I_{D1} = \frac{1}{2}(1 + \cos\Delta\phi), \qquad I_{D2} = \frac{1}{2}(1 - \cos\Delta\phi)$$

Energy conservation: $I_{D1} + I_{D2} = 1$. Outputs are complementary.

### MZI as a Quantum Circuit

The MZI maps directly onto a gate sequence acting on the state vector $|\psi\rangle = \binom{c_1}{c_2}$:

$$|\psi_{\text{out}}\rangle = H \cdot P(\phi) \cdot H \cdot |0\rangle$$

| Interferometer Component | Quantum Gate |
|---|---|
| Beam splitter (split) | Hadamard $H = \frac{1}{\sqrt{2}}\begin{pmatrix}1&1\\1&-1\end{pmatrix}$ |
| Path length difference | Phase gate $P(\phi) = \begin{pmatrix}1&0\\0&e^{i\phi}\end{pmatrix}$ |
| Beam splitter (recombine) | Hadamard $H$ |
| Detector | Measurement in $\{|0\rangle, |1\rangle\}$ basis |

Detection probabilities from the circuit:

$$P(D1) = \frac{1}{2}(1+\cos\phi), \qquad P(D2) = \frac{1}{2}(1-\cos\phi)$$

### Measurement

Measurement in the $\{|0\rangle,|1\rangle\}$ basis: probability of outcome $|k\rangle$ is $P(k) = |\langle k|\psi\rangle|^2$. Measurement collapses the state onto one basis vector.

### The Bloch Sphere

A qubit has 2 real degrees of freedom (after normalization and removing global phase):

$$|\psi\rangle = \cos\frac{\theta}{2}|0\rangle + e^{i\phi}\sin\frac{\theta}{2}|1\rangle$$

- $\theta \in [0,\pi]$: polar angle from north pole
- $\phi \in [0,2\pi)$: azimuthal angle

### Six Cardinal States

| State | $\theta$ | $\phi$ | Bloch Position |
|---|---|---|---|
| $\|0\rangle = \binom{1}{0}$ | $0$ | — | North pole (+z) |
| $\|1\rangle = \binom{0}{1}$ | $\pi$ | — | South pole (−z) |
| $\|+\rangle = \frac{1}{\sqrt{2}}\binom{1}{1}$ | $\pi/2$ | $0$ | +x |
| $\|-\rangle = \frac{1}{\sqrt{2}}\binom{1}{-1}$ | $\pi/2$ | $\pi$ | −x |
| $\|{+i}\rangle = \frac{1}{\sqrt{2}}\binom{1}{i}$ | $\pi/2$ | $\pi/2$ | +y |
| $\|{-i}\rangle = \frac{1}{\sqrt{2}}\binom{1}{-i}$ | $\pi/2$ | $3\pi/2$ | −y |

**Orthogonal states are antipodal on the Bloch sphere** (this is why $\theta/2$ appears in the parametrization).

### Gates as Rotations

- $P(\phi)$: rotation around the z-axis by angle $\phi$
- $H$: rotation by $\pi$ around the axis $(\hat{x}+\hat{z})/\sqrt{2}$; swaps z-basis ↔ x-basis

---

## Lecture 2.3 — Polarization and Measurement

### Polarization as a Qubit

For light propagating along z, the polarization is encoded in the Jones vector:

$$\vec{J} = \begin{pmatrix} E_x \\ E_y e^{i\phi} \end{pmatrix}$$

This is a 2D complex vector — identical to a qubit state.

### Polarization–Qubit Dictionary

| Polarization | Jones Vector | Qubit | Bloch |
|---|---|---|---|
| Horizontal $\|H\rangle$ | $(1,0)^T$ | $\|0\rangle$ | +z |
| Vertical $\|V\rangle$ | $(0,1)^T$ | $\|1\rangle$ | −z |
| Diagonal $\|D\rangle$ | $(1,1)^T/\sqrt{2}$ | $\|+\rangle$ | +x |
| Anti-diagonal $\|A\rangle$ | $(1,-1)^T/\sqrt{2}$ | $\|-\rangle$ | −x |
| Right circular $\|R\rangle$ | $(1,i)^T/\sqrt{2}$ | $\|{+i}\rangle$ | +y |
| Left circular $\|L\rangle$ | $(1,-i)^T/\sqrt{2}$ | $\|{-i}\rangle$ | −y |

### Malus's Law (Dirac Notation)

Fraction of power transmitted through polarizer $|\chi\rangle$:

$$T = |\langle\chi|\psi\rangle|^2$$

### Projective Measurement

A polarizer implements the projection operator $P_\chi = |\chi\rangle\langle\chi|$:

$$P_V = |V\rangle\langle V| = \begin{pmatrix}0&0\\0&1\end{pmatrix}, \qquad P_H = |H\rangle\langle H| = \begin{pmatrix}1&0\\0&0\end{pmatrix}$$

Output state: $|\psi_{\text{out}}\rangle = P_\chi|\psi\rangle$ (unnormalized). Projectors are **idempotent**: $P^2 = P$.

### Three-Polarizer Paradox

$|H\rangle$ source → $|V\rangle$ polarizer: **zero** transmission ($\langle V|H\rangle = 0$).

$|H\rangle$ → $|D\rangle$ polarizer → $|V\rangle$ polarizer: **25%** transmission. Inserting the intermediate polarizer *increases* transmission because measurement changes the state.

With $n$ evenly spaced polarizers between $0°$ and $90°$:

$$T(n) = \cos^{2(n+1)}\!\left(\frac{\pi}{2(n+1)}\right) \xrightarrow{n\to\infty} 1$$

### From Waves to Single Photons

Photon energy: $E = h\nu$. At low power, light arrives as discrete clicks at exactly one detector — never split, never missing. Statistics reproduce the classical interference pattern. Quantum description: superposition of paths with complex amplitudes.

### The Measurement Problem and Decoherence

When a photon hits a detector: absorption → electron excitation → avalanche → entanglement spreads to $\sim10^9$ particles → the two branches of the superposition become orthogonal → interference between branches is destroyed. This process (**decoherence**) is described entirely by the Schrödinger equation applied to system + environment. No additional collapse postulate is required.

---

## Lecture 2.4 — Complex Numbers, Rotations, and Group Theory

### Why Quantum Mechanics Requires Complex Numbers

The Schrödinger equation for a free particle:

$$i\hbar\frac{\partial\psi}{\partial t} = -\frac{\hbar^2}{2m}\frac{\partial^2\psi}{\partial x^2}$$

Quantum dispersion relation: $\omega = \hbar k^2/(2m)$ (quadratic, unlike $\omega = ck$).

Only $e^{-i\omega t}$ solves the time part (not $e^{+i\omega t}$). Real functions ($\cos$, $\sin$) do not solve the Schrödinger equation. Complex numbers are **mandatory** in quantum mechanics, not a bookkeeping trick.

### Complex Multiplication as Rotation

$$e^{i\theta_1}\cdot e^{i\theta_2} = e^{i(\theta_1+\theta_2)}$$

Multiplying by $e^{i\theta}$ rotates a point in the complex plane by angle $\theta$.

### Group Theory: The Four Axioms

A **group** is a set with an operation satisfying: (1) closure, (2) associativity, (3) identity, (4) inverses.

### U(1): Unit Complex Numbers Under Multiplication

$\{e^{i\theta}\}$ with multiplication. Identity: $e^{i\cdot 0}=1$. Inverse: $(e^{i\theta})^{-1} = e^{-i\theta}$.

### SO(2): 2D Rotation Matrices

$$R(\theta) = \begin{pmatrix}\cos\theta & -\sin\theta \\ \sin\theta & \cos\theta\end{pmatrix}$$

- **S**pecial: $\det R = 1$
- **O**rthogonal: $R^T R = I$ (preserves lengths)
- $R(\theta_1)R(\theta_2) = R(\theta_1+\theta_2)$ → **commutative** (abelian)

Complex multiplication by $e^{i\theta}$ and matrix multiplication by $R(\theta)$ are two representations of the same rotation.

### SO(3): 3D Rotations — Non-Commutative

$$R_x(\theta_1)R_z(\theta_2) \neq R_z(\theta_2)R_x(\theta_1)$$

Non-commutativity of rotations is the mathematical origin of the uncertainty principle.

---

## Lecture 2.5 — Generators, Pauli Matrices, and SU(2)

### Generators from Infinitesimal Rotations

For small $\delta\theta$:

$$R(\delta\theta) \approx I + \delta\theta\, G, \qquad G = \begin{pmatrix}0&-1\\1&0\end{pmatrix}$$

$G$ is the **generator** of SO(2). Finite rotations via exponentiation:

$$R(\theta) = e^{\theta G}$$

### Key Property: $G^2 = -I$

This is the matrix version of $i^2 = -1$. The rotation formula follows:

$$e^{\theta G} = \cos\theta\, I + \sin\theta\, G$$

Compare to Euler: $e^{i\theta} = \cos\theta + i\sin\theta$. The generator $G$ plays the role of $i$.

| | Complex numbers | Matrices |
|---|---|---|
| Element | $e^{i\theta}$ | $R(\theta) = e^{\theta G}$ |
| Generator | $i$ | $G$ |
| Key property | $i^2 = -1$ | $G^2 = -I$ |
| Rotation formula | $e^{i\theta} = \cos\theta + i\sin\theta$ | $e^{\theta G} = \cos\theta\,I + \sin\theta\,G$ |

### SO(3) Generators and Commutation Relations

Three generators $J_x, J_y, J_z$ (each is a $3\times3$ antisymmetric matrix with the SO(2) block embedded in the relevant plane):

$$[J_x,J_y] = J_z, \qquad [J_y,J_z] = J_x, \qquad [J_z,J_x] = J_y$$

Compactly: $[J_i, J_j] = \epsilon_{ijk}J_k$.

### SU(2): The Quantum Rotation Group

Group of $2\times 2$ **unitary** matrices with $\det = 1$. Acts on complex state vectors in $\mathbb{C}^2$.

- **S**pecial: $\det U = 1$
- **U**nitary: $U^\dagger U = I$ (preserves $|c_0|^2 + |c_1|^2$)

### The Pauli Matrices: Generators of SU(2)

$$\sigma_x = \begin{pmatrix}0&1\\1&0\end{pmatrix}, \qquad \sigma_y = \begin{pmatrix}0&-i\\i&0\end{pmatrix}, \qquad \sigma_z = \begin{pmatrix}1&0\\0&-1\end{pmatrix}$$

Spin operators: $S_i = \frac{\hbar}{2}\sigma_i$.

### Pauli Matrix Properties

- **Hermitian:** $\sigma_i^\dagger = \sigma_i$ → represent observables
- **Traceless:** $\text{Tr}(\sigma_i) = 0$
- **Square to identity:** $\sigma_i^2 = I$ → eigenvalues $\pm 1$
- **Eigenstates:** The six cardinal Bloch sphere states

| Pauli | +1 eigenstate | −1 eigenstate | Bloch axis |
|---|---|---|---|
| $\sigma_z$ | $\|0\rangle$ | $\|1\rangle$ | z |
| $\sigma_x$ | $\|+\rangle$ | $\|-\rangle$ | x |
| $\sigma_y$ | $\|{+i}\rangle$ | $\|{-i}\rangle$ | y |

### Pauli Algebra

**Commutators:**

$$[\sigma_i, \sigma_j] = 2i\,\epsilon_{ijk}\,\sigma_k$$

$$[\sigma_x,\sigma_y] = 2i\sigma_z, \qquad [\sigma_y,\sigma_z] = 2i\sigma_x, \qquad [\sigma_z,\sigma_x] = 2i\sigma_y$$

**Anticommutators:**

$$\{\sigma_i, \sigma_j\} = 2\delta_{ij}\,I$$

**Master formula:**

$$\sigma_i\sigma_j = \delta_{ij}\,I + i\,\epsilon_{ijk}\,\sigma_k$$

**Products:** $\sigma_x\sigma_y = i\sigma_z$, $\sigma_y\sigma_z = i\sigma_x$, $\sigma_z\sigma_x = i\sigma_y$ (cyclic with factor $i$).

### Uncertainty Principle

Non-commuting generators → corresponding observables cannot be simultaneously known. Since $[\sigma_x, \sigma_z] \neq 0$: no state is an eigenstate of both $S_x$ and $S_z$.

### The General SU(2) Rotation

$$R_{\hat{n}}(\theta) = e^{-i\theta(\hat{n}\cdot\vec{\sigma})/2} = \cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,(\hat{n}\cdot\vec{\sigma})$$

where $\hat{n}\cdot\vec{\sigma} = n_x\sigma_x + n_y\sigma_y + n_z\sigma_z$ and $(\hat{n}\cdot\vec{\sigma})^2 = I$ for unit $\hat{n}$.

### Explicit Axis Rotations

$$R_z(\theta) = \begin{pmatrix}e^{-i\theta/2}&0\\0&e^{i\theta/2}\end{pmatrix}, \qquad R_x(\theta) = \begin{pmatrix}\cos\frac{\theta}{2}&-i\sin\frac{\theta}{2}\\-i\sin\frac{\theta}{2}&\cos\frac{\theta}{2}\end{pmatrix}, \qquad R_y(\theta) = \begin{pmatrix}\cos\frac{\theta}{2}&-\sin\frac{\theta}{2}\\\sin\frac{\theta}{2}&\cos\frac{\theta}{2}\end{pmatrix}$$

### Gate–Rotation Connections

- Phase gate: $P(\phi) = e^{i\phi/2}\,R_z(\phi)$ (z-rotation up to global phase)
- X gate: $\sigma_x = iR_x(\pi)$ (π rotation about x; NOT gate, swaps $|0\rangle\leftrightarrow|1\rangle$)
- Z gate: $\sigma_z = iR_z(\pi)$ (π rotation about z; flips equatorial states)
- Y gate: $\sigma_y = iR_y(\pi)$ (π rotation about y)
- Hadamard: $H = \frac{1}{\sqrt{2}}(\sigma_x + \sigma_z)$ = π rotation about $(\hat{x}+\hat{z})/\sqrt{2}$; swaps z-basis ↔ x-basis

**Every single-qubit unitary is a rotation on the Bloch sphere.**

### The Double Cover: SU(2) vs SO(3)

$$R_{\text{SU(2)}}(2\pi) = -I \neq +I = R_{\text{SO(3)}}(2\pi)$$

Need $4\pi$ to return to identity in SU(2). The $-1$ phase is physically observable in interference experiments (neutron interferometry). **Fermions** (half-integer spin) transform under SU(2); **bosons** (integer spin) under SO(3). Connected to the spin-statistics theorem.

### Summary Table

| | SO(2) | SO(3) | SU(2) |
|---|---|---|---|
| Dimension | 1 axis | 3 axes | 3 axes |
| Acts on | $\mathbb{R}^2$ | $\mathbb{R}^3$ | $\mathbb{C}^2$ |
| Generators | $G$ | $J_x, J_y, J_z$ | $\sigma_x, \sigma_y, \sigma_z$ |
| Commutative? | Yes | No | No |
| $R(2\pi)$ | $+I$ | $+I$ | $-I$ |

---

## Lecture 2.6 — Time Evolution and Rabi Oscillations

### The Stern-Gerlach Experiment (1922)

Silver atoms sent through an inhomogeneous magnetic field split into **exactly two spots** (not a continuum). Force $F_z = \gamma S_z \,\partial B_z/\partial z$ is proportional to $S_z$, which is quantized to $\pm\hbar/2$. The Stern-Gerlach apparatus is a qubit measurement device.

### The Schrödinger Equation and Time Evolution

$$i\hbar\frac{\partial}{\partial t}|\psi(t)\rangle = H|\psi(t)\rangle$$

**Time evolution operator** (for time-independent $H$):

$$|\psi(t)\rangle = U(t)|\psi(0)\rangle, \qquad U(t) = e^{-iHt/\hbar}$$

The Hamiltonian is the **generator of time evolution**: $U(\Delta t) \approx I - \frac{iH}{\hbar}\Delta t$.

### Unitarity and Probability Conservation

For Hermitian $H$ ($H^\dagger = H$):

$$U^\dagger U = e^{+iHt/\hbar}e^{-iHt/\hbar} = I$$

Unitarity guarantees $\langle\psi(t)|\psi(t)\rangle = 1$ for all $t$.

### The Generator Pattern

| Transformation | Unitary Operator | Hermitian Generator |
|---|---|---|
| Spin rotation about $\hat{n}$ | $R_{\hat{n}}(\theta) = e^{-i\theta(\hat{n}\cdot\vec{\sigma})/2}$ | $\hat{n}\cdot\vec{\sigma}$ (Pauli matrices) |
| Time evolution | $U(t) = e^{-iHt/\hbar}$ | $H$ (Hamiltonian) |
| Space translation | $T(a) = e^{-i\hat{p}a/\hbar}$ | $\hat{p} = -i\hbar\frac{d}{dx}$ (momentum) |

**Symmetries (unitary transformations) are generated by observables (Hermitian operators).**

### Example 1: Larmor Precession ($H = \frac{\hbar\omega_0}{2}\sigma_z$)

$$U(t) = R_z(\omega_0 t) = \begin{pmatrix}e^{-i\omega_0 t/2}&0\\0&e^{i\omega_0 t/2}\end{pmatrix}$$

- $|0\rangle, |1\rangle$: eigenstates, acquire only global phase (stationary on Bloch sphere)
- Equatorial states ($|+\rangle$, etc.): precess around z-axis at $\omega_0$
- $P_0$ and $P_1$ are **constant** — populations don't change, only relative phase evolves

### Example 2: Rabi Oscillations ($H = \frac{\hbar\Omega}{2}\sigma_x$)

$$U(t) = R_x(\Omega t), \qquad |\psi(t)\rangle = \cos\frac{\Omega t}{2}|0\rangle - i\sin\frac{\Omega t}{2}|1\rangle$$

$$P_0(t) = \cos^2\frac{\Omega t}{2}, \qquad P_1(t) = \sin^2\frac{\Omega t}{2}$$

Population **oscillates** between $|0\rangle$ and $|1\rangle$ at the Rabi frequency $\Omega$.

### Special Pulses

| Pulse | Condition | Effect |
|---|---|---|
| **π/2-pulse** | $\Omega t = \pi/2$ | $\|0\rangle \to \frac{1}{\sqrt{2}}(\|0\rangle - i\|1\rangle)$; pole → equator |
| **π-pulse** | $\Omega t = \pi$ | $\|0\rangle \to -i\|1\rangle$; complete inversion |

### Eigenstates Are Stationary

If $H|\psi_n\rangle = E_n|\psi_n\rangle$, then $e^{-iHt/\hbar}|\psi_n\rangle = e^{-iE_n t/\hbar}|\psi_n\rangle$. Eigenstates of $H$ evolve only by a global phase. Superpositions of energy eigenstates oscillate.

### Conserved Quantities

If $[H, \sigma_k] = 0$, then $\langle\sigma_k\rangle$ is conserved. For $H\propto\sigma_x$: $\langle\sigma_x\rangle$ is constant; $\langle\sigma_y\rangle$ and $\langle\sigma_z\rangle$ oscillate.

### Space Translation Operator

$$T(a) = e^{-i\hat{p}a/\hbar} = e^{-a\,d/dx} \qquad \Longrightarrow \qquad T(a)\psi(x) = \psi(x-a)$$

Momentum generates spatial translations, just as the Hamiltonian generates time translations.

---

## Lecture 2.7 — Atomic Clocks and Noisy Qubits

### Definition of the Second

Historical progression: astronomical (1/86,400 of a solar day) → ephemeris (orbital, 1956) → **atomic (1967)**:

$$\boxed{1\text{ second} = 9{,}192{,}631{,}770 \text{ oscillations of cesium-133}}$$

This is the hyperfine transition frequency of Cs-133: $\nu_0 = 9{,}192{,}631{,}770$ Hz.

### Hyperfine Structure

The electron spin and nuclear spin ($I=7/2$ for Cs-133) couple like two bar magnets. Aligned vs. anti-aligned orientations give different energies → hyperfine splitting. The transition frequency is extremely precise and stable because every Cs atom is exactly identical.

### The Ramsey Interferometer (Atomic Clock Mechanism)

1. Initialize in $|0\rangle$ (one hyperfine state)
2. **π/2-pulse** ($R_x(\pi/2)$): create superposition on equator
3. **Free evolution for time $T$**: state precesses, accumulates phase $\phi = \Delta T$
4. **Second π/2-pulse**: convert phase to population
5. **Measure**: $P_0 = \sin^2(\Delta T/2)$

Feedback loop: adjust $\omega_L$ until $P_0 = 0$ (on resonance, $\Delta = 0$).

### Atom-Light Interaction Hamiltonian

Lab frame (time-dependent):

$$H = \frac{\hbar\omega_0}{2}\sigma_z + \hbar\Omega\cos(\omega_L t)\,\sigma_x$$

### The Rotating Frame Transformation

Transform to a frame rotating at $\omega_L$ via $U_R = e^{-i\omega_L t\,\sigma_z/2}$. After the **rotating wave approximation** (drop terms oscillating at $2\omega_L$):

$$\boxed{H_R = \frac{\hbar\Delta}{2}\sigma_z + \frac{\hbar\Omega}{2}\sigma_x}$$

where $\Delta = \omega_0 - \omega_L$ is the **detuning**.

This transforms a hard time-dependent problem into an easy time-independent one.

### Rotating Frame Physics

- **On resonance** ($\Delta = 0$): $H_R = \frac{\hbar\Omega}{2}\sigma_x$ → pure Rabi oscillations about x
- **No drive** ($\Omega = 0$): $H_R = \frac{\hbar\Delta}{2}\sigma_z$ → precession about z at detuning $\Delta$
- **Off resonance** ($|\Delta|\gg\Omega$): mostly precession, weak oscillations
- **Strong drive** ($\Omega\gg|\Delta|$): mostly Rabi oscillations

### Clock Precision

Frequency resolution: $\delta\omega \sim 1/T$. Fractional precision: $\delta\nu/\nu_0$.

| Clock Type | Transition | Precision |
|---|---|---|
| Cesium fountain (NIST-F2) | 9.2 GHz microwave | $\sim 10^{-16}$ (1 s in 300 Myr) |
| Optical (e.g. Sr) | 429 THz optical | $\sim 10^{-18}$ (1 s in age of universe) |

Optical clocks can detect gravitational redshift from a 1 cm height difference.

### Decoherence

Random phase kicks from the environment scramble the interference pattern.

**$T_2$ (dephasing time):** time until phase information is lost. Random z-rotations spread equatorial states → fringe contrast decays as $\sim e^{-T/T_2}$.

**$T_1$ (relaxation time):** time until population decays (energy exchange with environment). All states drift toward ground state.

**Constraint:** $T_2 \leq 2T_1$ (energy relaxation also causes dephasing).

### Typical Coherence Times

| System | $T_1$ | $T_2$ |
|---|---|---|
| Trapped ions | minutes | seconds |
| Neutral atoms | seconds | ~1 second |
| Superconducting qubits | ~100 μs | ~100 μs |
| NV centers in diamond | ms | μs–ms |
| Quantum dots | ns–μs | ns |

### Physical Noise Sources

| System | Main noise |
|---|---|
| Neutral atoms | Stray magnetic fields |
| Superconducting qubits | Flux noise, charge noise |
| Trapped ions | Magnetic field fluctuations |
| NV centers | Nuclear spin bath ($^{13}$C), nitrogen impurities |
| Quantum dots | Nuclear spins, charge fluctuations |

### The Fundamental Quantum Computing Challenge

$$\text{Need: } \underbrace{\text{perfect isolation from environment}}_{\text{long coherence}} \;+\; \underbrace{\text{strong coupling between qubits}}_{\text{fast gates}}$$

Quantum computing = controlled interference. Dephasing destroys the phases that make interference work. The engineering challenge is achieving isolation from noise while maintaining precise control.

---

## Notation and Conventions Summary

| Symbol | Meaning |
|---|---|
| $\|0\rangle, \|1\rangle$ | Computational basis (z-eigenstates) |
| $\|+\rangle, \|-\rangle$ | x-eigenstates: $\frac{1}{\sqrt{2}}(\|0\rangle \pm \|1\rangle)$ |
| $\|{+i}\rangle, \|{-i}\rangle$ | y-eigenstates: $\frac{1}{\sqrt{2}}(\|0\rangle \pm i\|1\rangle)$ |
| $H$ | Hadamard gate: $\frac{1}{\sqrt{2}}\begin{pmatrix}1&1\\1&-1\end{pmatrix}$ |
| $P(\phi)$ | Phase gate: $\begin{pmatrix}1&0\\0&e^{i\phi}\end{pmatrix}$ |
| $\sigma_x, \sigma_y, \sigma_z$ | Pauli matrices |
| $R_{\hat{n}}(\theta)$ | SU(2) rotation by $\theta$ about axis $\hat{n}$ |
| $U(t) = e^{-iHt/\hbar}$ | Time evolution operator |
| $\Omega$ | Rabi frequency (drive strength) |
| $\Delta = \omega_0 - \omega_L$ | Detuning |
| $T_1$ | Relaxation time |
| $T_2$ | Dephasing time |

---

## Homework Topics by Lecture

| Lecture | Key HW Problems |
|---|---|
| 2.1 | Separation of variables, traveling wave plots, complex exponential conversions, time-averaged intensity |
| 2.2 | Hadamard and phase gate definition/unitarity, MZI simulation and interference plotting (Python) |
| 2.3 | Basis changes (H/V ↔ R/L), sequential polarizers (three-polarizer paradox, $n$-polarizer limit), measurement problem essay |
| 2.4 | Group axiom verification, SO(2) matrix properties, SO(3) non-commutativity, generator discovery ($G^2=-I$) |
| 2.5 | Lorentz boost generators ($K^2=+I$ → hyperbolic functions), Pauli algebra, SU(2) rotations of cardinal states |
| 2.6 | $\sigma_x$ eigenvectors, time evolution under $\sigma_z$ and $\sigma_x$, Rabi oscillation derivation, Hadamard as rotation, space translation operator |
| 2.7 | Full rotating frame derivation (step-by-step RWA), dephasing simulation with random phase kicks and $T_2$ extraction (Qiskit) |
