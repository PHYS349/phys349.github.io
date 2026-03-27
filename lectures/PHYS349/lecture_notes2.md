# Lecture Notes 2 — IBM Classroom Modules (Computer Science)

These are from the standalone IBM Quantum Learning classroom modules designed for undergraduate use. Each has Qiskit code, assessment questions, and instructor answer keys.

---

## Module: The Deutsch-Jozsa Algorithm

Source: quantum.cloud.ibm.com/learning/en/modules/computer-science/deutsch-jozsa

**Core "proof of concept" quantum algorithm. First demonstration of exponential quantum speedup (over deterministic classical).**

### Pedagogical Flow

1. Quantum parallelism and its limits (superposition evaluates all inputs, but measurement collapses)
2. Two-bit function example (concrete, 4 possible functions)
3. Deutsch's algorithm (single-qubit version, historical: Deutsch 1985)
4. Deutsch-Jozsa algorithm (n-qubit generalization, Deutsch-Jozsa 1992)
5. Bernstein-Vazirani problem (variant using same circuit)

### Problem Definition

**Deutsch-Jozsa:** Given $f: \{0,1\}^n \to \{0,1\}$ promised to be either *constant* (same output for all inputs) or *balanced* (returns 0 for half, 1 for half). Determine which.

| Strategy | Queries needed |
|----------|---------------|
| Classical deterministic | $2^{n-1} + 1$ (worst case) |
| Classical probabilistic | 2 (high probability) |
| **Quantum** | **1** |

**Bernstein-Vazirani:** Given $f(x) = s \cdot x \pmod{2}$ for secret string $s$. Find $s$.

| Strategy | Queries needed |
|----------|---------------|
| Classical | $n$ |
| **Quantum** | **1** |

### Key Concepts Introduced

- **Quantum parallelism** — superposition evaluates $f$ on all inputs simultaneously
- **Phase kickback** — oracle converts function output into phase: $U_f|x\rangle|-\rangle = (-1)^{f(x)}|x\rangle|-\rangle$
- **Interference** — final Hadamard layer converts phase information into measurable bit values
- **Oracle / black box** — function evaluation treated as a gate $U_f$

### The Algorithm — Mathematical Derivation

**Oracle operation:**

$$U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$$

**Phase kickback trick:** When output qubit is in $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$:

$$U_f|x\rangle|-\rangle = (-1)^{f(x)}|x\rangle|-\rangle$$

The function output is encoded as a **phase** on the input register.

**Initial state (after X on output qubit, then Hadamards on all):**

$$|\Psi\rangle = |-\rangle \otimes \frac{1}{\sqrt{2^n}}\sum_{x \in \{0,1\}^n}|x\rangle$$

**After oracle:**

$$|\Psi\rangle = |-\rangle \otimes \frac{1}{\sqrt{2^n}}\sum_{x \in \{0,1\}^n}(-1)^{f(x)}|x\rangle$$

**Key Hadamard identity:**

$$H^{\otimes n}|x\rangle = \frac{1}{\sqrt{2^n}}\sum_{y \in \{0,1\}^n}(-1)^{x \cdot y}|y\rangle$$

**After final Hadamards on input register:**

- If $f$ is **constant**: all phases are equal, interference produces $|0^n\rangle$ with certainty
- If $f$ is **balanced**: phases cancel, probability of measuring $|0^n\rangle$ is exactly 0

**For Bernstein-Vazirani** ($f(x) = s \cdot x$): the final state is simply $|s\rangle$ — the secret string directly.

### Deutsch's Algorithm (n=1 case) — Step by Step

**State $|\pi_1\rangle$ (after first Hadamards):**

$$|\pi_1\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}} \otimes \frac{|0\rangle + |1\rangle}{\sqrt{2}}$$

**State $|\pi_2\rangle$ (after oracle):**

$$|\pi_2\rangle = \begin{cases}
\pm\frac{|0\rangle - |1\rangle}{\sqrt{2}} \otimes \frac{|0\rangle + |1\rangle}{\sqrt{2}} & \text{if } f(0) = f(1) \text{ (constant)} \\
\pm\frac{|0\rangle - |1\rangle}{\sqrt{2}} \otimes \frac{|0\rangle - |1\rangle}{\sqrt{2}} & \text{if } f(0) \neq f(1) \text{ (balanced)}
\end{cases}$$

**State $|\pi_3\rangle$ (after final Hadamard on input):**

$$|\pi_3\rangle = \begin{cases}
\pm|-\rangle \otimes |0\rangle & \text{constant} \\
\pm|-\rangle \otimes |1\rangle & \text{balanced}
\end{cases}$$

Measure input qubit: 0 → constant, 1 → balanced. **One query.**

### Qiskit Implementation

**Two-bit function generator:**
```python
from qiskit import QuantumCircuit

def twobit_function(case: int):
    f = QuantumCircuit(2)
    if case in [2, 3]:
        f.cx(0, 1)
    if case in [3, 4]:
        f.x(1)
    return f
```

**Deutsch's algorithm circuit:**
```python
qc = QuantumCircuit(2, 1)
qc.x(1)              # Output qubit to |1⟩
qc.h(range(2))       # Hadamards on both
qc.barrier()
qc.compose(twobit_function(2), inplace=True)  # Oracle
qc.barrier()
qc.h(0)              # Final Hadamard on input
qc.measure(0, 0)     # Measure input qubit
```

**D-J oracle (random constant/balanced):**
```python
import numpy as np

def dj_function(num_qubits):
    qc = QuantumCircuit(num_qubits + 1)
    if np.random.randint(0, 2):
        qc.x(num_qubits)  # Flip output
    if np.random.randint(0, 2):
        return qc  # Constant
    # Balanced: apply controlled-X for random subset of inputs
    on_states = np.random.choice(
        range(2**num_qubits), 2**num_qubits // 2, replace=False
    )
    for state in on_states:
        bit_string = f"{state:0b}"
        zero_inds = [q for q, bit in enumerate(reversed(bit_string)) if bit == "1"]
        qc.x(zero_inds)  # Not shown: full add_cx helper
        qc.mcx(list(range(num_qubits)), num_qubits)
        qc.x(zero_inds)
    return qc
```

**Full D-J algorithm:**
```python
n = 3
oracle = dj_function(n)

qc = QuantumCircuit(n + 1, n)
qc.x(n)              # Output qubit → |1⟩
qc.h(range(n + 1))   # Hadamards on all
qc.barrier()
qc.compose(oracle, inplace=True)
qc.barrier()
qc.h(range(n))       # Final Hadamards on input register
qc.measure(range(n), range(n))

# Result: all zeros → constant; any nonzero → balanced
```

**Bernstein-Vazirani oracle:**
```python
def bv_function(s):
    qc = QuantumCircuit(len(s) + 1)
    for index, bit in enumerate(reversed(s)):
        if bit == "1":
            qc.cx(index, len(s))
    return qc
```

