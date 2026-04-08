# Outline — Lecture 4.4: Quantum Teleportation and Superdense Coding

## Bridge from Lecture 4.3

The QKD lecture ended with quantum repeaters: entanglement swapping = chained teleportation. Students know Bell pairs, Bell measurement (from 4.1), CHSH correlations (4.2), and that entanglement enables secure communication (4.3). They've also seen that long-distance QKD is bottlenecked by fiber loss and that repeaters are the missing piece. Teleportation is what makes repeaters work.

---

## Part 1: The Teleportation Problem (~5 min)

**Goal:** Frame the question sharply before giving the answer.

- Can we transmit a quantum state from Alice to Bob?
- Classical information: just copy and send the bits. Quantum information: no-cloning theorem (4.1) says you can't copy $|\psi\rangle$.
- Alice can't measure $|\psi\rangle$ and send the result — measurement collapses it, losing the amplitudes.
- What if they share an entangled pair? We saw in 4.3 that entanglement lets Alice and Bob do things that classical channels can't. Can it help here?
- **Setup:** Alice has an unknown qubit $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$. Alice and Bob each hold one half of a Bell pair $|\Phi^+\rangle$. Total: 3 qubits.

**iClicker (warm-up):** Alice wants to send Bob an unknown qubit $|\psi\rangle$. Why can't she just measure it and send the result? (A) Measurement is too slow, (B) Measurement collapses the state — she can't recover $\alpha$ and $\beta$ from one measurement, (C) Bob doesn't have a quantum computer, (D) The no-cloning theorem prevents measurement.

---

## Part 2: The Teleportation Protocol (~15 min)

**Goal:** Full step-by-step derivation. This is the heart of the lecture.

### Step-by-step protocol
1. **Shared resource:** Alice and Bob pre-share $|\Phi^+\rangle_{AB} = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$
2. **Initial state:** $|\Psi_0\rangle = |\psi\rangle_C \otimes |\Phi^+\rangle_{AB} = (\alpha|0\rangle + \beta|1\rangle)_C \otimes \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)_{AB}$
3. **Alice's CNOT:** Alice applies CNOT with her unknown qubit (C) as control and her Bell half (A) as target
4. **Alice's Hadamard:** Alice applies H to her unknown qubit (C)
5. **Alice measures** both her qubits (C and A), getting two classical bits $(m_1, m_2)$
6. **Classical communication:** Alice sends $(m_1, m_2)$ to Bob over a classical channel
7. **Bob's correction:** Bob applies the appropriate Pauli gate based on Alice's bits

### Full algebraic derivation
Write out the 3-qubit state after each step. The key algebraic move: rewrite the state in the Bell basis for Alice's two qubits. This separates into four terms, each with a different Pauli acting on Bob's qubit:

$$|\Psi\rangle = \frac{1}{2}\Big[|\Phi^+\rangle(\alpha|0\rangle + \beta|1\rangle) + |\Phi^-\rangle(\alpha|0\rangle - \beta|1\rangle) + |\Psi^+\rangle(\alpha|1\rangle + \beta|0\rangle) + |\Psi^-\rangle(\alpha|1\rangle - \beta|0\rangle)\Big]$$

### Correction table

| Alice measures | Bob's state | Bob applies | Result |
|---------------|-------------|-------------|--------|
| $\|00\rangle$ ($\|\Phi^+\rangle$) | $\alpha\|0\rangle + \beta\|1\rangle$ | $I$ | $\|\psi\rangle$ |
| $\|01\rangle$ ($\|\Psi^+\rangle$) | $\alpha\|1\rangle + \beta\|0\rangle$ | $X$ | $\|\psi\rangle$ |
| $\|10\rangle$ ($\|\Phi^-\rangle$) | $\alpha\|0\rangle - \beta\|1\rangle$ | $Z$ | $\|\psi\rangle$ |
| $\|11\rangle$ ($\|\Psi^-\rangle$) | $\alpha\|1\rangle - \beta\|0\rangle$ | $XZ$ | $\|\psi\rangle$ |

### Circuit diagram
- 3 wires: qubit C (unknown), qubit A (Alice's Bell half), qubit B (Bob's Bell half)
- CNOT from C → A, then H on C, measure C and A
- Classical wires from measurements to controlled-X and controlled-Z on B
- Draw this explicitly; also show as Qiskit circuit

**iClicker (after derivation):** In teleportation, Alice measures her two qubits and gets result (1, 0). What gate must Bob apply? (A) $I$, (B) $X$, (C) $Z$, (D) $XZ$.

