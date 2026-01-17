# Lecture 3.2: Entanglement and EPR

## The Strangeness of Quantum Correlations

Last lecture, we learned the mathematics of two-qubit states: tensor products, product states, entangled states, Bell states. We saw that in a Bell state like $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$, measuring one qubit instantly determines the other.

Today we dig into what this means — and why Einstein thought it proved quantum mechanics must be incomplete.

This isn't just philosophy. Understanding what's strange about entanglement is essential for understanding why quantum computers can do things classical computers cannot. The weirdness is the feature, not a bug.

---

## Historical Context: A 90-Year Debate

```{admonition} Timeline: From EPR to Nobel Prize
:class: note

**1935:** Einstein, Podolsky, and Rosen publish their famous paper arguing quantum mechanics is "incomplete." Bohr responds, but the debate seems philosophical — no experiment can decide.

**1935-1964:** Most physicists adopt "shut up and calculate." The EPR puzzle is dismissed as philosophy, not physics.

**1964:** John Bell proves that any local hidden variable theory must satisfy certain inequalities. Suddenly the question is experimentally testable!

**1972:** Clauser and Freedman perform the first Bell test. Result: quantum mechanics wins.

**1982:** Alain Aspect closes the "locality loophole" with fast-switching measurements. Quantum mechanics wins again.

**2015:** Three groups simultaneously close all major loopholes. Definitive result.

**2022:** Clauser, Aspect, and Zeilinger receive the Nobel Prize in Physics "for experiments with entangled photons, establishing the violation of Bell inequalities."

What started as a philosophical argument became one of the most precisely tested predictions in all of physics.
```

---

## A Classical Analogy: Bertlmann's Socks

Before we get to quantum mechanics, let's think about classical correlations.

The physicist John Bell told a story about his colleague Reinhold Bertlmann, who had a habit of wearing mismatched socks — one pink, one not pink (say, blue).

Suppose Bertlmann puts on his socks in the morning, then his two feet go separate ways. Later, you see his left foot and notice a pink sock.

**Question:** What color is his right sock?

**Answer:** Not pink (blue). You know this instantly, with certainty.

Is this mysterious? Does information travel faster than light from left foot to right foot?

Of course not. The correlation was established in the past, when Bertlmann put on his socks. Seeing the left sock doesn't *cause* the right sock to be blue — it just *reveals* information that was already determined.

This is a **classical correlation**: the "hidden variable" is which sock went on which foot. Once you know the hidden variable, there's no mystery.

```{admonition} Classical Correlations
:class: note

Classical correlations arise from shared history or common causes. Learning about one part of a correlated system reveals information about the other part, but doesn't affect it.

The information was always there — you just didn't know it.
```

---

## Quantum Correlations: The Same... Or Are They?

Now let's look at entangled qubits.

Alice and Bob share the Bell state:
$$
|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}
$$

They separate, taking their qubits far apart. Alice measures her qubit and gets 0.

**Question:** What will Bob get if he measures his qubit?

**Answer:** 0, with certainty.

This looks exactly like Bertlmann's socks! Maybe the qubits were "secretly" in state $|00\rangle$ all along, and measurement just reveals it?

This is Einstein's intuition: there must be **hidden variables** that determine the outcomes in advance. The quantum description (a superposition) is just incomplete — it doesn't tell us which hidden state the system is "really" in.

Let's test this hypothesis.

---

## The Hidden Variable Hypothesis

Suppose Einstein is right. The Bell state $|\Phi^+\rangle$ isn't really a superposition — it's a statistical mixture:
- Half the time, the system is "really" in $|00\rangle$
- Half the time, the system is "really" in $|11\rangle$
- We just don't know which

This **hidden variable model** would explain the correlations in the Z basis perfectly:
- If the hidden state is $|00\rangle$: Alice gets 0, Bob gets 0 ✓
- If the hidden state is $|11\rangle$: Alice gets 1, Bob gets 1 ✓
- 50/50 probability for each ✓

So far, the hidden variable model matches quantum mechanics.

But quantum mechanics makes predictions about *other* measurement bases too. Let's check those.

---

## Measuring in the X Basis: The Critical Test

### What Quantum Mechanics Predicts

Recall the X basis states:
$$
|+\rangle = \frac{|0\rangle + |1\rangle}{\sqrt{2}}, \qquad |-\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}}
$$

There's a beautiful identity. The Bell state $|\Phi^+\rangle$ can be rewritten in the X basis:
$$
|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}} = \frac{|++\rangle + |--\rangle}{\sqrt{2}}
$$

