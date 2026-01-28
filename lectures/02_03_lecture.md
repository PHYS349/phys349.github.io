# Lecture 2.3: Polarization and Measurement

## Review: The Bloch Sphere

In Lecture 2.2, we developed the Bloch sphere as a way to visualize qubit states:

$$|\psi\rangle = \cos\frac{\theta}{2}|0\rangle + e^{i\phi}\sin\frac{\theta}{2}|1\rangle$$

- **Two parameters:** polar angle $\theta$ (amplitude mixing) and azimuthal angle $\phi$ (relative phase)
- **North pole:** $|0\rangle$
- **South pole:** $|1\rangle$  
- **Equator:** equal superpositions with different relative phases

Today we'll see a beautiful physical realization of the Bloch sphere: **the polarization of light**. This will be our first concrete qubit, and we can manipulate it with simple optical elements.

---

## The Electric Field of Light

Light is an electromagnetic wave. For light traveling along the $z$-axis, the electric field oscillates in the transverse $x$-$y$ plane:

$$\vec{E}(z,t) = \begin{pmatrix} E_x(z,t) \\ E_y(z,t) \end{pmatrix}$$

Each component oscillates sinusoidally. Using the complex representation:

$$E_x = \mathcal{E}_x e^{i(kz - \omega t + \phi_x)}, \qquad E_y = \mathcal{E}_y e^{i(kz - \omega t + \phi_y)}$$

The **polarization** of light describes the pattern traced by the electric field vector as the wave passes.

```{figure} polarization_states.svg
:name: polarization-states
:width: 100%

Electric field patterns for different polarization states. Top row: linear polarizations (H, V, D, A). Bottom row: circular polarizations (R, L). The arrows show the E-field direction as seen looking into the beam.
```

---

## The Jones Vector

We can factor out the propagation $e^{i(kz - \omega t)}$ and focus on what makes polarizations different. What remains is the **Jones vector**:

$$\vec{J} = \begin{pmatrix} \mathcal{E}_x e^{i\phi_x} \\ \mathcal{E}_y e^{i\phi_y} \end{pmatrix}$$

This is a 2D complex vector — exactly like our qubit states!

For normalized states (total intensity = 1):
$$|\mathcal{E}_x|^2 + |\mathcal{E}_y|^2 = 1$$

### The Polarization-Qubit Dictionary

| Polarization | Symbol | Jones Vector | Qubit | Bloch Position |
|--------------|--------|--------------|-------|----------------|
| Horizontal | $\|H\rangle$ | $\begin{pmatrix} 1 \\ 0 \end{pmatrix}$ | $\|0\rangle$ | North pole (+z) |
| Vertical | $\|V\rangle$ | $\begin{pmatrix} 0 \\ 1 \end{pmatrix}$ | $\|1\rangle$ | South pole (−z) |
| Diagonal (+45°) | $\|D\rangle$ | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix}$ | $\|+\rangle$ | +x axis |
| Anti-diagonal (−45°) | $\|A\rangle$ | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ -1 \end{pmatrix}$ | $\|-\rangle$ | −x axis |
| Right circular | $\|R\rangle$ | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ i \end{pmatrix}$ | $\|+i\rangle$ | +y axis |
| Left circular | $\|L\rangle$ | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ -i \end{pmatrix}$ | $\|-i\rangle$ | −y axis |

### Visualizing on the Bloch Sphere

```python
import numpy as np
from qiskit.visualization import plot_bloch_multivector
from qiskit.quantum_info import Statevector

# Horizontal polarization |H⟩ = |0⟩ (north pole)
H = Statevector([1, 0])
print("Horizontal |H⟩ = |0⟩:")
plot_bloch_multivector(H)
```

```python
# Vertical polarization |V⟩ = |1⟩ (south pole)
V = Statevector([0, 1])
print("Vertical |V⟩ = |1⟩:")
plot_bloch_multivector(V)
```

```python
# Diagonal polarization |D⟩ = |+⟩ (+x axis)
D = Statevector([1/np.sqrt(2), 1/np.sqrt(2)])
print("Diagonal |D⟩ = |+⟩:")
plot_bloch_multivector(D)
```

