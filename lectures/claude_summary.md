# PHYS 349 Lecture Summary — Claude Reference File

**Purpose:** This file summarizes lectures 01_01 through 03_04 for PHYS 349 (Quantum Computing for undergraduates). It is intended as a reference for Claude when helping write future lectures. Each summary captures the main narrative, pedagogy, and key equations.

**Course Style Notes:**
- Written in Jupyter Book 2 / MyST markdown format
- Heavy use of admonitions (`{admonition}`, `{tip}`, `{warning}`, `{dropdown}`)
- Figure directives with `{figure}` blocks, downloadable files with `{download}`
- iClicker questions for active learning, Python/Qiskit code cells
- Emphasizes physical intuition over formalism; connects to real hardware
- Uses interferometers (especially Mach-Zehnder) as the bridge between optics and quantum computing
- Homework problems embedded at end of lectures
- Audience: undergraduates, likely with intro QM or modern physics background

---

## Chapter 1: Why Quantum Computing?

### Lecture 01_01 — Why Quantum? Why Now?

This lecture motivates the entire course by framing quantum computing within the two quantum revolutions. The first revolution (early 1900s) gave us the key equations of quantum mechanics — $E = \hbar\omega$, $p = h/\lambda$, the Schrödinger equation $i\hbar \partial_t |\psi\rangle = \hat{H}|\psi\rangle$, and the uncertainty principle $\Delta x \Delta p \geq \hbar/2$ — which underpin all modern technology (lasers, semiconductors, MRI). The second revolution is about *engineering* quantum superposition and entanglement directly, enabling quantum computing, communication, and sensing.

The lecture introduces the qubit as a two-level quantum system with superposition $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$ where $|\alpha|^2 + |\beta|^2 = 1$, contrasting it with a classical bit. Measurement collapses the state to $|0\rangle$ or $|1\rangle$ with probabilities $|\alpha|^2$ and $|\beta|^2$. The tone is big-picture and motivational, setting up the "why should I care" framing for the rest of the course.

### Lecture 01_02 — What Are Classical Computers Bad At?

This lecture establishes the computational motivation for quantum computing by examining what classical computers struggle with. It walks through CPU/GPU clock speeds and FLOPS, estimates for LLM computation, and then identifies problems where classical scaling fails: the traveling salesman problem ($N!$ scaling) and spin system ground states ($2^N$ configurations). The central message is the distinction between polynomial and exponential scaling — classical computers hit a wall when the problem size grows exponentially, and this is precisely where quantum computers may offer advantage.

Key numbers used for intuition: a 300-spin system has $2^{300} \approx 10^{90}$ configurations (more than atoms in the universe). The lecture also touches on current LLM computational costs to ground the discussion in something students interact with daily.

### Lecture 01_03 — What Makes Quantum Hard?

This lecture bridges classical difficulty and quantum opportunity using the Ising model $H = -J\sum_i s_i s_{i+1} - B\sum_i s_i$ as a concrete example. It introduces simulated annealing via the Metropolis algorithm (accept spin flips with probability $e^{-\Delta E / k_B T}$) as the best classical approach, then contrasts this with the quantum idea: a quantum computer can exist in a superposition over all $2^N$ configurations simultaneously. The key quantum concepts introduced are the state vector $|\psi\rangle = \sum_i c_i |s_i\rangle$, the Born rule $P(s_i) = |c_i|^2$, entanglement (correlations that have no classical explanation), and measurement collapse.

The lecture's punchline is that quantum computing is really about *engineering interference* — arranging for wrong answers to destructively interfere and correct answers to constructively interfere. This is the first time the interference framing appears, and it becomes a recurring theme throughout the course.

### Lecture 01_04 — Quantum Hardware

