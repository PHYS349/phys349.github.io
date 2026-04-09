# Problem Bank: Teleportation, Superdense Coding, and Quantum Repeaters

**For PHYS 349 — compiled as a reference to pick from when building homework.**

Each problem is rated on:
- **Difficulty:** 1–5 (1 = straightforward, 5 = challenging)
- **Type:** Qiskit, Analytical, Conceptual, or Mixed
- **Time:** estimated student time
- **Pedagogical value:** what the problem actually teaches

---

## Problem 1: Build and Run the Teleportation Circuit

**Type:** Qiskit | **Difficulty:** 2/5 | **Time:** 30–40 min

Build the standard 3-qubit teleportation circuit in Qiskit. Alice has an unknown state $|\psi\rangle = \cos(\theta/2)|0\rangle + e^{i\phi}\sin(\theta/2)|1\rangle$ on qubit 0. Qubits 1 and 2 share $|\Phi^+\rangle$. Implement: (a) Bell measurement on qubits 0 and 1, (b) classically conditioned X and Z corrections on qubit 2. Use `Statevector` to verify Bob's qubit ends in $|\psi\rangle$ for at least three different input states (e.g., $|0\rangle$, $|+\rangle$, and a random state with $\theta = \pi/3, \phi = \pi/4$).

**Rating:** Great warm-up. Every student should be able to do this. Directly implements the lecture circuit. Low creativity required but builds Qiskit fluency.

---

## Problem 2: Teleportation State Evolution — Step by Step

**Type:** Analytical | **Difficulty:** 2/5 | **Time:** 30 min

Starting from $|\psi\rangle|\Phi^+\rangle$ where $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$, carry out the full teleportation derivation by hand:

(a) Write out the 3-qubit state.
(b) Rewrite Alice's two qubits (0 and 1) in the Bell basis.
(c) Show that each of the four Bell measurement outcomes on Alice's side leaves Bob's qubit in a state related to $|\psi\rangle$ by a known Pauli operation.
(d) Write out the explicit correction for each outcome.

**Rating:** Important for building algebraic fluency with Bell basis decomposition. Somewhat mechanical but forces students to actually do the tensor product algebra. Pairs well with Problem 1.

---

## Problem 3: Teleportation with a Different Bell State

**Type:** Mixed (Analytical + Qiskit) | **Difficulty:** 3/5 | **Time:** 40–50 min

Suppose Alice and Bob share $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$ instead of $|\Phi^+\rangle$.

(a) Rederive the teleportation protocol. What corrections does Bob need for each of Alice's four measurement outcomes?
(b) Implement this modified protocol in Qiskit and verify it works for at least two input states.
(c) In 2–3 sentences: does the choice of shared Bell state affect the probability of success? Does it affect the corrections Bob must apply?

**Rating:** Excellent. Forces students to redo the derivation rather than memorize it. The answer — any Bell state works, only the correction table changes — is a key insight.

---

## Problem 4: What If Alice Measures in the Wrong Basis?

**Type:** Mixed (Analytical + Qiskit) | **Difficulty:** 3/5 | **Time:** 40 min

In the teleportation protocol, Alice performs a Bell measurement (CNOT then Hadamard, then measure in computational basis). What happens if Alice instead measures both her qubits directly in the computational basis (no CNOT, no Hadamard)?

(a) Work out the post-measurement state of Bob's qubit for each of Alice's four possible outcomes.
(b) Can Bob recover $|\psi\rangle$ with any correction? Why or why not?
(c) Verify your answer in Qiskit by computing the fidelity $|\langle\psi|\rho_B|\psi\rangle|$ for a random input state.

**Rating:** Very good. Tests understanding of *why* the Bell measurement is necessary, not just *that* it's used. The answer — Bob gets a random state, teleportation fails completely — drives home that the basis choice matters.

---

## Problem 5: Superdense Coding — Encode and Decode

**Type:** Qiskit | **Difficulty:** 2/5 | **Time:** 30 min

Implement the superdense coding protocol:

(a) Alice and Bob share $|\Phi^+\rangle$. Alice wants to send the 2-bit message $m \in \{00, 01, 10, 11\}$. Implement Alice's encoding (apply the correct Pauli gate to her qubit for each message).
(b) Bob receives Alice's qubit and performs a Bell measurement. Implement this and verify he recovers the correct 2-bit message for all four possibilities.
(c) Run on the simulator for 1024 shots each and show histograms confirming deterministic decoding.

