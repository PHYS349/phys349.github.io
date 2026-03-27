---
kernelspec:
  name: python3
  display_name: Python 3
---

# Lecture 3.7 — Quantum Key Distribution (BB84)

## Where We Left Off

We've built up a powerful toolkit: qubits, gates, circuits, entanglement, and the no-cloning theorem. We even proved that quantum mechanics gives a measurable advantage in the CHSH game.

Today we put all of this to practical use. We'll see how the laws of quantum mechanics — specifically the impossibility of cloning unknown states and the disturbance caused by measurement — enable a provably secure method for sharing secret keys. This is the BB84 protocol, invented by Charles Bennett and Gilles Brassard in 1984, and it's one of the first real-world applications of quantum information.

------------------------------------------------------------------------

## Part 1: The Key Distribution Problem

### Why Do We Need Quantum Cryptography?

Suppose Alice wants to send Bob a secret message — say, troop movements, financial data, or just a surprise party plan. The standard approach is encryption: Alice scrambles the message with a **key**, sends the scrambled message over a public channel, and Bob unscrambles it with the same key.

The strongest classical encryption scheme is the **one-time pad (OTP)**. The idea is simple: Alice and Bob share a random key that is as long as the message. To encrypt, Alice XORs each bit of her message with the corresponding key bit:

$$
\text{ciphertext} = \text{message} \oplus \text{key}
$$

To decrypt, Bob XORs again:

$$
\text{message} = \text{ciphertext} \oplus \text{key}
$$

The one-time pad is **provably unbreakable** — as long as the key is truly random, as long as the message, and used only once. An eavesdropper who intercepts the ciphertext has literally zero information about the message, because every possible message is equally likely for any given ciphertext.

### The Catch

The one-time pad shifts the problem rather than solving it. To send a secret message, Alice and Bob first need a secret key — and that key needs to be as long as the message itself. How do they share it securely?

If they meet in person and exchange a key on a USB drive, that works — but it's impractical for most real-world communication. If they send the key over the internet, an eavesdropper can copy it without detection (classical bits can be copied freely).

This is the **key distribution problem**, and it's the central weakness of classical cryptography.

```{admonition} Classical vs. Quantum
:class: tip
Modern internet encryption (RSA, AES) sidesteps the key distribution problem by using **computational** assumptions — factoring large numbers is *believed* to be hard. But it's not *proven* to be hard, and a sufficiently powerful quantum computer (running Shor's algorithm, which we'll see in a few weeks) could break RSA. The one-time pad, combined with BB84, is secure based on the **laws of physics**, not computational assumptions.
```

------------------------------------------------------------------------

## Part 2: The BB84 Protocol

BB84 uses single qubits — no entanglement needed — and only two gates: $X$ and $H$. The security comes entirely from the properties of quantum measurement that we've already studied.

### Two Bases

The protocol uses two measurement bases:

- **Z-basis** (computational basis): $\{|0\rangle, |1\rangle\}$
- **X-basis** (Hadamard basis): $\{|+\rangle, |-\rangle\}$

Alice encodes her random bit in a randomly chosen basis:

| Basis | Bit = 0 | Bit = 1 |
|-------|---------|---------|
| Z | $\|0\rangle$ | $\|1\rangle$ |
| X | $\|+\rangle$ | $\|-\rangle$ |

Bob measures each qubit in a randomly chosen basis. The crucial point: if Bob measures in the **same** basis Alice used, he gets the correct bit with certainty. If he measures in the **wrong** basis, he gets a random result — 50/50.

This is just what we learned about the Stern-Gerlach experiment back in the measurement lectures. Measuring a Z-basis state in the X-basis is like measuring a spin-up-in-$z$ particle along $x$ — you get a completely random outcome.

### The Protocol

**Step 1 — Alice prepares:** Alice generates a string of random bits and, for each bit, randomly chooses Z or X basis. She prepares the corresponding qubit state and sends it to Bob through a quantum channel (e.g., a fiber-optic cable carrying single photons).

**Step 2 — Bob measures:** Bob independently chooses a random basis (Z or X) for each qubit and measures. He records his measurement outcomes.

**Step 3 — Basis reconciliation:** Over a public classical channel (phone, internet — no secrecy needed), Alice and Bob announce which **basis** they used for each qubit. They do NOT reveal their bit values. They keep only the rounds where their bases matched and discard the rest.