This lecture surveys the practical landscape of quantum hardware. It introduces the NISQ (Noisy Intermediate-Scale Quantum) era and the three main sources of decoherence: energy relaxation ($T_1$), dephasing ($T_2$), and gate errors. The no-cloning theorem is stated (you cannot copy an unknown quantum state), which makes quantum error correction fundamentally different from classical error correction — you need $O(1000)$ physical qubits per logical qubit.

Three major hardware platforms are compared: superconducting qubits (IBM/Google, ~1000 qubits, $T_2 \sim 100\,\mu s$, gate times ~20 ns), trapped ions (IonQ/Quantinuum, ~30–50 qubits, $T_2 \sim 1\,s$, gate times ~100 $\mu$s, all-to-all connectivity), and neutral atoms (QuEra/Pasqal, ~200+ qubits, $T_2 \sim 1\,s$, reconfigurable arrays). The lecture grounds the abstract qubit concept in real physical systems.

---

## Chapter 2: Single-Qubit Physics

### Lecture 02_01 — Waves, Complex Numbers, and Interference

This lecture builds the mathematical foundation for quantum mechanics through classical wave physics. Starting from the wave equation $\frac{d^2 E}{dx^2} = -k^2 E$ with solutions $E(x) = E_0 e^{ikx}$, it develops Euler's formula $e^{i\theta} = \cos\theta + i\sin\theta$, traveling waves $E_0 e^{i(kx - \omega t)}$, and the key result that intensity is the modulus squared: $I \propto |E|^2 = E^* E$.

The critical physics is in interference: $|E_1 + E_2|^2 = |E_1|^2 + |E_2|^2 + 2\text{Re}(E_1^* E_2)$, where the cross term creates constructive or destructive interference depending on relative phase. The lecture also establishes that global phase ($e^{i\alpha}|\psi\rangle$) is unobservable since it cancels in $|\psi|^2$, while relative phase between components is physically meaningful. This sets up everything needed for the interferometer lectures.

### Lecture 02_02 — Interferometers and the Bloch Sphere

This is a pivotal lecture that connects classical optics to quantum computing. It analyzes the Mach-Zehnder interferometer (MZI), deriving the output intensity $I_{D1} = \frac{1}{2}(1 + \cos\Delta\phi)$. The beam splitter introduces a $\pi$ phase shift on reflection, which maps directly to the Hadamard gate $H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$. The phase shifter maps to $P(\phi) = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\phi} \end{pmatrix}$. The full MZI circuit is $H \cdot P(\phi) \cdot H$, establishing that an interferometer *is* a quantum circuit.

The Bloch sphere representation is introduced: $|\psi\rangle = \cos(\theta/2)|0\rangle + e^{i\phi}\sin(\theta/2)|1\rangle$, where $\theta$ is the polar angle and $\phi$ is the azimuthal angle. Gates become rotations on this sphere: $H$ swaps the $z$-axis and $x$-axis, $P(\phi)$ rotates around $z$, and general single-qubit gates trace paths on the Bloch sphere. The X, Y, Z gates and S, T gates are catalogued.

### Lecture 02_03 — Polarization and Measurement

This lecture uses photon polarization as a concrete qubit implementation. Jones vectors map polarization states to qubit states: $|H\rangle = |0\rangle$, $|V\rangle = |1\rangle$, diagonal $|D\rangle = (|H\rangle + |V\rangle)/\sqrt{2}$, anti-diagonal $|A\rangle = (|H\rangle - |V\rangle)/\sqrt{2}$, and circular $|R\rangle = (|H\rangle - i|V\rangle)/\sqrt{2}$, $|L\rangle = (|H\rangle + i|V\rangle)/\sqrt{2}$. These states are mapped onto the Bloch sphere, with H/V on the poles, D/A on the equator along $x$, and R/L on the equator along $y$.

Measurement is formalized using projectors: $P_V = |V\rangle\langle V|$ gives Malus's law $P(\text{transmitted}) = |\langle V|\psi\rangle|^2$. The lecture discusses single-photon detection (APDs, SNSPDs), single-photon interference in the MZI (the photon interferes with itself), and the measurement problem. Decoherence is explained as entanglement with the environment — when the environment can distinguish the paths, interference is lost.

