---
kernelspec:
  name: python3
  display_name: Python 3
---

# Lecture 4.3 — Quantum Key Distribution

## Where We Left Off

We've built up a powerful toolkit: qubits, gates, circuits, entanglement, and the no-cloning theorem. We proved that quantum mechanics gives a measurable advantage in the CHSH game — entanglement lets Alice and Bob win 85.4% of the time, beating any classical strategy.

Today we put all of this to practical use. We'll see how the laws of quantum mechanics — specifically the impossibility of cloning unknown states and the disturbance caused by measurement — enable a **provably secure** method for sharing secret keys — secure under the laws of quantum mechanics, given the assumptions of the protocol and devices. This is the BB84 protocol, invented by Charles Bennett and Gilles Brassard in 1984, and it's one of the first real-world applications of quantum information.

We'll also see why the textbook version isn't the whole story — real systems face challenges that required new protocols and clever engineering to solve.

```{admonition} iClicker — Warm-Up
:class: question
What does the no-cloning theorem say?

A) You can't measure a quantum state without disturbing it &emsp; B) You can't copy an arbitrary unknown quantum state

C) You can't entangle two qubits without a CNOT gate &emsp; D) You can't send information faster than light
```

```{admonition} iClicker — Warm-Up
:class: question
Alice and Bob share a Bell pair $|\Phi^+\rangle$ and both measure in the Z-basis. Their outcomes are:

A) Always the same &emsp; B) Always opposite &emsp; C) Same or opposite depending on the Bell state &emsp; D) Completely uncorrelated
```

------------------------------------------------------------------------

## Part 1: The Key Distribution Problem

### Why Do We Need Quantum Cryptography?

Suppose Alice wants to send Bob a secret message. The standard approach is encryption: Alice scrambles the message with a **key**, sends the scrambled message over a public channel, and Bob unscrambles it with the same key.

The strongest classical encryption scheme is the **one-time pad (OTP)**. Alice and Bob share a random key that is as long as the message. To encrypt, Alice XORs each bit of her message with the corresponding key bit:

$$
\text{ciphertext} = \text{message} \oplus \text{key}
$$

To decrypt, Bob XORs again:

$$
\text{message} = \text{ciphertext} \oplus \text{key}
$$

The one-time pad is **provably unbreakable** — as long as the key is truly random, as long as the message, and used only once. An eavesdropper who intercepts the ciphertext has literally zero information about the message.

### The Catch

The one-time pad shifts the problem rather than solving it. To send a secret message, Alice and Bob first need a secret key — and that key needs to be as long as the message itself. How do they share it securely?

If they meet in person, that works — but it's impractical for most real-world communication. If they send the key over the internet, an eavesdropper can copy it without detection. Classical bits can be copied freely.

This is the **key distribution problem**, and it's the central weakness of classical cryptography.

```{admonition} Classical vs. Quantum Security
:class: tip
Modern internet encryption (RSA, AES) sidesteps the key distribution problem by using **computational** assumptions — factoring large numbers is *believed* to be hard. But it's not *proven* to be hard, and a sufficiently powerful quantum computer running Shor's algorithm could break RSA. The one-time pad combined with QKD is secure based on the **laws of physics**, not computational assumptions. No future algorithm or computer can break it.
```