Since each independently picks Z or X with equal probability, their bases match about **50%** of the time. So roughly half the qubits are discarded — but the remaining bits should be identical on both sides.

**Step 4 — Verification:** Alice and Bob sacrifice a small random subset of their sifted key and compare those bits publicly. If no eavesdropper is present, these should agree perfectly. If an eavesdropper has tampered with the qubits, errors will appear. If the error rate is below a threshold, they keep the remaining bits as their shared secret key.

Let's trace through a small example:

```{code-cell} python
:tags: [hide-input]
import numpy as np

# Small example: 10 qubits
np.random.seed(42)

alice_bits  = np.random.randint(0, 2, 10)
alice_bases = np.random.randint(0, 2, 10)  # 0 = Z, 1 = X
bob_bases   = np.random.randint(0, 2, 10)

# When bases match, Bob gets the correct bit
# When bases don't match, Bob gets a random bit
bob_bits = np.zeros(10, dtype=int)
for i in range(10):
    if alice_bases[i] == bob_bases[i]:
        bob_bits[i] = alice_bits[i]  # Correct
    else:
        bob_bits[i] = np.random.randint(0, 2)  # Random

print("Qubit #:     ", "  ".join(str(i) for i in range(10)))
print("Alice bit:   ", "  ".join(str(b) for b in alice_bits))
print("Alice basis: ", "  ".join("Z" if b == 0 else "X" for b in alice_bases))
print("Bob basis:   ", "  ".join("Z" if b == 0 else "X" for b in bob_bases))
print("Bases match? ", "  ".join("✓" if alice_bases[i] == bob_bases[i] else "✗" for i in range(10)))
print("Bob bit:     ", "  ".join(str(b) for b in bob_bits))
print()

# Sifting: keep only matching bases
sifted_alice = [alice_bits[i] for i in range(10) if alice_bases[i] == bob_bases[i]]
sifted_bob   = [bob_bits[i]   for i in range(10) if alice_bases[i] == bob_bases[i]]
print(f"Sifted key (Alice): {sifted_alice}")
print(f"Sifted key (Bob):   {sifted_bob}")
print(f"Agreement: {sum(a == b for a, b in zip(sifted_alice, sifted_bob))}/{len(sifted_alice)}")
```

When their bases match, Alice and Bob always agree — that's the shared secret key.

------------------------------------------------------------------------

## Part 3: Why Is This Secure?

### Eve's Dilemma

Suppose an eavesdropper, Eve, intercepts the qubits in transit. She wants to learn Alice's bits without being detected. What can she do?

**Eve's best classical-style attack** is called **intercept-resend**: she measures each qubit, reads the result, and sends a fresh qubit to Bob in the state she measured.

But Eve has a problem: she doesn't know Alice's basis. For each qubit, she has to guess — Z or X — and measure accordingly.

```{admonition} Connection to No-Cloning
:class: tip
Why can't Eve just copy the qubit, measure her copy, and forward the original? Because of the **no-cloning theorem** from Lecture 3.5! There is no physical process that can duplicate an arbitrary unknown quantum state. Eve is forced to measure the original, which disturbs it.
```

### The 25% Error Rate

Let's trace the probability of Eve introducing an error on a sifted bit (one where Alice and Bob used the same basis):

**Case 1 — Eve guesses the right basis** (probability $1/2$): Eve measures in the same basis Alice used, gets the correct bit, and re-prepares the correct state. Bob measures correctly. **Error rate: 0%.**

**Case 2 — Eve guesses the wrong basis** (probability $1/2$): This is where things go wrong. Let's trace a concrete example.

Suppose Alice sends $|0\rangle$ (Z-basis, bit = 0) and Eve measures in the X-basis. Eve's measurement gives $|+\rangle$ or $|-\rangle$ with equal probability — regardless of which she gets, she re-prepares that state and sends it to Bob. Now Bob measures in the Z-basis (matching Alice). Whether he received $|+\rangle$ or $|-\rangle$, his Z-measurement gives 0 or 1 with equal probability. **Error rate: 50%.**

The key point: when Eve uses the wrong basis, it doesn't matter whether her measurement outcome "matches" Alice's bit or not — she's encoding in the wrong basis either way, and Bob gets a random result.

Combining:

$$
P(\text{error}) = \underbrace{\frac{1}{2}}_{\text{Eve right basis}} \times 0 + \underbrace{\frac{1}{2}}_{\text{Eve wrong basis}} \times \underbrace{\frac{1}{2}}_{\text{Bob gets random}} = \frac{1}{4} = 25\%
$$

Let's verify this with a concrete table. For a qubit where Alice and Bob both use the Z-basis:

| Eve's basis | Eve disturbs state? | P(Bob error) | Contribution to QBER |
|:-----------:|:-------------------:|:------------:|:--------------------:|
| Z (correct) | No | 0 | $\frac{1}{2} \times 0 = 0$ |
| X (wrong)   | Yes | $\frac{1}{2}$ | $\frac{1}{2} \times \frac{1}{2} = \frac{1}{4}$ |
| **Total** | | | **25%** |

```{admonition} iClicker Check
:class: question
Alice sends $|+\rangle$ to Bob. Eve intercepts and measures in the Z-basis, getting $|0\rangle$. She sends $|0\rangle$ to Bob, who measures in the X-basis. What is the probability Bob gets the correct bit (0)?

- **(A)** 0%
- **(B)** 25%
- **(C)** 50%
- **(D)** 100%
```

Without Eve, the sifted key has 0% errors. With Eve performing intercept-resend on every qubit, the sifted key has **25% errors**. That's a huge, easily detectable gap. Alice and Bob just need to sacrifice a few dozen bits to check — if the error rate is significantly above zero, they know someone is listening and they abort.

```{admonition} Can Eve Be Sneakier?
:class: note
What if Eve only intercepts *some* of the qubits? Then the error rate scales linearly: if Eve intercepts a fraction $f$ of the qubits, the error rate is $f/4$. Alice and Bob can detect even small amounts of eavesdropping by checking enough bits. More sophisticated attacks exist (e.g., entangling a probe with each qubit), but information-theoretic security proofs show that **any** eavesdropping strategy introduces detectable errors. The security ultimately rests on the laws of quantum mechanics.
```

------------------------------------------------------------------------

## Part 4: BB84 in Qiskit

Let's implement the full protocol as a quantum circuit. We'll use 20 qubits and run on a simulator.

### Alice Prepares

```{code-cell} python
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator
import numpy as np

rng = np.random.default_rng(seed=2026)
num_qubits = 20

# Alice's random bits and bases
alice_bits  = rng.integers(0, 2, num_qubits)
alice_bases = rng.integers(0, 2, num_qubits)  # 0 = Z, 1 = X

qc = QuantumCircuit(num_qubits, num_qubits)

# Encode each qubit
for n in range(num_qubits):
    if alice_bits[n] == 1:
        qc.x(n)            # Flip to |1⟩
    if alice_bases[n] == 1:
        qc.h(n)            # Rotate to X-basis: |0⟩→|+⟩, |1⟩→|−⟩

qc.barrier()
```

The encoding logic: start with $|0\rangle$, apply $X$ if the bit is 1, then apply $H$ if using the X-basis. This gives us the four states: $|0\rangle$, $|1\rangle$, $|+\rangle$, $|-\rangle$.

### Bob Measures

```{code-cell} python
# Bob's random bases
bob_bases = rng.integers(0, 2, num_qubits)

# If Bob uses X-basis, apply H before measuring (rotates X-basis → Z-basis)
for n in range(num_qubits):
    if bob_bases[n] == 1:
        qc.h(n)
    qc.measure(n, n)
```

The full 20-qubit circuit is too wide to read, so let's draw a single-qubit example showing each of the four cases:

```{code-cell} python
:tags: [hide-input]
import matplotlib.pyplot as plt
fig, axes = plt.subplots(1, 4, figsize=(14, 1.8))

cases = [
    ("Z-basis, bit=0", [], []),             # |0⟩, measure Z
    ("Z-basis, bit=1", ["x"], []),           # |1⟩, measure Z
    ("X-basis, bit=0", ["h"], ["h"]),        # |+⟩, Bob measures X
    ("X-basis, bit=1", ["x", "h"], ["h"]),   # |−⟩, Bob measures X
]

for ax, (label, alice_gates, bob_gates) in zip(axes, cases):
    qc_ex = QuantumCircuit(1, 1)
    for g in alice_gates:
        getattr(qc_ex, g)(0)
    qc_ex.barrier()
    for g in bob_gates:
        getattr(qc_ex, g)(0)
    qc_ex.measure(0, 0)
    qc_ex.draw("mpl", ax=ax)
    ax.set_title(label, fontsize=9)

plt.tight_layout()
plt.show()
```