Let me verify this. Using $|0\rangle = \frac{1}{\sqrt{2}}(|+\rangle + |-\rangle)$ and $|1\rangle = \frac{1}{\sqrt{2}}(|+\rangle - |-\rangle)$:

$$
|00\rangle = \frac{1}{2}(|+\rangle + |-\rangle)(|+\rangle + |-\rangle) = \frac{1}{2}(|++\rangle + |+-\rangle + |-+\rangle + |--\rangle)
$$

$$
|11\rangle = \frac{1}{2}(|+\rangle - |-\rangle)(|+\rangle - |-\rangle) = \frac{1}{2}(|++\rangle - |+-\rangle - |-+\rangle + |--\rangle)
$$

Adding:
$$
|00\rangle + |11\rangle = |++\rangle + |--\rangle
$$

So:
$$
|\Phi^+\rangle = \frac{|++\rangle + |--\rangle}{\sqrt{2}}
$$

This means if Alice and Bob both measure in the X basis:
- They get $++$ with probability $1/2$
- They get $--$ with probability $1/2$
- They **never** get $+-$ or $-+$

The correlations are perfect in the X basis too!

### What the Hidden Variable Model Predicts

Now let's check the hidden variable model (mixture of $|00\rangle$ and $|11\rangle$).

If the hidden state is $|00\rangle$:
- Alice measures in X basis: gets $+$ or $-$ with probability $1/2$ each
- Bob measures in X basis: gets $+$ or $-$ with probability $1/2$ each
- Their results are **independent** (since $|00\rangle = |0\rangle \otimes |0\rangle$ is a product state)
- All four outcomes $(++, +-, -+, --)$ occur with probability $1/4$ each

If the hidden state is $|11\rangle$:
- Same analysis: all four X-basis outcomes have probability $1/4$

So the hidden variable model predicts:
- $++$: probability $1/4$
- $+-$: probability $1/4$
- $-+$: probability $1/4$
- $--$: probability $1/4$

No correlations in the X basis!

### The Verdict

| Outcome | Quantum Mechanics | Hidden Variable Model |
|---------|-------------------|----------------------|
| $++$ | $1/2$ | $1/4$ |
| $+-$ | $0$ | $1/4$ |
| $-+$ | $0$ | $1/4$ |
| $--$ | $1/2$ | $1/4$ |

**The predictions are different.**

Quantum mechanics says the X-basis correlations are perfect. The simple hidden variable model says there are no X-basis correlations.

### The Decisive Experiment

Here's how you could actually test this:

1. **Prepare many Bell pairs** — thousands of entangled photon pairs, sent to Alice and Bob
2. **Randomly choose measurement basis** — for each pair, flip a coin: measure in Z or measure in X
3. **Record all outcomes** — build up statistics
4. **Analyze Z-basis pairs** — both models predict perfect correlation. Check. ✓
5. **Analyze X-basis pairs** — hidden variable model predicts no correlation; quantum mechanics predicts perfect correlation
6. **Look at the data**

When this experiment is done (and it has been done, many times, with increasing precision), the result is unambiguous:

**Quantum mechanics wins. The X-basis correlations are perfect.**

The hidden variable model — the idea that the qubits were "secretly" in $|00\rangle$ or $|11\rangle$ all along — is experimentally falsified.

```{admonition} The Key Distinction
:class: important

Classical correlations (like Bertlmann's socks) exist in **one** basis — the basis where the hidden variable is defined.

Quantum correlations (entanglement) exist in **all** bases simultaneously.

This is what makes entanglement fundamentally different from classical correlation.
```

---

## What's Special About Entanglement

Let me emphasize this point, because it's the heart of the matter.

**Classical correlations:**
- The sock was pink or blue *before* you looked
- The color is a property the sock *has*
- Correlations are "revealed" by measurement

**Quantum correlations:**
- The qubit wasn't $|0\rangle$ or $|1\rangle$ before measurement
- The measurement *creates* the outcome
- But somehow, the outcomes are still correlated — even in multiple incompatible bases

The entangled state $|\Phi^+\rangle$ has the remarkable property that:
- Measure both qubits in Z → perfect correlation (00 or 11)
- Measure both qubits in X → perfect correlation (++ or --)
- Measure both qubits in Y → perfect correlation

No classical hidden variable can do this. The correlations are too strong to be explained by pre-existing values.

---

## EPR: Einstein's Challenge

In 1935, Einstein, Podolsky, and Rosen published a famous paper arguing that quantum mechanics must be incomplete. Their argument went roughly like this:

### The EPR Argument

**Setup:** Alice and Bob share an entangled state and separate to distant locations.