```{admonition} In-Class Activity: One-Time Pad
:class: tip
Pair up with a neighbor. Each of you pick a 4-letter word and convert each letter to a number (A=0, B=1, ..., Z=25). Agree on a 4-number key (each 0–25). To encrypt: add each letter-number to the corresponding key-number, mod 26. Exchange your ciphertexts. To decrypt: subtract the key, mod 26. Did you recover each other's words?
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
| Z | $\lvert 0\rangle$ | $\lvert 1\rangle$ |
| X | $\lvert +\rangle$ | $\lvert -\rangle$ |

Bob measures each qubit in a randomly chosen basis. The crucial point: if Bob measures in the **same** basis Alice used, he gets the correct bit with certainty. If he measures in the **wrong** basis, he gets a random result — 50/50.

This is exactly the Stern-Gerlach physics from Chapter 2. Measuring a Z-basis state in the X-basis is like measuring a spin-up-in-$z$ particle along $x$ — you get a completely random outcome.

### The Protocol

**Step 1 — Alice prepares:** Alice generates a string of random bits and, for each bit, randomly chooses Z or X basis. She prepares the corresponding qubit state and sends it to Bob through a quantum channel (e.g., a fiber-optic cable carrying single photons).

**Step 2 — Bob measures:** Bob independently chooses a random basis (Z or X) for each qubit and measures. He records his measurement outcomes.

**Step 3 — Basis reconciliation:** Over a public classical channel (phone, internet — no secrecy needed), Alice and Bob announce which **basis** they used for each qubit. They do NOT reveal their bit values. They keep only the rounds where their bases matched and discard the rest.

Since each independently picks Z or X with equal probability, their bases match about **50%** of the time. So roughly half the qubits are discarded — but the remaining bits should be identical on both sides.

**Step 4 — Verification:** Alice and Bob sacrifice a small random subset of their sifted key and compare those bits publicly. In the ideal noiseless model, these should agree perfectly; in real systems there is always some nonzero background QBER from detector noise and channel imperfections. If an eavesdropper has tampered with the qubits, additional errors will appear above this baseline.

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

```{admonition} iClicker
:class: question
Alice prepares $|+\rangle$ (X-basis, bit = 0). Bob measures in the X-basis. What is the probability Bob's result matches Alice's bit?

A) 0% &emsp; B) 50% &emsp; C) 75% &emsp; D) 100%
```

```{admonition} iClicker
:class: question
Alice sends $|+\rangle$ (encoding bit = 0 in the X-basis) to Bob. Bob measures in the Z-basis. What is the probability Bob's result matches Alice's bit?

A) 0% &emsp; B) 25% &emsp; C) 50% &emsp; D) 100%
```

------------------------------------------------------------------------

## Part 3: Why Eavesdropping Fails

### Eve's Dilemma

Suppose an eavesdropper, Eve, intercepts the qubits in transit. She wants to learn Alice's bits without being detected. Her best classical-style attack is **intercept-resend**: she measures each qubit, reads the result, and sends a fresh qubit to Bob in the state she measured.

But Eve has a problem: she doesn't know Alice's basis. For each qubit, she has to guess — Z or X — and measure accordingly.

```{admonition} Connection to No-Cloning
:class: tip
Why can't Eve just copy the qubit, measure her copy, and forward the original? Because of the **no-cloning theorem** from Lecture 4.1! There is no physical process that can duplicate an arbitrary unknown quantum state. Eve is forced to measure the original, which disturbs it.
```

### QBER: The Quantum Bit Error Rate

Before analyzing Eve's attack, let's name the key quantity. The **Quantum Bit Error Rate (QBER)** is the fraction of sifted bits on which Alice and Bob disagree:

$$
\text{QBER} = \frac{\text{number of disagreements in sifted key}}{\text{total sifted key length}}
$$

A low QBER means the channel is clean. A high QBER means something is wrong — noise, detector problems, or eavesdropping. The critical point is that if the QBER is too high, Alice and Bob cannot extract a secure key and must abort.

### The 25% Error Rate

Let's calculate the QBER when Eve performs intercept-resend on every qubit. Consider only the sifted bits — rounds where Alice and Bob used the same basis.

**Case 1 — Eve guesses the right basis** (probability $1/2$): Eve measures in the same basis Alice used, gets the correct bit, and re-prepares the correct state. Bob measures correctly. **QBER contribution: 0%.**

**Case 2 — Eve guesses the wrong basis** (probability $1/2$): Eve's measurement collapses the state into the wrong basis. She re-prepares a state in the wrong basis and sends it to Bob. When Bob measures in the correct basis (matching Alice), he gets a random result. **QBER contribution: 50%.**

Combining:

$$
\text{QBER} = \underbrace{\frac{1}{2}}_{\text{Eve right}} \times 0 + \underbrace{\frac{1}{2}}_{\text{Eve wrong}} \times \underbrace{\frac{1}{2}}_{\text{Bob random}} = \frac{1}{4} = 25\%
$$

| Eve's basis | Disturbs state? | P(Bob error) | Contribution to QBER |
|:-----------:|:-------------------:|:------------:|:--------------------:|
| Correct | No | 0 | $\frac{1}{2} \times 0 = 0$ |
| Wrong   | Yes | $\frac{1}{2}$ | $\frac{1}{2} \times \frac{1}{2} = \frac{1}{4}$ |
| **Total** | | | **25%** |

In the ideal noiseless model, the QBER without Eve is 0%. With Eve intercepting every qubit, the QBER is **25%**. That's a huge, easily detectable gap.

```{admonition} iClicker
:class: question
Alice sends $|+\rangle$ to Bob. Eve intercepts and measures in the Z-basis, getting $|0\rangle$. She sends $|0\rangle$ to Bob, who measures in the X-basis. What is the probability Bob gets the correct bit (0)?