```python
# Anti-diagonal polarization |A⟩ = |-⟩ (-x axis)
A = Statevector([1/np.sqrt(2), -1/np.sqrt(2)])
print("Anti-diagonal |A⟩ = |-⟩:")
plot_bloch_multivector(A)
```

```python
# Right circular polarization |R⟩ = |+i⟩ (+y axis)
R = Statevector([1/np.sqrt(2), 1j/np.sqrt(2)])
print("Right circular |R⟩ = |+i⟩:")
plot_bloch_multivector(R)
```

```python
# Left circular polarization |L⟩ = |-i⟩ (-y axis)
L = Statevector([1/np.sqrt(2), -1j/np.sqrt(2)])
print("Left circular |L⟩ = |-i⟩:")
plot_bloch_multivector(L)
```

---

## Relative Phase Creates Different Polarizations

Look carefully at the Jones vectors:

**Diagonal vs. Anti-diagonal:**
$$|D\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix}, \qquad |A\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ -1 \end{pmatrix}$$

Same amplitudes! The only difference is a relative phase of $\pi$ (the minus sign = $e^{i\pi}$).

**Right circular vs. Left circular:**
$$|R\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ i \end{pmatrix}, \qquad |L\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ -i \end{pmatrix}$$

Same amplitudes! The difference is a relative phase of $\pi$ (since $-i = e^{i\pi} \cdot i$).

```{admonition} Key Insight
:class: important

**Relative phase is physical!**

Two states with the same amplitudes but different relative phases represent completely different physical polarizations. This is exactly what we saw in the MZI: relative phase determines interference.
```

---

## iClicker Question 1

**The states $|D\rangle = \frac{1}{\sqrt{2}}(|H\rangle + |V\rangle)$ and $|A\rangle = \frac{1}{\sqrt{2}}(|H\rangle - |V\rangle)$ differ only by a relative phase. On the Bloch sphere, they are located:**

- (A) At the same point
- (B) On opposite sides of the sphere (antipodal) ✓
- (C) Both on the equator but 90° apart
- (D) One at a pole, one on the equator

**Solution:** $|D\rangle$ is at +x and $|A\rangle$ is at −x. They are antipodal (opposite sides), which means they are orthogonal states: $\langle D|A\rangle = 0$.

---

## Three Orthogonal Bases

The six polarization states form three pairs of orthogonal states. Each pair defines a **basis** for the 2D space.

### The z-axis: H/V Basis

$$\langle H|V\rangle = \begin{pmatrix} 1 & 0 \end{pmatrix}\begin{pmatrix} 0 \\ 1 \end{pmatrix} = 0$$

Horizontal and vertical are orthogonal. Any polarization can be written:
$$|\psi\rangle = \alpha|H\rangle + \beta|V\rangle$$

### The x-axis: D/A Basis

$$\langle D|A\rangle = \frac{1}{2}\begin{pmatrix} 1 & 1 \end{pmatrix}\begin{pmatrix} 1 \\ -1 \end{pmatrix} = \frac{1}{2}(1 - 1) = 0$$

Diagonal and anti-diagonal are orthogonal. Any polarization can also be written:
$$|\psi\rangle = \gamma|D\rangle + \delta|A\rangle$$

### The y-axis: R/L Basis

$$\langle R|L\rangle = \frac{1}{2}\begin{pmatrix} 1 & -i \end{pmatrix}\begin{pmatrix} 1 \\ -i \end{pmatrix} = \frac{1}{2}(1 + i^2) = \frac{1}{2}(1-1) = 0$$

Right and left circular are orthogonal. Any polarization can also be written:
$$|\psi\rangle = \epsilon|R\rangle + \zeta|L\rangle$$

### Changing Bases

We can express any state in any basis:

$$|D\rangle = \frac{1}{\sqrt{2}}|H\rangle + \frac{1}{\sqrt{2}}|V\rangle$$

$$|H\rangle = \frac{1}{\sqrt{2}}|D\rangle + \frac{1}{\sqrt{2}}|A\rangle$$

$$|H\rangle = \frac{1}{\sqrt{2}}|R\rangle + \frac{1}{\sqrt{2}}|L\rangle$$