Notice the pattern: **$H$ before measurement = measure in the X-basis**. If Alice prepared in the X-basis ($H$ on her end) and Bob also uses the X-basis ($H$ on his end), the two Hadamards cancel: $HH = I$, and Bob's measurement in the Z-basis reads out Alice's original bit perfectly.

### Running and Sifting

```{code-cell} python
# Run on simulator (1 shot — this is a single key exchange)
simulator = AerSimulator()
job = simulator.run(qc, shots=1)
result = job.result()
counts = result.get_counts()
measured = list(counts.keys())[0]

# Qiskit returns bits in reverse order (qubit 0 is rightmost)
bob_bits = [int(measured[num_qubits - 1 - i]) for i in range(num_qubits)]

# Sift: keep only rounds where bases matched
sifted_indices = [i for i in range(num_qubits) if alice_bases[i] == bob_bases[i]]
sifted_alice = [int(alice_bits[i]) for i in sifted_indices]
sifted_bob   = [bob_bits[i] for i in sifted_indices]

# Check fidelity
matches = sum(a == b for a, b in zip(sifted_alice, sifted_bob))
fidelity = matches / len(sifted_alice) if sifted_alice else 0

print(f"Total qubits: {num_qubits}")
print(f"Bases matched: {len(sifted_indices)} ({100*len(sifted_indices)/num_qubits:.0f}%)")
print(f"Sifted key (Alice): {sifted_alice}")
print(f"Sifted key (Bob):   {sifted_bob}")
print(f"Fidelity: {fidelity:.1%}")
```

With no eavesdropper, the fidelity should be **100%** — every sifted bit agrees perfectly.

------------------------------------------------------------------------

## Part 5: Eve's Attack

Now let's add Eve. She intercepts each qubit, measures in a random basis, and re-prepares the state she measured before forwarding it to Bob.

We implement this as a two-circuit approach: the first circuit is Alice → Eve, and the second is Eve → Bob.

```{code-cell} python
# Fresh run with a different seed so it's independent
rng2 = np.random.default_rng(seed=9999)

alice_bits_e  = rng2.integers(0, 2, num_qubits)
alice_bases_e = rng2.integers(0, 2, num_qubits)

# ---- Circuit 1: Alice → Eve ----
qc1 = QuantumCircuit(num_qubits, num_qubits)

# Alice encodes
for n in range(num_qubits):
    if alice_bits_e[n] == 1:
        qc1.x(n)
    if alice_bases_e[n] == 1:
        qc1.h(n)

qc1.barrier()

# Eve measures in random bases
eve_bases = rng2.integers(0, 2, num_qubits)
for n in range(num_qubits):
    if eve_bases[n] == 1:
        qc1.h(n)
    qc1.measure(n, n)

# Run to get Eve's results
job1 = simulator.run(qc1, shots=1)
result1 = job1.result().get_counts()
measured1 = list(result1.keys())[0]
eve_bits = [int(measured1[num_qubits - 1 - i]) for i in range(num_qubits)]

# ---- Circuit 2: Eve → Bob ----
qc2 = QuantumCircuit(num_qubits, num_qubits)

# Eve re-prepares based on her measurement results and bases
for n in range(num_qubits):
    if eve_bits[n] == 1:
        qc2.x(n)
    if eve_bases[n] == 1:
        qc2.h(n)

qc2.barrier()

# Bob measures in random bases (same seed progression)
bob_bases_e = rng2.integers(0, 2, num_qubits)
for n in range(num_qubits):
    if bob_bases_e[n] == 1:
        qc2.h(n)
    qc2.measure(n, n)

# Run to get Bob's results
job2 = simulator.run(qc2, shots=1)
result2 = job2.result().get_counts()
measured2 = list(result2.keys())[0]
bob_bits_e = [int(measured2[num_qubits - 1 - i]) for i in range(num_qubits)]

# Sift (Alice and Bob announce bases)
sifted_idx_e = [i for i in range(num_qubits) if alice_bases_e[i] == bob_bases_e[i]]
sifted_alice_e = [int(alice_bits_e[i]) for i in sifted_idx_e]
sifted_bob_e   = [bob_bits_e[i] for i in sifted_idx_e]

matches_e = sum(a == b for a, b in zip(sifted_alice_e, sifted_bob_e))
fidelity_e = matches_e / len(sifted_alice_e) if sifted_alice_e else 0

print(f"=== With Eve eavesdropping ===")
print(f"Total qubits: {num_qubits}")
print(f"Bases matched (Alice-Bob): {len(sifted_idx_e)}")
print(f"Sifted key (Alice): {sifted_alice_e}")
print(f"Sifted key (Bob):   {sifted_bob_e}")
print(f"Fidelity: {fidelity_e:.1%}")
print(f"Error rate: {1 - fidelity_e:.1%}")
print(f"Expected error rate: ~25%")
```