A) 0% &emsp; B) 25% &emsp; C) 50% &emsp; D) 100%
```

### The Two Pillars of BB84 Security

BB84's security rests on two quantum mechanical facts:

**1. The No-Cloning Theorem.** Eve cannot copy the qubits. If she could, she'd keep a copy, forward the original to Bob undisturbed, and measure her copy later after Alice announces the basis.

**2. Non-Distinguishability of Non-Orthogonal States.** The four states Alice uses — $|0\rangle$, $|1\rangle$, $|+\rangle$, $|-\rangle$ — are not all mutually orthogonal. In particular, $\langle 0|+\rangle = 1/\sqrt{2} \neq 0$. Because non-orthogonal states cannot be perfectly distinguished by any single measurement, Eve cannot determine Alice's basis from one qubit. Any measurement she performs to try will sometimes disturb the state.

Together: **any attempt by Eve to extract information will inevitably introduce errors that Alice and Bob can detect.**

```{admonition} Can Eve Be Sneakier?
:class: note
What if Eve only intercepts *some* of the qubits? Then the QBER scales linearly: if Eve intercepts a fraction $f$ of the qubits, the QBER is $f/4$. Alice and Bob can detect even small amounts of eavesdropping by checking enough bits. More sophisticated attacks exist, but information-theoretic security proofs (Shor & Preskill, 2000) show that **any** eavesdropping strategy introduces detectable errors. The security ultimately rests on the laws of quantum mechanics.
```

```{admonition} iClicker
:class: question
Eve performs intercept-resend on every qubit. What is the QBER in the sifted key?

A) 0% &emsp; B) 12.5% &emsp; C) 25% &emsp; D) 50%
```

```{admonition} iClicker
:class: question
The security of BB84 ultimately rests on:

A) The difficulty of factoring large numbers &emsp; B) The speed of light

C) The laws of quantum mechanics &emsp; D) The randomness of thermal noise
```

------------------------------------------------------------------------

## Part 4: BB84 in Qiskit

Let's implement the protocol as a quantum circuit and verify the error rates.

### Alice Prepares, Bob Measures

The encoding logic: start with $|0\rangle$, apply $X$ if the bit is 1, then apply $H$ if using the X-basis. Bob applies $H$ before measuring if he chose the X-basis — this rotates back to the Z-basis so the standard measurement gives the right answer.

When Alice and Bob both use the X-basis, the two Hadamards cancel: $HH = I$, and the measurement reads out Alice's original bit perfectly.

```{code-cell} python
:tags: [hide-input]
import matplotlib.pyplot as plt

fig, axes = plt.subplots(1, 4, figsize=(14, 1.8))

from qiskit import QuantumCircuit