**Step 1: Locality**
Since Alice and Bob are far apart, Alice's measurement cannot instantly affect Bob's physical state. Whatever Bob's qubit "is," it must be the same before and after Alice measures (assuming no faster-than-light influences).

**Step 2: Correlation implies predetermination**
But wait — Alice can predict Bob's Z-measurement outcome with certainty (by measuring her own qubit in Z). If Alice's measurement doesn't affect Bob, then Bob's Z-outcome must have been determined all along.

**Step 3: Apply to multiple bases**
Alice could also choose to measure in X. Then she could predict Bob's X-outcome with certainty. By the same reasoning, Bob's X-outcome must have been determined all along.

**Step 4: Quantum mechanics doesn't specify these values**
But quantum mechanics doesn't assign definite values to both Z and X simultaneously — that would violate the uncertainty principle!

**Conclusion:** Quantum mechanics must be incomplete. There must be additional "hidden variables" that determine the outcomes, which quantum mechanics doesn't describe.

### Einstein's View

Einstein didn't think quantum mechanics was *wrong* — he thought it was *incomplete*. Like statistical mechanics before the discovery of atoms, quantum mechanics might be a correct but coarse-grained description, missing some deeper layer of reality.

He famously wrote to Max Born:

> "I am convinced that God does not play dice."

And about entanglement:

> "Spooky action at a distance."

### Einstein's Worldview: Why He Cared So Deeply

To understand Einstein's position, we need to understand his philosophical commitments:

**Local realism:** Physical properties exist whether or not we measure them. The moon is there even when no one is looking. Measurement *reveals* pre-existing values; it doesn't create them.

**Separability:** If two systems are far apart, they have independent physical states. What's true about system A shouldn't depend on what happens to system B (and vice versa).

**No action at a distance:** Newton himself called instantaneous gravitational influence "so great an absurdity that I believe no man who has in philosophical matters any competent faculty of thinking can ever fall into it." Einstein agreed — he spent years developing general relativity precisely to eliminate action at a distance from gravity.

For Einstein, quantum entanglement violated all three principles. If measuring Alice's qubit instantly determines Bob's qubit, then either:
- Bob's qubit didn't have a definite state before Alice measured (violates realism), or
- Alice's measurement physically affected Bob's qubit (violates locality and no-action-at-a-distance)

Neither option was acceptable to Einstein. Hence his conclusion: quantum mechanics must be missing something.

Einstein's position was reasonable given what was known in 1935. The question seemed philosophical, not experimentally testable.

That changed in 1964.

---

## Bell's Key Insight (Preview)

John Bell asked a simple question: **Can any hidden variable theory reproduce all the predictions of quantum mechanics?**

Not just the simple model we tested above, but *any* model where:
1. Measurement outcomes are determined by hidden variables
2. Alice's choice of measurement doesn't affect Bob's outcome (locality)
3. Bob's choice doesn't affect Alice's outcome

```{admonition} Locality (Bell's Definition)
:class: tip

**Locality** means Alice's measurement outcome can depend on:
- Her measurement setting (which axis she chooses)
- The hidden variable $\lambda$ (shared information from the past)

But NOT on:
- Bob's measurement setting
- Bob's outcome

And vice versa for Bob. This captures "no faster-than-light influence."
```

Bell proved that any such theory must satisfy certain inequalities — constraints on how strong correlations can be.

And he showed that quantum mechanics *violates* these inequalities.

This means:

```{admonition} Bell's Theorem (Preview)
:class: warning

No local hidden variable theory can reproduce all the predictions of quantum mechanics.

Either:
- Measurement outcomes are NOT predetermined, or
- There IS some kind of "spooky action at a distance" (nonlocality)

You cannot have both locality and predetermined outcomes.
```

We'll derive the Bell/CHSH inequality and see how quantum mechanics violates it in the next lecture. For now, let's build more intuition about what entanglement can and cannot do.

---

## What Entanglement Is NOT

Before continuing, let's clear up common misconceptions.

### Misconception 1: Entanglement sends information faster than light

**Wrong.** Although Alice's measurement "affects" Bob's qubit, Bob cannot tell this happened.

From Bob's perspective:
- Before Alice measures: Bob sees random outcomes (50% $|0\rangle$, 50% $|1\rangle$)
- After Alice measures: Bob still sees random outcomes (50% $|0\rangle$, 50% $|1\rangle$)

The statistics are identical. Bob learns nothing about whether or when Alice measured.

Only when Alice and Bob *compare* their results (using ordinary classical communication) do they discover the correlations.

### Misconception 2: Entanglement is like a wormhole