**iClicker (conceptual):** After Alice measures but before she sends her classical bits to Bob, what does Bob know about his qubit? (A) It's already in state $|\psi\rangle$, (B) It's in one of four states, equally likely — he doesn't know which, (C) It's still entangled with Alice's qubits, (D) It's in state $|0\rangle$.

---

## Part 3: Why Teleportation Doesn't Break Physics (~10 min)

**Goal:** Address the obvious objections. Students will have them.

### No faster-than-light communication
- Bob's qubit is in a random state until he gets Alice's classical bits
- Without the classical message, Bob's reduced density matrix is $\frac{1}{2}I$ — maximally mixed, no information
- The classical bits travel at most at the speed of light
- Teleportation = entanglement + classical communication. Neither alone suffices.

### No-cloning is respected
- Alice's original qubit is destroyed by her measurement — it collapses into part of a Bell state
- The quantum information moves from Alice to Bob; it's not copied
- "Teleportation" not "replication"

### The e-bit is consumed
- The Bell pair is used up. After teleportation, Alice and Bob are no longer entangled.
- To teleport again, they need a fresh Bell pair.
- Resource accounting: 1 e-bit + 2 classical bits → transmit 1 qubit

### Alice learns nothing
- Alice's measurement outcomes (00, 01, 10, 11) are each equally likely regardless of $|\psi\rangle$
- She gains no information about $\alpha$ and $\beta$

**iClicker:** Does teleportation allow faster-than-light communication? (A) Yes — Bob gets the state instantly, (B) No — Bob needs Alice's classical bits, which travel at light speed or slower, (C) Only if they use more entangled pairs, (D) It depends on the distance.

---

## Part 4: Teleportation in Qiskit (~10 min)

**Goal:** Build and run the circuit. Verify it works.

### Circuit construction
- Build the 3-qubit teleportation circuit
- Use `c_if` / classical conditioning for Bob's corrections
- Alternative: deferred measurement principle — replace classical conditioning with quantum gates (controlled-X and controlled-Z from Alice's qubits to Bob's, then measure everything at the end). Discuss equivalence.

### Verification strategy
- Alice prepares a known state (e.g., $|\psi\rangle = \cos(\pi/8)|0\rangle + \sin(\pi/8)|1\rangle$)
- After teleportation, apply $U^\dagger$ to Bob's qubit, then measure — should get $|0\rangle$ with certainty
- Run on AerSimulator, show histogram
- Try several input states to verify it's not a fluke

### Statevector verification
- Use `Statevector` to inspect Bob's qubit after applying corrections
- Show that it matches the input state exactly

**Code cells:**
1. Build circuit with `QuantumCircuit(3, 2)` — draw it
2. Deferred measurement version — draw it
3. Run both, compare histograms
4. Statevector verification for a specific input

---

## Part 5: Superdense Coding (~10 min)

**Goal:** The dual protocol. Teleportation in reverse.

### Motivation
- Teleportation: 1 e-bit + 2 classical bits → transmit 1 qubit
- Can we go the other way? Use entanglement to boost classical communication?
- **Superdense coding:** 1 e-bit + 1 qubit → transmit 2 classical bits

### The protocol
1. Alice and Bob pre-share $|\Phi^+\rangle$
2. Alice wants to send two classical bits $(b_1, b_2)$
3. Alice applies a Pauli gate to her half based on the message:
   - (0,0) → $I$: state stays $|\Phi^+\rangle$
   - (0,1) → $X$: state becomes $|\Psi^+\rangle$
   - (1,0) → $Z$: state becomes $|\Phi^-\rangle$
   - (1,1) → $XZ$: state becomes $|\Psi^-\rangle$
4. Alice sends her qubit to Bob (quantum channel)
5. Bob performs a **Bell measurement** (CNOT then H, then measure both) to determine which Bell state he has → recovers $(b_1, b_2)$

### Key insight
- The four Bell states are orthogonal → perfectly distinguishable
- Alice's single-qubit Pauli operation maps between all four Bell states
- This is the reverse of the Bell state construction circuit from Lecture 4.1

### Duality table

| | Teleportation | Superdense Coding |
|---|---|---|
| **Transmits** | 1 unknown qubit | 2 classical bits |
| **Consumes** | 1 e-bit + 2 cbits | 1 e-bit + 1 qubit sent |
| **Alice does** | Bell measurement | Pauli encoding |
| **Bob does** | Pauli correction | Bell measurement |
| **Direction** | quantum → classical → quantum | classical → quantum → classical |