**Execution pattern (all algorithms):**
```python
from qiskit.transpiler.preset_passmanagers import generate_preset_pass_manager
from qiskit_ibm_runtime import SamplerV2 as Sampler

pm = generate_preset_pass_manager(target=backend.target, optimization_level=3)
qc_isa = pm.run(qc)
job = sampler.run([qc_isa], shots=1)
counts = job.result()[0].data.c.get_counts()
```

### Gates and Classes Used

**Gates:** H, X, CX (CNOT), MCX (multi-controlled X)
**Classes:** QuantumCircuit, SamplerV2, BackendSamplerV2, AerSimulator, NoiseModel, QiskitRuntimeService
**Key methods:** `.compose()`, `.barrier()`, `.to_gate()`, `.decompose()`, `generate_preset_pass_manager()`, `plot_histogram()`

### Assessment Questions

**Check-your-understanding:**
1. How many runs needed for quantum parallelism to learn $f$? (Answer: ≥2 due to measurement collapse — no better than classical)
2. Classical queries for 100% certainty on D-J? (Answer: $2^{n-1} + 1$)
3. Is Bernstein-Vazirani a special case of D-J? (Answer: Yes — if $s = 0^n$ the function is constant, otherwise balanced)

**True/False:**
- D-J is a special case of Deutsch's algorithm with single qubit input (TRUE)
- D-J requires multiple function evaluations (FALSE — only 1)

**Challenge:** Work out $|\pi_1\rangle, |\pi_2\rangle, |\pi_3\rangle$ for $n=2$ case.

### Pedagogical Notes for PHYS 349

**What's new:** Phase kickback, oracle model, interference as computational tool, exponential speedup concept. This is the first "real quantum algorithm" students will see.

**What students already know:** Hadamard, CNOT, measurement, tensor products. The D-J algorithm is essentially: "apply what you already know in a clever order."

**Good exercises:** Have students trace the state vector through each step for $n=1$ and $n=2$. The algebra is tractable and the "aha" moment when interference produces the answer is powerful.

---

## Module: Quantum Key Distribution (BB84)

Source: quantum.cloud.ibm.com/learning/en/modules/computer-science/quantum-key-distribution

**Complete QKD protocol with classical crypto motivation, eavesdropping detection, and real hardware demo. Excellent standalone lecture.**

### Pedagogical Flow

