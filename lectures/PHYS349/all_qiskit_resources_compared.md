# IBM Qiskit Resources — Comparison for PHYS 349

Two IBM sources compared: (A) IBM Quantum Documentation Tutorials, (D) IBM Quantum Learning — Classroom Modules.

---

## Source A: IBM Quantum Documentation Tutorials

**URL:** quantum.cloud.ibm.com/docs/en/tutorials

Professional-grade tutorials demonstrating Qiskit on real hardware. Current API (Qiskit v2.0+). These are more advanced and research-oriented — best used for in-class demos or as stretch material for motivated students.

(Full detailed summaries in qiskit_tutorials_summary.md)

| # | Tutorial | Difficulty | Time | Relevance | Notes |
|---|----------|-----------|------|-----------|-------|
| A1 | CHSH Inequality | 3/10 | 1–1.5 hr | **10/10** | Bell violation on real hardware. Uses EstimatorV2, parameterized circuits, SparsePauliOp. Sweeps measurement angle to show S exceeding classical bound ±2. |
| A2 | Grover's Algorithm | 5/10 | 1.5–2 hr | **9/10** | Oracle + amplification, 3 qubits. Uses SamplerV2, grover_operator(), MCMTGate. |
| A3 | Shor's Algorithm | 8/10 | 3–4 hr | 6/10 | Factors 15 using phase estimation. Controlled modular multiplication, inverse QFT, continued fractions. Dynamical decoupling + gate twirling. Very detailed. |
| A4 | VQE (Heisenberg Chain) | 7/10 | 2–3 hr | 5/10 | 10-spin Heisenberg chain. EfficientSU2 ansatz, COBYLA optimizer, Estimator with Sessions, Qiskit Serverless. |
| A5 | Fractional Gates | 7/10 | 2 hr | 3/10 | Heron-specific RZZ(θ)/RX(θ) native gates. Quantum kernel application. Too specialized. |
| A6 | Long-Range Entanglement | 8/10 | 3–4 hr | 4/10 | Dynamic circuits for constant-depth CNOT via gate teleportation. Mid-circuit measurement, if_test, classical feedforward. Compares to unitary SWAP chains. |
| A7 | Repetition Codes | 4/10 | 1–1.5 hr | **8/10** | 3-qubit bit-flip code. 5 qubits total (3 data + 2 ancilla). Mid-circuit measurement, syndrome decoding, conditional correction. Shows corrected vs uncorrected results. |
| A8 | Cut Bell Pair Benchmark | 7/10 | 2–3 hr | 4/10 | Benchmarks teleportation fidelity across entire 156-qubit processor. MSE of ZZ/XX stabilizers. Shows order-of-magnitude variation in qubit quality. |
| A9 | SABRE Transpilation | 6/10 | 2 hr | 4/10 | Qubit layout + gate routing optimization. SABRE parameters for 100+ qubit circuits. Qiskit Serverless for parallel transpilation. |
| A10 | QAOA (Max-Cut) | 7/10 | 2.5–3 hr | 5/10 | Max-Cut on 5-node and 100-node graphs. QUBO → Pauli Hamiltonian mapping. COBYLA optimization. Dynamical decoupling + twirling at utility scale. |
| A11 | Error Mitigation | 6/10 | 2 hr | 5/10 | Progressive walkthrough: (1) dynamical decoupling, (2) TREX readout mitigation, (3) gate twirling, (4) zero-noise extrapolation. 10-qubit and 50-qubit tests. |
| A12 | Quantum Kernels | 7/10 | 2 hr | 3/10 | Quantum ML — ZZ feature maps, kernel matrix computation via Sampler, binary classification. Qiskit Serverless deployment. |

**Strengths:** Current Qiskit v2.0 API, runs on real hardware, professional quality, well-maintained.
**Weaknesses:** Some tutorials require paid IBM access (Sessions), many are too advanced/specialized for undergrads. No pedagogical narrative connecting them.

---

## Source D: IBM Quantum Learning — Classroom Modules

**URL:** quantum.cloud.ibm.com/learning/en/modules

**Purpose-built for undergraduate teaching.** Current Qiskit v2.1+ API. Jupyter notebook format. Each module has experiments students run, assessment questions, and instructor answer keys (available on request). Free QPU access via the Open Plan. Designed to fit into existing courses.

### Quantum Mechanics Modules

| # | Module | Difficulty | Time | Relevance | Notes |
|---|--------|-----------|------|-----------|-------|
| D1 | Get Started with Qiskit | 1/10 | 30 min | **7/10** | First circuit, basic setup. Good warm-up. |
| D2 | Superposition | 2/10 | 1 hr | 5/10 | Your students already know this — could skip or use as Qiskit intro. |
| D3 | Stern-Gerlach | 3/10 | 1 hr | 4/10 | Emulates SG experiment. Nice but you've covered measurement. |
| D4 | Uncertainty | 3/10 | 1 hr | 5/10 | Probes uncertainty relations experimentally. Connects to 03_02. |
| D5 | Bell's Inequality | 4/10 | 1–1.5 hr | **10/10** | Test hidden variables vs QM on real hardware. Directly extends 03_04. |

### Computer Science Modules