cases = [
    ("Z-basis, bit=0", [], []),
    ("Z-basis, bit=1", ["x"], []),
    ("X-basis, bit=0", ["h"], ["h"]),
    ("X-basis, bit=1", ["x", "h"], ["h"]),
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

### Running the Protocol

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
        qc.x(n)
    if alice_bases[n] == 1:
        qc.h(n)

qc.barrier()

# Bob's random bases and measurement
bob_bases = rng.integers(0, 2, num_qubits)
for n in range(num_qubits):
    if bob_bases[n] == 1:
        qc.h(n)
    qc.measure(n, n)

# Run on simulator (1 shot — single key exchange)
simulator = AerSimulator()
job = simulator.run(qc, shots=1)
result = job.result()
counts = result.get_counts()
measured = list(counts.keys())[0]

# Qiskit returns bits in reverse order
bob_bits = [int(measured[num_qubits - 1 - i]) for i in range(num_qubits)]

# Sift: keep only matching bases
sifted_indices = [i for i in range(num_qubits) if alice_bases[i] == bob_bases[i]]
sifted_alice = [int(alice_bits[i]) for i in sifted_indices]
sifted_bob   = [bob_bits[i] for i in sifted_indices]

matches = sum(a == b for a, b in zip(sifted_alice, sifted_bob))
fidelity = matches / len(sifted_alice) if sifted_alice else 0

print(f"Total qubits: {num_qubits}")
print(f"Bases matched: {len(sifted_indices)} ({100*len(sifted_indices)/num_qubits:.0f}%)")
print(f"Sifted key (Alice): {sifted_alice}")
print(f"Sifted key (Bob):   {sifted_bob}")
print(f"QBER: {1 - fidelity:.1%}")
```

In the ideal protocol and noiseless simulation, the QBER is **0%** — every sifted bit agrees perfectly.

### Adding Eve

Now let's add an eavesdropper. Eve intercepts each qubit, measures in a random basis, and re-prepares the state she measured.

```{code-cell} python
def run_bb84(num_qubits, eavesdrop=False, seed=None):
    """Run BB84 protocol. Returns (QBER, number of sifted bits)."""
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
        # Eve intercepts and measures
        eve_bases = rng.integers(0, 2, num_qubits)
        for n in range(num_qubits):
            if eve_bases[n] == 1:
                qc.h(n)
            qc.measure(n, n)

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
        return 0.0, 0
    errors = sum(a != b for a, b in sifted)
    return errors / len(sifted), len(sifted)
```

```{code-cell} python
# Run multiple trials at different sizes
print(f"{'Qubits':>8} {'QBER (no Eve)':>14} {'QBER (with Eve)':>16} {'Sifted bits':>12}")
print("-" * 54)
for n in [20, 50, 100, 200]:
    qber_clean, sift_clean = run_bb84(n, eavesdrop=False, seed=100)
    qber_eve, sift_eve     = run_bb84(n, eavesdrop=True,  seed=100)
    print(f"{n:>8} {qber_clean:>13.1%} {qber_eve:>15.1%} {sift_clean:>12}")
```

As the number of qubits increases, the QBER with Eve converges to 25%, while without Eve it stays at 0%. The gap is unmistakable.

------------------------------------------------------------------------

## Part 5: From Ideal to Real

### The Weak Coherent Pulse Problem

In the ideal BB84 story, Alice sends exactly one photon per round. But building a source that reliably produces exactly one photon on demand is extremely difficult. What real QKD systems use instead is a much simpler device: an **attenuated laser**.

A laser pulse, even when heavily attenuated, doesn't produce a definite number of photons. The photon number follows a **Poisson distribution**:

$$
P(n) = \frac{e^{-\mu} \mu^n}{n!}
$$

where $\mu$ is the mean photon number per pulse. In a typical QKD system, $\mu \approx 0.1$–$0.5$. Let's see what that looks like:

```{code-cell} python
:tags: [hide-input]
import matplotlib.pyplot as plt
import numpy as np
from math import factorial

fig, ax = plt.subplots(figsize=(7, 3.5))

for mu, color in [(0.1, '#1f77b4'), (0.3, '#ff7f0e'), (0.5, '#2ca02c')]:
    ns = np.arange(0, 6)
    probs = [np.exp(-mu) * mu**n / factorial(n) for n in ns]
    ax.bar(ns + {'0.1': -0.25, '0.3': 0, '0.5': 0.25}[str(mu)],
           probs, width=0.22, label=f'μ = {mu}', color=color)

ax.set_xlabel('Photon number n')
ax.set_ylabel('Probability P(n)')
ax.set_title('Photon number distribution for weak coherent pulses')
ax.legend()
ax.set_xticks(range(6))
plt.tight_layout()
plt.show()
```

Most pulses contain **zero** photons (vacuum — useless). Many contain exactly one photon (good — this is what BB84 needs). But some contain **two or more** photons, and that's a security problem.

### The Photon-Number-Splitting Attack

If a pulse contains two photons, both encode the same polarization (the same bit). Eve can exploit this with a **photon-number-splitting (PNS) attack**:

1. Eve measures the photon number of each pulse without disturbing the polarization. (In the security analysis, we allow Eve any attack permitted by quantum mechanics, including ideal quantum non-demolition measurements. This is an adversarial upper bound, not a lab instruction manual.)
2. If it contains only one photon, she lets it pass (or blocks it — Bob attributes this to normal fiber loss).
3. If it contains two photons, she **keeps one** and forwards the other to Bob.
4. After Alice and Bob publicly announce their bases, Eve measures her stored photon in the correct basis.

Eve learns the bit with certainty. **No QBER is introduced.** This is fundamentally different from the intercept-resend attack — Eve isn't guessing a basis and disturbing the state. She's exploiting the fact that the source sometimes emits more than one photon.

```{admonition} Why not just turn down the intensity?
:class: note
If multi-photon pulses are the problem, why not make $\mu$ extremely small? Because lowering $\mu$ makes most pulses vacuum — Bob almost never detects anything, and the key rate collapses. Without a fix, you're stuck in an unpleasant tradeoff: useful key rate vs. multi-photon security risk.
```

```{admonition} iClicker
:class: question
What makes the PNS attack different from intercept-resend?

A) Eve introduces a higher QBER &emsp; B) Eve introduces no QBER — she gets information without detectable errors