### Lecture 02_04 — Complex Numbers, Rotations, and Group Theory

This lecture deepens the mathematical framework by arguing that complex numbers in quantum mechanics are *mandatory*, not a convenience. The Schrödinger equation $i\hbar\partial_t \psi = -\frac{\hbar^2}{2m}\nabla^2 \psi$ requires $i$ to give the correct dispersion relation $\omega = \hbar k^2 / 2m$ (without $i$, you get exponential decay instead of wave propagation).

The lecture introduces group theory through rotations: the U(1) group ($e^{i\theta}$ — phases), SO(2) ($2\times 2$ rotation matrices in the plane), and SO(3) (3D rotations, which don't commute: $R_x R_z \neq R_z R_x$). Group axioms are stated: closure, associativity, identity, and inverses. The non-commutativity of rotations foreshadows the commutation relations of Pauli matrices and the fundamental role of non-commutativity in quantum mechanics.

### Lecture 02_05 — Generators, Pauli Matrices, and SU(2)

This lecture develops the generator formalism and arrives at the Pauli matrices. A generator $G$ produces continuous rotations via $R(\theta) = e^{\theta G}$, and the key property $G^2 = -I$ (paralleling $i^2 = -1$) ensures rotations are periodic. The SO(3) generators $J_x, J_y, J_z$ satisfy $[J_i, J_j] = \varepsilon_{ijk} J_k$.

The Pauli matrices $\sigma_x, \sigma_y, \sigma_z$ are introduced as the $2\times 2$ representation, with the fundamental relations: commutator $[\sigma_i, \sigma_j] = 2i\varepsilon_{ijk}\sigma_k$, anti-commutator $\{\sigma_i, \sigma_j\} = 2\delta_{ij}I$, and the master product formula $\sigma_i \sigma_j = \delta_{ij}I + i\varepsilon_{ijk}\sigma_k$. SU(2) is the group of $2\times 2$ unitary matrices with determinant 1, forming a double cover of SO(3): a $2\pi$ rotation gives $R(2\pi) = -I$ (not identity!), requiring $4\pi$ to return. This connects to the fermion/boson distinction — fermions pick up a minus sign under $2\pi$ rotation.

### Lecture 02_06 — Time Evolution and Rabi Oscillations

This lecture connects the mathematical framework to physical dynamics. The Schrödinger equation $H|\psi\rangle = i\hbar\partial_t|\psi\rangle$ gives time evolution $U(t) = e^{-iHt/\hbar}$, making the Hamiltonian the *generator* of time translation. Unitarity $U^\dagger U = I$ is guaranteed because $H = H^\dagger$.

Two key examples: (1) A magnetic field along $z$, $H = -\frac{\hbar\omega_0}{2}\sigma_z$, produces Larmor precession $R_z(\omega_0 t)$ — the qubit rotates around the $z$-axis on the Bloch sphere, and eigenstates $|0\rangle, |1\rangle$ are stationary. (2) A field along $x$, $H = \frac{\hbar\Omega}{2}\sigma_x$, drives Rabi oscillations $P_1(t) = \sin^2(\Omega t/2)$ — complete population transfer between $|0\rangle$ and $|1\rangle$. A $\pi$-pulse ($\Omega t = \pi$) acts as a NOT gate (X gate), and a $\pi/2$-pulse creates equal superposition (like Hadamard). This is how quantum gates are physically implemented in hardware.

### Lecture 02_07 — Atomic Clocks and Noisy Qubits

This lecture applies the Rabi oscillation framework to the most precise measurement device ever built: the atomic clock. The SI second is defined by the cesium hyperfine transition at 9,192,631,770 Hz. The Ramsey sequence ($\pi/2$ – wait $T$ – $\pi/2$ – measure) is analyzed using the rotating frame, where the Hamiltonian becomes $H_R = \frac{\hbar\Delta}{2}\sigma_z + \frac{\hbar\Omega}{2}\sigma_x$ with detuning $\Delta = \omega_0 - \omega_L$. This produces Ramsey fringes with clock precision scaling as $\delta\omega \sim 1/T$.

Optical clocks using strontium achieve fractional stability $\sim 10^{-18}$ (losing ~1 second over the age of the universe). The lecture closes by introducing decoherence and dephasing — real qubits lose coherence over time due to coupling to the environment, characterized by $T_1$ (energy relaxation) and $T_2$ (dephasing) times. This connects back to the hardware limitations discussed in 01_04 and motivates the need for fast gates and error correction.

---

## Chapter 3: Two Qubits and Entanglement

### Lecture 03_01 — Two Qubits and the Tensor Product

This lecture extends the formalism from one to two qubits. It begins with classical probability: independent systems satisfy $P(i,j) = P_A(i) \cdot P_B(j)$, and correlations mean $P(i,j) \neq P_A(i) \cdot P_B(j)$. The quantum analog uses the tensor product: $|\psi\rangle \otimes |\phi\rangle$, producing the computational basis $\{|00\rangle, |01\rangle, |10\rangle, |11\rangle\}$.

A general two-qubit state has 4 complex amplitudes (8 real parameters minus normalization and global phase = 6 degrees of freedom), which decompose as 2 (qubit A) + 2 (qubit B) + 2 (correlations). The amplitude table $C = \vec{u}\,\vec{v}^T$ is introduced as a matrix whose structure reveals whether the state is a product state or entangled. Product states have $C$ as an outer product of two vectors; entangled states cannot be decomposed this way.

### Lecture 03_02 — Entanglement and Bell States

This lecture introduces entanglement detection and the Bell states. The determinant test provides a clean criterion: $\det(C) = 0$ if and only if the state is a product state; $\det(C) \neq 0$ means the state is entangled. The four maximally entangled Bell states are:

$$|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}, \quad |\Phi^-\rangle = \frac{|00\rangle - |11\rangle}{\sqrt{2}}$$
$$|\Psi^+\rangle = \frac{|01\rangle + |10\rangle}{\sqrt{2}}, \quad |\Psi^-\rangle = \frac{|01\rangle - |10\rangle}{\sqrt{2}}$$