**Misleading.** Nothing physical travels through entanglement. No matter, energy, or information passes from Alice to Bob.

What entanglement provides is *correlated randomness* — a shared random variable that both parties can access through measurement.

### Misconception 3: Measuring one qubit "collapses" the other

**Subtle.** This language suggests something physical happens to Bob's qubit when Alice measures. But collapse is not a physical process — it's an update to our description of the system.

A better way to think about it:
- The entangled state describes *correlations*
- When Alice measures, she learns her outcome
- Given her outcome, she can predict Bob's outcome (for the same basis)
- This is a correlation, not a signal

### What Entanglement IS

Entanglement is a *resource* — a pattern of correlations that can be distributed and later consumed.

It enables:
- **Teleportation:** Transfer a quantum state using entanglement + classical communication
- **Superdense coding:** Send 2 classical bits using 1 qubit + entanglement
- **Quantum key distribution:** Create secure shared randomness
- **Computational speedup:** Certain algorithms require entanglement between qubits

We'll explore these applications in later lectures.

---

## The Singlet State: EPR's Original Example

EPR's original paper actually used position and momentum (continuous variables), which made the math complicated. In 1951, David Bohm reformulated the argument using spin-1/2 particles, which is much cleaner. This is now the standard presentation.

The **singlet state** is:
$$
|\Psi^-\rangle = \frac{|01\rangle - |10\rangle}{\sqrt{2}}
$$

This has a beautiful physical interpretation: two spin-1/2 particles with total spin zero. Angular momentum is conserved, so if one spin is "up," the other must be "down."

### Why the Singlet Is Special

The singlet state has a remarkable property that makes it central to Bell tests:

**Rotational invariance:** The singlet looks the same in every basis. Whether you write it in Z, X, Y, or any other basis, it always has the form "opposite spins."

This is why the correlation function has such a simple form:
$$
E(\hat{a}, \hat{b}) = -\hat{a} \cdot \hat{b} = -\cos\theta
$$

The minus sign reflects anti-correlation (opposite outcomes), and $\cos\theta$ is the natural measure of how "aligned" the two measurement axes are.

### Comparison: $|\Phi^+\rangle$ vs $|\Psi^-\rangle$

Both are maximally entangled Bell states. The key differences:

| Property | $\|\Phi^+\rangle = \frac{\|00\rangle + \|11\rangle}{\sqrt{2}}$ | $\|\Psi^-\rangle = \frac{\|01\rangle - \|10\rangle}{\sqrt{2}}$ |
|----------|-------------------------------------------|-------------------------------------------|
| Same-axis measurement | Correlated (same outcomes) | Anti-correlated (opposite outcomes) |
| Correlation function | $E(\theta) = +\cos\theta$ | $E(\theta) = -\cos\theta$ |
| Physical interpretation | "Both same" | "Always opposite" (total spin 0) |

For testing Bell inequalities, either state works — they both give "quantum" correlations that violate classical bounds.

### Correlations in the Z Basis

$$
|\Psi^-\rangle = \frac{|01\rangle - |10\rangle}{\sqrt{2}}
$$

Measuring both in Z:
- Outcome $01$: probability $1/2$
- Outcome $10$: probability $1/2$
- Outcomes $00$ and $11$: probability $0$

The results are perfectly **anti-correlated**: if Alice gets 0, Bob gets 1, and vice versa.

### Correlations in the X Basis

Rewriting in the X basis (you can verify):
$$
|\Psi^-\rangle = \frac{|-+\rangle - |+-\rangle}{\sqrt{2}}
$$

So measuring both in X also gives perfect anti-correlation: if Alice gets $+$, Bob gets $-$, and vice versa.

### Correlations in ANY Basis

Here's the remarkable fact about the singlet state:

```{admonition} Singlet State Property
:class: important

If Alice and Bob both measure the singlet state $|\Psi^-\rangle$ along the **same** axis (any axis), their results are always **opposite**.

This holds for Z, X, Y, or any direction in between.
```

This is what Einstein found so troubling. Alice can choose to measure along any axis, and she'll always be able to predict Bob's result. But Bob's qubit can't "know" in advance which axis Alice will choose!

---

## The Correlation Function

To quantify correlations, we define a correlation function.

Suppose Alice measures along axis $\hat{a}$ and Bob measures along axis $\hat{b}$. Each gets a result $\pm 1$ (we use $+1$ for spin-up, $-1$ for spin-down along their chosen axis).

The **correlation** is the average of the product of their outcomes:
$$
E(\hat{a}, \hat{b}) = \langle A \cdot B \rangle
$$

