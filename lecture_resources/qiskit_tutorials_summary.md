# IBM Qiskit Tutorials — Summary for PHYS 349

Compiled for course planning. Each tutorial is rated on difficulty (1–10), estimated time to work through, and relevance to this undergraduate quantum computing course.

---

## 1. CHSH Inequality

**URL:** quantum.cloud.ibm.com/docs/tutorials/chsh-inequality

**Summary:** This tutorial creates a Bell state |Φ⁺⟩ on two qubits and sweeps a parameterized Y-rotation to measure the CHSH witness ⟨S₁⟩ and ⟨S₂⟩ as a function of measurement angle. It uses the Estimator primitive to directly compute expectation values of two-qubit Pauli observables (ZZ, ZX, XZ, XX) without manually reconstructing them from counts. The result is a plot showing the CHSH quantity exceeding the classical bound of ±2, reaching toward the Tsirelson bound ±2√2 — a direct experimental violation of Bell's inequality on real hardware.

**Tools:** Qiskit QuantumCircuit, SparsePauliOp, EstimatorV2, parameterized circuits, generate_preset_pass_manager (optimization level 3), matplotlib plotting. Runs on 127-qubit Heron backend.

**Difficulty:** 3/10. The circuit is just H + CNOT + Ry(θ). The Estimator handles the hard part. Students need to understand Bell states, Pauli observables, and expectation values — all of which they've learned.

**Time:** ~1–1.5 hours to work through, including understanding the theory and running on hardware (~2 min QPU time).