C) Eve needs entanglement to perform it &emsp; D) Eve can only attack Z-basis qubits
```

### The Decoy-State Fix

The breakthrough solution is the **decoy-state protocol** (Hwang 2003; Lo, Ma, Chen 2005). The idea: Alice still does BB84, but she randomly varies the **intensity** of each pulse.

She uses several intensity settings — for example:

- **Signal** intensity ($\mu \approx 0.5$): these pulses generate the key
- **Decoy** intensity ($\mu' \approx 0.1$): these pulses probe the channel
- **Vacuum** ($\mu = 0$): these measure the detector dark count rate

Different intensities have different photon-number distributions. If Eve uses a PNS strategy (treating single-photon and multi-photon pulses differently), the detection rates for signal and decoy pulses will be **inconsistent** with what's expected from a normal lossy channel. Alice and Bob can detect this statistically. The crucial observables are not just the error rates, but the **detection rates for each intensity class** — that's what lets them bound how many single-photon events actually contributed to the key.

```{admonition} The Key Insight
:class: important
**Signal pulses make the key. Decoy pulses tell Alice and Bob how much of the detected data really came from safe single-photon events.**

Decoy-state BB84 is what virtually all deployed QKD systems use today.
```

```{admonition} iClicker
:class: question
Alice and Bob run BB84 and find QBER = 0.1%. What's the most likely explanation?

A) Eve is intercepting 0.4% of qubits &emsp; B) Normal detector noise and channel imperfections