```python
# Verify: |H⟩ in the D/A basis
# |H⟩ = (1/√2)|D⟩ + (1/√2)|A⟩

H_from_DA = (1/np.sqrt(2)) * D.data + (1/np.sqrt(2)) * A.data
print(f"|H⟩ from D/A basis: {H_from_DA}")
print(f"|H⟩ directly: {H.data}")
print(f"Equal? {np.allclose(H_from_DA, H.data)}")
```

### The Three Axes on the Bloch Sphere

| Axis | Basis | Physical Measurement |
|------|-------|---------------------|
| z | $\|H\rangle$, $\|V\rangle$ | Horizontal/Vertical polarizer |
| x | $\|D\rangle$, $\|A\rangle$ | Polarizer at ±45° |
| y | $\|R\rangle$, $\|L\rangle$ | Circular polarizer (with quarter-wave plate) |

Each axis corresponds to a different type of polarization measurement!

---

## iClicker Question 2

**What is $|\langle H|D\rangle|^2$?**

- (A) 0
- (B) 1/4
- (C) 1/2 ✓
- (D) 1

**Solution:** 
$$\langle H|D\rangle = \begin{pmatrix} 1 & 0 \end{pmatrix} \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix} = \frac{1}{\sqrt{2}}$$

$$|\langle H|D\rangle|^2 = \frac{1}{2}$$

This is the probability that D-polarized light passes through an H polarizer.

---

## Polarizers: Projective Measurement

A **polarizer** transmits light polarized along one axis and blocks the orthogonal polarization.

### Horizontal Polarizer

A horizontal polarizer projects onto $|H\rangle$:

$$P_H = |H\rangle\langle H| = \begin{pmatrix} 1 \\ 0 \end{pmatrix}\begin{pmatrix} 1 & 0 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}$$

**What happens to various input states?**

**H-polarized input:**
$$P_H|H\rangle = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}\begin{pmatrix} 1 \\ 0 \end{pmatrix} = \begin{pmatrix} 1 \\ 0 \end{pmatrix} = |H\rangle$$

All light passes, state unchanged.

**V-polarized input:**
$$P_H|V\rangle = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}\begin{pmatrix} 0 \\ 1 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}$$

No light passes.

**D-polarized input:**
$$P_H|D\rangle = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}\frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix} = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 0 \end{pmatrix}$$

The output has norm $1/\sqrt{2}$, so the **intensity** transmitted is $|1/\sqrt{2}|^2 = 1/2$.

After normalizing, the output state is $|H\rangle$.

### Malus's Law

For a polarizer oriented along direction $|\chi\rangle$, the transmitted intensity is:

$$\boxed{I_{out} = |\langle\chi|\psi_{in}\rangle|^2 \cdot I_{in}}$$

This is **Malus's Law** (1809), written in modern notation.

The quantity $|\langle\chi|\psi\rangle|^2$ is the fraction of light that passes through.

---

## iClicker Question 3

**R-polarized light passes through a horizontal polarizer. What fraction of the intensity is transmitted?**

- (A) 0
- (B) 1/4
- (C) 1/2 ✓
- (D) 1

**Solution:**
$$|R\rangle = \frac{1}{\sqrt{2}}(|H\rangle + i|V\rangle)$$

$$\langle H|R\rangle = \frac{1}{\sqrt{2}}$$

$$|\langle H|R\rangle|^2 = \frac{1}{2}$$

Half the light passes through.

```python
# Verify with Qiskit
R = Statevector([1/np.sqrt(2), 1j/np.sqrt(2)])
H = Statevector([1, 0])

# Probability of measuring |H⟩
prob_H = np.abs(np.vdot(H.data, R.data))**2
print(f"P(H|R) = {prob_H}")  # Should be 0.5
```

---

## From Waves to Photons: The Quantum Leap

So far, everything we've done is classical wave optics. Malus's Law tells us what fraction of the **intensity** passes through a polarizer.

Now we take the quantum leap.

### Light is Quantized

Light comes in discrete packets called **photons**, each carrying energy $E = \hbar\omega$.

What happens when we turn down the intensity until only one photon at a time hits the polarizer?

### The Experiment

Send D-polarized photons through an H polarizer, one at a time.

**Classical prediction:** Each photon should "half pass" — we'd see a continuous dim beam.

**What actually happens:** Each photon either passes completely OR is absorbed completely. There is no "half a photon."