1. Classical cryptography (replacement ciphers, Caesar cipher, one-time pad)
2. The key distribution problem
3. Quantum states and two bases (Z and X)
4. BB84 Step 1: Alice's state preparation
5. BB84 Step 2: Bob's measurement
6. BB84 Step 3: Public basis comparison, key sifting
7. BB84 Step 4: Verification and secret sharing
8. Eavesdropping detection (Eve's attack)
9. Caveats (secure channels, privacy amplification)
10. Qiskit implementation (simulator and 127-qubit real hardware)

### Classical Cryptography Background

**One-Time Pad (OTP):** Theoretically unbreakable if key is random, as long as the message, and used only once.

$$\text{Encrypted} = (\text{message} + \text{key}) \bmod 26$$
$$\text{Decrypted} = (\text{encrypted} - \text{key}) \bmod 26$$

**The problem:** How to securely share the key? This is where quantum mechanics helps.

### The BB84 Protocol

**Alice's state preparation table:**

| Basis | bit = 0 | bit = 1 |
|-------|---------|---------|
| Z | $\|0\rangle$ | $\|1\rangle$ |
| X | $\|+\rangle = \frac{1}{\sqrt{2}}(\|0\rangle + \|1\rangle)$ | $\|-\rangle = \frac{1}{\sqrt{2}}(\|0\rangle - \|1\rangle)$ |

**Step 1:** Alice generates random bits and random bases, prepares qubits accordingly.

**Step 2:** Bob generates random measurement bases, measures each qubit.
- Matching basis → deterministic outcome (correct bit)
- Mismatched basis → random outcome (50/50)

**Step 3:** Alice and Bob publicly announce bases (NOT bit values). Keep only rounds where bases matched. Expected: ~50% of qubits retained.

**Step 4:** Sacrifice subset of sifted key for verification. Should have ~100% agreement (no eavesdropper). Remaining bits become the shared secret key.

### Eavesdropping Detection

**Eve's dilemma:**
1. Doesn't know Alice's bases → must guess randomly
2. Can't copy quantum states (no-cloning theorem)
3. Her measurements perturb the states

**Error analysis with Eve present:**
- Eve uses wrong basis: 50% of the time
- When Eve uses wrong basis, she sends wrong state: 50% of the time
- Bob then gets wrong result: 50% of that subset
- **Total error rate:** $0.5 \times 0.5 \times 0.5 = 12.5\%$

**Detection:** Without Eve: fidelity ≈ 100%. With Eve: fidelity ≈ 75%. The gap is clearly detectable.

**Real hardware results (127 qubits on ibm_brisbane):**
- No eavesdropper: ~97% fidelity (3% quantum noise)
- With eavesdropper: ~76% fidelity — clearly distinguishable

### Qiskit Implementation

**Alice's state preparation:**
```python
import numpy as np
from qiskit import QuantumCircuit

rng = np.random.default_rng()
bit_num = 20

qc = QuantumCircuit(bit_num, bit_num)
abits = np.round(rng.random(bit_num))   # Random bits
abase = np.round(rng.random(bit_num))   # Random bases (0=Z, 1=X)

for n in range(bit_num):
    if abits[n] == 0:
        if abase[n] == 1:
            qc.h(n)       # |+⟩
    if abits[n] == 1:
        if abase[n] == 0:
            qc.x(n)       # |1⟩
        if abase[n] == 1:
            qc.x(n)
            qc.h(n)       # |−⟩

qc.barrier()
```

**Bob's measurement:**
```python
bbase = np.round(rng.random(bit_num))

for m in range(bit_num):
    if bbase[m] == 1:
        qc.h(m)           # Rotate to X-basis before measuring
    qc.measure(m, m)
```

**Key sifting and fidelity calculation:**
```python
agoodbits = []
bgoodbits = []
match_count = 0

for n in range(bit_num):
    if abase[n] == bbase[n]:       # Bases matched
        agoodbits.append(int(abits[n]))
        bgoodbits.append(bbits[n])
        if int(abits[n]) == bbits[n]:
            match_count += 1

fidelity = match_count / len(agoodbits)
loss = 1 - fidelity
```

**Eve's eavesdropping (intercept-resend attack):**
```python
# Eve measures in random basis
ebase = np.round(rng.random(bit_num))
for m in range(bit_num):
    if ebase[m] == 1:
        qc.h(m)
    qc.measure(qr[m], cr[m])

# Eve re-prepares based on her measurements
# (second circuit with Eve's bits and bases)
# Bob then measures Eve's re-prepared states
```

### Gates and Methods

**Gates:** H, X only (elegantly simple protocol!)
**Key pattern:** H before measurement = measure in X-basis
**New methods:** `np.random.default_rng()`, little-endian bit reversal for results

### Assessment Questions

**True/False:**
1. Alice and Bob measure in the same basis (FALSE — random, match ~50%)
2. Eavesdropper prevented from copying by no-cloning theorem (TRUE)

**Multiple Choice:**
- Usable key fraction without Eve? (~50%)
- Agreement rate when bases match, no Eve? (100%)
- Agreement rate when bases match, with Eve? (~75%)

**Discussion:** Convince your partner that 12.5% of all qubits will show mismatches when Eve eavesdrops. (Hint: $0.5 \times 0.5 \times 0.5 = 0.125$)

### Pedagogical Notes for PHYS 349

**What's new:** Complete cryptographic protocol, one-time pad, eavesdropping detection, privacy amplification concept.

**What students know:** Z and X bases, measurement collapse, no-cloning (from lecture_notes.md). This module puts it all together into a real-world application.

**Strong connection to:** No-cloning theorem (just covered), non-distinguishability of non-orthogonal states, Bell inequality (already covered in 03_04).

**In-class activity:** The OTP encryption exercise (pair up, share 4-letter key, encrypt/decrypt) is excellent for engagement before the quantum part.

---

## Module: Quantum Teleportation

Source: quantum.cloud.ibm.com/learning/en/modules/computer-science/quantum-teleportation

**Full teleportation protocol with step-by-step derivation, Qiskit implementation, and long-distance extension using SWAP chains.**

### Pedagogical Flow

1. Background: qubits, superposition, entanglement
2. Quantum gates needed: H, CNOT, X, Z (with matrices)
3. Theory: step-by-step mathematical derivation
4. Experiment 1: basic teleportation on hardware
5. Experiment 2: long-distance teleportation via SWAP gates
6. Assessment

### The Protocol — Full Derivation

**Setup:** Three qubits — Q (Alice's secret), A (Alice's half of e-bit), B (Bob's half of e-bit).

**Step 1 — Create e-bit:**

$$\text{CNOT}(A,B) \cdot H_A |0\rangle_B|0\rangle_A = \frac{1}{\sqrt{2}}(|0\rangle_B|0\rangle_A + |1\rangle_B|1\rangle_A)$$

**Step 2 — Secret state:**

$$|\psi\rangle_Q = \alpha_0|0\rangle_Q + \alpha_1|1\rangle_Q$$

**Step 3 — Alice's operations (CNOT then H):**

$$H_Q \cdot \text{CNOT}(Q,A) \text{ applied to full 3-qubit state}$$

**Result after Alice's gates:**

$$\frac{1}{2}\bigl[(\alpha_0|0\rangle_B + \alpha_1|1\rangle_B)|00\rangle_{AQ} + (\alpha_0|0\rangle_B - \alpha_1|1\rangle_B)|01\rangle_{AQ} + (\alpha_1|0\rangle_B + \alpha_0|1\rangle_B)|10\rangle_{AQ} + (-\alpha_1|0\rangle_B + \alpha_0|1\rangle_B)|11\rangle_{AQ}\bigr]$$

**Step 4 — Alice measures, Bob corrects:**

| Alice $(A, Q)$ | Bob's state | Bob applies | Result |
|-----------------|-------------|-------------|--------|
| $00$ | $\alpha_0\|0\rangle + \alpha_1\|1\rangle$ | $I$ | $\alpha_0\|0\rangle + \alpha_1\|1\rangle$ |
| $01$ | $\alpha_0\|0\rangle - \alpha_1\|1\rangle$ | $Z$ | $\alpha_0\|0\rangle + \alpha_1\|1\rangle$ |
| $10$ | $\alpha_1\|0\rangle + \alpha_0\|1\rangle$ | $X$ | $\alpha_0\|0\rangle + \alpha_1\|1\rangle$ |
| $11$ | $-\alpha_1\|0\rangle + \alpha_0\|1\rangle$ | $XZ$ | $\alpha_0\|0\rangle + \alpha_1\|1\rangle$ |

Each outcome has probability 1/4. In all cases, Bob recovers the original state.

### Qiskit Implementation

**Basic teleportation circuit:**
```python
from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister
import numpy as np

secret = QuantumRegister(1, "Q")
Alice = QuantumRegister(1, "A")
Bob = QuantumRegister(1, "B")
cr = ClassicalRegister(3, "c")

qc = QuantumCircuit(secret, Alice, Bob, cr)

# Step 1: Create e-bit
qc.h(Alice)
qc.cx(Alice, Bob)
qc.barrier()

# Step 2: Prepare secret state
np.random.seed(42)
theta = np.random.uniform(0, 1) * np.pi
varphi = np.random.uniform(0, 2) * np.pi
qc.u(theta, varphi, 0.0, secret)
qc.barrier()

# Step 3: Alice's operations
qc.cx(secret, Alice)
qc.h(secret)
qc.barrier()

# Step 4: Measure
qc.measure(Alice, cr[1])
qc.measure(secret, cr[0])

# Step 5: Bob's corrections
with qc.if_test((cr[1], 1)):
    qc.x(Bob)
with qc.if_test((cr[0], 1)):
    qc.z(Bob)

# Step 6: Verify (apply inverse of preparation)
qc.barrier()
qc.u(theta, varphi, 0.0, Bob).inverse()
qc.measure(Bob, cr[2])
```

**Verification trick:** Apply $U^{-1}$ to Bob's qubit after teleportation. If successful, measuring Bob gives $|0\rangle$ with certainty.

**Long-distance teleportation (Experiment 2):**
```python
# Uses SWAP chains to move entangled qubits to opposite ends of processor
ebitsa = QuantumRegister(6, "A")
ebitsb = QuantumRegister(6, "B")

# Create e-bit at center
qc.h(ebitsa[5])
qc.cx(ebitsa[5], ebitsb[0])

# SWAP to opposite ends
for n in range(5):
    qc.swap(ebitsb[n], ebitsb[n + 1])
    qc.swap(ebitsa[5 - n], ebitsa[4 - n])

# Then perform standard teleportation protocol
```

**New Qiskit feature:** `IfElseOp` must be explicitly added to backend target:
```python
from qiskit.circuit import IfElseOp
backend.target.add_instruction(IfElseOp, name="if_else")
```

### Gates and Methods

**Gates:** H, CX, X, Z, U (general single-qubit unitary), SWAP
**Key new methods:** `.u(theta, phi, lam, qubit)`, `.inverse()`, `if_test()`, `.swap()`

### Assessment Questions

**True/False:**
1. Teleportation sends information faster than light (FALSE)
2. Modern evidence suggests quantum state collapse propagates faster than light (TRUE)
3. Qiskit uses little-endian ordering $|q_3, q_2, q_1, q_0\rangle$ (TRUE)

**Multiple Choice:**
- When does entanglement affect Bob's qubit? (Instantly, within experimental tolerance)
- Identify entangled vs. separable states (multi-select)

**Discussion:** Is the Bell state $\frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$ special, or could other entangled states work for teleportation?

### Pedagogical Notes for PHYS 349

**What's new:** The full teleportation protocol with circuit implementation, classical conditioning (`if_test`), SWAP-chain long-distance extension.

**What overlaps:** Bell states (03_01), entanglement (03_01–03_04). The Watrous teleportation derivation in lecture_notes.md covers the same math but without the Qiskit code.

**This module adds:** Working code, hardware execution, the verification trick ($U^{-1}$ test), and the long-distance SWAP experiment. Use this module *alongside* the Watrous derivation — Watrous for the math, this module for the code.

---

## Module: Grover's Algorithm

Source: quantum.cloud.ibm.com/learning/en/modules/computer-science/grovers

**Unstructured search with quadratic speedup. Three hands-on activities: single target, partner query game, constraint-based search.**

### Pedagogical Flow

1. Unstructured search problem definition
2. Classical vs. quantum complexity: $O(N)$ vs. $O(\sqrt{N})$
3. Oracle function $Z_f$ (phase flip on marked states)
4. Diffusion operator (amplitude amplification)
5. Grover operator = oracle + diffusion
6. Optimal iterations formula
7. Two-qubit worked example (complete state evolution)
8. Activity 1: Single target state search
9. Activity 2: Query model workflow (partner exchange via QPY files)
10. Activity 3: Constraint-based search (Hamming weight)

### Problem Definition

**Given:** $f: \{0,1\}^n \to \{0,1\}$ where $f(x) = 1$ for marked/solution states, $f(x) = 0$ otherwise.

**Goal:** Find a marked state.

| Strategy | Queries |
|----------|---------|
| Classical | $O(N) = O(2^n)$ |
| **Quantum (Grover)** | $O(\sqrt{N}) = O(2^{n/2})$ |

**Key point:** This is a **quadratic** speedup, not exponential. Still significant: for $N = 10^6$, classical needs ~500,000 queries, Grover needs ~785.

### The Algorithm

**Oracle gate** $Z_f$ (phase flip):

$$Z_f|x\rangle = (-1)^{f(x)}|x\rangle$$

Marked states get a $-1$ phase. Non-solution states unchanged.

**Diffusion operator** $Z_{\text{OR}}$:

$$Z_{\text{OR}}|x\rangle = \begin{cases} |x\rangle & \text{if } x = 0^n \\ -|x\rangle & \text{if } x \neq 0^n \end{cases}$$

**Grover operator** $G = (H^{\otimes n} \cdot Z_{\text{OR}} \cdot H^{\otimes n}) \cdot Z_f$

Apply $G$ repeatedly $t$ times to amplify marked state amplitude.

**Optimal number of iterations:**

$$t = \left\lfloor \frac{\pi}{4}\sqrt{\frac{N}{|A_1|}} - \frac{1}{2} \right\rfloor$$

where $N = 2^n$ and $|A_1|$ = number of marked states.

**Scaling:**
- More solutions → fewer iterations ($t \sim 1/\sqrt{|A_1|}$)
- Larger search space → more iterations ($t \sim \sqrt{N}$)
- **Too many iterations** → probability *decreases* (amplitude oscillates!)

### Two-Qubit Worked Example (n=2, target = |01⟩)

$$|\psi_0\rangle = |00\rangle$$

$$|\psi_1\rangle = \frac{1}{2}(|00\rangle + |01\rangle + |10\rangle + |11\rangle) \quad \text{(after } H^{\otimes 2}\text{)}$$

$$|\psi_2\rangle = \frac{1}{2}(|00\rangle - |01\rangle + |10\rangle + |11\rangle) \quad \text{(after oracle, phase flip on } |01\rangle\text{)}$$

$$|\psi_3\rangle = \frac{1}{2}(|00\rangle + |01\rangle - |10\rangle + |11\rangle) \quad \text{(after } H^{\otimes 2}\text{)}$$

$$|\psi_4\rangle = \frac{1}{2}(|00\rangle - |01\rangle + |10\rangle - |11\rangle) \quad \text{(after } Z_{\text{OR}}\text{)}$$

$$|\psi_5\rangle = |01\rangle \quad \text{(after final } H^{\otimes 2}\text{)}$$

**One iteration → deterministic result for $n=2$!**

### Qiskit Implementation

**Oracle construction (multi-controlled Z with open controls):**
```python
from qiskit import QuantumCircuit
from qiskit.circuit.library import MCMTGate, ZGate

def grover_oracle(marked_states):
    if not isinstance(marked_states, list):
        marked_states = [marked_states]
    num_qubits = len(marked_states[0])
    qc = QuantumCircuit(num_qubits)
    for target in marked_states:
        rev_target = target[::-1]
        zero_inds = [i for i in range(num_qubits) if rev_target[i] == "0"]
        qc.x(zero_inds)
        qc.compose(MCMTGate(ZGate(), num_qubits - 1, 1), inplace=True)
        qc.x(zero_inds)
    return qc
```

**Full Grover circuit:**
```python
import math
from qiskit.circuit.library import grover_operator

marked_states = ["1110"]
oracle = grover_oracle(marked_states)
grover_op = grover_operator(oracle)

optimal_num_iterations = math.floor(
    math.pi / (4 * math.asin(math.sqrt(len(marked_states) / 2**grover_op.num_qubits)))
)

qc = QuantumCircuit(grover_op.num_qubits)
qc.h(range(grover_op.num_qubits))
qc.compose(grover_op.power(optimal_num_iterations), inplace=True)
qc.measure_all()
```

**Hamming weight oracle (constraint-based search):**
```python
def grover_oracle_hamming_weight(num_qubits, target_weight):
    qc = QuantumCircuit(num_qubits)
    for x in range(2**num_qubits):
        bitstr = format(x, f"0{num_qubits}b")
        if bitstr.count("1") == target_weight:
            rev_target = bitstr[::-1]
            zero_inds = [i for i, b in enumerate(rev_target) if b == "0"]
            qc.x(zero_inds)
            qc.h(num_qubits - 1)
            qc.mcx(list(range(num_qubits - 1)), num_qubits - 1)
            qc.h(num_qubits - 1)
            qc.x(zero_inds)
    return qc
```

**Circuit exchange via QPY (partner activity):**
```python
from qiskit import qpy

# Save
with open("my_circuit.qpy", "wb") as f:
    qpy.dump(oracle, f)

# Load
with open("partner_circuit.qpy", "rb") as f:
    circuits = qpy.load(f)
oracle_partner = circuits[0]
```

### Gates and Classes

**Gates:** H, X, Z, MCX (multi-controlled X), MCMTGate(ZGate())
**Key library functions:** `grover_operator()` (builds full Grover operator from oracle), `grover_op.power(n)` (repeat $n$ times)
**New tools:** `qpy.dump()`/`qpy.load()` for circuit serialization, `plot_distribution()` for results

### Assessment Questions

**True/False:**
1. Grover provides *exponential* speedup (FALSE — quadratic only)
2. More iterations always helps (FALSE — probability oscillates)

**Multiple Choice:**
- Best strategy on noisy hardware? (Fewer iterations may suffice — reduces depth and errors)
- 3 marked states from 128 total → optimal iterations? ($t \approx 5$)

**Activities:**
1. Find single target "1110" in 4 qubits (expected: 3 iterations)
2. Partner exchange: create secret oracle, trade QPY files, find each other's targets
3. Hamming weight search: find all 4-bit strings with weight 2 (6 solutions)

### Pedagogical Notes for PHYS 349

**What's new:** First "useful" quantum algorithm (D-J is a toy problem). Amplitude amplification, oracle model, quadratic vs. exponential speedup distinction.

**The partner activity is gold** — students experience the query model firsthand: one person is the oracle holder, the other is the searcher. They can't peek at each other's circuits.

**Important nuance:** Grover is quadratic, not exponential. Students often confuse this. Worth spending time on why $O(\sqrt{N})$ vs. $O(N)$ matters but is not as dramatic as $O(\text{poly}(n))$ vs. $O(2^n)$.

---

## Module: Shor's Algorithm

Source: quantum.cloud.ibm.com/learning/en/modules/computer-science/shors-algorithm

**Factoring via order finding. Covers modular arithmetic, QPE, QFT, and classical post-processing. The crown jewel of quantum algorithms.**

### Pedagogical Flow

1. Introduction (Shor 1994, threat to RSA)
2. The factoring problem and RSA
3. Modular arithmetic primer ($\mathbb{Z}_N$, $\mathbb{Z}_N^*$, order)
4. Reduction: factoring → order finding
5. Quantum Phase Estimation (QPE) overview
6. Eigenstates of modular multiplication
7. Implementation: $M_2 \bmod 15$ from SWAP gates
8. Full QPE circuit
9. Classical post-processing (continued fractions)
10. Assessment

### Mathematical Foundation

**Modular arithmetic:**
- $\mathbb{Z}_N = \{0, 1, \ldots, N-1\}$
- $\mathbb{Z}_N^* = \{a \in \mathbb{Z}_N : \gcd(a, N) = 1\}$ (elements coprime to $N$)
- Example: $\mathbb{Z}_{15}^* = \{1, 2, 4, 7, 8, 11, 13, 14\}$

**Order:** The smallest positive integer $r$ such that $a^r \equiv 1 \pmod{N}$.

Examples for $N = 15$:
| $a$ | $r$ (order) |
|-----|-------------|
| 2 | 4 |
| 4 | 2 |
| 7 | 4 |
| 11 | 2 |
| 14 | 2 |

### The Key Insight: Factoring → Order Finding

If $x^2 \equiv 1 \pmod{N}$ and $x \not\equiv \pm 1 \pmod{N}$, then:

$$x^2 - 1 = (x+1)(x-1) \equiv 0 \pmod{N}$$

So $N$ divides $(x+1)(x-1)$ but divides neither factor alone. Therefore:

$$p = \gcd(x-1, N), \quad q = \gcd(x+1, N)$$

are non-trivial factors of $N$.

**Example:** $N = 15$, $x = 11$ (since $11^2 = 121 = 8 \times 15 + 1$):
- $\gcd(11-1, 15) = \gcd(10, 15) = 5$
- $\gcd(11+1, 15) = \gcd(12, 15) = 3$
- $15 = 5 \times 3$ ✓

### Shor's Algorithm (5 Steps)

1. Pick random $a$ where $1 < a < N$
2. **Find order $r$ of $a$ modulo $N$** ← the quantum step (QPE)
3. If $r$ is odd, go back to step 1
4. Compute $x = a^{r/2} \bmod N$; if $x \equiv \pm 1 \pmod{N}$, go back to step 1
5. Factors: $p = \gcd(x-1, N)$, $q = \gcd(x+1, N)$

### Quantum Phase Estimation for Order Finding

**Modular multiplication unitary:**

$$M_a|y\rangle = |ay \bmod N\rangle$$

**Eigenstates of $M_a$:**

$$|\psi_j\rangle = \frac{1}{\sqrt{r}}\sum_{k=0}^{r-1} e^{2\pi i jk/r} |a^k \bmod N\rangle$$

**Eigenvalues:**

$$M_a|\psi_j\rangle = e^{2\pi i j/r}|\psi_j\rangle$$

**The trick:** We don't need to prepare individual eigenstates! The state $|1\rangle$ is a uniform superposition over all eigenstates:

$$|1\rangle = \frac{1}{\sqrt{r}}\sum_{j=0}^{r-1}|\psi_j\rangle$$

QPE applied to $|1\rangle$ will produce a random $j/r$, from which we extract $r$ via continued fractions.

**Example eigenstates for $a=2$, $N=15$ ($r=4$):**

$$|\psi_0\rangle = \frac{1}{2}(|1\rangle + |2\rangle + |4\rangle + |8\rangle)$$
$$|\psi_1\rangle = \frac{1}{2}(|1\rangle + i|2\rangle - |4\rangle - i|8\rangle)$$
$$|\psi_2\rangle = \frac{1}{2}(|1\rangle - |2\rangle + |4\rangle - |8\rangle)$$
$$|\psi_3\rangle = \frac{1}{2}(|1\rangle - i|2\rangle - |4\rangle + i|8\rangle)$$

### Implementation: $M_2 \bmod 15$ from SWAP Gates

The modular multiplication $M_2$ on 4-qubit states maps:

$$|0001\rangle \to |0010\rangle \to |0100\rangle \to |1000\rangle \to |0001\rangle$$

This is just a cyclic shift — implementable with three SWAP gates:

$$M_2 = \text{SWAP}(0,1) \cdot \text{SWAP}(1,2) \cdot \text{SWAP}(2,3)$$

**Powers:** $a^{2^k} \bmod 15$:
- $k=0$: $2^1 = 2$ → use $M_2$ (SWAPs)
- $k=1$: $2^2 = 4$ → use $M_4$ (different SWAP pattern)
- $k \geq 2$: $2^{2^k} \equiv 1 \pmod{15}$ → identity, skip!

### Qiskit Implementation

**$M_2 \bmod 15$:**
```python
def M2mod15():
    U = QuantumCircuit(4)
    U.swap(2, 3)
    U.swap(1, 2)
    U.swap(0, 1)
    U = U.to_gate()
    U.name = "M_2"
    return U
```

**$M_4 \bmod 15$:**
```python
def M4mod15():
    U = QuantumCircuit(4)
    U.swap(1, 3)
    U.swap(0, 2)
    U = U.to_gate()
    U.name = "M_4"
    return U
```

**Computing $a^{2^k} \bmod N$:**
```python
def a2kmodN(a, k, N):
    for _ in range(k):
        a = int(np.mod(a**2, N))
    return a
```

**Full QPE circuit:**
```python
from qiskit.circuit.library import QFTGate

N, a = 15, 2
num_target = floor(log(N - 1, 2)) + 1    # 4 qubits for mod-15
num_control = 2 * num_target              # 8 control qubits for precision

control = QuantumRegister(num_control, "C")
target = QuantumRegister(num_target, "T")
output = ClassicalRegister(num_control, "out")
circuit = QuantumCircuit(control, target, output)

circuit.x(num_control)    # Initialize target to |1⟩

for k, qubit in enumerate(control):
    circuit.h(k)
    b = a2kmodN(a, k, N)
    if b == 2:
        circuit.compose(M2mod15().control(), qubits=[qubit] + list(target), inplace=True)
    elif b == 4:
        circuit.compose(M4mod15().control(), qubits=[qubit] + list(target), inplace=True)

circuit.compose(QFTGate(num_control).inverse(), qubits=control, inplace=True)
circuit.measure(control, output)
```

**Classical post-processing:**
```python
from fractions import Fraction
from math import gcd

for bitstring in counts_keep:
    decimal = int(bitstring, 2)
    phase = decimal / (2**num_control)
    frac = Fraction(phase).limit_denominator(N)
    r = frac.denominator

    if phase != 0 and r % 2 == 0:
        x = pow(a, r // 2, N) - 1
        d = gcd(x, N)
        if d > 1:
            print(f"Factor found: {d}")
```

**Error mitigation options:**
```python
sampler.options.dynamical_decoupling.enable = True
sampler.options.dynamical_decoupling.sequence_type = "XpXm"
sampler.options.twirling.enable_gates = True
```

### Gates and Classes

**Gates:** H, SWAP, X, controlled unitaries, QFTGate (inverse)
**New classes:** `QFTGate` from `qiskit.circuit.library`
**New methods:** `.control()` (create controlled gate), `.power()`, dynamical decoupling options, gate twirling
**Classical tools:** `Fraction().limit_denominator()`, `gcd()`, `pow(base, exp, mod)`

### Assessment Questions

**True/False:**
1. Shor's algorithm threatens RSA encryption (TRUE)
2. Can be efficiently executed on any modern quantum computer (FALSE)
3. Uses QPE as a key subroutine (TRUE)
4. Classical part involves GCD computation (TRUE)
5. Only works for even numbers (FALSE)
6. Successful run always guarantees correct factors (FALSE — probabilistic)

**Short Answer:**
- Why does Shor threaten RSA? (Exponential speedup for factoring)
- Why is order finding useful for factoring? ($a^{r/2}$ gives non-trivial square root of 1 mod $N$)

**Challenge:** Factor 21 using the same procedure.

### Pedagogical Notes for PHYS 349

**What's new:** Modular arithmetic, order finding, QPE, QFT (inverse), continued fractions, the full classical-quantum hybrid workflow.

**This is a 2-lecture topic minimum.** Lecture 1: modular arithmetic + the factoring→order-finding reduction (all classical). Lecture 2: QPE circuit, QFT, implementation, post-processing.

**Students need to understand:** The quantum computer doesn't "factor" — it finds the *period* of a modular exponential. The factoring happens classically afterward. This hybrid structure is important conceptually.

**The $N=15$ example is perfect** because $M_2$ is just SWAP gates — students can see exactly what the unitary does without black-box abstractions. The $M_4$ variant shows the pattern extends.

**Connect to:** Phase estimation is the key primitive. Once students understand QPE, they can understand not just Shor's but also quantum chemistry algorithms (VQE, etc.).

---

## Module: Quantum Fourier Transform

Source: quantum.cloud.ibm.com/learning/en/modules/computer-science/qft

**The mathematical backbone of Shor's algorithm. Covers QFT definition, circuit decomposition, phase estimation, and applications.**

### Pedagogical Flow

1. Classical Fourier transform motivation (signal processing, frequency analysis)
2. Discrete Fourier Transform (DFT) definition
3. QFT definition: computational basis → Fourier basis
4. Fourier basis states and phase winding intuition
5. Practical examples: QFT of single basis state, QFT of two-state superposition
6. QFT circuit decomposition (1-qubit → 2-qubit → n-qubit, "Russian nesting doll")
7. Quantum Phase Estimation (QPE) using QFT
8. Connection to Shor's algorithm

### QFT Mathematical Definition

The QFT on $m$ qubits ($N = 2^m$ basis states) transforms computational basis to Fourier basis:

$$|\phi_y\rangle = \frac{1}{\sqrt{N}}\sum_{x=0}^{N-1}\omega_N^{yx}|x\rangle$$

where $\omega_N^{yx} = e^{2\pi i yx/N}$.

**As a matrix:**

$$\text{QFT}_N = \frac{1}{\sqrt{N}}\sum_{x=0}^{N-1}\sum_{y=0}^{N-1}\omega_N^{xy}|x\rangle\langle y|$$

**4-state QFT matrix ($N=4$, 2 qubits):**

$$\text{QFT}_4 = \frac{1}{2}\begin{pmatrix} 1 & 1 & 1 & 1 \\ 1 & i & -1 & -i \\ 1 & -1 & 1 & -1 \\ 1 & -i & -1 & i \end{pmatrix}$$

### Fourier Basis States — Phase Winding

**Single qubit ($N=2$):**
$$|\phi_0\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) = |+\rangle$$
$$|\phi_1\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle) = |-\rangle$$

