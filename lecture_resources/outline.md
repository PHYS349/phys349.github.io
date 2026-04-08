# PHYS 349 — Second Half Outline (Weeks 10–14)

## Series 03: Entanglement → Circuits → Protocols → Hardware

### Week 10 — Circuits and Qiskit Foundations

**Lecture 03_05: The Circuit Model + No-Cloning Theorem**
- Classical circuits (Boolean, gates, fan-out) as motivation
- Quantum circuit model: wires = qubits, gates = unitaries, left-to-right flow
- Gate symbols: H, X, CNOT (dot + ⊕), SWAP (×—×), controlled-U, Toffoli, measurement (meter)
- Qiskit qubit ordering convention (top = index 0 = rightmost in ket)
- The e-bit circuit: H then CNOT → Bell states from computational basis
- No-cloning theorem: statement + linearity proof
- No fan-out as a consequence (contrast with classical circuits)
- Non-distinguishability of non-orthogonal states (sets up QKD security)
- Sources: Watrous circuits page, Watrous limitations page

**Lecture 03_06: Qiskit Hands-On**
- QuantumCircuit, Statevector, Operator classes
- Building gates from matrices: `Operator.from_label()`, tensor products with `^`
- SamplerV2 vs EstimatorV2 — when to use each
- Transpilation: `generate_preset_pass_manager()`, optimization levels
- Running on real hardware via `SamplerV2(mode=backend)`
- Bloch sphere visualization, `plot_histogram()`
- Exercise: quantum half-adder (CX for XOR, CCX for AND)
- Exercise: Stern-Gerlach experiments in Qiskit (basis rotation, collapse demo)
- Sources: Get Started module, Superposition module, Stern-Gerlach module

### Week 11 — Entanglement Protocols

**Lecture 03_07: Quantum Key Distribution (BB84)**
- Classical crypto motivation: replacement ciphers → one-time pad
- The key distribution problem
- BB84 protocol: Alice prepares (Z/X basis), Bob measures (Z/X basis)
- Basis reconciliation (public channel), key sifting (~50% retained)
- Eavesdropping detection: Eve's intercept-resend attack
- Error rate derivation: 0.5 × 0.5 × 0.5 = 12.5%
- Connection to no-cloning + non-distinguishability (from 03_05)
- Qiskit implementation: 20-qubit simulator, 127-qubit real hardware
- In-class activity: one-time pad encryption with a partner
- Sources: QKD module, Watrous limitations (no-cloning)

**Lecture 03_08: Quantum Teleportation + Superdense Coding**
- Teleportation protocol: setup, Alice's CNOT+H, measurement, Bob's corrections
- Full 3-qubit derivation (state evolution through each step)
- Correction table: (a,b) → {I, Z, X, ZX}
- Key properties: e-bit consumed, no-cloning respected, Alice learns nothing, no FTL
- Superdense coding as the dual protocol (2 classical bits via 1 qubit + 1 e-bit)
- Encoding: Pauli operations on one half map between all four Bell states
- Decoding: inverse Bell circuit
- Duality table: teleportation vs superdense coding
- Qiskit: `if_test()` for classical conditioning, verification trick (U then U†)
- Sources: Watrous teleportation/superdense, Teleportation module

### Week 12 — Quantum Hardware + Sensing

**Lecture 03_09: Quantum Hardware Platforms**
- Superconducting qubits: transmons, microwave control, dilution refrigerators, IBM/Google
- Trapped ions: electromagnetic traps, laser-driven gates, IonQ/Quantinuum
- Neutral atoms: optical tweezers, Rydberg interactions, QuEra
- Photonic qubits: polarization encoding, linear optical gates, PsiQuantum/Xanadu
- Comparison: gate fidelity, T1/T2 times, connectivity, scalability
- How gates are physically implemented (microwave pulses, laser pulses, etc.)
- Current state of the field: QPU sizes, error rates, quantum volume
- Sources: instructor expertise (AMO), IBM hardware docs

**Lecture 03_10: Quantum Sensing and Squeezing**
- Shot noise limit: $\Delta\phi \sim 1/\sqrt{N}$ (standard quantum limit)
- Heisenberg limit: $\Delta\phi \sim 1/N$
- Squeezed states: reducing uncertainty in one quadrature at the expense of another
- LIGO: squeezed light injection, gravitational wave sensitivity improvement
- Atomic clocks: Ramsey interferometry, spin squeezing, optical lattice clocks
- Atom interferometry: inertial sensing, gravimetry, tests of general relativity
- Connection to uncertainty relations (covered in 02_04–02_05, Uncertainty module)
- Sources: instructor expertise, review articles

---

## Series 04: Quantum Algorithms