```{admonition} iClicker Check
:class: question
In BB84, Alice and Bob compare a random subset of their sifted key and find a 23% error rate. What should they conclude?

- **(A)** Their key is fine — small errors are normal
- **(B)** There is likely an eavesdropper — they should abort
- **(C)** They need more qubits to decide
- **(D)** Bob's equipment is broken
```

The theoretical error rate with Eve is 25%, but with only 20 qubits and random sampling, you'll see fluctuations. Try running the cell a few times (change the seed) to see the distribution.

------------------------------------------------------------------------

## Part 6: Scaling Up

With 20 qubits, the statistics are noisy. Let's run with more qubits to see the error rates converge:

```{code-cell} python
def run_bb84(num_qubits, eavesdrop=False, seed=None):
    """Run BB84 protocol and return the fidelity."""
    rng = np.random.default_rng(seed=seed)
    sim = AerSimulator()

    alice_bits  = rng.integers(0, 2, num_qubits)
    alice_bases = rng.integers(0, 2, num_qubits)

    # Alice encodes
    qc = QuantumCircuit(num_qubits, num_qubits)
    for n in range(num_qubits):
        if alice_bits[n] == 1:
            qc.x(n)
        if alice_bases[n] == 1:
            qc.h(n)
    qc.barrier()

    if eavesdrop:
        # Eve intercepts
        eve_bases = rng.integers(0, 2, num_qubits)
        for n in range(num_qubits):
            if eve_bases[n] == 1:
                qc.h(n)
            qc.measure(n, n)

        # Get Eve's results
        job = sim.run(qc, shots=1)
        measured = list(job.result().get_counts().keys())[0]
        eve_bits = [int(measured[num_qubits - 1 - i]) for i in range(num_qubits)]

        # Eve re-prepares
        qc = QuantumCircuit(num_qubits, num_qubits)
        for n in range(num_qubits):
            if eve_bits[n] == 1:
                qc.x(n)
            if eve_bases[n] == 1:
                qc.h(n)
        qc.barrier()

    # Bob measures
    bob_bases = rng.integers(0, 2, num_qubits)
    for n in range(num_qubits):
        if bob_bases[n] == 1:
            qc.h(n)
        qc.measure(n, n)

    job = sim.run(qc, shots=1)
    measured = list(job.result().get_counts().keys())[0]
    bob_bits = [int(measured[num_qubits - 1 - i]) for i in range(num_qubits)]

    # Sift
    sifted = [(int(alice_bits[i]), bob_bits[i])
              for i in range(num_qubits)
              if alice_bases[i] == bob_bases[i]]

    if not sifted:
        return 1.0, 0
    matches = sum(a == b for a, b in sifted)
    return matches / len(sifted), len(sifted)

# Run multiple trials
print(f"{'Qubits':>8} {'No Eve':>12} {'With Eve':>12} {'Sifted (no Eve)':>16}")
print("-" * 52)
for n in [20, 50, 100, 200]:
    fid_clean, sift_clean = run_bb84(n, eavesdrop=False, seed=100)
    fid_eve, sift_eve     = run_bb84(n, eavesdrop=True,  seed=100)
    print(f"{n:>8} {fid_clean:>11.1%} {fid_eve:>11.1%} {sift_clean:>16}")
```

As the number of qubits increases, the error rate with Eve converges to 25%, while without Eve it stays at 0%. The gap is unmistakable.

------------------------------------------------------------------------

## Part 7: Using the Key — One-Time Pad Encryption