The 1-qubit QFT is just the Hadamard gate!

**Two qubits ($N=4$):**
$$|\phi_0\rangle = \frac{1}{2}(|00\rangle + |01\rangle + |10\rangle + |11\rangle)$$
$$|\phi_1\rangle = \frac{1}{2}(|00\rangle + i|01\rangle - |10\rangle - i|11\rangle)$$
$$|\phi_2\rangle = \frac{1}{2}(|00\rangle - |01\rangle + |10\rangle - |11\rangle)$$
$$|\phi_3\rangle = \frac{1}{2}(|00\rangle - i|01\rangle - |10\rangle + i|11\rangle)$$

**Key property:** QFT of any single computational basis state produces an **equal superposition** of all states (uniform magnitudes, varying phases). The phase pattern encodes which basis state you started from.

### QFT Circuit Decomposition

**Gates needed:** Hadamard (H), Controlled-Phase (CP), SWAP

$$\text{CP}_\alpha = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & e^{i\alpha} \end{pmatrix}$$

**Recursive structure ("Russian nesting doll"):**
1. Apply QFT$_{2^{m-1}}$ to the bottom $m-1$ qubits
2. Apply controlled-phase gates from top qubit to each of the bottom $m-1$ qubits
3. Apply Hadamard to the top qubit
4. SWAP gates at the end to reverse qubit order