where $A, B \in \{+1, -1\}$ are Alice's and Bob's outcomes.

- $E = +1$: perfect correlation (always same outcomes)
- $E = -1$: perfect anti-correlation (always opposite outcomes)
- $E = 0$: no correlation (uncorrelated random)

### Quantum Prediction for the Singlet

For the singlet state $|\Psi^-\rangle$, quantum mechanics predicts:
$$
E(\hat{a}, \hat{b}) = -\hat{a} \cdot \hat{b} = -\cos\theta
$$

where $\theta$ is the angle between Alice's and Bob's measurement axes.

**Special cases:**
- $\theta = 0$ (same axis): $E = -1$ (perfect anti-correlation) ✓
- $\theta = 90°$ (perpendicular): $E = 0$ (no correlation)
- $\theta = 180°$ (opposite axes): $E = +1$ (perfect correlation)

This formula will be crucial for Bell inequality violations.

---

## Lab: Testing Quantum vs Classical Correlations

Let's run the critical experiments that distinguish quantum mechanics from hidden variable theories.

### Setup

```{code-block} python
import numpy as np
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit_aer import AerSimulator
import matplotlib.pyplot as plt

simulator = AerSimulator()

def create_phi_plus():
    """Create the Bell state |Φ+⟩ = (|00⟩ + |11⟩)/√2"""
    qc = QuantumCircuit(2)
    qc.h(0)
    qc.cx(0, 1)
    return qc
```

---

### Experiment 1: Z-Basis Correlations (Both Models Agree)

**Goal:** Verify that measuring both qubits in the Z basis gives perfect correlation.

**Prediction (both models):** Outcomes 00 and 11 only, 50% each.

```{code-block} python
def experiment_1_zbasis(shots=10000):
    """Measure both qubits in Z basis"""
    qc = create_phi_plus()
    qc.measure_all()
    
    result = simulator.run(qc, shots=shots).result()
    counts = result.get_counts()
    
    print("EXPERIMENT 1: Z-Basis Measurements")
    print("="*40)
    print("Prediction (both models): 00 and 11 only")
    print("\nResults:")
    for outcome in ['00', '01', '10', '11']:
        count = counts.get(outcome, 0)
        print(f"  |{outcome}⟩: {count:5d} ({100*count/shots:5.1f}%)")
    
    # Check correlation
    correlated = counts.get('00', 0) + counts.get('11', 0)
    print(f"\nCorrelated outcomes: {100*correlated/shots:.1f}%")
    print("✓ Both models predict this correctly")
    
    return counts

counts_z = experiment_1_zbasis()
```

**What to observe:** You should see only 00 and 11, approximately 50% each. This is consistent with *both* the quantum prediction AND the hidden variable model. This experiment alone cannot distinguish them.

---

### Experiment 2: X-Basis Correlations (The Critical Test!)

**Goal:** Verify that measuring both qubits in the X basis also gives perfect correlation.

**Quantum prediction:** Outcomes ++ and -- only, 50% each.

**Hidden variable prediction:** All four outcomes (++, +-, -+, --) equally likely, 25% each.

```{code-block} python
def experiment_2_xbasis(shots=10000):
    """Measure both qubits in X basis (apply H before measuring)"""
    qc = create_phi_plus()
    qc.h(0)  # Rotate qubit 0 to X basis
    qc.h(1)  # Rotate qubit 1 to X basis
    qc.measure_all()
    
    result = simulator.run(qc, shots=shots).result()
    counts = result.get_counts()
    
    print("\nEXPERIMENT 2: X-Basis Measurements (THE CRITICAL TEST)")
    print("="*50)
    print("Quantum prediction:        ++ and -- only (perfect correlation)")
    print("Hidden variable prediction: all four outcomes equally likely")
    print("\nResults (note: 0→+, 1→- in X basis):")
    for outcome in ['00', '01', '10', '11']:
        count = counts.get(outcome, 0)
        x_outcome = outcome.replace('0', '+').replace('1', '-')
        print(f"  |{x_outcome}⟩: {count:5d} ({100*count/shots:5.1f}%)")
    
    # Check for "forbidden" outcomes
    forbidden = counts.get('01', 0) + counts.get('10', 0)
    print(f"\n'Forbidden' outcomes (+- or -+): {forbidden} ({100*forbidden/shots:.2f}%)")
    print(f"Hidden variable model predicts: ~{shots//2} ({50}%)")
    
    if forbidden < shots * 0.01:
        print("\n★ QUANTUM MECHANICS WINS! ★")
        print("The correlations persist in the X basis.")
        print("Hidden variable model is FALSIFIED.")
    
    return counts

counts_x = experiment_2_xbasis()
```