C) Bob is using the wrong basis every time &emsp; D) The no-cloning theorem has been violated
```

### The Protocol Landscape

BB84 was just the beginning. Over the past 40 years, the field developed several protocols to address different vulnerabilities. Here's the hierarchy:

```{admonition} What's core and what's enrichment
:class: important
For this course, the essential protocols are **BB84** and **entanglement-based QKD (E91/BBM92)**. You should understand how each works and why they're secure. Decoy states, MDI-QKD, and DI-QKD are important extensions — I want you to know the **basic idea** of each, but we won't test you on the details.
```

| Protocol | Core idea | What it fixes |
|----------|-----------|---------------|
| **BB84** | Prepare-and-measure | Original protocol |
| **Decoy-state BB84** | BB84 + varying pulse intensities | Multi-photon / PNS attacks |
| **E91 / BBM92** | Entanglement-based, shared Bell pairs | Security from quantum correlations |
| **MDI-QKD** | Both send to untrusted middle node | Detector side-channel attacks |
| **DI-QKD** | Security from Bell violation alone | Trust nothing — even your own devices |

**E91 / BBM92** (Ekert 1991; Bennett, Brassard, Mermin 1992) takes a fundamentally different approach. Instead of Alice preparing states and sending them to Bob, a source distributes entangled Bell pairs — one photon to each side. Alice and Bob measure their halves in randomly chosen bases. The conceptual picture: when their bases match, the entangled correlations produce correlated outcomes that become the key. A subset of their data is used to run a **Bell test** — if the CHSH inequality is violated, the entanglement is genuine and no eavesdropper has tampered with it. This is the same CHSH game from Lecture 4.2, now used as a security test. (The precise basis structure and which rounds are used for key vs. parameter estimation differ between E91 and BBM92; BBM92 is the simpler experimental version.)

**MDI-QKD** (measurement-device-independent) addresses a practical problem: real detectors can be hacked. Alice and Bob *both* prepare states and send them to a central node, Charlie, who performs a Bell-state measurement. **Charlie can be Eve** — it doesn't matter. The protocol is still secure, eliminating all detector side-channel attacks.

**DI-QKD** (device-independent) is the most ambitious idea. Security is certified by Bell inequality violation alone — you don't even need to trust that your own devices work as advertised. Loophole-closing building blocks and early DI-QKD demonstrations have appeared only very recently (2022–2023), and the approach remains extremely challenging to implement.

```{admonition} A Clean Summary
:class: tip
BB84 trusts your devices. Decoy-state BB84 trusts your devices but handles imperfect sources. MDI-QKD trusts your source but not your detectors. DI-QKD trusts nothing except the laws of physics.
```

------------------------------------------------------------------------

## Part 6: QKD in the Real World

We've learned the protocol logic — BB84, decoy states, entanglement-based QKD, and different levels of device trust. Now we ask: what did real experiments actually build, what hardware did they use, and what limits were they trying to overcome?

Three experiments tell the story cleanly. Each one solved a different physical problem.

### Experiment 1: Micius Satellite — Escaping the Fiber (2017)

The first experiment to highlight is the **Micius satellite**, operated by the Chinese Academy of Sciences. The paper is Liao et al., *Nature* (2017).

Micius carried a transmitter implementing **decoy-state BB84**. It sent weak coherent pulses from low Earth orbit down to a ground station. The results: QKD over distances up to **1,200 km**, with a secure key rate exceeding **1 kilohertz**.

Why go to space? The answer is simple: fiber is glass, and glass absorbs photons. At telecom wavelengths, fiber attenuation is about 0.2 dB/km. That means the transmission drops exponentially:

$$
\eta = 10^{-\alpha L / 10}
$$

Let's see what that looks like:

```{code-cell} python
:tags: [hide-input]
import numpy as np
import matplotlib.pyplot as plt

distances = np.linspace(0, 500, 1000)
alpha = 0.2  # dB/km
transmission = 10**(-alpha * distances / 10)

fig, ax = plt.subplots(figsize=(7, 3.5))
ax.semilogy(distances, transmission * 100)
ax.set_xlabel('Distance (km)')
ax.set_ylabel('Transmission (%)')
ax.set_title('Photon survival probability in optical fiber')
ax.axhline(y=1, color='gray', linestyle='--', alpha=0.5)
ax.text(110, 1.5, '1% (100 km)', fontsize=9, color='gray')
ax.axhline(y=0.01, color='gray', linestyle='--', alpha=0.5)
ax.text(210, 0.015, '0.01% (200 km)', fontsize=9, color='gray')
ax.set_xlim(0, 500)
ax.set_ylim(1e-5, 110)
plt.tight_layout()
plt.show()
```

At 100 km, only 1% of photons arrive. At 200 km, it's one in 10,000. At 300 km, one in a million. Fiber is an exponential killer.

A satellite link avoids this entirely. The 1,200 km path from Micius to the ground station passes mostly through **vacuum** — only the last ~10 km is atmosphere. The satellite channel is roughly **$10^{20}$ times more efficient** than fiber at that distance. That number is worth pausing on: twenty orders of magnitude.

```{admonition} iClicker
:class: question
Fiber optic loss is about 0.2 dB/km. Roughly what fraction of photons survive 100 km of fiber?