The lecture develops the projective measurement recipe for two-qubit systems and connects to the generalized uncertainty principle $\Delta A \cdot \Delta B \geq \frac{1}{2}|\langle[\hat{A}, \hat{B}]\rangle|$, showing that non-commuting observables enforce fundamental measurement limits.

### Lecture 03_03 — Two-Qubit Measurement and EPR

This lecture formalizes two-qubit measurement using tensor product operators $\hat{A} \otimes \hat{B}$ and partial measurement via $|m\rangle\langle m| \otimes I$. Measuring one qubit of an entangled pair instantly determines the other — "spooky action at a distance." The no-signaling theorem is proved: the reduced density matrix of qubit B is $\frac{1}{2}I$ regardless of what Alice measures, so no information is transmitted.

Correlation tables for Bell states in different measurement bases (ZZ, XX, YY) are constructed, showing perfect correlations/anti-correlations. The EPR argument is presented: assuming locality (no faster-than-light influence), realism (properties exist before measurement), and completeness (QM describes everything), one reaches a contradiction. The lecture outlines three interpretations: QM is incomplete (hidden variables), locality fails (nonlocal correlations), or realism fails (properties don't exist until measured).

### Lecture 03_04 — Bell's Theorem

This lecture resolves the EPR debate with Bell's theorem. It first shows that a hidden variable model with two measurement settings can reproduce quantum predictions — the contradiction only appears with three or more settings. Introducing a third measurement axis $\hat{Q}$ at 45° between $\hat{Z}$ and $\hat{X}$ creates a testable disagreement.

The quantum prediction for spin correlations is $E(\hat{a}, \hat{b}) = -\cos\theta_{ab}$. The Venn diagram / pigeonhole argument shows that any hidden variable theory predicts $P(A_X = A_Z) \geq 0.70$ (at least 70% of particles must have pre-determined $X$ and $Z$ values that agree), while quantum mechanics predicts only $P(A_X = A_Z) = \cos^2(22.5°) \approx 0.854$ for same-outcome probability, giving $P(A_X \neq A_Z) \approx 0.146$ — far below the hidden variable bound of 0.30 needed. The Bell inequality is violated by experiment (loophole-free tests in 2015 by Hensen et al., Giustina et al., Shalm et al.), ruling out local hidden variables.

---

## Quick Reference: Key Equations

| Topic | Equation |
|-------|----------|
| Qubit state | $\|\psi\rangle = \alpha\|0\rangle + \beta\|1\rangle$, $\|\alpha\|^2 + \|\beta\|^2 = 1$ |
| Born rule | $P(i) = \|c_i\|^2$ |
| Bloch sphere | $\|\psi\rangle = \cos(\theta/2)\|0\rangle + e^{i\phi}\sin(\theta/2)\|1\rangle$ |
| Euler's formula | $e^{i\theta} = \cos\theta + i\sin\theta$ |
| Interference | $\|E_1 + E_2\|^2 = \|E_1\|^2 + \|E_2\|^2 + 2\text{Re}(E_1^* E_2)$ |
| Hadamard | $H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$ |
| Phase gate | $P(\phi) = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\phi} \end{pmatrix}$ |
| Pauli commutator | $[\sigma_i, \sigma_j] = 2i\varepsilon_{ijk}\sigma_k$ |
| Pauli anti-commutator | $\{\sigma_i, \sigma_j\} = 2\delta_{ij}I$ |
| Master formula | $\sigma_i\sigma_j = \delta_{ij}I + i\varepsilon_{ijk}\sigma_k$ |
| Time evolution | $U(t) = e^{-iHt/\hbar}$ |
| Rabi oscillations | $P_1(t) = \sin^2(\Omega t/2)$ |
| Ramsey detuning | $H_R = \frac{\hbar\Delta}{2}\sigma_z + \frac{\hbar\Omega}{2}\sigma_x$ |
| Tensor product state | $\|\psi\rangle \otimes \|\phi\rangle$ in basis $\{\|00\rangle, \|01\rangle, \|10\rangle, \|11\rangle\}$ |
| Bell state | $\|\Phi^+\rangle = (\|00\rangle + \|11\rangle)/\sqrt{2}$ |
| Entanglement test | $\det(C) \neq 0 \Leftrightarrow$ entangled |
| Uncertainty principle | $\Delta A \cdot \Delta B \geq \frac{1}{2}\|\langle[\hat{A}, \hat{B}]\rangle\|$ |
| Bell correlation | $E(\hat{a}, \hat{b}) = -\cos\theta_{ab}$ |

---

## Pedagogical Arc

**Chapter 1** motivates "why quantum computing" through computational hardness, ending with real hardware.
**Chapter 2** builds single-qubit physics bottom-up: waves → interference → interferometers → Bloch sphere → group theory → Pauli matrices → time evolution → Rabi/Ramsey → decoherence. The MZI is the unifying thread.
**Chapter 3** extends to two qubits: tensor products → entanglement → Bell states → EPR → Bell's theorem. The course proves that nature is nonlocal (or non-realistic), grounding quantum weirdness in experimental fact.

**Next likely topics:** Quantum gates and circuits (CNOT, universal gate sets), quantum algorithms (Deutsch-Jozsa, Grover, Shor), quantum error correction, or deeper dives into specific hardware platforms.