**Decomposition examples:**
- 1-qubit QFT = single H gate
- 2-qubit QFT = H on q₀, CP($\pi/2$), H on q₁, SWAP
- Each additional qubit adds controlled-phase gates with increasingly small angles

### Qiskit Implementation

**Using QFTGate from library:**
```python
from qiskit import QuantumCircuit
from qiskit.circuit.library import QFTGate

qubits = 4
qc = QuantumCircuit(qubits)
qc.compose(QFTGate(qubits), inplace=True)
qc.measure_all()

# See the decomposition
qc.decompose().draw("mpl")
```

**Example: QFT of a random computational basis state:**
```python
qc = QuantumCircuit(4)

# Prepare random basis state with X gates
for i in range(1, 4):
    if np.random.randint(0, 2):
        qc.x(i)

qc_qft = qc.copy()
qc_qft.compose(QFTGate(4), inplace=True)
qc_qft.measure_all()
```

**Result:** Uniform distribution over all 16 basis states (equal superposition with varying phases — but measurement only sees magnitudes).

**Example: QFT of superposition $|\psi\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |N/2\rangle)$:**
```python
qc = QuantumCircuit(4)
qc.h(3)  # Creates |0000⟩ + |1000⟩ superposition
qc_qft = qc.copy()
qc_qft.compose(QFTGate(4), inplace=True)
qc_qft.measure_all()
```