**Course relevance:** 10/10. Maps directly to lecture 03_04 (Bell's theorem). Students would be running exactly the experiment that won the 2022 Nobel Prize. Perfect first Qiskit exercise.

---

## 2. Grover's Algorithm

**URL:** quantum.cloud.ibm.com/docs/tutorials/grovers-algorithm

**Summary:** Implements Grover's search algorithm on 3 qubits, marking two states ("011" and "100") out of 8. The tutorial builds a custom oracle using multi-controlled Z gates with X-gate sandwiching for open controls, then uses Qiskit's built-in `grover_operator()` to construct the full amplification circuit. The optimal number of iterations is calculated analytically. Output is a probability distribution showing the marked states with high probability.

**Tools:** Qiskit QuantumCircuit, grover_operator from circuit library, MCMTGate, ZGate, SamplerV2, generate_preset_pass_manager, plot_distribution. Runs on 127-qubit Eagle backend.

**Difficulty:** 5/10. The oracle construction (multi-controlled Z with open controls) requires some thought. The grover_operator library call simplifies the amplification step. Students need to understand superposition, interference, and the oracle concept.

**Time:** ~1.5–2 hours. The oracle-building logic takes time to digest. Hardware execution is under 1 minute.

**Course relevance:** 9/10. Grover's algorithm is a canonical quantum algorithm with a clean √N speedup story. Excellent for demonstrating "quantum computing = engineering interference." Could be a capstone Qiskit exercise.

---

## 3. Shor's Algorithm

**URL:** quantum.cloud.ibm.com/docs/tutorials/shors-algorithm

**Summary:** Full implementation of Shor's algorithm to factor N=15 using a=2. Constructs the order-finding circuit from quantum phase estimation: 8 control qubits, 4 target qubits, controlled modular multiplication operators (M₂, M₄) built from SWAP gates, inverse QFT, and measurement. The tutorial manually constructs M_a operators as permutation matrices, compares manual vs. automated synthesis (showing manual is far shallower: 9 vs. 96 two-qubit gates), runs on real hardware with dynamical decoupling and gate twirling, then completes the classical post-processing (continued fractions to extract the order r=4, then gcd to find factor 3).

**Tools:** Qiskit QuantumCircuit, QFT from circuit library, UnitaryGate, SamplerV2, CouplingMap, dynamical decoupling, gate twirling, continued fractions (Python Fraction), gcd. Transpiled circuit has 260 CZ gates and 2q-depth of 187.

**Difficulty:** 8/10. This is a substantial tutorial. The number theory (modular arithmetic, order finding, continued fractions), phase estimation protocol, and circuit construction are all non-trivial. Understanding *why* it works requires significant mathematical background.

**Time:** ~3–4 hours to fully work through and understand. Hardware execution is a few seconds but the conceptual depth is the bottleneck.

**Course relevance:** 6/10. Too advanced to teach in full at this level, but the *idea* of Shor's is important for motivation (breaking RSA). Best used as a "look what's possible" demo — show the circuit, run it, explain the punchline without deriving all the number theory. The tutorial itself even says RSA-scale factoring needs millions of qubits with error correction.

---

## 4. Ground State Energy with VQE (Heisenberg Chain)

**URL:** quantum.cloud.ibm.com/docs/tutorials/spin-chain-vqe

**Summary:** Implements the Variational Quantum Eigensolver (VQE) to estimate the ground state energy of a 10-spin Heisenberg chain. The Hamiltonian includes XX, YY, ZZ couplings on device-native connectivity plus random Z fields. Uses an EfficientSU2 ansatz with 3 repetitions, transpiled with dynamical decoupling. The classical optimizer (COBYLA) iteratively adjusts circuit parameters to minimize the energy expectation value computed by the Estimator primitive. Also demonstrates deployment via Qiskit Serverless.

**Tools:** Qiskit efficient_su2, SparsePauliOp, EstimatorV2 with Sessions, generate_preset_pass_manager, dynamical decoupling (ALAPScheduleAnalysis, PadDynamicalDecoupling), scipy.optimize.minimize (COBYLA), QiskitServerless for cloud deployment.

**Difficulty:** 7/10. Requires understanding variational algorithms (hybrid quantum-classical loop), Hamiltonians as sums of Pauli strings, parameterized circuits, and classical optimization. The Heisenberg model itself connects to their Ising model background from 01_03.

**Time:** ~2–3 hours. The optimization loop and serverless deployment add complexity beyond just circuit building.

**Course relevance:** 5/10. VQE is important for NISQ-era quantum computing, and the Heisenberg chain connects to physics they know. But the variational optimization machinery (ansatz design, classical optimizers, cost landscapes) might be too much to teach properly in one lecture. Best as a demo or advanced homework.

---

## 5. Introduction to Fractional Gates

**URL:** quantum.cloud.ibm.com/docs/tutorials/fractional-gates

**Summary:** Demonstrates fractional (parameterized) RZZ(θ) and RX(θ) gates natively supported on IBM Heron processors, which eliminate the need to decompose arbitrary rotations into multiple basis gates. The tutorial uses quantum kernel circuits as a testbed, comparing transpiled circuit depth and fidelity with and without fractional gates. Shows ~2x reduction in non-local gate count and significant fidelity improvement at larger qubit counts (4–40 qubits). Covers the FoldRzzAngle transpiler pass and constraint that RZZ angles must be in (0, π/2].

**Tools:** Qiskit QuantumCircuit, ParameterVector, unitary_overlap, SamplerV2, generate_preset_pass_manager, use_fractional_gates backend option, FoldRzzAngle pass, qiskit_basis_constructor plugin.

**Difficulty:** 7/10. The fractional gate concept is simple, but the workflow details (angle constraints, transpilation ordering, FoldRzzAngle) are fiddly. The quantum kernel application adds ML complexity.

**Time:** ~2 hours. Much of the time is in understanding the transpilation workflow differences.

**Course relevance:** 3/10. Too specialized for this course. The concept that hardware has native gates and circuits must be compiled is worth mentioning in a hardware lecture, but the details of fractional gate workflows are engineer-level, not pedagogical.

---

## 6. Long-Range Entanglement with Dynamic Circuits

**URL:** quantum.cloud.ibm.com/docs/tutorials/long-range-entanglement

**Summary:** Implements a long-range controlled-X (LRCX) gate using mid-circuit measurement and feedforward, based on Bäumer et al. (2023). Instead of SWAP chains (linear depth), this creates Bell pairs along a chain of ancilla qubits, performs Bell measurements, and applies classically conditioned Pauli corrections — achieving constant circuit depth regardless of qubit separation. The tutorial compares dynamic vs. unitary implementations across distances 0–60 qubits, showing the dynamic approach maintains constant 2-qubit depth while unitary depth grows linearly. On current hardware, fidelity crossover occurs around distance ~20.

**Tools:** Qiskit QuantumCircuit with if_test (classical expressions, expr.bit_xor), mid-circuit measurement (MidCircuitMeasure), SamplerV2 with Batch mode, dynamical decoupling (XY4), Layer Fidelity for qubit selection, Bell state fidelity calculation F = (1 + ⟨XX⟩ - ⟨YY⟩ + ⟨ZZ⟩)/4.

**Difficulty:** 8/10. The teleportation-based gate implementation requires deep understanding of Bell measurements, Pauli corrections, and classical feedforward. The circuit construction code is complex.

**Time:** ~3–4 hours including understanding the protocol and analyzing results.

**Course relevance:** 4/10. The *concept* of gate teleportation is beautiful and connects to Bell states and measurement. But the implementation details are too advanced. Could extract the core idea (entanglement swapping, teleportation) for a lecture, pointing to this tutorial for motivated students.

---

## 7. Repetition Codes (Bit-Flip Error Correction)

**URL:** quantum.cloud.ibm.com/docs/tutorials/repetition-codes

**Summary:** Implements the simplest quantum error correction code: the 3-qubit bit-flip code. Encodes a logical |1⟩ into 3 physical qubits using CNOT gates, measures two stabilizer ancillas (parity checks) via mid-circuit measurement, applies conditional X corrections based on the syndrome, then measures the data qubits. Compares corrected vs. uncorrected results, demonstrating that error correction reduces final parity errors (from ~120/1000 to ~100/1000 trials). Uses dynamic circuits with if_test for conditional correction gates.

**Tools:** Qiskit QuantumCircuit, QuantumRegister, ClassicalRegister, MidCircuitMeasure, if_test for conditional gates, SamplerV2, generate_preset_pass_manager. 5 qubits total (3 data + 2 ancilla).

**Difficulty:** 4/10. The bit-flip code is conceptually clean. The stabilizer measurement and syndrome decoding are straightforward. Mid-circuit measurement adds a small layer of complexity but the circuit is small.

**Time:** ~1–1.5 hours. The circuit is only 5 qubits and the logic is clear.

**Course relevance:** 8/10. Excellent introduction to quantum error correction. Students can see the full encode → detect → correct → verify pipeline. Connects to no-cloning (why you can't just copy), decoherence (from 01_04 and 02_07), and mid-circuit measurement. The modest improvement on hardware is itself pedagogically valuable — it shows QEC is real but current hardware is noisy.

---

## 8. Benchmark Dynamic Circuits with Cut Bell Pairs

**URL:** quantum.cloud.ibm.com/docs/tutorials/edc-cut-bell-pair-benchmarking

**Summary:** Benchmarks the quality of entanglement teleportation across an entire quantum processor by running a 4-qubit teleportation circuit on every possible chain of 4 connected qubits. Creates a Bell pair on the middle two qubits, teleports it to the edge qubits via mid-circuit measurement and feedforward, then measures ZZ and XX stabilizers to compute Bell state fidelity. Results show over an order of magnitude variation in fidelity across different qubit groups on the same chip, demonstrating that hardware quality is highly position-dependent.

**Tools:** Qiskit QuantumCircuit with if_test, MidCircuitMeasure, SamplerV2, generate_preset_pass_manager, coupling map analysis, MSE computation, CDF plotting. Runs 16-qubit parallel experiments across the full 156-qubit device.

**Difficulty:** 7/10. The teleportation circuit itself is standard, but the benchmarking infrastructure (finding all 4-qubit chains, parallel execution, MSE analysis, CDF plotting) adds engineering complexity.

**Time:** ~2–3 hours including analysis.

**Course relevance:** 4/10. The *concept* that hardware is heterogeneous and some qubits are better than others is worth mentioning in a hardware lecture. But the benchmarking workflow is too specialized. Better as a reference for understanding IBM hardware than as a teaching tool.

---

## 9. Transpilation Optimizations with SABRE

**URL:** quantum.cloud.ibm.com/docs/tutorials/transpilation-optimizations-with-sabre

**Summary:** Demonstrates how to optimize quantum circuit transpilation using the SABRE algorithm (SWAP-Based Bidirectional heuristic search). Covers two fundamental transpilation stages: qubit layout (mapping logical to physical qubits) and gate routing (inserting SWAP gates for connectivity). Shows how to tune SABRE parameters (swap_trials, layout_trials, max_iterations) to reduce circuit depth for 100+ qubit circuits. Part II explores parallel transpilation via Qiskit Serverless.

**Tools:** Qiskit SabreLayout, SabreSwap transpiler passes, generate_preset_pass_manager, EstimatorV2, Qiskit Serverless for parallel execution.

**Difficulty:** 6/10. Requires understanding of device connectivity, coupling maps, and why transpilation matters. The SABRE algorithm details are somewhat advanced.

**Time:** ~2 hours.

**Course relevance:** 4/10. The *concept* of transpilation (your abstract circuit must be compiled to hardware-native gates + connectivity) is important for a hardware lecture. But the SABRE parameter tuning is too specialized. Better to mention transpilation exists and show a before/after circuit than to work through this tutorial.

---

## 10. Quantum Approximate Optimization Algorithm (QAOA)

**URL:** quantum.cloud.ibm.com/docs/tutorials/quantum-approximate-optimization-algorithm

**Summary:** Implements QAOA to solve the Max-Cut graph problem at two scales: 5-node (small) and 100-node (utility scale). Maps the Max-Cut problem to a quantum Hamiltonian via QUBO notation, constructs parameterized QAOA circuits with alternating cost and mixer layers, optimizes parameters classically (COBYLA), and post-processes measurement results to extract the best cut. The utility-scale version runs on a Heron r3 processor with dynamical decoupling and twirling.

**Tools:** Qiskit QuantumCircuit, SparsePauliOp, EstimatorV2/SamplerV2 with Sessions, RustWorkX for graphs, scipy.optimize, generate_preset_pass_manager, dynamical decoupling, gate twirling.

**Difficulty:** 7/10. Requires understanding variational algorithms, graph problems, Hamiltonian encoding, and classical optimization. The mapping from Max-Cut to Pauli operators is non-trivial.

**Time:** ~2.5–3 hours for the full tutorial including the utility-scale section.

**Course relevance:** 5/10. QAOA connects to the Ising model motivation from 01_03 and is a major NISQ algorithm. The Max-Cut problem is intuitive. But the variational machinery and Hamiltonian encoding are significant overhead for undergrads. Could work as a high-level demo: "here's how you'd use a quantum computer to solve an optimization problem."

---

## 11. Combine Error Mitigation Options with the Estimator

**URL:** quantum.cloud.ibm.com/docs/tutorials/combine-error-mitigation-techniques

**Summary:** Progressive walkthrough of four error mitigation techniques, applied cumulatively to show their individual and combined effects on expectation value accuracy. Starts with no mitigation, then adds: (1) dynamical decoupling (suppresses decoherence during idle times), (2) measurement error mitigation / TREX (corrects readout errors), (3) gate twirling (randomizes gate errors into Pauli channels), and (4) zero-noise extrapolation / ZNE (extrapolates to zero-noise limit). Tested on 10-qubit and 50-qubit circuits.

**Tools:** Qiskit QuantumCircuit, SparsePauliOp, EstimatorV2 with various options (dynamical_decoupling, twirling, resilience_level, zne), generate_preset_pass_manager.

**Difficulty:** 6/10. Each individual technique is understandable conceptually, but the combination and the details of configuring Estimator options require familiarity with the Qiskit Runtime API.

**Time:** ~2 hours.

**Course relevance:** 5/10. Error mitigation is important for understanding why NISQ results are noisy and what people do about it. The progressive structure is pedagogically nice. Could extract the key ideas (DD, readout correction, twirling, ZNE) for a hardware lecture without requiring students to run the full tutorial.

---

## 12. Quantum Kernel Training

**URL:** quantum.cloud.ibm.com/docs/tutorials/quantum-kernel-training

**Summary:** Demonstrates quantum kernel methods for binary classification — using quantum circuits to compute similarity measures (kernel matrix entries) between classical data points, which are then fed to classical ML algorithms. Constructs a ZZ feature map circuit, computes inner products via the Sampler primitive, and evaluates the kernel on real hardware. Also demonstrates Qiskit Serverless deployment for cloud execution.

**Tools:** Qiskit QuantumCircuit, ParameterVector, SamplerV2, generate_preset_pass_manager, QiskitServerless, pandas, numpy.

**Difficulty:** 7/10. Requires understanding of both quantum circuits and machine learning concepts (kernels, feature maps, classification). The intersection of QC and ML adds conceptual overhead.

**Time:** ~2 hours.

**Course relevance:** 3/10. Quantum ML is trendy but peripheral to the core QC curriculum. The feature map circuit itself is interesting (it's just parameterized rotations), but the ML framing would require teaching kernel methods, which is outside scope. Could mention as a "quantum computing applications" bullet point.

---

## Summary Table

| # | Tutorial | Difficulty | Time | Course Relevance | Best Use |
|---|----------|-----------|------|-----------------|----------|
| 1 | CHSH Inequality | 3/10 | 1–1.5 hr | **10/10** | First Qiskit lab — violate Bell's inequality |
| 2 | Grover's Algorithm | 5/10 | 1.5–2 hr | **9/10** | Capstone algorithm lab |
| 3 | Shor's Algorithm | 8/10 | 3–4 hr | 6/10 | Demo/motivation only |
| 4 | VQE (Heisenberg) | 7/10 | 2–3 hr | 5/10 | Advanced demo of NISQ algorithm |
| 5 | Fractional Gates | 7/10 | 2 hr | 3/10 | Skip for this course |
| 6 | Long-Range Entanglement | 8/10 | 3–4 hr | 4/10 | Extract teleportation concept only |
| 7 | Repetition Codes | 4/10 | 1–1.5 hr | **8/10** | QEC intro lab |
| 8 | Cut Bell Pair Benchmark | 7/10 | 2–3 hr | 4/10 | Hardware heterogeneity demo |
| 9 | SABRE Transpilation | 6/10 | 2 hr | 4/10 | Mention in hardware lecture |
| 10 | QAOA (Max-Cut) | 7/10 | 2.5–3 hr | 5/10 | Optimization demo, connects to Ising |
| 11 | Error Mitigation | 6/10 | 2 hr | 5/10 | Extract concepts for hardware lecture |
| 12 | Quantum Kernel Training | 7/10 | 2 hr | 3/10 | Skip for this course |

## Recommended for PHYS 349 (in order of priority)

1. **CHSH Inequality** — First Qiskit exercise, directly after Bell's theorem
2. **Grover's Algorithm** — Main algorithm they implement and understand deeply
3. **Repetition Codes** — Accessible QEC introduction, dynamic circuits
4. **Shor's Algorithm** — Show as demo, don't require full understanding
5. **QAOA / VQE** — Pick one as a "real-world application" demo