| # | Module | Difficulty | Time | Relevance | Notes |
|---|--------|-----------|------|-----------|-------|
| D6 | Deutsch-Jozsa Algorithm | 5/10 | 1.5 hr | **8/10** | Phase kickback, quantum parallelism, includes Bernstein-Vazirani. Runs on hardware. |
| D7 | Grover's Algorithm | 5/10 | 1.5 hr | **9/10** | Oracle construction, amplitude amplification. Runs on hardware. |
| D8 | Quantum Teleportation | 4/10 | 1.5 hr | **9/10** | Full protocol with 2 experiments (local + long-distance via SWAPs). Assessment included. |
| D9 | Quantum Key Distribution | 4/10 | 1.5 hr | **10/10** | BB84 protocol, one-time pads, eavesdropper detection. Exactly what you want for QKD. |
| D10 | Quantum Fourier Transform | 5/10 | 1.5 hr | **7/10** | QFT circuit construction and applications. Foundation for Shor's. |
| D11 | Shor's Algorithm | 7/10 | 2 hr | 6/10 | Uses QFT module as prereq. Full factoring demo. |
| D12 | Variational Quantum Eigensolver | 6/10 | 2 hr | 4/10 | Chemistry application, variational optimization. |

**Strengths:** ⭐ Purpose-built for undergraduate teaching. Current API. Jupyter notebooks. Assessment questions. Instructor answer keys. Free QPU access. QKD, Teleportation, and Bell's Inequality modules are exactly matched to PHYS 349 content.

**Weaknesses:** Modules are somewhat self-contained — they re-explain basics your students already know (like what a qubit is), so you may want to point students to start at the relevant section rather than page 1.

### IBM Quantum Learning — Full Courses (for reference, not direct use)

IBM also offers four 15-hour video courses by John Watrous: Basics of Quantum Information, Fundamentals of Quantum Algorithms, General Formulation of QI, and Foundations of QEC. These are graduate-level and too long for direct use, but the Fundamentals of Quantum Algorithms course has excellent treatments of Deutsch-Jozsa, Simon's, and Grover's that you could point motivated students to as supplementary material.

---

## Comparison: Documentation Tutorials (A) vs. Classroom Modules (D)

| Topic | Docs Tutorial (A) | Classroom Module (D) | **Recommendation** |
|-------|-------------------|---------------------|-------------------|
| **Bell/CHSH** | A1 — advanced, Estimator | **D5** — classroom-ready | **D5 for lab**, A1 for motivated students |
| **QKD (BB84)** | — | **D9** — purpose-built | **D9** ⭐ |
| **Teleportation** | A6 — very advanced | **D8** — 2 experiments | **D8** ⭐ |
| **Deutsch-Jozsa** | — | **D6** — includes BV bonus | **D6** |
| **Grover** | A2 — hardware-focused | **D7** — classroom-ready | **D7 for lab**, A2 for follow-up |
| **QFT** | — | **D10** | **D10** |
| **Shor's** | **A3** — very detailed | D11 — lighter version | **A3 for demo**, D11 for homework |
| **VQE/QAOA** | A4, A10 | D12 | Demo only — **A10** or **D12** |
| **QEC** | **A7** — bit-flip code | — | **A7** — only option, and it's good |
| **Error mitigation** | **A11** — 4 techniques | — | **A11** concepts for hardware lecture |
| **Hardware/transpilation** | A5, A8, A9 | — | Mention concepts only, skip tutorials |
| **Quantum ML** | A12 | — | Skip for this course |

---

## Recommendations for PHYS 349

### Strategy: Classroom Modules for Labs, Doc Tutorials for Demos

The **Classroom Modules (D)** should be the backbone of your Qiskit assignments — they're designed for exactly this purpose. The **Documentation Tutorials (A)** are best for in-class demos and as advanced reference material.

**For STUDENT LABS / HOMEWORK (Classroom Modules):**

1. **D5 — Bell's Inequality**: First lab. Violate Bell's inequality on real hardware. ⭐
2. **D9 — QKD**: BB84 protocol with eavesdropper detection. ⭐
3. **D8 — Teleportation**: Two experiments, assessment questions included. ⭐
4. **D6 — Deutsch-Jozsa**: Phase kickback, quantum parallelism. Includes Bernstein-Vazirani bonus.
5. **D7 — Grover**: Oracle construction, amplitude amplification on hardware.
6. **D10 — QFT**: Foundation for understanding Shor's.

**For IN-CLASS DEMOS (Documentation Tutorials):**

- **A1 — CHSH**: Advanced Bell violation with Estimator — show the Tsirelson bound plot.
- **A3 — Shor's**: Factor 15 on real hardware. Walk through, don't assign.
- **A7 — Repetition Codes**: QEC lab. Simple enough for homework despite being from docs.
- **A11 — Error Mitigation**: Extract the 4 techniques for hardware lecture.

**Skip for this course:** A4, A5, A6, A8, A9, A10, A12, D1–D4, D12.

### Final Suggested Lecture Plan

| Lecture | Topic | Lab/HW | Qiskit? |
|---------|-------|--------|---------|
| 1 | CNOT, Bell circuit, Intro to Qiskit | **D5 — Bell's Inequality** | **Yes** ⭐ |
| 2 | BB84 / QKD | **D9 — QKD Module** | **Yes** ⭐ |
| 3 | Teleportation + E91 | **D8 — Teleportation** | **Yes** ⭐ |
| 4 | Deutsch & Deutsch-Jozsa | **D6 — DJ Module** | **Yes** |
| 5 | Grover's Algorithm | **D7 — Grover Module** | **Yes** |
| 6 | Quantum Sensing / Metrology | — | Light |
| 7 | Quantum Hardware Deep Dive | A11 concepts (demo) | Demo |
| 8 | QFT + Shor's motivation | **D10 — QFT** + **A3** demo | **Yes** |
| 9 | QEC intro | **A7 — Repetition Codes** | **Yes** |
| 10 | Review + Big Picture | — | — |

**This gives students 8 Qiskit-active lectures out of 10**, with 7 IBM modules/tutorials they can work through on real quantum hardware.