A) 50% &emsp; B) 10% &emsp; C) 1% &emsp; D) 0.001%
```

This 2017 *Nature* paper was **not** entanglement-based — the protocol was standard decoy-state BB84, the same protocol we discussed in Part 5. (Students often conflate "satellite QKD" with "satellite entanglement," so it's worth being explicit.) The source was eight fiber-coupled laser diodes pulsed at 100 MHz, producing weak coherent pulses at 850 nm. The hard part was not the quantum protocol. It was the engineering: **pointing, tracking, and timing** an optical link from a satellite moving at 7.5 km/s to a telescope on the ground.

This is an important lesson: in many real QKD systems, the challenge is not inventing a new protocol. It's making an existing one survive the physical channel.

```{admonition} Micius Entanglement-Based QKD
:class: note
Micius also carried an entangled-photon source — a PPKTP crystal producing photon pairs via spontaneous parametric down-conversion (SPDC) at ~810 nm, generating 5.9 million entangled pairs per second. In 2020, Yin et al. (*Nature*) used this source to demonstrate entanglement-based QKD between **two ground stations** separated by 1,120 km, with a key rate of 0.12 bits/sec. This is the entanglement-based paradigm (E91/BBM92) implemented on real hardware — no trusted relay needed.
```

### Experiment 2: Boaron et al. — Pushing Fiber to the Limit (2018)

If Micius showed how to escape fiber, **Boaron et al.** (*PRL*, 2018) showed how far you can push fiber before you hit the wall. Their result: secure BB84 over **421 km** of ultralow-loss fiber, with a key rate of 6.5 bits/sec at 405 km.

They used a 2.5 GHz system with a three-state time-bin encoding protocol and one decoy intensity. Instead of encoding in photon polarization (which drifts unpredictably over long fiber), they encode in the **arrival time** of the photon — early or late within a pulse window. The detectors were **superconducting nanowire single-photon detectors (SNSPDs)** — the highest-performance single-photon detectors available, with ultralow dark counts and excellent timing resolution. The fiber was specially selected for minimal loss.

This is the right contrast with Micius. In Micius, the protocol is standard BB84 and the channel is free-space from orbit. In Boaron, the channel is fiber, the encoding is time-bin instead of polarization, and the detectors must be superconducting because long-distance fiber QKD needs every advantage in signal-to-noise. The choice of channel drives the choice of hardware.

421 km is roughly the hard limit for "ordinary" point-to-point fiber QKD. Beyond that, the signal is simply too weak — even perfect detectors can't find the photons in the noise. Breaking through required a fundamentally different approach.

### Experiment 3: Liu et al. — Twin-Field QKD Beyond 1,000 km (2023)

The breakthrough came from a **protocol innovation**, not just better hardware. **Liu et al.** (*PRL*, 2023) demonstrated twin-field QKD over **1,002 km** of fiber — more than double the direct-fiber record.

How does twin-field QKD work? Instead of Alice sending photons directly to Bob, **both Alice and Bob send weak pulses to a central station**, where the pulses interfere on a beam splitter. A single-photon detection at the central station — without being able to tell which side the photon came from — generates a shared bit.

This should sound familiar. The central station is doing **single-photon interference**, just like the Mach-Zehnder interferometer from Chapter 2. The key difference is that the two "arms" of the interferometer are each 500 km of fiber.

The physics payoff is dramatic. In ordinary point-to-point QKD, the key rate scales with the channel transmission: $K \sim \eta$. In twin-field QKD, it scales as $K \sim \sqrt{\eta}$. Taking the square root of an exponentially small number makes an enormous difference:

| Distance | $\eta$ (ordinary) | $\sqrt{\eta}$ (twin-field) |
|----------|--------------------|----------------------------|
| 200 km | $10^{-4}$ | $10^{-2}$ |
| 400 km | $10^{-8}$ | $10^{-4}$ |
| 1000 km | $10^{-20}$ | $10^{-10}$ |

This improved scaling is why twin-field QKD could reach 1,002 km — it beats the **repeaterless bound**, a fundamental limit on how much secret key you can extract from a lossy channel without a quantum repeater (Pirandola et al., *Nature Communications*, 2017).

The key rate at 1,002 km was tiny: about 0.003 bits per second ($\sim 10^{-11}$ bits per pulse). Students sometimes ask: if the rate is that low, does it really count? The answer is yes — because it answered the question **"can you do it at all?"** Once the proof of principle exists, the engineering question becomes how to improve the rate.

### Three Problems, Three Solutions

| Experiment | Channel | Physical problem solved | Distance | Key rate |
|------------|---------|------------------------|----------|----------|
| Micius (2017) | Satellite → ground | Fiber loss is hopeless at global distances → go to space | 1,200 km | ~kHz |
| Boaron (2018) | Optical fiber | How far can great hardware push direct fiber QKD? | 421 km | 6.5 bps |
| Liu (2023) | Optical fiber | Can a smarter protocol beat the repeaterless scaling? | 1,002 km | 0.003 bps |

### What's Missing: Quantum Repeaters

All of today's long-distance QKD — whether satellite or fiber — is limited in some way. Satellite QKD requires line-of-sight. Fiber QKD hits the loss wall. Twin-field QKD pushed the wall further, but it's still there.

The long-term route to scalable quantum networks is expected to involve **quantum repeaters**: a device that extends entanglement over long distances by **chaining together short entangled links**. The idea is to create entanglement over manageable distances (say 50 km each), then use **entanglement swapping** — which is just teleportation applied to one half of each pair — to connect the links into one long-distance entangled pair.

Nobody has deployed a quantum repeater for real-world QKD yet. The experimental frontier is ion-ion entanglement over 10 km of fiber (Liu et al., *Nature*, 2026). But the concept is clear: quantum repeaters require **teleportation**, which is exactly the subject of our next lecture.

------------------------------------------------------------------------

## Summary

- **The key distribution problem:** the one-time pad is provably secure, but requires a shared secret key that classical channels can't distribute safely.

- **BB84:** Alice prepares qubits in two non-orthogonal bases (Z and X), Bob measures in a random basis. Sifting keeps matching-basis rounds. Security rests on the **no-cloning theorem** and the **non-distinguishability of non-orthogonal states**.

- **QBER:** the fraction of sifted bits that disagree. Eve's intercept-resend attack introduces QBER = 25%. Any eavesdropping introduces detectable errors.

- **Real sources use weak coherent pulses**, not single photons. This creates a **photon-number-splitting** vulnerability. The **decoy-state** protocol fixes it by randomly varying pulse intensities.

- **Protocol hierarchy:** BB84 → decoy-state BB84 → E91 (entanglement-based, security from Bell violation) → MDI-QKD (untrusted detectors) → DI-QKD (trust nothing).

- **Fiber QKD** is limited by exponential photon loss (~0.2 dB/km). **Satellite QKD** (Micius) bypasses this by transmitting through vacuum.

- **Quantum repeaters** — which use entanglement swapping (chained teleportation) — are the missing technology for long-distance quantum networks. That brings us to our next lecture.

------------------------------------------------------------------------

## What's Next

Next lecture: **Quantum Teleportation and Superdense Coding** — how to transmit a quantum state using entanglement and classical communication. Teleportation is the key primitive for quantum repeaters and the future quantum internet.

------------------------------------------------------------------------

## References

- C. H. Bennett and G. Brassard, "Quantum cryptography: Public key distribution and coin tossing," *Proceedings of IEEE International Conference on Computers, Systems, and Signal Processing*, Bangalore, India (1984).
- A. K. Ekert, "Quantum cryptography based on Bell's theorem," *Phys. Rev. Lett.* **67**, 661 (1991).
- W.-Y. Hwang, "Quantum key distribution with high loss: Toward global secure communication," *Phys. Rev. Lett.* **91**, 057901 (2003).
- H.-K. Lo, X. Ma, and K. Chen, "Decoy state quantum key distribution," *Phys. Rev. Lett.* **94**, 230504 (2005).
- P. W. Shor and J. Preskill, "Simple proof of security of the BB84 quantum key distribution protocol," *Phys. Rev. Lett.* **85**, 441 (2000).
- S.-K. Liao et al., "Satellite-to-ground quantum key distribution," *Nature* **549**, 43 (2017).
- J. Yin et al., "Entanglement-based secure quantum cryptography over 1,120 kilometres," *Nature* **582**, 501 (2020).
- S.-K. Liao et al., "Satellite-relayed intercontinental quantum network," *Phys. Rev. Lett.* **120**, 030501 (2018).
- A. Boaron et al., "Secure quantum key distribution over 421 km of optical fiber," *Phys. Rev. Lett.* **121**, 190502 (2018).
- Y. Liu et al., "Experimental twin-field quantum key distribution over 1000 km fiber distance," *Phys. Rev. Lett.* **130**, 210801 (2023).
- S. Pirandola et al., "Fundamental limits of repeaterless quantum communications," *Nature Communications* **8**, 15043 (2017).