**What to observe:** You should see only 00 (meaning ++) and 11 (meaning --), approximately 50% each. The "forbidden" outcomes 01 and 10 should be essentially zero.

This is the key result: the hidden variable model predicts 50% forbidden outcomes, but we see ~0%. **Quantum mechanics wins.**

---

### Experiment 3: Correlation vs Angle (Quantitative Test)

**Goal:** Measure the correlation function $E(\theta)$ as Bob rotates his measurement axis.

**Quantum prediction:** $E(\theta) = \cos\theta$ for $|\Phi^+\rangle$

```{code-block} python
def measure_correlation(theta_bob, shots=5000):
    """
    Measure correlation when Alice measures Z and Bob measures 
    at angle theta from Z (in the XZ plane).
    
    Returns correlation E = <A*B> where A,B ∈ {+1,-1}
    """
    qc = create_phi_plus()
    
    # Alice measures in Z (no rotation)
    # Bob rotates by theta around Y axis
    qc.ry(-theta_bob, 1)
    
    qc.measure_all()
    
    result = simulator.run(qc, shots=shots).result()
    counts = result.get_counts()
    
    # Calculate correlation
    # Same outcomes (00, 11) contribute +1
    # Different outcomes (01, 10) contribute -1
    same = counts.get('00', 0) + counts.get('11', 0)
    diff = counts.get('01', 0) + counts.get('10', 0)
    
    return (same - diff) / shots

def experiment_3_correlation_curve():
    """Scan correlation vs angle and compare to theory"""
    print("\nEXPERIMENT 3: Correlation vs Measurement Angle")
    print("="*50)
    
    angles = np.linspace(0, np.pi, 19)
    correlations = [measure_correlation(theta) for theta in angles]
    theory = np.cos(angles)
    
    # Print some key values
    print("\nKey measurements:")
    for theta, E_meas, E_theory in [(0, correlations[0], 1.0),
                                      (np.pi/4, correlations[4], np.cos(np.pi/4)),
                                      (np.pi/2, correlations[9], 0.0),
                                      (np.pi, correlations[-1], -1.0)]:
        print(f"  θ = {theta*180/np.pi:5.1f}°: measured E = {E_meas:+.3f}, theory = {E_theory:+.3f}")
    
    # Plot
    plt.figure(figsize=(10, 6))
    plt.plot(angles * 180/np.pi, correlations, 'bo-', label='Measured', markersize=8)
    plt.plot(angles * 180/np.pi, theory, 'r-', label='Theory: cos(θ)', linewidth=2)
    plt.axhline(y=0, color='gray', linestyle='--', alpha=0.5)
    plt.xlabel("Bob's measurement angle θ (degrees)", fontsize=12)
    plt.ylabel('Correlation E(0, θ)', fontsize=12)
    plt.title('Quantum Correlations in |Φ+⟩: Experiment vs Theory', fontsize=14)
    plt.legend(fontsize=11)
    plt.grid(True, alpha=0.3)
    plt.xlim(0, 180)
    plt.ylim(-1.1, 1.1)
    plt.show()
    
    print("\n✓ The correlation follows cos(θ) exactly as quantum mechanics predicts.")
    
    return angles, correlations

angles, correlations = experiment_3_correlation_curve()
```

**What to observe:** The measured correlation should trace out $\cos\theta$:
- $\theta = 0°$: $E = +1$ (perfect correlation, same axis)
- $\theta = 90°$: $E = 0$ (no correlation, perpendicular axes)
- $\theta = 180°$: $E = -1$ (perfect anti-correlation, opposite axes)

This continuous curve, matching quantum predictions at all angles, is strong evidence for quantum mechanics.

---

### Bonus: The Singlet State

```{code-block} python
def create_singlet():
    """Create the singlet state |Ψ-⟩ = (|01⟩ - |10⟩)/√2"""
    qc = QuantumCircuit(2)
    # Start with |Φ+⟩ = (|00⟩ + |11⟩)/√2, then transform
    qc.h(0)
    qc.cx(0, 1)
    # Apply Z to qubit 0: gives (|00⟩ - |11⟩)/√2 = |Φ-⟩
    qc.z(0)
    # Apply X to qubit 1: swaps |0⟩↔|1⟩ on qubit 1
    # |00⟩ → |01⟩, |11⟩ → |10⟩
    # Result: (|01⟩ - |10⟩)/√2 = |Ψ-⟩
    qc.x(1)
    return qc

# Verify singlet state
qc = create_singlet()
state = Statevector(qc)
print("Singlet state |Ψ-⟩:")
for i, amp in enumerate(state):
    if abs(amp) > 1e-10:
        print(f"  |{format(i, '02b')}⟩: {amp:.4f}")
```