Over many photons, 50% pass and 50% are absorbed. But each individual event is all-or-nothing.

```{admonition} The Fundamental Discreteness
:class: important

**A photon is indivisible.**

When a photon encounters a polarizer:
- It either passes through completely, OR
- It is absorbed completely

The classical "intensity fraction" becomes a **probability** for individual photon events.
```

### The Born Rule

This forces us to reinterpret Malus's Law:

**Classical (waves):** $I_{out}/I_{in} = |\langle\chi|\psi\rangle|^2$ — "What fraction of intensity passes?"

**Quantum (photons):** $P(\text{pass}) = |\langle\chi|\psi\rangle|^2$ — "What is the probability this photon passes?"

This is the **Born Rule**, a fundamental postulate of quantum mechanics.

---

## Measurement Changes the State

Here's something crucial: **if a photon passes through an H polarizer, what is its polarization afterward?**

**Answer:** It's $|H\rangle$, regardless of what it was before!

The polarizer doesn't just filter — it **prepares** a definite state. A photon that passes through an H polarizer emerges horizontally polarized.

$$|\psi\rangle \xrightarrow{\text{passes H polarizer}} |H\rangle$$

This is called **state collapse** or **projection**.

---

## iClicker Question 4

**A photon in state $|D\rangle$ passes through an H polarizer. What is its state afterward?**

- (A) $|D\rangle$ (unchanged)
- (B) $|H\rangle$ ✓
- (C) $\frac{1}{\sqrt{2}}|H\rangle$ (reduced amplitude)
- (D) Could be either $|H\rangle$ or $|V\rangle$

**Solution:** If the photon passes, it must now be horizontally polarized. The measurement has changed (collapsed) its state to $|H\rangle$.

The factor of $1/\sqrt{2}$ determined the *probability* of passing, not the output state.

---

## The Three-Polarizer Paradox

Now we can understand a striking quantum effect.

### Setup 1: H polarizer → V polarizer

1. Light starts H-polarized: $|H\rangle$
2. H polarizer: all passes (state remains $|H\rangle$)
3. V polarizer: $P = |\langle V|H\rangle|^2 = 0$

**Result: 0% transmission.** H and V are orthogonal.

```python
# Setup 1: H → V
print("Setup 1: H → V")
prob_V_given_H = np.abs(np.vdot(V.data, H.data))**2
print(f"Transmission: {prob_V_given_H * 100}%")
```

### Setup 2: H polarizer → D polarizer → V polarizer

1. Light starts H-polarized: $|H\rangle$
2. H polarizer: all passes (state is $|H\rangle$)
3. D polarizer: $P = |\langle D|H\rangle|^2 = 1/2$ passes, state becomes $|D\rangle$
4. V polarizer: $P = |\langle V|D\rangle|^2 = 1/2$ of that passes, state becomes $|V\rangle$

**Total transmission:** $1 \times \frac{1}{2} \times \frac{1}{2} = \frac{1}{4}$

**Result: 25% transmission!**

```python
# Setup 2: H → D → V
print("\nSetup 2: H → D → V")
prob_D_given_H = np.abs(np.vdot(D.data, H.data))**2
prob_V_given_D = np.abs(np.vdot(V.data, D.data))**2
total = prob_D_given_H * prob_V_given_D
print(f"P(D|H) = {prob_D_given_H}")
print(f"P(V|D) = {prob_V_given_D}")
print(f"Total transmission: {total * 100}%")
```

### The Paradox

**Adding a polarizer INCREASED the transmission from 0% to 25%!**

Classically, this makes no sense. A polarizer can only absorb light, never create it. How can adding an obstacle let more light through?

### The Resolution

The key is that **measurement changes the state**.

After the H polarizer, the state is $|H\rangle$. In the H/V basis, this state is definite — it has zero probability of passing a V polarizer.

But the D polarizer asks a *different* question. In the D/A basis, $|H\rangle$ is NOT definite:

$$|H\rangle = \frac{1}{\sqrt{2}}|D\rangle + \frac{1}{\sqrt{2}}|A\rangle$$

So there's a 50% chance of passing. Crucially, photons that pass are now in state $|D\rangle$ — they've been *changed* by the measurement.

In the H/V basis, $|D\rangle$ is again not definite:

$$|D\rangle = \frac{1}{\sqrt{2}}|H\rangle + \frac{1}{\sqrt{2}}|V\rangle$$

So there's now a 50% chance of passing the V polarizer!

The intermediate measurement "scrambles" the H/V relationship, giving the photon another chance.

```{admonition} The Lesson
:class: important

**Measurement is not passive observation — it's an active intervention that changes the state.**

The D polarizer doesn't just "check" the polarization. It projects onto a new basis, fundamentally altering what the photon "is."
```

---

## iClicker Question 5

**In the three-polarizer setup H → D → V, we get 25% transmission. If we replace the D polarizer with an A polarizer (H → A → V), what transmission do we get?**

- (A) 0%
- (B) 12.5%
- (C) 25% ✓
- (D) 50%

**Solution:** 
- $P(A|H) = |\langle A|H\rangle|^2 = 1/2$
- $P(V|A) = |\langle V|A\rangle|^2 = 1/2$
- Total: $1/2 \times 1/2 = 1/4 = 25\%$

Same answer! Both D and A are at 45° to H and V.

---

## Visualizing on the Bloch Sphere

The three-polarizer paradox has a beautiful interpretation on the Bloch sphere.

```python
import numpy as np
from qiskit.visualization import plot_bloch_multivector
from qiskit.quantum_info import Statevector

def show_state(name, state_vector):
    """Display a state on the Bloch sphere"""
    state = Statevector(state_vector)
    print(f"\n{name}:")
    return plot_bloch_multivector(state)

# The three-polarizer sequence
print("=== Three-Polarizer Paradox on Bloch Sphere ===")

# Step 1: Start with |H⟩ (north pole)
show_state("Step 1: |H⟩ (after H polarizer)", [1, 0])

# Step 2: Project onto D (moves to +x axis)
show_state("Step 2: |D⟩ (after D polarizer)", [1/np.sqrt(2), 1/np.sqrt(2)])

# Step 3: Project onto V (moves to south pole)
show_state("Step 3: |V⟩ (after V polarizer)", [0, 1])
```

**The path on the Bloch sphere:**
1. Start at north pole ($|H\rangle$)
2. D measurement projects to +x axis ($|D\rangle$) — moved to equator!
3. V measurement projects to south pole ($|V\rangle$) — reached the "forbidden" destination

Without the D polarizer, we'd try to go directly from north pole to south pole via a V measurement — impossible since they're orthogonal.

---

## Summary

1. **Polarization is a qubit:** The Jones vector $(E_x, E_y)^T$ is a 2D complex vector, just like qubit states

2. **Polarization ↔ Bloch sphere:**
   - H/V = north/south poles (z-axis)
   - D/A = ±x axis
   - R/L = ±y axis

3. **Relative phase is physical:** D and A have the same amplitudes but differ by a phase of $\pi$ — completely different polarizations

4. **Three orthogonal bases:** H/V, D/A, R/L each form a complete basis; correspond to three measurement types

5. **Polarizers = projective measurement:** Transmitted intensity given by Malus's Law: $I = |\langle\chi|\psi\rangle|^2$

6. **Photons are discrete:** Each photon passes or doesn't — Born Rule gives probability

7. **Measurement changes the state:** A photon that passes a polarizer emerges in that polarization state

8. **Three-polarizer paradox:** Adding a polarizer can increase transmission because measurement changes the state

---

## Looking Ahead

Next lecture: 
- Pauli matrices as the generators of rotations
- How to rotate states on the Bloch sphere
- Connection to spin-1/2 particles and the Stern-Gerlach experiment

---

## Homework

### Problem 1: Jones Vectors

Write the normalized Jones vector for each polarization state:

**(a)** Linear polarization at 30° from horizontal

**(b)** Linear polarization at 60° from horizontal

**(c)** Linear polarization at angle $\theta$ from horizontal (general formula)

**(d)** Verify that your answer to (c) gives the correct results for $\theta = 0°$ (H), $\theta = 45°$ (D), and $\theta = 90°$ (V).

---

### Problem 2: Orthogonality and Inner Products

**(a)** Verify that $\langle D|A\rangle = 0$ by explicit calculation.

**(b)** Verify that $\langle R|L\rangle = 0$ by explicit calculation.