**Result:** Only even-numbered basis states have nonzero probability — the Fourier transform of a signal with period 2 has peaks at even harmonics.

### Quantum Phase Estimation (QPE)

**Problem:** Given a unitary $U$ with eigenstate $|\psi\rangle$ and eigenvalue $\lambda = e^{2\pi i \theta}$, estimate $\theta$.

**Single-bit QPE (simplest case):**

$$|0\rangle \xrightarrow{H} \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) \xrightarrow{c\text{-}U} \frac{1}{\sqrt{2}}(|0\rangle + e^{2\pi i\theta}|1\rangle) \xrightarrow{H} \cos(\pi\theta)|0\rangle - i\sin(\pi\theta)|1\rangle$$

**Multi-qubit QPE with $m$ control qubits:**

After Hadamards and controlled-$U^{2^k}$ gates:

$$|\pi_2\rangle = |\psi\rangle \otimes \frac{1}{2^{m/2}}\sum_{k=0}^{2^m-1}e^{2\pi i k\theta}|k\rangle$$

This is a **Fourier basis state** on the control register! Applying inverse QFT extracts:

$$|\pi_3\rangle = |\psi\rangle \otimes |y\rangle \quad \text{where } \theta \approx y/2^m$$

**Key insight:** The controlled unitaries encode $\theta$ as phases in the Fourier basis. The inverse QFT converts these phases back into a computational basis state that we can read out.