**iClicker:** In superdense coding, Alice applies $Z$ to her half of $|\Phi^+\rangle$. The shared state becomes: (A) $|\Phi^+\rangle$, (B) $|\Phi^-\rangle$, (C) $|\Psi^+\rangle$, (D) $|\Psi^-\rangle$.

### Qiskit implementation
- Build superdense coding circuit
- Encode all four messages, show Bob decodes correctly each time
- Compare with teleportation circuit — point out the structural duality

---

## Part 6: Entanglement Swapping and Quantum Repeaters (~5 min)

**Goal:** Close the loop with QKD lecture. Show teleportation is a building block.

### Entanglement swapping
- Alice–Charlie share $|\Phi^+\rangle$. Charlie–Bob share $|\Phi^+\rangle$.
- Charlie does a Bell measurement on his two qubits (one from each pair).
- Result: Alice and Bob are now entangled, even though they never interacted!
- This is just teleportation where the "unknown state" is half of an entangled pair.

### Quantum repeater connection
- Chain together many short entangled links via entanglement swapping
- Each swap extends the entanglement range
- This is how quantum repeaters would work (callback to Lecture 4.3)
- Diagram: A—C₁—C₂—C₃—B, with swaps at each relay

### Quantum internet vision
- Brief mention: a network where any two nodes can share entanglement on demand
- Requires: entangled pair sources, quantum memories, entanglement swapping (teleportation)
- Active area of research — connects to hardware lecture (4.5) coming next

**iClicker (forward-looking):** In entanglement swapping, Charlie performs a Bell measurement on his two qubits. After this, Alice and Bob are entangled even though: (A) they shared a Bell pair all along, (B) Charlie sent them entangled photons, (C) they never directly interacted — the entanglement was "swapped" via Charlie's measurement, (D) this violates the no-cloning theorem.

---

## Summary (~2 min)

Bullet points:
- **Teleportation** transmits an unknown quantum state using 1 e-bit + 2 classical bits. The original is destroyed (no cloning). No FTL.
- **Correction table:** Alice's Bell measurement result → Bob applies $\{I, X, Z, XZ\}$.
- **Superdense coding** is the dual: transmits 2 classical bits using 1 e-bit + 1 qubit.
- **Entanglement swapping** = teleportation of entanglement. This is the building block for quantum repeaters and the future quantum internet.
- **Resource:** an e-bit is consumed in every use — entanglement is a resource, not free.

## What's Next

Lecture 4.5: Quantum Hardware Platforms — how qubits are physically built (superconducting, trapped ions, neutral atoms, photonics) and how the gates we've been using are actually implemented.

---

## Timing Estimate

| Part | Topic | Time |
|------|-------|------|
| 1 | The teleportation problem | 5 min |
| 2 | Protocol + full derivation + circuit | 15 min |
| 3 | Why it doesn't break physics | 10 min |
| 4 | Qiskit implementation | 10 min |
| 5 | Superdense coding | 10 min |
| 6 | Entanglement swapping + repeaters | 5 min |
| — | Summary + iClickers throughout | 5 min |
| **Total** | | **~60 min** |

## iClicker Questions (7 embedded)

1. **(Warm-up, Part 1)** Why can't Alice just measure and send? → measurement collapses
2. **(Part 2, after derivation)** Alice gets (1,0) — Bob applies? → $Z$
3. **(Part 2, conceptual)** Before classical bits arrive, what does Bob know? → one of four equally likely states
4. **(Part 3)** Does teleportation allow FTL? → No, needs classical bits
5. **(Part 5)** $Z$ on $|\Phi^+\rangle$ gives? → $|\Phi^-\rangle$
6. **(Part 6)** Entanglement swapping — why are Alice and Bob entangled? → never interacted, swapped via Charlie
7. **(Reserve)** How many classical bits does Alice send Bob in teleportation? → 2

## Homework Ideas

- **Teleportation with different Bell pairs:** Redo the protocol starting from $|\Psi^-\rangle$ instead of $|\Phi^+\rangle$. Derive the new correction table.
- **Superdense coding circuit:** Build and run in Qiskit. Encode all four messages, verify Bob decodes correctly.
- **Entanglement swapping:** Implement the 4-qubit circuit. Verify Alice and Bob end up entangled (check correlations).
- **Teleportation fidelity with noise:** Add depolarizing noise to the Bell pair. Plot teleportation fidelity vs. noise parameter.