**(c)** Calculate $\langle H|R\rangle$ and $|\langle H|R\rangle|^2$. Interpret physically.

**(d)** Calculate $\langle D|R\rangle$ and $|\langle D|R\rangle|^2$. Interpret physically.

---

### Problem 3: Polarizer Calculations

A photon is prepared in state $|R\rangle = \frac{1}{\sqrt{2}}(|H\rangle + i|V\rangle)$.

**(a)** What is the probability it passes an H polarizer?

**(b)** What is the probability it passes a V polarizer?

**(c)** What is the probability it passes a D polarizer?

**(d)** What is the probability it passes an R polarizer?

**(e)** What is the probability it passes an L polarizer?

**(f)** Verify that $P(H) + P(V) = 1$, $P(D) + P(A) = 1$, and $P(R) + P(L) = 1$.

---

### Problem 4: Three-Polarizer Paradox — Quantitative

**(a)** Calculate the transmission for H → V (verify it's 0%).

**(b)** Calculate the transmission for H → D → V (verify it's 25%).

**(c)** Calculate the transmission for H → (polarizer at 30°) → V.

**(d)** Calculate the transmission for H → (polarizer at 60°) → V.

**(e)** For an intermediate polarizer at angle $\theta$, show that the transmission through H → θ → V is:
$$T(\theta) = \cos^2\theta \sin^2\theta = \frac{1}{4}\sin^2(2\theta)$$

**(f)** What angle $\theta$ maximizes the transmission? What is the maximum?

---

### Problem 5: Many Intermediate Polarizers

Consider inserting $n$ polarizers evenly spaced in angle between H (0°) and V (90°).

**(a)** For $n = 1$ (one polarizer at 45°), the transmission is 25%. Verify this.

**(b)** For $n = 2$ (polarizers at 30° and 60°), calculate the transmission.

**(c)** For $n = 3$ (polarizers at 22.5°, 45°, 67.5°), calculate the transmission.

**(d)** For general $n$, the polarizers are at angles $\theta_k = k \cdot 90°/(n+1)$ for $k = 1, 2, ..., n$. Each step rotates by angle $\Delta\theta = 90°/(n+1)$. Show that the total transmission is:
$$T(n) = \cos^{2(n+1)}\left(\frac{\pi}{2(n+1)}\right)$$

*Hint:* Each polarizer transmits $\cos^2(\Delta\theta)$ of the incoming light.

**(e)** Calculate $T(n)$ numerically for $n = 1, 2, 3, 5, 10, 100$.

**(f)** What happens as $n \to \infty$? Explain physically why adding more polarizers *increases* transmission.

---

### Problem 6: Bloch Sphere and Measurement

**(a)** A state has Bloch coordinates $\theta = \pi/3$, $\phi = 0$. Write out the state in the form $\alpha|H\rangle + \beta|V\rangle$.

**(b)** For this state, calculate $P(H)$, $P(V)$, $P(D)$, and $P(A)$.

**(c)** Where on the Bloch sphere is this state located? Sketch it.

**(d)** What state is orthogonal to this one? Give its Bloch coordinates.

---

### Problem 7: Circular Polarization

**(a)** Show that $|R\rangle$ and $|L\rangle$ can be written as:
$$|R\rangle = \frac{1}{\sqrt{2}}(|D\rangle + i|A\rangle)$$
$$|L\rangle = \frac{1}{\sqrt{2}}(|D\rangle - i|A\rangle)$$

**(b)** A right-circularly polarized photon hits a D polarizer. What is the probability it passes? What is its state after passing?

**(c)** That photon then hits an A polarizer. What is the probability it passes?

**(d)** What is the total probability of passing both D then A polarizers, starting from $|R\rangle$?

---

### Problem 8: Quantum vs Classical (Conceptual)

**(a)** In classical wave optics, we say "half the intensity passes through." In quantum mechanics, we say "each photon has a 50% probability of passing." Explain why these give the same experimental predictions for large numbers of photons.

**(b)** Describe an experiment that would distinguish between the classical and quantum descriptions. *Hint:* Think about what happens with single photons.

**(c)** The three-polarizer paradox seems impossible classically: adding an absorber increases transmission. Explain in your own words why this happens quantum mechanically.