**Rating:** Clean Qiskit exercise. Complements teleportation (dual protocol). Good for building circuit-building confidence.

---

## Problem 6: The Teleportation–Superdense Coding Duality

**Type:** Conceptual | **Difficulty:** 3/5 | **Time:** 20–25 min

Teleportation consumes 1 Bell pair + 2 classical bits to send 1 qubit. Superdense coding consumes 1 Bell pair + 1 qubit to send 2 classical bits.

(a) Fill in a table comparing the two protocols: what is consumed, what is sent, what is received.
(b) In 3–4 sentences, explain why these two protocols are "duals" of each other. What resource trade-off do they illustrate?
(c) Can Alice use teleportation to send 2 classical bits? Can she use superdense coding to send a qubit? Explain.

**Rating:** Good conceptual problem. Forces students to think about the resource accounting rather than just running circuits. Short and clean.

---

## Problem 7: No-Cloning and Teleportation

**Type:** Conceptual | **Difficulty:** 2/5 | **Time:** 15–20 min

(a) State the no-cloning theorem in one sentence.
(b) In the teleportation protocol, Alice's qubit is destroyed by her Bell measurement. Explain why this *must* happen — i.e., why teleportation would violate no-cloning if Alice's state survived.
(c) A student claims: "teleportation copies a quantum state from Alice to Bob." What's wrong with this statement? Fix it.

**Rating:** Quick conceptual check. Important that students internalize why destruction of the original is a feature, not a bug. Could be an exam problem.

---

## Problem 8: Entanglement Swapping — Pen and Paper

**Type:** Analytical | **Difficulty:** 4/5 | **Time:** 45–60 min

Alice holds qubit A, entangled with Charlie's qubit C₁ in state $|\Phi^+\rangle_{AC_1}$. Bob holds qubit B, entangled with Charlie's qubit C₂ in state $|\Phi^+\rangle_{C_2 B}$.

(a) Write the full 4-qubit state $|\Phi^+\rangle_{AC_1} \otimes |\Phi^+\rangle_{C_2 B}$.
(b) Rewrite this state by expanding qubits C₁ and C₂ in the Bell basis.
(c) Show that when Charlie performs a Bell measurement on C₁C₂ and gets outcome $|\Phi^+\rangle$, Alice and Bob are left in state $|\Phi^+\rangle_{AB}$.
(d) What state do Alice and Bob share for each of the four possible outcomes of Charlie's measurement?

**Rating:** The hardest analytical problem on this list. The 4-qubit Bell basis decomposition is non-trivial algebra. Very rewarding for students who complete it — they see entanglement created between particles that never interacted.

---

## Problem 9: Entanglement Swapping in Qiskit

**Type:** Qiskit | **Difficulty:** 3/5 | **Time:** 40–50 min

Implement entanglement swapping on 4 qubits in Qiskit:

(a) Create two Bell pairs: qubits (0,1) and qubits (2,3).
(b) Perform a Bell measurement on qubits 1 and 2 (Charlie's qubits).
(c) Apply classically conditioned corrections to qubit 3.
(d) Verify that qubits 0 and 3 are entangled by measuring correlations in the Z-basis and X-basis (run 1024 shots for each). Show that $\langle ZZ \rangle = +1$ and $\langle XX \rangle = +1$, confirming $|\Phi^+\rangle$.

**Rating:** Excellent. The Qiskit implementation of entanglement swapping is the natural next step after the teleportation circuit. Measuring correlations (ZZ, XX) teaches students how to verify entanglement experimentally.

---

## Problem 10: Quantum Repeater Chain

**Type:** Qiskit | **Difficulty:** 4/5 | **Time:** 50–60 min

Build a 3-hop quantum repeater chain: create Bell pairs across segments (0,1), (2,3), and (4,5). Perform entanglement swapping at the two relay nodes (qubits 1–2 and qubits 3–4) to establish entanglement between qubits 0 and 5.

(a) Implement the full 6-qubit circuit.
(b) Verify end-to-end entanglement between qubits 0 and 5 by measuring ZZ and XX correlations.
(c) In 2–3 sentences: how does this circuit scale? If you wanted entanglement over $N$ segments, how many qubits and how many Bell measurements would you need?

**Rating:** Challenging but very satisfying. Students build a mini quantum internet. The scaling question connects to real quantum repeater engineering.

---

## Problem 11: Teleportation Fidelity with Noise

**Type:** Qiskit | **Difficulty:** 3/5 | **Time:** 40–50 min

Run the teleportation circuit on a noisy simulator (use the same depolarizing noise model from the DJ homework: 1% single-qubit, 2% two-qubit error). Teleport the state $|+\rangle$ using 1000 shots.

(a) Measure Bob's qubit in the X-basis. What fraction of the time does he get the correct outcome $|+\rangle$?
(b) This fraction is the teleportation fidelity. Compare it to the ideal fidelity (1.0) and to the "classical fidelity" (the best you can do without entanglement — for qubits, this is 2/3).
(c) Increase the noise rate to 5% and 10%. Plot fidelity vs. noise rate. At what noise level does teleportation become no better than classical?

**Rating:** Very good. Connects the abstract protocol to real hardware limitations. The 2/3 classical fidelity bound is a key benchmark that students should know.

---

## Problem 12: Can You Teleport Entanglement?

**Type:** Conceptual + Qiskit | **Difficulty:** 4/5 | **Time:** 40 min

Alice has two qubits, A₁ and A₂, in the entangled state $|\Phi^+\rangle_{A_1 A_2}$. She wants to teleport qubit A₂ to Bob using a shared Bell pair.

(a) Before doing anything: which qubits are entangled with which? Draw a diagram.
(b) After teleportation: qubit A₂ is destroyed and Bob has qubit B. Are A₁ and B now entangled? Predict the answer, then verify in Qiskit by building the circuit and measuring ZZ and XX correlations on qubits A₁ and B.
(c) In 2–3 sentences: explain the connection to entanglement swapping.

**Rating:** This is really Problem 9 from a different angle, but the framing ("can you teleport entanglement?") is more engaging and conceptually richer. Good for students who found entanglement swapping confusing in the abstract.

---

## Problem 13: Superdense Coding Capacity

**Type:** Analytical + Conceptual | **Difficulty:** 3/5 | **Time:** 25–30 min

(a) In superdense coding, Alice sends 2 classical bits per qubit. Classically, one bit per qubit is the maximum (Holevo bound). Explain in 2–3 sentences where the "extra" bit comes from.
(b) Alice and Bob share $N$ Bell pairs. What is the maximum number of classical bits Alice can send to Bob using superdense coding? What is the minimum number of qubits she must physically send?
(c) If entanglement were free and unlimited, could Alice send $2N$ classical bits by sending $N$ qubits? Is there a catch?

**Rating:** Good for building intuition about information-theoretic resource counting. Part (c) is subtle — the entanglement isn't free, it takes prior distribution of Bell pairs.

---

## Problem 14: Teleportation Without the Classical Channel

**Type:** Conceptual | **Difficulty:** 2/5 | **Time:** 15 min

Suppose Alice performs her Bell measurement but the classical communication channel is broken — she can't send her 2-bit result to Bob.

(a) What state is Bob's qubit in? (Hint: think about the reduced density matrix.)
(b) Can Bob extract any information about $|\psi\rangle$?
(c) Explain why this means teleportation cannot be used for faster-than-light communication.

**Rating:** Short and clean. Directly addresses the most common student misconception about teleportation. The answer — Bob's qubit is maximally mixed, $\rho_B = I/2$ — is elegant and important.

---

## Problem 15: Bell Measurement Decomposition

**Type:** Qiskit + Analytical | **Difficulty:** 3/5 | **Time:** 35–40 min

The Bell measurement in the standard circuit is: CNOT, then Hadamard on the control qubit, then measure both in the computational basis.

(a) Show analytically that this maps the four Bell states to the four computational basis states:
$|\Phi^+\rangle \to |00\rangle$, $|\Phi^-\rangle \to |10\rangle$, $|\Psi^+\rangle \to |01\rangle$, $|\Psi^-\rangle \to |11\rangle$.
(b) Verify in Qiskit: prepare each Bell state, apply CNOT + H, measure. Show 1024 shots give a single deterministic outcome for each.
(c) Why can't we build a Bell measurement from *only* single-qubit gates and measurements (no entangling gates)? Answer in 2–3 sentences.

**Rating:** Excellent. The Bell measurement circuit is used everywhere but students often treat it as a black box. This forces them to understand why CNOT + H constitutes a basis change.

---

## Problem 16: Teleporting a Gate

**Type:** Mixed (Analytical + Qiskit) | **Difficulty:** 5/5 | **Time:** 60+ min

Gate teleportation: instead of teleporting a state, you can teleport a *gate*. Suppose Alice and Bob share the state $(I \otimes U)|\Phi^+\rangle$, where $U$ is some unitary. Alice has input state $|\psi\rangle$ and performs a Bell measurement on her two qubits.

(a) Show that Bob's qubit ends up in a state of the form $P_i U|\psi\rangle$, where $P_i$ is a Pauli correction that depends on Alice's measurement outcome.
(b) For $U = T$ (the $\pi/8$ gate), work out what correction Bob needs. Is it still a Pauli gate?
(c) Implement gate teleportation in Qiskit for $U = H$ (Hadamard). Verify that Bob's qubit ends in $H|\psi\rangle$ after correction.

**Rating:** Very challenging. This is the concept behind magic state distillation and fault-tolerant quantum computing. Only for the strongest students. Part (b) is where it gets interesting — the T gate correction is *not* a simple Pauli.

---

## Problem 17: Entanglement Monogamy and Teleportation

**Type:** Conceptual | **Difficulty:** 3/5 | **Time:** 20–25 min

Monogamy of entanglement: if qubit A is maximally entangled with qubit B, it cannot be entangled with any other qubit C.

(a) Before teleportation, Alice's qubit 0 is in state $|\psi\rangle$ (not entangled with anything), and qubits 1–2 are in $|\Phi^+\rangle$. Which pairs are entangled?
(b) After Alice's Bell measurement, qubits 0 and 1 are in a Bell state (maximally entangled). By monogamy, what can you conclude about qubit 2's entanglement with qubit 1?
(c) Use monogamy to argue that the entanglement "moved" from the (1,2) pair to the (0,1) pair during teleportation.

**Rating:** Nice conceptual problem. Connects teleportation to a deep property of entanglement (monogamy/CKW inequality). Helps students see teleportation as entanglement redistribution.

---

## Problem 18: Quantum State Tomography on the Teleported Qubit

**Type:** Qiskit | **Difficulty:** 3/5 | **Time:** 45–50 min

Teleport the state $|\psi\rangle = \cos(\pi/8)|0\rangle + \sin(\pi/8)|1\rangle$ from Alice to Bob.

(a) Perform state tomography on Bob's qubit: measure in the X, Y, and Z bases (1000 shots each). Compute $\langle X \rangle$, $\langle Y \rangle$, $\langle Z \rangle$.
(b) From these expectation values, reconstruct the Bloch vector $(r_x, r_y, r_z)$. Plot the expected and measured Bloch vectors.
(c) Repeat with the noisy simulator (1% depolarizing). How does the Bloch vector shrink? What is the purity $\text{Tr}(\rho^2)$ of the noisy teleported state?

**Rating:** Excellent for connecting teleportation to Bloch sphere visualization and measurement theory from Chapter 2. The noisy version shows decoherence concretely.

---

## Problem 19: Classical Teleportation Is Impossible

**Type:** Analytical + Conceptual | **Difficulty:** 3/5 | **Time:** 25–30 min

Suppose Alice tries to "classically teleport" a qubit: she measures her qubit, sends the result (0 or 1) to Bob, and Bob prepares the corresponding state.

(a) If $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$, what is the average fidelity of Bob's reconstructed state over all possible $|\psi\rangle$ on the Bloch sphere? (Hint: average over measurement outcomes and over the uniform distribution of input states. The answer is 2/3.)
(b) Quantum teleportation achieves fidelity 1. The gap from 2/3 to 1 is the "quantum advantage" of teleportation. In 2–3 sentences, explain what makes this possible (what resource is consumed?).
(c) In an experiment, you measure a teleportation fidelity of 0.85. Is this above the classical bound? Can you claim "quantum teleportation" was demonstrated?

**Rating:** Connects theory to experimental benchmarks. The 2/3 bound is standard in quantum optics experiments. Part (c) is what experimentalists actually report in papers.

---

## Problem 20: Design Your Own Quantum Network

**Type:** Mixed (Conceptual + Qiskit) | **Difficulty:** 4/5 | **Time:** 50–60 min

Three cities — Alice (A), Bob (B), and Charlie (C) — are connected in a triangle. Each pair shares one Bell pair: $|\Phi^+\rangle_{AB}$, $|\Phi^+\rangle_{BC}$, $|\Phi^+\rangle_{AC}$.

(a) Alice wants to teleport a qubit to Bob. Which Bell pair does she use? Write the circuit.
(b) After teleportation, Alice has consumed $|\Phi^+\rangle_{AB}$. She now wants to send another qubit to Bob but has no direct Bell pair left. Can she use the remaining Bell pairs ($AC$ and $BC$) plus Charlie as a relay to create a new $AB$ Bell pair via entanglement swapping? Draw the protocol and implement it in Qiskit.
(c) After this round, how many Bell pairs remain? In 2–3 sentences: what is the "cost" of quantum communication in terms of entanglement resources?

**Rating:** Ties together teleportation, entanglement swapping, and resource accounting in a concrete network scenario. Creative problem that asks students to *design* rather than just implement.

---

## Summary Table

| # | Topic | Type | Difficulty | Time | Best For |
|---|-------|------|:----------:|:----:|----------|
| 1 | Teleportation circuit | Qiskit | 2/5 | 30–40 min | Core — everyone should do this |
| 2 | Teleportation derivation | Analytical | 2/5 | 30 min | Algebraic practice, pairs with #1 |
| 3 | Different Bell state | Mixed | 3/5 | 40–50 min | Deepens understanding beyond memorization |
| 4 | Wrong measurement basis | Mixed | 3/5 | 40 min | Tests *why* Bell measurement matters |
| 5 | Superdense coding circuit | Qiskit | 2/5 | 30 min | Core — dual protocol, builds fluency |
| 6 | Teleportation–SDC duality | Conceptual | 3/5 | 20–25 min | Resource accounting, big picture |
| 7 | No-cloning connection | Conceptual | 2/5 | 15–20 min | Quick check, exam-style |
| 8 | Entanglement swapping (pen) | Analytical | 4/5 | 45–60 min | Hardest algebra, very rewarding |
| 9 | Entanglement swapping (Qiskit) | Qiskit | 3/5 | 40–50 min | Core — verifying entanglement via correlations |
| 10 | Repeater chain | Qiskit | 4/5 | 50–60 min | Mini quantum internet, scaling insight |
| 11 | Noisy teleportation | Qiskit | 3/5 | 40–50 min | Connects to real hardware, fidelity benchmarks |
| 12 | Teleporting entanglement | Mixed | 4/5 | 40 min | Alternate framing of ent. swapping |
| 13 | SDC capacity | Analytical | 3/5 | 25–30 min | Information-theoretic reasoning |
| 14 | No classical channel | Conceptual | 2/5 | 15 min | Kills FTL misconception, quick and clean |
| 15 | Bell measurement decomposition | Mixed | 3/5 | 35–40 min | Demystifies the Bell measurement circuit |
| 16 | Gate teleportation | Mixed | 5/5 | 60+ min | Advanced — magic states, fault tolerance |
| 17 | Entanglement monogamy | Conceptual | 3/5 | 20–25 min | Deep conceptual insight |
| 18 | State tomography | Qiskit | 3/5 | 45–50 min | Connects to Bloch sphere, measurement theory |
| 19 | Classical fidelity bound | Analytical | 3/5 | 25–30 min | Experimental benchmarks, 2/3 bound |
| 20 | Quantum network design | Mixed | 4/5 | 50–60 min | Creative, ties everything together |

## Recommended Groupings

**If picking 4–5 for a homework (100 pts):**
- Core circuit skills: #1, #5 (teleportation + superdense coding)
- Deeper understanding: #3 or #4 (forces rederivation)
- Entanglement swapping: #9 (Qiskit verification)
- Conceptual: #7 or #14 (quick, important)
- Noise/real-world: #11 (fidelity with noise)

**If picking 2–3 to combine with DJ homework:**
- #1 (teleportation circuit, 30 min)
- #9 (entanglement swapping, 40 min)
- #14 (no classical channel, 15 min — as a short-answer)