### Assessment Questions

**Understanding checks:**
- Why does QFT of a single basis state give uniform distribution? (All magnitudes equal, phases differ but aren't measured)
- What superposition gives QFT peaks on odd numbers only? ($|0\rangle - |N/2\rangle$, relative phase of $\pi$)

**True/False:**
1. QFT is the quantum analog of classical DFT (TRUE)
2. QFT can be implemented using only H and CNOT (FALSE — needs controlled-phase gates)
3. QFT is a key component of Shor's algorithm (TRUE)
4. QPE output is a quantum state representing the eigenvector (FALSE — it estimates the eigenvalue)
5. QPE requires inverse QFT (TRUE)

**Challenge:** Create a 4-qubit equal superposition of all odd computational basis states, apply QFT, and explain the result.

### Pedagogical Notes for PHYS 349

**What's new:** The QFT itself, controlled-phase gates, the connection between Fourier basis and phase information, QPE algorithm.

**What students know:** Hadamard (which is the 1-qubit QFT!), measurement, superposition, controlled gates. The QFT is a natural extension — "what if we generalize the Hadamard to $N$ dimensions?"

**The classical analogy is powerful.** Students who've seen Fourier transforms in math/physics courses will recognize the structure. The quantum version just operates on superposition amplitudes instead of function values.

**QPE is the bridge to Shor's.** Once students understand that QPE extracts eigenvalues by converting phases to measurement outcomes via inverse QFT, Shor's algorithm follows naturally (the eigenvalues encode the period).

---

## Module: Variational Quantum Eigensolver (VQE)

Source: quantum.cloud.ibm.com/learning/en/modules/computer-science/variational-quantum-eigensolver

**Hybrid quantum-classical algorithm for quantum chemistry. Computes H and H₂ ground state energies and the H+H→H₂ reaction energy. Excellent capstone module.**

### Pedagogical Flow

1. Quantum chemistry motivation (electronic structure, molecular orbitals)
2. Why quantum computers? (classical scaling: caffeine = $10^{48}$ bits vs. 160 qubits)
3. Variational principle (detailed derivation with harmonic oscillator)
4. VQE components: Hamiltonian + Ansatz + Optimizer
5. Ansatz design: Bloch sphere coverage comparison (1, 2, 3 parameters)
6. Hydrogen atom: define Hamiltonian, run VQE, check convergence
7. Hydrogen molecule: H₂ Hamiltonian, nuclear repulsion, VQE
8. Reaction energy: $\Delta E = E_{H_2} - 2E_H$ with energy diagram
9. Advanced: ansatz complexity vs. optimizer overhead

### Core Concepts

**Variational Principle:**

$$E_\text{approx} = \frac{\langle\Psi_\text{trial}|\hat{H}|\Psi_\text{trial}\rangle}{\langle\Psi_\text{trial}|\Psi_\text{trial}\rangle} \geq E_0$$

Any trial wavefunction gives an energy that is an **upper bound** to the true ground state. Minimizing over parameters approaches $E_0$.

**VQE = Three Components:**

1. **Hamiltonian** (the observable) — represents total energy of the molecule
2. **Ansatz** (parameterized circuit) — the trial wavefunction $|\Psi(\vec{\theta})\rangle$
3. **Classical optimizer** — adjusts $\vec{\theta}$ to minimize $\langle\hat{H}\rangle$

### Variational Principle Derivation (Harmonic Oscillator)

**Trial wavefunction:** $\Psi_\text{trial}(\alpha, x) = Ae^{-\alpha x^2}$

**Normalization:** $A = \left(\frac{2\alpha}{\pi}\right)^{1/4}$

**Hamiltonian:** $\hat{H} = -\frac{\hbar^2}{2m}\frac{d^2}{dx^2} + \frac{1}{2}m\omega^2 x^2$

**Expectation values:**
$$\langle T \rangle = \frac{\hbar^2\alpha}{2m}, \qquad \langle V \rangle = \frac{m\omega^2}{4\alpha}$$

$$E_\text{approx}(\alpha) = \frac{\hbar^2\alpha}{2m} + \frac{m\omega^2}{4\alpha}$$

**Optimize:** $\frac{dE}{d\alpha} = 0 \implies \alpha_\text{opt} = \frac{m\omega}{2\hbar}$

$$E_\text{min} = \frac{\hbar\omega}{2} \quad \text{(exact ground state!)}$$

This is a beautiful pedagogical example — the Gaussian trial function happens to be the exact ground state of the harmonic oscillator, so the variational method gives the exact answer.

### Molecular Hamiltonians

**Hydrogen atom (1 qubit, STO-6G basis, Jordan-Wigner mapping):**

$$\hat{H}_H = -0.2355\,I + 0.2355\,Z$$

Exact ground state: $E_H = -0.5$ Hartree

**Hydrogen molecule (1 qubit after symmetry reduction):**

$$\hat{H}_{H_2} = -1.04886\,I - 0.79674\,Z + 0.18122\,X$$

Nuclear repulsion energy: $E_\text{rep} = 1/R = 0.71997$ Hartree (at $R = 0.735$ Å $= 1.389\,a_0$)

### Ansatz Design — Bloch Sphere Coverage

Three ansätze compared for a single-qubit problem:

| Ansatz | Gates | Parameters | Bloch sphere coverage |
|--------|-------|------------|----------------------|
| 1 | $R_X(\theta)$ | 1 | Ring (great circle) |
| 2 | $R_X(\theta) \cdot R_Z(\phi)$ | 2 | Most of sphere, concentrated at poles |
| 3 | $R_X(\theta) \cdot R_Z(\phi) \cdot R_X(\lambda)$ | 3 | **Uniform coverage** |

Ansatz 3 is chosen — it can reach any point on the Bloch sphere, so it can represent any single-qubit state (including the ground state).

### Qiskit Implementation

**Define parameterized ansatz:**
```python
from qiskit import QuantumCircuit
from qiskit.circuit import Parameter

theta = Parameter("θ")
phi = Parameter("φ")
lam = Parameter("λ")

ansatz = QuantumCircuit(1)
ansatz.rx(theta, 0)
ansatz.rz(phi, 0)
ansatz.rx(lam, 0)
```

**Define Hamiltonian:**
```python
from qiskit.quantum_info import SparsePauliOp

H = SparsePauliOp.from_list([("I", -0.2355), ("Z", 0.2355)])
```

**Cost function (the VQE loop core):**
```python
def cost_func(params, ansatz, hamiltonian, estimator):
    pub = (ansatz, [hamiltonian], [params])
    result = estimator.run(pubs=[pub]).result()
    energy = result[0].data.evs[0]
    cost_history_dict["iters"] += 1
    cost_history_dict["cost_history"].append(energy)
    return energy
```

**Run optimization:**
```python
from scipy.optimize import minimize
from qiskit_ibm_runtime import EstimatorV2 as Estimator, Batch

batch = Batch(backend=backend)
estimator = Estimator(mode=batch)
estimator.options.default_shots = 10000

x0 = [1, 1, 0]  # Initial parameters

res = minimize(
    cost_func, x0,
    args=(ansatz_isa, H_isa, estimator),
    method="cobyla",
    options={"maxiter": 10, "tol": 0.01}
)

batch.close()
```

**Apply layout to Hamiltonian (critical step!):**
```python
H_isa = H.apply_layout(layout=ansatz_isa.layout)
```

**Bloch sphere visualization of ansatz coverage:**
```python
from qiskit.quantum_info import Statevector, DensityMatrix, Pauli

bound_qc = ansatz.assign_parameters({theta: val1, phi: val2, lam: val3})
state = Statevector.from_instruction(bound_qc)
rho = DensityMatrix(state)
X = rho.expectation_value(Pauli("X")).real
Y = rho.expectation_value(Pauli("Y")).real
Z = rho.expectation_value(Pauli("Z")).real
```

### Reaction Energy Calculation

$$E_\text{reaction} = E_{H_2} - 2E_H$$

- Theoretical: $\approx -0.17$ Hartree (exothermic — H₂ is more stable)
- VQE: close agreement despite hardware noise
- Visualized as energy level diagram with arrows showing $\Delta E$

### Key Qiskit Patterns

**New classes/methods:**
- `Parameter("θ")`, `ParameterVector("α", n)` — symbolic circuit parameters
- `SparsePauliOp.from_list()` — build Hamiltonians from Pauli strings
- `EstimatorV2` — compute $\langle\hat{H}\rangle$ (not sampling counts!)
- `.apply_layout()` — map Hamiltonian to physical qubit layout after transpilation
- `.assign_parameters()` — bind numerical values to symbolic parameters
- `Batch` — group multiple estimator calls for efficiency
- `scipy.optimize.minimize(method="cobyla")` — gradient-free classical optimizer

### Assessment Questions

**Harmonic oscillator exercise:** Full derivation — normalize $\Psi = Ae^{-\alpha x^2}$, compute $\langle T\rangle$ and $\langle V\rangle$, minimize to get $E_0 = \hbar\omega/2$.

**Fill-in-the-blank:** VQE combines (1) quantum computing and classical computing to find (2) ground state energy using (3) molecular Hamiltonian, (4) ansatz, (5) classical optimizer, specifically (6) COBYLA, until (7) convergence.

**True/False:**
- Variational principle: $\langle\hat{H}\rangle \geq E_0$ (TRUE)
- VQE finds the exact ground state (FALSE — approximate)
- Optimizer choice doesn't matter (FALSE)

**Discussion:** What improvements would give better VQE results? (Larger basis set, better ansatz, error mitigation, more shots, different optimizer)

### Pedagogical Notes for PHYS 349

**What's new:** Hybrid quantum-classical algorithm concept, parameterized circuits, classical optimization loop, molecular Hamiltonians from chemistry, the EstimatorV2 primitive.

**What students know:** Variational principle from QM (Griffiths Ch. 8!), Pauli matrices, expectation values, Bloch sphere. This module directly connects their QM knowledge to quantum computing applications.

**The harmonic oscillator warm-up is genius** — students have done this exact problem in Griffiths. Seeing it reframed as "this is what VQE does, but on a quantum computer" makes the algorithm immediately concrete.

**This is a great capstone topic.** It combines: quantum circuits (ansatz), measurement theory (expectation values), linear algebra (Hamiltonians), optimization (classical loop), and real-world application (chemistry). It also introduces the NISQ-era philosophy: noisy hardware + clever algorithms = useful results.

**Practical note:** The 1-qubit Hamiltonians mean this runs fast and reliably on real hardware. Students can actually get results in class. Multi-qubit molecular Hamiltonians (LiH, H₂O) are possible extensions for projects.

**Connect to:** EstimatorV2 was introduced in the Superposition module. QPE from the QFT module solves the same problem (finding eigenvalues) but requires deeper circuits. VQE is the NISQ-friendly alternative — shallower circuits, classical optimization fills the gap.
