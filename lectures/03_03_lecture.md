# Lecture 3.3: Bell's Theorem and the CHSH Inequality

## From Philosophy to Physics

Last lecture, we saw that quantum correlations are different from classical correlations: they persist across multiple measurement bases. We previewed Bell's insight that this difference can be quantified.

Today we make it precise. We'll derive the **CHSH inequality** — a mathematical bound that any local hidden variable theory must satisfy — and prove that quantum mechanics **violates** it.

This is not philosophy. It's a theorem with experimental consequences. Bell tests have been performed thousands of times, with increasing precision, and the verdict is unambiguous: **quantum mechanics wins**.

---

## The Setup: A Correlation Experiment

### The Scenario

Alice and Bob share many copies of a two-qubit entangled state. They're far apart and can't communicate during the experiment.

For each copy:
1. Alice randomly chooses one of two measurement settings: $A$ or $A'$
2. Bob randomly chooses one of two measurement settings: $B$ or $B'$
3. Each measures their qubit and records the outcome: $+1$ or $-1$
4. They repeat many times and compute correlations

```{figure} ./03_03_lecture_files/bell_test_setup.svg
:width: 450px
:name: bell-test-setup

Bell test setup: Alice chooses $A$ or $A'$, Bob chooses $B$ or $B'$, each gets $\pm 1$.
```

### The Correlation

For a given pair of settings (say $A$ and $B$), the **correlation** is the average product of outcomes:

$$
E(A, B) = \langle a \cdot b \rangle
$$

where $a, b \in \{+1, -1\}$ are Alice's and Bob's outcomes.

- $E = +1$: perfect correlation (always same)
- $E = -1$: perfect anti-correlation (always opposite)
- $E = 0$: no correlation (random)

### The Four Correlations