### Week 13 — First Algorithms

**Lecture 04_01: Deutsch-Jozsa Algorithm**
- Quantum parallelism: superposition evaluates all inputs, but measurement collapses
- Oracle / black-box model: $U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$
- Phase kickback: $U_f|x\rangle|-\rangle = (-1)^{f(x)}|x\rangle|-\rangle$
- Deutsch's algorithm (n=1): full state evolution $|\pi_1\rangle → |\pi_2\rangle → |\pi_3\rangle$
- Deutsch-Jozsa (general n): H⊗n → oracle → H⊗n → measure
- Classical cost: $2^{n-1}+1$ deterministic; quantum cost: 1 query
- Bernstein-Vazirani variant: find secret string $s$ where $f(x) = s \cdot x$
- Qiskit implementation with oracle construction
- Sources: D-J module, Watrous quantum query algorithms

**Lecture 04_02: Grover's Algorithm**
- Unstructured search: find marked item in unsorted database
- Oracle $Z_f$: phase flip on marked states
- Diffusion operator: "inversion about the mean" / amplitude amplification
- Grover operator: $G = H^{\otimes n} Z_{\text{OR}} H^{\otimes n} \cdot Z_f$
- Optimal iterations: $t = \lfloor \frac{\pi}{4}\sqrt{N/|A_1|} - \frac{1}{2} \rfloor$
- 2-qubit worked example (complete state evolution, deterministic in 1 step)
- Quadratic speedup: $O(\sqrt{N})$ vs $O(N)$ — not exponential!
- Too many iterations → probability decreases (amplitude oscillation)
- Qiskit: `grover_operator()`, `MCMTGate(ZGate())`, partner QPY exchange activity
- Sources: Grover's module, Watrous unstructured search

### Week 14 — QFT, Shor's, and Course Wrap-Up

**Lecture 04_03: Quantum Fourier Transform + Phase Estimation**
- Classical DFT review: $O(N^2)$; FFT: $O(N \log N)$; QFT: $O(n^2)$ on $n$ qubits
- QFT definition: $|j\rangle \to \frac{1}{\sqrt{N}}\sum_{k=0}^{N-1} e^{2\pi i jk/N}|k\rangle$
- QFT circuit: Hadamards + controlled phase rotations + bit reversal
- Product representation of QFT output (tensor product of individual qubit states)
- Quantum Phase Estimation: find eigenvalue $e^{2\pi i\phi}$ of unitary $U$
- QPE circuit: Hadamards on control register, controlled-$U^{2^k}$, inverse QFT
- Sources: QFT module, Watrous phase estimation, Shor's module (QPE section)

**Lecture 04_04: Shor's Algorithm (Overview)**
- RSA and the factoring problem
- Modular arithmetic: $\mathbb{Z}_N^*$, order of $a$ mod $N$
- Key reduction: factoring → order finding → QPE
- The 5 steps: pick $a$, find order $r$, check parity, compute $\gcd$
- Example: $N=15$, $a=2$, $M_2$ as SWAP gates
- Eigenstates of modular multiplication, $|1\rangle$ as superposition trick
- Classical post-processing: continued fractions
- Why this threatens RSA (and why we're not there yet — qubit overhead)
- Sources: Shor's module, Watrous phase estimation and factoring

---

## Final Projects (Weeks 14–15)

Students choose one topic for a final project/presentation. Possible topics:

- **Grover's algorithm** — implement and run on IBM hardware, analyze noise effects
- **Shor's algorithm** — deeper dive into $N=21$ or other small composites
- **VQE** — variational quantum eigensolver for H₂ or HeH⁺ molecular energy
- **QFT applications** — period finding, quantum counting
- **Quantum error correction** — repetition code, surface code basics
- **CHSH game on hardware** — run Bell test, compare with Tsirelson bound
- **Quantum sensing** — deeper dive into LIGO, atomic clocks, or atom interferometry
- **Quantum teleportation on hardware** — long-distance via SWAP chains

---

## Lecture Numbering Summary

| Lecture | Topic | Week |
|---------|-------|------|
| 03_05 | Circuit model + no-cloning | 10 |
| 03_06 | Qiskit hands-on | 10 |
| 03_07 | QKD (BB84) | 11 |
| 03_08 | Teleportation + superdense coding | 11 |
| 03_09 | Quantum hardware platforms | 12 |
| 03_10 | Quantum sensing + squeezing | 12 |
| 04_01 | Deutsch-Jozsa | 13 |
| 04_02 | Grover's algorithm | 13 |
| 04_03 | QFT + phase estimation | 14 |
| 04_04 | Shor's algorithm | 14 |

Review week + final projects: Weeks 14–15