---

## The Deep Questions (Brief Philosophical Aside)

The results we've discussed raise profound questions that physics alone may not answer.

**Is the quantum state "real"?** Does the superposition $|\Phi^+\rangle$ physically exist, or is it just a tool for calculating probabilities? Bell's theorem pushes us toward treating it as real — if it were just ignorance, hidden variables would work.

**What happens when Alice measures?** Options include: (1) physical collapse that updates the global state, (2) branching into parallel worlds (many-worlds), or (3) the state being relative to each observer (relational QM). All make identical experimental predictions.

**Why can't we send information?** The no-signaling theorem works because Alice can't *control* her random outcome. Random correlated noise can't encode a chosen message. This is not a coincidence — theories allowing FTL signaling lead to logical paradoxes.

These interpretational questions remain open. What's settled is the experimental fact: quantum correlations are real and violate Bell inequalities. The universe is non-classical in a precise, testable way.

---

## Why This Matters for Quantum Computing

You might wonder: why does a course on quantum computing care about EPR and Bell? This seems like philosophy, not engineering.

Here's the connection:

**If hidden variables worked, quantum computers would be useless.**

Think about it. If every quantum state were "secretly" a classical probability distribution over hidden states, then:
- A quantum computer would just be sampling from that distribution
- A classical computer could simulate it by sampling from the same distribution
- There would be no quantum speedup

Bell's theorem — showing that hidden variables don't work — is the first hint that quantum mechanics has computational power beyond classical physics.

More precisely:
- **Entanglement** is a resource that quantum computers exploit
- **Bell inequality violation** proves entanglement is real and non-classical
- **Quantum algorithms** use interference and entanglement to solve problems faster
- **Quantum error correction** relies on entanglement to protect information

The correlations we've studied aren't just philosophically interesting — they're the fuel that powers quantum computation.

```{admonition} The Punchline
:class: important

Bell's theorem proves that quantum correlations cannot be simulated classically (with local hidden variables). 

This is the theoretical foundation for believing quantum computers can outperform classical ones.
```

---

## Summary

Today we explored what makes quantum correlations different from classical ones:

1. **Classical correlations** (like Bertlmann's socks) can be explained by hidden variables — predetermined values that are revealed by measurement.

2. **Quantum correlations** (entanglement) persist across multiple measurement bases. The correlations are "too strong" to be explained by any simple hidden variable model.

3. **The EPR argument** challenged quantum mechanics: if measurements are correlated, and distant measurements can't affect each other, then outcomes must be predetermined — but quantum mechanics doesn't specify these values.

4. **Bell's insight** was that any local hidden variable theory must satisfy certain inequalities, which quantum mechanics violates.

5. **What entanglement is NOT:**
   - It's not faster-than-light communication
   - It's not "one qubit knowing what the other does"
   - It's not measurement affecting a distant system (in a detectable way)

6. **What entanglement IS:**
   - Correlated randomness that can be distributed
   - A resource for quantum protocols
   - A fundamentally non-classical phenomenon

---

## Homework

### Problem 1: Classical vs Quantum Correlations

**(a)** Bertlmann randomly chooses to put the pink sock on his left foot or right foot with equal probability. Write down the joint probability distribution $P(\text{left}, \text{right})$ for sock colors.

**(b)** What is the correlation $E = \langle L \cdot R \rangle$ if we assign pink $= +1$ and blue $= -1$?

**(c)** Now Bertlmann randomly rotates both socks before putting them on, so each foot independently gets pink or blue with 50% probability. What is the correlation now?

**(d)** Explain why case (a) is analogous to entanglement measured in one basis, but case (c) is what the hidden variable model predicts for other bases.

---

### Problem 2: Verifying the X-Basis Expansion

Prove that $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle) = \frac{1}{\sqrt{2}}(|++\rangle + |--\rangle)$.

**(a)** Express $|0\rangle$ and $|1\rangle$ in terms of $|+\rangle$ and $|-\rangle$.

**(b)** Substitute into $|00\rangle$ and expand.

**(c)** Substitute into $|11\rangle$ and expand.

**(d)** Add and simplify to get $|00\rangle + |11\rangle = |++\rangle + |--\rangle$.

---

### Problem 3: The Singlet State

Consider the singlet state $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$.

**(a)** What are the probabilities of the four outcomes when both qubits are measured in the Z basis?

**(b)** Show that $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|-+\rangle - |+-\rangle)$ in the X basis.