With two settings each, there are four correlations to measure:
- $E(A, B)$: Alice uses $A$, Bob uses $B$
- $E(A, B')$: Alice uses $A$, Bob uses $B'$
- $E(A', B)$: Alice uses $A'$, Bob uses $B$
- $E(A', B')$: Alice uses $A'$, Bob uses $B'$

The CHSH inequality puts a constraint on these four numbers.

---

## The Local Hidden Variable Assumption

Before deriving the inequality, let's be precise about what we're assuming.

### Hidden Variables

A **hidden variable** $\lambda$ is some additional information that:
- Is determined when the entangled pair is created
- Travels with each particle
- Determines measurement outcomes

Think of $\lambda$ as an "instruction set" that tells each particle what to do for any measurement.

### Locality

**Locality** means:
- Alice's outcome depends only on her setting ($A$ or $A'$) and the hidden variable $\lambda$
- Alice's outcome does NOT depend on Bob's setting or outcome
- And vice versa for Bob

```{admonition} The Local Hidden Variable Assumption
:class: important

There exists a hidden variable $\lambda$ (with some probability distribution $\rho(\lambda)$) such that:

$$
a = A(\lambda) \in \{+1, -1\} \quad \text{(Alice's outcome for setting } A \text{)}
$$
$$
b = B(\lambda) \in \{+1, -1\} \quad \text{(Bob's outcome for setting } B \text{)}
$$

The functions $A(\lambda)$, $A'(\lambda)$, $B(\lambda)$, $B'(\lambda)$ give predetermined outcomes for each setting.

**Crucially:** $A(\lambda)$ doesn't depend on Bob's choice, and $B(\lambda)$ doesn't depend on Alice's choice.
```

This captures Einstein's vision: outcomes are determined by information from the shared past ($\lambda$), and distant measurements don't influence each other.

---

## Deriving the CHSH Inequality

Now we prove that any local hidden variable theory satisfies a constraint.

### The CHSH Quantity

Define the combination:
$$
S = E(A, B) + E(A, B') + E(A', B) - E(A', B')
$$

This particular combination was chosen by Clauser, Horne, Shimony, and Holt (1969) because it has nice properties.

### Step 1: Write Correlations in Terms of Hidden Variables

In a local hidden variable theory:
$$
E(A, B) = \int A(\lambda) B(\lambda) \, \rho(\lambda) \, d\lambda
$$

This is just the average of the product, weighted by the probability distribution of $\lambda$.

### Step 2: Consider a Single $\lambda$

For a fixed $\lambda$, define:
$$
S(\lambda) = A(\lambda) B(\lambda) + A(\lambda) B'(\lambda) + A'(\lambda) B(\lambda) - A'(\lambda) B'(\lambda)
$$

We can rewrite this by factoring:
$$
S(\lambda) = A(\lambda) \big[ B(\lambda) + B'(\lambda) \big] + A'(\lambda) \big[ B(\lambda) - B'(\lambda) \big]
$$

### Step 3: The Key Observation

Since $B(\lambda), B'(\lambda) \in \{+1, -1\}$, there are only four possibilities for the pair $(B(\lambda), B'(\lambda))$:

| $B(\lambda)$ | $B'(\lambda)$ | $B + B'$ | $B - B'$ |
|--------------|---------------|----------|----------|
| +1 | +1 | +2 | 0 |
| +1 | -1 | 0 | +2 |
| -1 | +1 | 0 | -2 |
| -1 | -1 | -2 | 0 |

In every case, **one of the brackets is $\pm 2$ and the other is $0$**.

### Step 4: Bound on $S(\lambda)$

Since $A(\lambda), A'(\lambda) \in \{+1, -1\}$:

$$
S(\lambda) = A(\lambda) \cdot (\pm 2 \text{ or } 0) + A'(\lambda) \cdot (0 \text{ or } \pm 2)
$$

Only one term is nonzero, and it equals $(\pm 1) \cdot (\pm 2) = \pm 2$.

Therefore:
$$
S(\lambda) \in \{-2, +2\}
$$

for every possible $\lambda$.

### Step 5: Average Over $\lambda$

The measured $S$ is the average:
$$
S = \int S(\lambda) \, \rho(\lambda) \, d\lambda
$$

Since $S(\lambda) \in \{-2, +2\}$ for each $\lambda$, and we're averaging with positive weights that sum to 1:

$$
|S| \leq 2
$$

```{admonition} The CHSH Inequality
:class: warning

**Any local hidden variable theory must satisfy:**

$$
|S| = |E(A,B) + E(A,B') + E(A',B) - E(A',B')| \leq 2
$$

This is a mathematical theorem, not an approximation.
```

---

## Quantum Mechanics Violates CHSH

Now let's compute what quantum mechanics predicts for an entangled state.

### The Setup

Use the singlet state:
$$
|\Psi^-\rangle = \frac{|01\rangle - |10\rangle}{\sqrt{2}}
$$

Recall the correlation function (from last lecture):
$$
E(\hat{a}, \hat{b}) = -\hat{a} \cdot \hat{b} = -\cos\theta_{ab}
$$

where $\theta_{ab}$ is the angle between measurement axes $\hat{a}$ and $\hat{b}$.

### Choosing Optimal Angles

We want to maximize $|S|$. Let's work in a plane (2D suffices).

Define angles from some reference direction:
- Alice's setting $A$: angle $0°$
- Alice's setting $A'$: angle $90°$
- Bob's setting $B$: angle $45°$
- Bob's setting $B'$: angle $-45°$

```{figure} ./03_03_lecture_files/chsh_angles.svg
:width: 350px
:name: chsh-angles

Optimal measurement angles for CHSH violation: $A=0°$, $A'=90°$, $B=45°$, $B'=-45°$.
```

### Computing the Correlations

The angle differences are:
- $\theta_{AB} = |0° - 45°| = 45°$
- $\theta_{AB'} = |0° - (-45°)| = 45°$
- $\theta_{A'B} = |90° - 45°| = 45°$
- $\theta_{A'B'} = |90° - (-45°)| = 135°$

Now compute the correlations (for singlet, $E = -\cos\theta$):
$$
E(A, B) = -\cos(45°) = -\frac{1}{\sqrt{2}}
$$
$$
E(A, B') = -\cos(45°) = -\frac{1}{\sqrt{2}}
$$
$$
E(A', B) = -\cos(45°) = -\frac{1}{\sqrt{2}}
$$
$$
E(A', B') = -\cos(135°) = +\frac{1}{\sqrt{2}}
$$

### The CHSH Quantity

$$
S = E(A,B) + E(A,B') + E(A',B) - E(A',B')
$$
$$
= -\frac{1}{\sqrt{2}} - \frac{1}{\sqrt{2}} - \frac{1}{\sqrt{2}} - \frac{1}{\sqrt{2}}
$$
$$
= -\frac{4}{\sqrt{2}} = -2\sqrt{2}
$$

Therefore:
$$
|S| = 2\sqrt{2} \approx 2.83
$$

```{admonition} Quantum Violation
:class: important

Quantum mechanics predicts $|S| = 2\sqrt{2} \approx 2.83$.

The CHSH bound is $|S| \leq 2$.

**Quantum mechanics exceeds the classical bound by about 41%!**
```

---

## The Tsirelson Bound

Is $2\sqrt{2}$ the maximum quantum violation, or can we do better?

Boris Tsirelson proved in 1980 that:

$$
|S|_{\text{quantum}} \leq 2\sqrt{2}
$$

This is called the **Tsirelson bound**. No quantum state and no measurements can exceed it.

So we have three regimes:

| Theory | Maximum $|S|$ |
|--------|---------------|
| Local hidden variables | 2 |
| Quantum mechanics | $2\sqrt{2} \approx 2.83$ |
| "Super-quantum" (no-signaling but nonlocal) | 4 |

The gap between 2 and $2\sqrt{2}$ is the "quantum advantage" in correlations. It's not unlimited — quantum mechanics is nonlocal but not maximally so.

---

## Experimental Tests

### The Challenge

To test Bell inequalities rigorously, experiments must close various "loopholes" — ways that a clever hidden variable theory might still explain the results.

**Locality loophole:** What if Alice's measurement somehow influences Bob's outcome (or vice versa)? 

*Solution:* Make sure Alice and Bob are far enough apart that no signal (even at light speed) could travel between them during the measurement. Alain Aspect's 1982 experiment used fast-switching polarizers to close this.

**Detection loophole:** What if the detectors don't catch all the particles, and the detected ones are a biased sample?

*Solution:* Use high-efficiency detectors. Modern experiments achieve >95% efficiency.

**Freedom-of-choice loophole:** What if the random choices of measurement settings aren't really random?

*Solution:* Use cosmic sources (light from distant quasars) to determine settings — randomness from billions of years ago!

### The Results

Every rigorous Bell test has confirmed quantum mechanics:
- Aspect (1982): first strong violation with spacelike separation
- Zeilinger et al. (2015): "loophole-free" Bell test
- Many others with increasing precision

The measured values of $|S|$ consistently match the quantum prediction, not the classical bound.

```{admonition} The 2022 Nobel Prize
:class: note

John Clauser, Alain Aspect, and Anton Zeilinger received the 2022 Nobel Prize in Physics "for experiments with entangled photons, establishing the violation of Bell inequalities and pioneering quantum information science."

This recognized both the fundamental importance of Bell tests and the birth of quantum information as a field.
```

---

## What Bell's Theorem Means

Let's be precise about what Bell's theorem does and doesn't say.

### What It DOES Say

**No local hidden variable theory can reproduce all predictions of quantum mechanics.**

At least one of these assumptions must be false:
1. **Realism:** Measurement outcomes are determined by pre-existing values
2. **Locality:** Distant measurements don't influence each other

### What It Does NOT Say

**Bell's theorem does NOT prove faster-than-light communication is possible.**

The no-signaling theorem remains intact: Alice cannot send a message to Bob using entanglement alone. The correlations only become apparent when Alice and Bob compare their results later.

**Bell's theorem does NOT tell us which assumption to give up.**

You could reject realism (measurement creates outcomes, not reveals them). Or you could accept nonlocality but of a subtle kind that doesn't allow signaling. Different interpretations of quantum mechanics make different choices.

### The Options

**Option 1: Give up realism (Copenhagen, QBism)**
Measurement outcomes are not predetermined. The quantum state is the complete description. There's nothing "underneath" to discover.

**Option 2: Accept nonlocality (Bohmian mechanics)**
Hidden variables exist, but they're nonlocal — Alice's measurement really does influence Bob's particle, instantaneously, but in a way that can't be used for communication.

**Option 3: Many worlds**
There's no measurement problem because all outcomes happen — in different branches of the wavefunction. No hidden variables, no collapse, no nonlocality (in a sense).

**Option 4: Superdeterminism**
Reject the assumption that Alice and Bob can freely choose their settings. If everything (including their choices) is predetermined by the initial conditions of the universe, the loophole opens. Most physicists find this unsatisfying.

None of these options is ruled out by Bell's theorem. What IS ruled out is the comfortable classical picture where particles have definite properties and measurements just reveal them.

---

## Bell Inequality and Quantum Computing

Why does this matter for quantum computing?

### The Computational Connection

Bell inequality violation proves that quantum correlations cannot be efficiently simulated by classical probability distributions.

More precisely:

```{admonition} Classical Simulation Bound
:class: important

If a physical system's correlations satisfy Bell inequalities, then a classical computer can efficiently simulate the system.

If they violate Bell inequalities, classical simulation is (provably, in some sense) hard.
```

This connects entanglement to computational complexity. Quantum computers get their power from manipulating states that violate Bell inequalities.

### Entanglement as a Resource

In quantum computing:
- **Separable states** can be simulated classically — they have no "quantum advantage"
- **Entangled states** that violate Bell inequalities are genuinely quantum
- Algorithms like Shor's and Grover's create and manipulate entanglement

Bell's theorem is the theoretical foundation for believing quantum computers are fundamentally more powerful than classical ones.

---

## Lab: Testing CHSH in Qiskit

Let's measure the CHSH quantity using a simulated quantum computer.

### Setup

```{code-block} python
import numpy as np
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator
import matplotlib.pyplot as plt

simulator = AerSimulator()
```

### Creating the Singlet State

```{code-block} python
def create_singlet():
    """Create |Ψ-⟩ = (|01⟩ - |10⟩)/√2"""
    qc = QuantumCircuit(2)
    qc.h(0)
    qc.cx(0, 1)
    qc.z(0)
    qc.x(1)
    return qc
```

### Measuring Correlation for Given Angles

```{code-block} python
def measure_correlation(theta_a, theta_b, shots=10000):
    """
    Measure correlation E(a,b) for singlet state
    with Alice measuring at angle theta_a and Bob at theta_b.
    
    Angles are measured from Z axis in the XZ plane.
    """
    qc = create_singlet()
    
    # Rotate Alice's qubit to measure at angle theta_a
    # Ry(-theta) rotates the measurement basis
    qc.ry(-theta_a, 0)
    
    # Rotate Bob's qubit to measure at angle theta_b
    qc.ry(-theta_b, 1)
    
    # Measure
    qc.measure_all()
    
    # Run
    result = simulator.run(qc, shots=shots).result()
    counts = result.get_counts()
    
    # Calculate correlation
    # Same outcomes: +1, Different outcomes: -1
    same = counts.get('00', 0) + counts.get('11', 0)
    diff = counts.get('01', 0) + counts.get('10', 0)
    
    E = (same - diff) / shots
    return E
```

### Computing the CHSH Quantity

```{code-block} python
def compute_CHSH(angle_A, angle_A_prime, angle_B, angle_B_prime, shots=10000):
    """
    Compute S = E(A,B) + E(A,B') + E(A',B) - E(A',B')
    """
    E_AB = measure_correlation(angle_A, angle_B, shots)
    E_AB_prime = measure_correlation(angle_A, angle_B_prime, shots)
    E_A_prime_B = measure_correlation(angle_A_prime, angle_B, shots)
    E_A_prime_B_prime = measure_correlation(angle_A_prime, angle_B_prime, shots)
    
    S = E_AB + E_AB_prime + E_A_prime_B - E_A_prime_B_prime
    
    return S, E_AB, E_AB_prime, E_A_prime_B, E_A_prime_B_prime

# Optimal angles (in radians)
A = 0
A_prime = np.pi/2
B = np.pi/4
B_prime = -np.pi/4  # or equivalently 3*np.pi/4

print("CHSH TEST WITH OPTIMAL ANGLES")
print("="*50)
print(f"Alice: A = 0°, A' = 90°")
print(f"Bob:   B = 45°, B' = -45°")
print()

S, E_AB, E_ABp, E_ApB, E_ApBp = compute_CHSH(A, A_prime, B, B_prime, shots=50000)

print("Individual correlations:")
print(f"  E(A, B)   = {E_AB:+.4f}  (theory: {-np.cos(np.pi/4):+.4f})")
print(f"  E(A, B')  = {E_ABp:+.4f}  (theory: {-np.cos(np.pi/4):+.4f})")
print(f"  E(A', B)  = {E_ApB:+.4f}  (theory: {-np.cos(np.pi/4):+.4f})")
print(f"  E(A', B') = {E_ApBp:+.4f}  (theory: {-np.cos(3*np.pi/4):+.4f})")
print()
print(f"CHSH quantity S = {S:.4f}")
print(f"Classical bound: |S| ≤ 2")
print(f"Quantum prediction: |S| = 2√2 ≈ {2*np.sqrt(2):.4f}")
print()

if abs(S) > 2:
    print("★ BELL INEQUALITY VIOLATED! ★")
    print(f"  Violation: |S| - 2 = {abs(S) - 2:.4f}")
else:
    print("No violation (check your code!)")
```

### Scanning Angles to Find Maximum Violation

```{code-block} python
def scan_bob_angle(angle_B_prime_range, shots=5000):
    """
    Fix A=0, A'=90°, B=45°, and scan B' to see how S varies.
    """
    A = 0
    A_prime = np.pi/2
    B = np.pi/4
    
    S_values = []
    for B_prime in angle_B_prime_range:
        S, _, _, _, _ = compute_CHSH(A, A_prime, B, B_prime, shots)
        S_values.append(S)
    
    return S_values

# Scan B' from -90° to +90°
B_prime_range = np.linspace(-np.pi/2, np.pi/2, 21)
S_values = scan_bob_angle(B_prime_range, shots=10000)

# Plot
plt.figure(figsize=(10, 6))
plt.plot(B_prime_range * 180/np.pi, S_values, 'bo-', markersize=8, label='Measured S')
plt.axhline(y=2, color='r', linestyle='--', linewidth=2, label='Classical bound (+2)')
plt.axhline(y=-2, color='r', linestyle='--', linewidth=2, label='Classical bound (-2)')
plt.axhline(y=2*np.sqrt(2), color='g', linestyle=':', linewidth=2, label=f'Tsirelson bound (+2√2)')
plt.axhline(y=-2*np.sqrt(2), color='g', linestyle=':', linewidth=2, label=f'Tsirelson bound (-2√2)')
plt.axvline(x=-45, color='purple', linestyle=':', alpha=0.5)
plt.xlabel("Bob's setting B' (degrees)", fontsize=12)
plt.ylabel('CHSH quantity S', fontsize=12)
plt.title("CHSH Violation vs Bob's Measurement Angle", fontsize=14)
plt.legend(loc='upper right')
plt.grid(True, alpha=0.3)
plt.xlim(-90, 90)
plt.ylim(-3.5, 3.5)
plt.show()

# Find maximum violation
max_idx = np.argmax(np.abs(S_values))
print(f"\nMaximum |S| = {abs(S_values[max_idx]):.3f} at B' = {B_prime_range[max_idx]*180/np.pi:.1f}°")
```

### Comparing Entangled vs Product States

```{code-block} python
def create_product_state():
    """Create a product state |+⟩|+⟩ for comparison"""
    qc = QuantumCircuit(2)
    qc.h(0)
    qc.h(1)
    return qc

def measure_correlation_product(theta_a, theta_b, shots=10000):
    """Measure correlation for product state"""
    qc = create_product_state()
    qc.ry(-theta_a, 0)
    qc.ry(-theta_b, 1)
    qc.measure_all()
    
    result = simulator.run(qc, shots=shots).result()
    counts = result.get_counts()
    
    same = counts.get('00', 0) + counts.get('11', 0)
    diff = counts.get('01', 0) + counts.get('10', 0)
    
    return (same - diff) / shots

print("\nCOMPARISON: ENTANGLED vs PRODUCT STATE")
print("="*50)

# Compute CHSH for product state
E_AB = measure_correlation_product(0, np.pi/4)
E_ABp = measure_correlation_product(0, -np.pi/4)
E_ApB = measure_correlation_product(np.pi/2, np.pi/4)
E_ApBp = measure_correlation_product(np.pi/2, -np.pi/4)
S_product = E_AB + E_ABp + E_ApB - E_ApBp

print(f"Product state |+⟩|+⟩:")
print(f"  S = {S_product:.4f}")
print(f"  |S| = {abs(S_product):.4f} ≤ 2  ✓ (satisfies classical bound)")
print()
print(f"Entangled singlet state:")
print(f"  S = {S:.4f}")
print(f"  |S| = {abs(S):.4f} > 2  ✗ (violates classical bound)")
```

---

## Summary

Today we proved one of the most profound results in physics:

1. **The CHSH inequality** constrains correlations in any local hidden variable theory:
$$
|S| = |E(A,B) + E(A,B') + E(A',B) - E(A',B')| \leq 2
$$

2. **Quantum mechanics violates CHSH**: For the singlet state with optimal angles,
$$
|S| = 2\sqrt{2} \approx 2.83 > 2
$$

3. **The Tsirelson bound** limits quantum violation to $2\sqrt{2}$ — quantum mechanics is nonlocal but not maximally so.

4. **Experiments confirm quantum mechanics**: All rigorous Bell tests show violation consistent with quantum predictions.

5. **Bell's theorem** forces us to abandon either realism (predetermined outcomes) or locality (no distant influences). We can't have both.

6. **Computational significance**: Bell violations prove quantum correlations can't be efficiently simulated classically — the foundation for quantum computational advantage.

---

## Homework

### Problem 1: CHSH Derivation

Reproduce the derivation of the CHSH inequality.

**(a)** For fixed $\lambda$, show that if $B(\lambda), B'(\lambda) \in \{+1, -1\}$, then exactly one of $B+B'$ and $B-B'$ equals $\pm 2$ and the other equals $0$.

**(b)** Using part (a), show that $S(\lambda) = A(B+B') + A'(B-B') \in \{-2, +2\}$.

**(c)** Explain why averaging over $\lambda$ gives $|S| \leq 2$.

---

### Problem 2: Quantum Prediction

For the singlet state $|\Psi^-\rangle$ with correlation $E(\theta) = -\cos\theta$:

**(a)** Compute $E(A,B)$, $E(A,B')$, $E(A',B)$, $E(A',B')$ for:
- $A$ at $0°$, $A'$ at $90°$, $B$ at $45°$, $B'$ at $-45°$

**(b)** Compute $S$ and verify $|S| = 2\sqrt{2}$.

**(c)** What if $B'$ were at $+45°$ instead of $-45°$? Compute $S$ for this case.

**(d)** What angles give $S = 0$ (no violation at all)?

---

### Problem 3: Optimizing the Violation

Suppose Alice measures at angles $0$ and $\alpha$, while Bob measures at angles $\beta$ and $\gamma$. For the singlet state, $E(\theta) = -\cos\theta$.

**(a)** Write $S$ in terms of $\alpha, \beta, \gamma$.

**(b)** By symmetry, assume the angles are evenly spaced: $\alpha = 2\phi$, $\beta = \phi$, $\gamma = 3\phi$. Show that:
$$
S = -3\cos\phi + \cos(3\phi)
$$

**(c)** Find the value of $\phi$ that maximizes $|S|$. (You can use calculus or numerical methods.)

**(d)** Verify that the maximum is $2\sqrt{2}$ at $\phi = \pi/4$.

---

### Problem 4: The Product State

Consider the product state $|+\rangle \otimes |+\rangle$ where $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$.

**(a)** For this state, what is the correlation $E(\theta)$ when Alice measures at angle $0$ and Bob at angle $\theta$?

*Hint: Product states have no correlation — each qubit is independent.*

**(b)** Compute the CHSH quantity $S$ for the product state with any choice of angles.

**(c)** Does the product state violate the CHSH inequality? Explain why this makes sense.

---

### Problem 5: Tsirelson Bound

The Tsirelson bound states that $|S| \leq 2\sqrt{2}$ for any quantum state and measurements.

**(a)** Suppose a theory allowed $|S| = 4$. What would the correlations look like? (What values of $E$ would be needed?)

**(b)** Such a theory would violate "no-signaling" — Alice could send information to Bob. Give an intuitive argument for why correlations that are "too strong" would allow communication.

**(c)** Quantum mechanics achieves $|S| = 2\sqrt{2}$, which is between the classical bound (2) and the "super-quantum" bound (4). What does this suggest about quantum mechanics?

---

### Problem 6: Bell State vs Singlet

The Bell state $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$ has correlation $E(\theta) = +\cos\theta$ (correlation, not anti-correlation).

**(a)** Compute the CHSH quantity for $|\Phi^+\rangle$ with the same angles as the singlet case.

**(b)** Does $|\Phi^+\rangle$ also violate CHSH? By how much?

**(c)** Is the violation the same, or different? Explain.

---

### Problem 7: Experimental Considerations

In a real Bell test experiment:

**(a)** Why must Alice and Bob be "spacelike separated" (far enough apart that light can't travel between them during the measurement)?

**(b)** What is the "detection loophole" and how do experimenters close it?

**(c)** The 2015 "loophole-free" Bell tests were considered definitive. What did they achieve that previous experiments hadn't?

**(d)** Despite closing all loopholes, some physicists still explore "superdeterminism." What is this loophole and why do most physicists find it unsatisfying?

---

### Problem 8: CHSH in Qiskit

**(a)** Implement the CHSH test in Qiskit using $|\Phi^+\rangle$ instead of $|\Psi^-\rangle$. Verify violation.

**(b)** Create a "classical" state by measuring the singlet immediately after creation, then running the CHSH protocol. Does this "decohered" state violate CHSH?

**(c)** Scan Alice's angle $A'$ from $0°$ to $180°$ (keeping $A=0°$, $B=45°$, $B'=-45°$). Plot $S$ vs $A'$. At what angle is the violation maximized?

**(d)** What happens if you introduce noise (apply depolarizing noise to the singlet)? At what noise level does the violation disappear?

---

### Problem 9: Conceptual Questions

**(a)** Bell's theorem rules out "local hidden variables." Does it rule out all hidden variable theories? If not, what kind does it allow?

**(b)** Explain in your own words why CHSH violation doesn't allow faster-than-light communication, even though it implies "nonlocality."

**(c)** A skeptic says: "Bell tests just show we don't understand quantum mechanics well enough. Eventually someone will find the hidden variables." How would you respond?

**(d)** Why is Bell's theorem important for quantum computing, not just for philosophy of physics?

---

## Looking Ahead

We've now established the theoretical foundation: entanglement is real, it's non-classical, and it's a resource.

Next lecture, we'll learn how to **create and control** entanglement using quantum gates. We'll study CNOT, CZ, and other two-qubit gates in detail — and connect them to the physical interactions that implement them in real quantum computers.