Once Alice and Bob have a shared secret key, they can use it for one-time pad encryption:

```{code-cell} python
def text_to_bits(text):
    """Convert text to a list of bits (8 bits per character, ASCII)."""
    bits = []
    for char in text:
        for i in range(7, -1, -1):
            bits.append((ord(char) >> i) & 1)
    return bits

def bits_to_text(bits):
    """Convert a list of bits back to text."""
    chars = []
    for i in range(0, len(bits), 8):
        byte = 0
        for j in range(8):
            byte = (byte << 1) | bits[i + j]
        chars.append(chr(byte))
    return "".join(chars)

def otp_encrypt(message_bits, key_bits):
    """XOR message with key."""
    return [m ^ k for m, k in zip(message_bits, key_bits)]

# Example
message = "QUANTUM"
msg_bits = text_to_bits(message)
key = [np.random.randint(0, 2) for _ in range(len(msg_bits))]

ciphertext = otp_encrypt(msg_bits, key)
decrypted  = otp_encrypt(ciphertext, key)  # XOR again to decrypt

print(f"Message:    '{message}'")
print(f"Key:        {key[:16]}... ({len(key)} bits)")
print(f"Ciphertext: {ciphertext[:16]}...")
print(f"Decrypted:  '{bits_to_text(decrypted)}'")
```

The ciphertext is indistinguishable from random noise without the key. And the key was distributed using quantum mechanics — no computational assumptions needed.

```{admonition} In-Class Activity: One-Time Pad
:class: tip
Pair up with a neighbor. Each of you pick a 4-letter word and convert each letter to a number (A=0, B=1, ..., Z=25). Agree on a 4-number key (each 0–25). To encrypt: add each letter-number to the corresponding key-number, mod 26. Exchange your ciphertexts. To decrypt: subtract the key, mod 26. Did you recover each other's words?
```

------------------------------------------------------------------------

## Part 8: Why BB84 Is Secure — The Quantum Ingredients

BB84's security rests on two theorems we proved in Lecture 3.5:

### 1. The No-Cloning Theorem

Eve cannot copy the qubits passing from Alice to Bob. If she could, she'd copy each one, keep one copy to measure later (after Alice announces the basis), and forward the original to Bob undisturbed. The no-cloning theorem makes this impossible.

### 2. Non-Distinguishability of Non-Orthogonal States

The four states Alice uses — $|0\rangle$, $|1\rangle$, $|+\rangle$, $|-\rangle$ — are **not** all mutually orthogonal. In particular, $|0\rangle$ and $|+\rangle$ have inner product $\langle 0|+\rangle = 1/\sqrt{2} \neq 0$. Because non-orthogonal states cannot be perfectly distinguished, Eve cannot determine Alice's basis with certainty from a single qubit. Any measurement she performs to try to learn the basis will sometimes disturb the state.

Together, these two facts mean that **any attempt by Eve to extract information from the qubits will inevitably introduce errors** that Alice and Bob can detect.

```{admonition} iClicker Check
:class: question
Which quantum principle is most directly responsible for BB84's security?

- **(A)** Superposition
- **(B)** Entanglement
- **(C)** No-cloning and measurement disturbance
- **(D)** The uncertainty principle
```

```{admonition} Practical Considerations
:class: note
Real-world QKD implementations face additional challenges: photon loss in fiber optics, detector dark counts, and the need for authenticated classical channels. The raw key from BB84 goes through **privacy amplification** — a classical post-processing step that shortens the key but removes any partial information Eve might have. BB84 has been demonstrated over fiber-optic links exceeding 400 km and even via satellite (the Micius experiment by the Chinese Academy of Sciences in 2017).
```

------------------------------------------------------------------------

## Summary

Today we saw how quantum mechanics solves a problem that classical physics cannot:

- The **one-time pad** is provably secure but requires a shared secret key.
- Classical key distribution is vulnerable to eavesdropping because bits can be copied.
- **BB84** distributes keys using qubits in two non-orthogonal bases (Z and X).
- An eavesdropper is forced to measure, which disturbs the states and introduces a detectable **25% error rate** in the sifted key.
- Security relies on the **no-cloning theorem** and the **non-distinguishability of non-orthogonal states** — both consequences of the linearity of quantum mechanics.

Next time: we'll see how entanglement enables even more surprising protocols — **quantum teleportation** and **superdense coding**.