**(c)** What are the probabilities of the four outcomes when both qubits are measured in the X basis?

**(d)** For the Y basis, $|+i\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ and $|-i\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$. Show that the singlet has perfect anti-correlation in the Y basis too.

---

### Problem 4: The Correlation Function

For the singlet state, the correlation function is $E(\hat{a}, \hat{b}) = -\hat{a} \cdot \hat{b} = -\cos\theta$.

**(a)** What is $E$ when Alice and Bob measure along the same axis ($\theta = 0$)?

**(b)** What is $E$ when they measure along perpendicular axes ($\theta = 90°$)?

**(c)** What is $E$ when they measure along opposite axes ($\theta = 180°$)?

**(d)** At what angle $\theta$ is $E = -1/2$?

---

### Problem 5: Designing Hidden Variable Models

The simple hidden variable model (mixture of $|00\rangle$ and $|11\rangle$) failed because it predicted no X-basis correlations. Let's try to do better.

**(a)** Design a hidden variable model where each pair is assigned one of four hidden states:
- State A: "return +1 for both Z and X measurements" for both qubits
- State B: "return -1 for both Z and X measurements" for both qubits  
- State C: "return +1 for Z, -1 for X" for both qubits
- State D: "return -1 for Z, +1 for X" for both qubits

With equal probability (25% each), what does this model predict for:
- Z-basis correlation?
- X-basis correlation?

**(b)** Can you choose different probabilities for states A, B, C, D to match the quantum predictions (perfect correlation in both bases)? Try to find probabilities that work, or explain why it's impossible.

**(c)** Now consider a third basis, Y. In quantum mechanics, $|\Phi^+\rangle$ also has perfect correlation in Y. Can your model from part (b) also give perfect Y-correlation? Why or why not?

**(d)** This illustrates why hidden variable models fail: they can match quantum predictions in one or two bases, but not all simultaneously. Explain in your own words why adding more bases makes the hidden variable task harder.

---

### Problem 6: No-Signaling

Alice and Bob share $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$.

**(a)** If Alice measures in the Z basis, what are Bob's probabilities of getting 0 or 1 when he measures in Z?

**(b)** If Alice measures in the X basis instead, what are Bob's probabilities of getting 0 or 1 when he measures in Z? 

*Hint: Consider all possible Alice outcomes and weight by probability.*

**(c)** Compare your answers to (a) and (b). Can Bob tell which measurement Alice performed by looking at his own results?

**(d)** Explain why this result guarantees that entanglement cannot be used for faster-than-light communication.

---

### Problem 7: EPR Argument Analysis

Reconstruct the EPR argument in your own words.

**(a)** What is the "locality" assumption?

**(b)** What is the "realism" assumption (that measurement reveals pre-existing values)?

**(c)** How do these assumptions, combined with quantum predictions, lead to the conclusion that quantum mechanics is "incomplete"?

**(d)** Bell's theorem shows that quantum mechanics violates certain inequalities derived from local realism. What does this imply about the EPR assumptions?

---

### Problem 8: Qiskit Exploration

**(a)** Create the singlet state $|\Psi^-\rangle$ in Qiskit and verify its amplitudes.

**(b)** Measure it 10,000 times in the Z basis. Verify perfect anti-correlation.

**(c)** Measure it 10,000 times in the X basis (apply H to both qubits before measuring). Verify perfect anti-correlation.

**(d)** Measure with Alice in Z and Bob at angle $\theta = 45°$ (apply $R_y(\pi/4)$ to Bob's qubit before measuring). Calculate the correlation $E$ from your results and compare to the theoretical prediction $E = -\cos(45°) = -1/\sqrt{2}$.

---

### Problem 9: Conceptual Questions

**(a)** Alice and Bob share an entangled state. Alice measures and gets a result. Has anything "physical" happened to Bob's qubit? Discuss different interpretations.

**(b)** Why can't we use the perfect correlations in entanglement to send a message faster than light?

**(c)** Einstein called entanglement "spooky action at a distance." Is there actually any "action" (physical influence) at a distance? What would Einstein say? What would a modern physicist say?

**(d)** If quantum correlations can't be explained by hidden variables (as Bell's theorem suggests), what are the alternatives?

---

## Looking Ahead

We've seen that quantum correlations are stronger than classical correlations — they persist across multiple measurement bases. And we've previewed Bell's key insight: this strength can be quantified, leading to testable inequalities.

Next lecture, we'll derive the CHSH inequality and prove that quantum mechanics violates it. This is the mathematical heart of the "quantum vs classical" distinction — and the foundation for understanding why quantum computers might outperform classical ones.