# Lecture 6: Polarization and the Qubit


# Lecture 6: Polarization — The Jones Vector and Unitaries

## From Interferometer to Polarization

Last lecture, we saw interference in a Michelson interferometer: light splits into two paths, accumulates relative phase, and recombines. The two paths formed a 2D state space.

Today we'll see another 2D state space hiding in plain sight: the **polarization** of light. This will be our first concrete qubit—and unlike interferometer paths, you can manipulate polarization with cheap plastic filters.

---

## The Electric Field of Light

Light is an electromagnetic wave. For light traveling along the $z$-axis, Maxwell's equations tell us the electric field oscillates in the transverse $x$-$y$ plane:

$$
\vec{E}(z,t) = \begin{pmatrix} E_x(z,t) \\ E_y(z,t) \end{pmatrix}
$$

Each component oscillates sinusoidally:

$$
E_x(z,t) = \mathcal{E}_x \cos(kz - \omega t + \phi_x)
$$
$$
E_y(z,t) = \mathcal{E}_y \cos(kz - \omega t + \phi_y)
$$

Using the complex representation from last lecture:

$$
\tilde{E}_x = \mathcal{E}_x e^{i\phi_x}, \qquad \tilde{E}_y = \mathcal{E}_y e^{i\phi_y}
$$

**Figure:** [Draw coordinate system with light propagating along z, electric field oscillating in x-y plane]

---

## The Jones Vector

We can strip away the propagation factor $e^{i(kz - \omega t)}$ and focus on what makes one polarization different from another. What remains is a 2D complex vector called the **Jones vector**:

$$
\vec{J} = \begin{pmatrix} \tilde{E}_x \\ \tilde{E}_y \end{pmatrix} = \begin{pmatrix} \mathcal{E}_x e^{i\phi_x} \\ \mathcal{E}_y e^{i\phi_y} \end{pmatrix}
$$

This is exactly the structure we saw in the interferometer: a superposition of two configurations (now $x$ and $y$ polarization) with complex amplitudes.

### Normalized Jones Vectors

For describing the *state* of polarization (not the intensity), we normalize:

$$
|\tilde{E}_x|^2 + |\tilde{E}_y|^2 = 1
$$

Now the Jones vector lives on the unit sphere in $\mathbb{C}^2$.

### Standard Polarization States

**Linear polarizations:**

| State | Symbol | Jones Vector | Physical Description |
|-------|--------|--------------|---------------------|
| Horizontal | $\|H\rangle$ | $\begin{pmatrix} 1 \\ 0 \end{pmatrix}$ | E-field along x |
| Vertical | $\|V\rangle$ | $\begin{pmatrix} 0 \\ 1 \end{pmatrix}$ | E-field along y |
| Diagonal (+45°) | $\|D\rangle$ | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix}$ | E-field along x+y |
| Anti-diagonal (-45°) | $\|A\rangle$ | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ -1 \end{pmatrix}$ | E-field along x-y |

**Circular polarizations:**

| State | Symbol | Jones Vector | Physical Description |
|-------|--------|--------------|---------------------|
| Right circular | $\|R\rangle$ | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ i \end{pmatrix}$ | E-field rotates clockwise |
| Left circular | $\|L\rangle$ | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ -i \end{pmatrix}$ | E-field rotates counter-clockwise |

**Figure:** [Draw the E-field traces for each polarization state: linear oscillations for H/V/D/A, circular helices for R/L]

### Relative Phase Creates Different States

Notice what distinguishes these states:

- $|H\rangle$ and $|V\rangle$ differ in *which* component is nonzero
- $|D\rangle$ and $|A\rangle$ have the same magnitudes, but differ by a relative phase of $\pi$ (the minus sign)
- $|R\rangle$ and $|L\rangle$ have the same magnitudes, but differ by a relative phase of $\pm\pi/2$ (the $\pm i$)

Just like in the interferometer: **relative phase is physical**. Same amplitudes, different phases, completely different polarization states.

---

## Three Pairs of Orthogonal States

Look at the states we've defined. They come in orthogonal pairs:

**H/V basis:**
$$
\langle H | V \rangle = \begin{pmatrix} 1 & 0 \end{pmatrix} \begin{pmatrix} 0 \\ 1 \end{pmatrix} = 0
$$

**D/A basis:**
$$
\langle D | A \rangle = \frac{1}{2}\begin{pmatrix} 1 & 1 \end{pmatrix} \begin{pmatrix} 1 \\ -1 \end{pmatrix} = \frac{1}{2}(1 - 1) = 0
$$

**R/L basis:**
$$
\langle R | L \rangle = \frac{1}{2}\begin{pmatrix} 1 & -i \end{pmatrix} \begin{pmatrix} 1 \\ -i \end{pmatrix} = \frac{1}{2}(1 + i^2) = \frac{1}{2}(1-1) = 0
$$

Each pair forms a complete basis for $\mathbb{C}^2$. Any polarization state can be written as a superposition of H and V, *or* as a superposition of D and A, *or* as a superposition of R and L.

For example:
$$
|D\rangle = \frac{1}{\sqrt{2}}|H\rangle + \frac{1}{\sqrt{2}}|V\rangle
$$

$$
|H\rangle = \frac{1}{\sqrt{2}}|D\rangle + \frac{1}{\sqrt{2}}|A\rangle
$$

$$
|H\rangle = \frac{1}{\sqrt{2}}|R\rangle + \frac{1}{\sqrt{2}}|L\rangle
$$

These three bases will correspond to three axes on the Bloch sphere—and eventually to the three Pauli matrices.

---

## Transformations: Waveplates as Unitaries

Optical elements can transform polarization states. The most important are **waveplates**: crystals that introduce a phase delay between the two polarization components.

### How Waveplates Work

A birefringent crystal has different refractive indices for light polarized along its "fast" and "slow" axes. Light polarized along the slow axis travels slower, accumulating extra phase.

If the fast axis is along $x$ and the slow axis is along $y$, the transformation is:

$$
\begin{pmatrix} E_x \\ E_y \end{pmatrix} \to \begin{pmatrix} E_x \\ e^{i\delta} E_y \end{pmatrix}
$$

where $\delta$ is the phase retardation (depends on crystal thickness and wavelength).

In matrix form:

$$
W(\delta) = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\delta} \end{pmatrix}
$$

### Special Cases

**Quarter-wave plate (QWP):** $\delta = \pi/2$

$$
W_{\pi/2} = \begin{pmatrix} 1 & 0 \\ 0 & i \end{pmatrix}
$$

This converts linear to circular polarization:
$$
W_{\pi/2} |D\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 0 \\ 0 & i \end{pmatrix}\begin{pmatrix} 1 \\ 1 \end{pmatrix} = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ i \end{pmatrix} = |R\rangle
$$

**Half-wave plate (HWP):** $\delta = \pi$

$$
W_{\pi} = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}
$$

This flips the sign of the $y$-component:
$$
W_{\pi} |D\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ -1 \end{pmatrix} = |A\rangle
$$

A half-wave plate converts D to A (and vice versa), or rotates linear polarization.

### Waveplates are Unitary

Notice that these matrices satisfy $W^\dagger W = I$:

$$
W(\delta)^\dagger W(\delta) = \begin{pmatrix} 1 & 0 \\ 0 & e^{-i\delta} \end{pmatrix}\begin{pmatrix} 1 & 0 \\ 0 & e^{i\delta} \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = I
$$

This means waveplates preserve the norm of the Jones vector: no light is lost, just transformed.

### Rotated Waveplates

If the waveplate's fast axis is at angle $\theta$ from horizontal, we first rotate to the waveplate frame, apply the phase, then rotate back:

$$
W(\delta, \theta) = R(-\theta) \begin{pmatrix} 1 & 0 \\ 0 & e^{i\delta} \end{pmatrix} R(\theta)
$$

where $R(\theta)$ is the rotation matrix:

$$
R(\theta) = \begin{pmatrix} \cos\theta & \sin\theta \\ -\sin\theta & \cos\theta \end{pmatrix}
$$

This is still unitary (product of unitaries is unitary).

```{admonition} Connection to SU(2)
:class: note

The group of $2 \times 2$ unitary matrices with determinant 1 is called $SU(2)$. All polarization-preserving transformations (waveplates at any angle) form a subgroup of $U(2)$.

Since global phase doesn't affect polarization (we'll see this soon), what matters is $SU(2)$. This is the same group that describes spin-1/2 rotations—not a coincidence!
```

---

## The Polarizer: Projection

A polarizer is different from a waveplate. Instead of transforming the state, it **projects** onto one polarization and blocks the orthogonal component.

### Horizontal Polarizer

A horizontal polarizer transmits only the $x$-component:

$$
P_H = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}
$$

Let's see what this does to various inputs:

**H input:**
$$
P_H |H\rangle = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}\begin{pmatrix} 1 \\ 0 \end{pmatrix} = \begin{pmatrix} 1 \\ 0 \end{pmatrix} = |H\rangle
$$
All light passes through, unchanged.

**V input:**
$$
P_H |V\rangle = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}\begin{pmatrix} 0 \\ 1 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}
$$
No light passes through.

**D input:**
$$
P_H |D\rangle = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}\frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix} = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 0 \end{pmatrix}
$$

The output has norm $1/\sqrt{2}$, so the intensity is $|1/\sqrt{2}|^2 = 1/2$.

Half the light passes through. After normalizing, the output state is $|H\rangle$.

### The Projection Rule

For a polarizer oriented along direction $|\chi\rangle$, the transmitted amplitude is:

$$
|\psi_{\text{out}}\rangle = \langle\chi|\psi_{\text{in}}\rangle \cdot |\chi\rangle
$$

The transmitted **intensity** (fraction of power) is:

$$
I_{\text{out}} = |\langle\chi|\psi_{\text{in}}\rangle|^2
$$

This is Malus's Law (1809), written in modern notation.

### Polarizers Are Not Unitary

Notice that $P_H^\dagger P_H = P_H \neq I$. The polarizer is a **projector**, not a unitary transformation. It reduces the dimension of the space—information is lost.

This distinction will become crucial:
- **Waveplates** = unitary = reversible = quantum gates
- **Polarizers** = projectors = irreversible = measurements

---

## Three Measurement Bases

We can build polarizers that project onto any of our three bases:

**H/V measurement:**
- H polarizer: transmits H, blocks V
- V polarizer: transmits V, blocks H

**D/A measurement:**
- D polarizer (at +45°): transmits D, blocks A
- A polarizer (at -45°): transmits A, blocks D

**R/L measurement:**
- Right circular polarizer: transmits R, blocks L
- Left circular polarizer: transmits L, blocks R

Given a state $|\psi\rangle$, the probability of passing through each polarizer is:

| Polarizer | Probability |
|-----------|-------------|
| H | $\|\langle H\|\psi\rangle\|^2$ |
| V | $\|\langle V\|\psi\rangle\|^2$ |
| D | $\|\langle D\|\psi\rangle\|^2$ |
| A | $\|\langle A\|\psi\rangle\|^2$ |
| R | $\|\langle R\|\psi\rangle\|^2$ |
| L | $\|\langle L\|\psi\rangle\|^2$ |

And for any basis: $P(\text{one outcome}) + P(\text{other outcome}) = 1$.

---

## Example: D State Through Various Polarizers

Let's analyze $|D\rangle = \frac{1}{\sqrt{2}}(|H\rangle + |V\rangle)$:

**Through H polarizer:**
$$
P(H) = |\langle H|D\rangle|^2 = \left|\frac{1}{\sqrt{2}}\right|^2 = \frac{1}{2}
$$

**Through V polarizer:**
$$
P(V) = |\langle V|D\rangle|^2 = \left|\frac{1}{\sqrt{2}}\right|^2 = \frac{1}{2}
$$

Check: $P(H) + P(V) = 1$ ✓

**Through D polarizer:**
$$
P(D) = |\langle D|D\rangle|^2 = 1
$$

**Through A polarizer:**
$$
P(A) = |\langle A|D\rangle|^2 = \left|\frac{1}{2}(1 - 1)\right|^2 = 0
$$

Check: $P(D) + P(A) = 1$ ✓

**Through R polarizer:**
$$
\langle R|D\rangle = \frac{1}{2}\begin{pmatrix} 1 & -i \end{pmatrix}\begin{pmatrix} 1 \\ 1 \end{pmatrix} = \frac{1}{2}(1 - i)
$$
$$
P(R) = \left|\frac{1-i}{2}\right|^2 = \frac{1 + 1}{4} = \frac{1}{2}
$$

**Through L polarizer:**
$$
P(L) = \frac{1}{2}
$$

Check: $P(R) + P(L) = 1$ ✓

The D state is:
- Definite in the D/A basis (100% D, 0% A)
- Completely uncertain in the H/V basis (50/50)
- Completely uncertain in the R/L basis (50/50)

This is a preview of the uncertainty principle: a state can't be definite in all bases simultaneously.

---

## Summary

1. **Jones vector:** Polarization is described by a 2D complex vector $\vec{J} = (E_x, E_y)^T$

2. **Three orthogonal bases:** H/V, D/A, R/L — each pair spans $\mathbb{C}^2$

3. **Relative phase matters:** D and A have the same amplitudes but opposite relative phase

4. **Waveplates are unitary:** They transform polarization without loss; form subgroup of $SU(2)$

5. **Polarizers are projectors:** They project onto one axis, blocking the orthogonal component; not reversible

6. **Malus's Law:** Transmitted intensity is $I = |\langle\chi|\psi\rangle|^2$

7. **Different bases give different outcomes:** A state definite in one basis is uncertain in the others

---

## Looking Ahead

So far, this has all been classical wave optics with complex notation. The Jones vector is a mathematical convenience, and Malus's Law is about intensity.

Next lecture, we confront the quantum world: What happens when we turn down the intensity until only *one photon at a time* passes through? The photon either passes or doesn't—there's no "half a photon." This discreteness forces us to interpret $|\langle\chi|\psi\rangle|^2$ as a **probability**, not just an intensity ratio.

And we'll see the three-polarizer paradox: adding a polarizer can *increase* transmission. This makes no sense classically—but follows naturally from the quantum rules.









# Lecture 7: Measurement and the Bloch Sphere

## From Waves to Photons

Last lecture, we described polarization using Jones vectors and found that a polarizer transmits intensity $I = |\langle\chi|\psi\rangle|^2$. This was classical wave optics.

Today we take the quantum leap. We'll see that:
- Light comes in discrete packets (photons)
- A single photon either passes a polarizer or doesn't—no partial transmission
- The Jones vector becomes a quantum state, and $|\langle\chi|\psi\rangle|^2$ becomes a probability
- Measurement fundamentally changes the state

---

## Light is Quantized

Recall from Lecture 1: Planck and Einstein discovered that light comes in discrete packets called **photons**, each carrying energy $E = \hbar\omega$.

This was forced by experiments like the photoelectric effect: light below a threshold frequency cannot eject electrons, no matter how intense. Each photon must individually have enough energy.

What does this mean for polarization?

### The Experiment

Imagine we send polarized light through a polarizer and detect what comes out. We can reduce the intensity until we're sending, on average, one photon at a time.

**Classical prediction:** If D-polarized light hits an H polarizer, half the intensity passes. We should see a continuous, dim beam.

**What actually happens:** We see individual detection events—clicks in a photon detector. Each photon either passes (click) or doesn't (no click). There's no "half a photon."

Over many photons, the *fraction* that pass approaches $1/2$. But each individual event is all-or-nothing.

```{admonition} The Fundamental Discreteness
:class: important

**A photon is indivisible.**

When a photon encounters a polarizer:
- It either passes through completely, OR
- It is absorbed completely

The classical "intensity fraction" becomes a **probability** for individual events.
```

---

## The Born Rule

This forces a reinterpretation of our formulas.

**Classical (waves):** 
$$
I_{\text{transmitted}} = |\langle\chi|\psi\rangle|^2 \cdot I_{\text{incident}}
$$
"What fraction of the intensity passes through?"

**Quantum (photons):**
$$
P(\text{pass}) = |\langle\chi|\psi\rangle|^2
$$
"What is the probability that this photon passes?"

This is the **Born rule**, one of the fundamental postulates of quantum mechanics.

### State After Measurement

Here's something crucial: if a photon passes through an H polarizer, what is its polarization afterward?

**Answer:** It's now $|H\rangle$, regardless of what it was before.

The polarizer doesn't just filter—it **prepares** a state. A photon that passes through an H polarizer emerges with definite H polarization.

This is state collapse, or projection:

$$
|\psi\rangle \xrightarrow{\text{measure H, get "pass"}} |H\rangle
$$

---

## When Does a Photon "Become" a Particle?

This is one of the deep questions of quantum mechanics.

Before the polarizer, the photon has a definite polarization state—say $|D\rangle$. This state tells us the *probabilities* for various measurements, but doesn't determine which outcome will occur.

The photon isn't "secretly" H or V before we measure. It's genuinely in a superposition:
$$
|D\rangle = \frac{1}{\sqrt{2}}|H\rangle + \frac{1}{\sqrt{2}}|V\rangle
$$

When does the definite outcome occur? **At the measurement apparatus.**

The polarizer plus detector constitutes a measurement. The interaction with this macroscopic system produces a definite, recorded outcome: the photon passed, or it didn't.

```{admonition} The Measurement Principle
:class: note

A quantum system has definite states (superpositions). It acquires definite *outcomes* only through interaction with a measurement apparatus.

The apparatus must be macroscopic—it records a result that can be read out, copied, and remembered. This irreversible recording is what distinguishes measurement from other interactions.
```

We won't fully resolve the "measurement problem" (it's still debated). But operationally: apply the Born rule at the detector, and you'll get the right statistics.

---

## The Three-Polarizer Paradox

Now we can understand a striking quantum effect.

### Setup 1: H then V

Send H-polarized light through:
1. H polarizer → all passes (state is $|H\rangle$)
2. V polarizer → none passes (H is orthogonal to V)

**Result:** No light gets through.

### Setup 2: H then D then V

Now insert a D polarizer between them:
1. H polarizer → all passes (state is $|H\rangle$)
2. D polarizer → half passes (probability $|\langle D|H\rangle|^2 = 1/2$), state becomes $|D\rangle$
3. V polarizer → half of that passes (probability $|\langle V|D\rangle|^2 = 1/2$), state becomes $|V\rangle$

**Result:** $1 \times 1/2 \times 1/2 = 1/4$ of the light gets through!

### The Paradox

Adding an intermediate polarizer *increased* the transmission from 0 to 25%.

Classically, this makes no sense. A polarizer can only remove light, never add it. How can adding an obstacle let more through?

### The Resolution

The key is that **measurement changes the state**.

After the H polarizer, the state is $|H\rangle$. In the H/V basis, this is definite—it *will* fail the V test.

But the D polarizer asks a different question. In the D/A basis, $|H\rangle$ is *not* definite:
$$
|H\rangle = \frac{1}{\sqrt{2}}|D\rangle + \frac{1}{\sqrt{2}}|A\rangle
$$

So there's a 50% chance of passing. And crucially, photons that pass are now in state $|D\rangle$—they've been *changed* by the measurement.

In the H/V basis, $|D\rangle$ is again not definite:
$$
|D\rangle = \frac{1}{\sqrt{2}}|H\rangle + \frac{1}{\sqrt{2}}|V\rangle
$$

So there's a 50% chance of passing the final V polarizer.

The intermediate measurement "scrambles" the definite H/V relationship, giving the photon another chance.

```{admonition} Key Insight
:class: important

**Measurement is not passive observation—it's an active intervention that changes the state.**

The D polarizer doesn't just "check" the polarization. It projects the state onto the D/A basis, fundamentally altering what the photon "is."
```

---

## Global Phase Doesn't Matter

Before we move to the Bloch sphere, let's establish a key fact.

Consider two states that differ only by a global phase:
$$
|\psi\rangle \quad \text{vs} \quad e^{i\gamma}|\psi\rangle
$$

Are they physically different?

**No.** All measurable quantities are the same:

$$
|\langle\chi|e^{i\gamma}\psi\rangle|^2 = |e^{i\gamma}|^2 |\langle\chi|\psi\rangle|^2 = |\langle\chi|\psi\rangle|^2
$$

The probability of any measurement outcome is unchanged.

This means the physical state of a qubit is not a vector in $\mathbb{C}^2$, but an equivalence class:

$$
|\psi\rangle \sim e^{i\gamma}|\psi\rangle \quad \text{for any } \gamma \in \mathbb{R}
$$

Mathematically, physical states are **rays** in Hilbert space, not vectors.

---

## The Bloch Sphere: Derivation

We now derive the Bloch sphere—a way to visualize all possible states of a two-level system (qubit) as points on a sphere.

### Step 1: Count the Parameters

A general state in $\mathbb{C}^2$ is:
$$
|\psi\rangle = \begin{pmatrix} \alpha \\ \beta \end{pmatrix}
$$

where $\alpha, \beta \in \mathbb{C}$.

Two complex numbers = 4 real parameters.

### Step 2: Apply Normalization

We require $|\alpha|^2 + |\beta|^2 = 1$.

This is one real constraint, leaving **3 real parameters**.

### Step 3: Remove Global Phase

Since $|\psi\rangle \sim e^{i\gamma}|\psi\rangle$, we can always choose $\alpha$ to be real and non-negative.

Write $\alpha = |\alpha| e^{i\phi_\alpha}$. We can multiply the whole state by $e^{-i\phi_\alpha}$ to make $\alpha$ real:
$$
|\psi\rangle \to e^{-i\phi_\alpha}|\psi\rangle = \begin{pmatrix} |\alpha| \\ \beta e^{-i\phi_\alpha} \end{pmatrix}
$$

Now $\alpha \geq 0$ is real. This removes one more parameter, leaving **2 real parameters**.

### Step 4: Parametrize

With $\alpha$ real and non-negative, and $|\alpha|^2 + |\beta|^2 = 1$, we can write:
$$
\alpha = \cos\frac{\theta}{2}, \qquad |\beta| = \sin\frac{\theta}{2}
$$

for some $\theta \in [0, \pi]$.

(The factor of $1/2$ is conventional and will be convenient later.)

The remaining freedom is the phase of $\beta$. Write $\beta = e^{i\phi}\sin\frac{\theta}{2}$ for $\phi \in [0, 2\pi)$.

### The Bloch Parametrization

Any qubit state can be written as:

$$
\boxed{|\psi\rangle = \cos\frac{\theta}{2}|0\rangle + e^{i\phi}\sin\frac{\theta}{2}|1\rangle}
$$

where:
- $\theta \in [0, \pi]$ is the **polar angle** from the north pole
- $\phi \in [0, 2\pi)$ is the **azimuthal angle** around the equator

These are exactly the coordinates of a point on a unit sphere!

**Figure:** [Draw the Bloch sphere with θ as angle from +z axis, φ as angle in x-y plane from +x axis]

---

## Points on the Bloch Sphere

Let's identify our polarization states on the Bloch sphere.

### The Poles: $|0\rangle$ and $|1\rangle$

**North pole** ($\theta = 0$):
$$
|\psi\rangle = \cos(0)|0\rangle + e^{i\phi}\sin(0)|1\rangle = |0\rangle
$$
This is $|H\rangle$ in polarization language.

**South pole** ($\theta = \pi$):
$$
|\psi\rangle = \cos(\pi/2)|0\rangle + e^{i\phi}\sin(\pi/2)|1\rangle = e^{i\phi}|1\rangle \sim |1\rangle
$$
This is $|V\rangle$ in polarization language.

### The Equator: Equal Superpositions

On the equator, $\theta = \pi/2$, so $\cos(\theta/2) = \sin(\theta/2) = 1/\sqrt{2}$:
$$
|\psi\rangle = \frac{1}{\sqrt{2}}\left(|0\rangle + e^{i\phi}|1\rangle\right)
$$

**$\phi = 0$ (positive x-axis):**
$$
|\psi\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) = |D\rangle = |+\rangle
$$

**$\phi = \pi$ (negative x-axis):**
$$
|\psi\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle) = |A\rangle = |-\rangle
$$

**$\phi = \pi/2$ (positive y-axis):**
$$
|\psi\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle) = |R\rangle = |{+i}\rangle
$$

**$\phi = 3\pi/2$ or $-\pi/2$ (negative y-axis):**
$$
|\psi\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle) = |L\rangle = |{-i}\rangle
$$

### Summary of Bloch Sphere Positions

| State | $\theta$ | $\phi$ | Position |
|-------|----------|--------|----------|
| $\|H\rangle = \|0\rangle$ | $0$ | — | North pole |
| $\|V\rangle = \|1\rangle$ | $\pi$ | — | South pole |
| $\|D\rangle = \|+\rangle$ | $\pi/2$ | $0$ | +x axis |
| $\|A\rangle = \|-\rangle$ | $\pi/2$ | $\pi$ | −x axis |
| $\|R\rangle = \|{+i}\rangle$ | $\pi/2$ | $\pi/2$ | +y axis |
| $\|L\rangle = \|{-i}\rangle$ | $\pi/2$ | $3\pi/2$ | −y axis |

**Figure:** [Draw Bloch sphere with all six states labeled at their positions]

---

## Orthogonal States are Antipodal

A beautiful fact: two orthogonal states are on **opposite sides** of the Bloch sphere.

- $|H\rangle$ (north pole) and $|V\rangle$ (south pole): orthogonal, antipodal ✓
- $|D\rangle$ (+x) and $|A\rangle$ (−x): orthogonal, antipodal ✓
- $|R\rangle$ (+y) and $|L\rangle$ (−y): orthogonal, antipodal ✓

**Proof:** If $|\psi_1\rangle$ has coordinates $(\theta, \phi)$, the orthogonal state $|\psi_2\rangle$ has coordinates $(\pi - \theta, \phi + \pi)$.

This is the antipodal point on the sphere.

---

## The Three Axes

The Bloch sphere has three distinguished axes:

| Axis | Poles | Measurement basis |
|------|-------|-------------------|
| z-axis | $\|0\rangle$, $\|1\rangle$ | Computational (H/V) |
| x-axis | $\|+\rangle$, $\|-\rangle$ | Diagonal (D/A) |
| y-axis | $\|{+i}\rangle$, $\|{-i}\rangle$ | Circular (R/L) |

These three axes will correspond to the three Pauli matrices when we introduce them—$\sigma_z$, $\sigma_x$, and $\sigma_y$.

A state on the +z axis is definite for z-measurements but uncertain for x and y-measurements. A state on the equator is uncertain for z-measurements. And so on.

---

## Measurement on the Bloch Sphere

Measurement in a basis corresponds to projection onto an axis.

**Z-measurement** (H/V basis):
- Project onto the z-axis
- Outcome probabilities: $P(0) = \cos^2(\theta/2)$, $P(1) = \sin^2(\theta/2)$
- A state at the north pole gives $P(0) = 1$
- A state on the equator gives $P(0) = P(1) = 1/2$

**X-measurement** (D/A basis):
- Project onto the x-axis
- A state at +x gives $P(+) = 1$
- A state at the poles gives $P(+) = P(-) = 1/2$

The closer a state is to one end of an axis, the more likely that measurement outcome.

---

## Why Is This a Sphere?

We showed mathematically that 2 complex numbers, minus normalization, minus global phase, gives 2 real parameters—the coordinates on a sphere.

But there's a deeper reason: **the group SU(2) acts on qubits as rotations of the Bloch sphere.**

Every single-qubit gate (unitary transformation) corresponds to a rotation of the Bloch sphere about some axis. We'll see this explicitly when we study the Pauli matrices and spin.

The sphere isn't just a visualization convenience—it reflects the underlying symmetry of two-level quantum systems.

---

## Summary

1. **Light is quantized:** Photons are indivisible; they either pass a polarizer or don't

2. **The Born rule:** Probability of outcome = $|\langle\text{outcome}|\psi\rangle|^2$

3. **Measurement changes the state:** A photon that passes becomes polarized along the polarizer axis

4. **Three-polarizer paradox:** Adding an intermediate polarizer can increase transmission because measurement projects onto a new basis

5. **Global phase doesn't matter:** Physical states are rays; $|\psi\rangle \sim e^{i\gamma}|\psi\rangle$

6. **Bloch sphere derivation:** 2 complex numbers − 1 (normalization) − 1 (global phase) = 2 real parameters = sphere

7. **Bloch parametrization:** $|\psi\rangle = \cos(\theta/2)|0\rangle + e^{i\phi}\sin(\theta/2)|1\rangle$

8. **Orthogonal states are antipodal:** Opposite points on the sphere

9. **Three axes = three bases:** z (H/V), x (D/A), y (R/L)

---

## Homework

### Problem 1: Jones Vector Practice

Write the Jones vector for each state. Verify they are normalized.

**(a)** Linear polarization at 30° from horizontal

**(b)** Linear polarization at 60° from horizontal

**(c)** Elliptical polarization: $E_x$ has amplitude 2, $E_y$ has amplitude 1, with $E_y$ leading $E_x$ by phase $\pi/4$

---

### Problem 2: Waveplate Transformations

**(a)** A half-wave plate (HWP) with fast axis horizontal has Jones matrix:
$$
W_{HWP} = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}
$$
Apply this to $|D\rangle$, $|R\rangle$, and $|H\rangle$. What does a HWP do to each?

**(b)** A quarter-wave plate (QWP) with fast axis horizontal has Jones matrix:
$$
W_{QWP} = \begin{pmatrix} 1 & 0 \\ 0 & i \end{pmatrix}
$$
Apply this to $|D\rangle$ and $|A\rangle$. What happens?

**(c)** Show that applying a QWP twice is equivalent to a HWP.

---

### Problem 3: Polarizer Probabilities

A photon is prepared in state $|R\rangle = \frac{1}{\sqrt{2}}(|H\rangle + i|V\rangle)$.

**(a)** What is the probability it passes an H polarizer?

**(b)** What is the probability it passes a V polarizer?

**(c)** What is the probability it passes a D polarizer?

**(d)** What is the probability it passes an R polarizer?

**(e)** What is the probability it passes an L polarizer?

---

### Problem 4: Three-Polarizer Quantitative

**(a)** Verify that H → V (no intermediate) gives 0% transmission.

**(b)** Verify that H → D → V gives 25% transmission.

**(c)** What transmission do you get with H → (polarizer at 30°) → V?

**(d)** What transmission do you get with H → (polarizer at 60°) → V?

**(e)** What angle $\theta$ for the intermediate polarizer maximizes the H → θ → V transmission? What is the maximum?

---

### Problem 5: Many Intermediate Polarizers

Consider inserting $n$ polarizers evenly spaced in angle between H and V.

**(a)** For $n = 1$ (one polarizer at 45°), we get 25% transmission. Verify this.

**(b)** For $n = 2$ (polarizers at 30° and 60°), calculate the transmission.

**(c)** For general $n$, the angles are $\theta_k = k \cdot 90°/(n+1)$ for $k = 1, ..., n$. Show that the transmission is:
$$
T(n) = \cos^{2(n+1)}\left(\frac{\pi}{2(n+1)}\right)
$$

**(d)** Calculate $T(n)$ for $n = 1, 2, 3, 5, 10, 100$. What happens as $n \to \infty$?

**(e)** Interpret this result physically. Why does adding more polarizers *increase* transmission?

---

### Problem 6: Bloch Sphere Coordinates

**(a)** What are the Bloch coordinates $(\theta, \phi)$ for $|0\rangle$? For $|1\rangle$?

**(b)** What are the Bloch coordinates for $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$?

**(c)** What are the Bloch coordinates for $\frac{1}{\sqrt{2}}(|0\rangle + e^{i\pi/4}|1\rangle)$?

**(d)** What state corresponds to $\theta = \pi/2$, $\phi = \pi/3$?

**(e)** Show that the state $|\psi\rangle = \frac{1}{2}|0\rangle + \frac{\sqrt{3}}{2}|1\rangle$ is on the Bloch sphere (i.e., find its coordinates). Note: is this state normalized?

---

### Problem 7: Orthogonality and Antipodal Points

**(a)** Prove that if $|\psi_1\rangle = \cos(\theta/2)|0\rangle + e^{i\phi}\sin(\theta/2)|1\rangle$, then the orthogonal state is:
$$
|\psi_2\rangle = \sin(\theta/2)|0\rangle - e^{i\phi}\cos(\theta/2)|1\rangle
$$
by computing $\langle\psi_1|\psi_2\rangle$.

**(b)** Show that $|\psi_2\rangle$ corresponds to Bloch coordinates $(\pi - \theta, \phi + \pi)$.

**(c)** Verify this for the pair $|D\rangle$ and $|A\rangle$.

---

### Problem 8: Measurement Probabilities from the Bloch Sphere

A state has Bloch coordinates $(\theta, \phi)$.

**(a)** Show that the probability of measuring $|0\rangle$ (z-basis) is $P(0) = \cos^2(\theta/2)$.

**(b)** For a state at $\theta = \pi/3$, $\phi = 0$, what are $P(0)$ and $P(1)$?

**(c)** For the same state, what is $P(+)$ (probability of measuring $|+\rangle$ in the x-basis)?

*Hint:* First find $\langle +|\psi\rangle$ where $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$.

---

### Problem 9: Bloch Sphere Visualization (Optional Coding)

Use Python to visualize the Bloch sphere.

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

def bloch_vector(theta, phi):
    """Convert Bloch angles to Cartesian coordinates on unit sphere."""
    x = np.sin(theta) * np.cos(phi)
    y = np.sin(theta) * np.sin(phi)
    z = np.cos(theta)
    return x, y, z

# Create sphere wireframe
fig = plt.figure(figsize=(8, 8))
ax = fig.add_subplot(111, projection='3d')

# Draw sphere
u = np.linspace(0, 2*np.pi, 30)
v = np.linspace(0, np.pi, 20)
x = np.outer(np.cos(u), np.sin(v))
y = np.outer(np.sin(u), np.sin(v))
z = np.outer(np.ones(np.size(u)), np.cos(v))
ax.plot_wireframe(x, y, z, alpha=0.1)

# Plot the six cardinal states
states = {
    '|0⟩': (0, 0),
    '|1⟩': (np.pi, 0),
    '|+⟩': (np.pi/2, 0),
    '|-⟩': (np.pi/2, np.pi),
    '|+i⟩': (np.pi/2, np.pi/2),
    '|-i⟩': (np.pi/2, 3*np.pi/2),
}

for label, (theta, phi) in states.items():
    x, y, z = bloch_vector(theta, phi)
    ax.scatter([x], [y], [z], s=100)
    ax.text(x*1.1, y*1.1, z*1.1, label, fontsize=12)

ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_zlabel('Z')
plt.title('Bloch Sphere')
plt.show()
```

**(a)** Run the code and verify the six states are in the correct positions.

**(b)** Add a state of your choice to the plot. Verify its position makes sense.

**(c)** Modify the code to show a "trajectory" of states $|\psi(\phi)\rangle = \frac{1}{\sqrt{2}}(|0\rangle + e^{i\phi}|1\rangle)$ as $\phi$ goes from $0$ to $2\pi$. What curve does this trace?

---

## Looking Ahead

We now have a complete picture of single-qubit states:
- States are points on the Bloch sphere
- Measurements project onto axes
- Orthogonal states are antipodal

Next we'll ask: how do states *evolve in time*? The answer involves the Schrödinger equation, which will lead us to the Pauli matrices as generators of rotations on the Bloch sphere. The three measurement axes (x, y, z) will become the three spin operators ($\sigma_x, \sigma_y, \sigma_z$).
















---



---




---




---

# Lecture 8: Time Evolution and the Pauli Matrices

## From Static States to Dynamics

So far we've studied:
- States: points on the Bloch sphere
- Measurements: projections onto axes
- Transformations: unitaries (like waveplates)

But we haven't asked: **how do quantum states evolve in time?**

Today we answer this question. The Schrödinger equation tells us that time evolution is generated by the Hamiltonian. For a qubit, this leads us inevitably to the **Pauli matrices**—the fundamental building blocks of single-qubit physics.

---

## The Schrödinger Equation

### The Fundamental Law

Quantum states evolve according to the Schrödinger equation:

$$
i\hbar \frac{d}{dt}|\psi(t)\rangle = H|\psi(t)\rangle
$$

Here $H$ is the **Hamiltonian**—the operator representing the total energy of the system.

This equation is:
- **First-order in time** (unlike the classical wave equation, which is second-order)
- **Linear** (superpositions evolve independently)
- **Deterministic** (given the initial state, the evolution is fixed)

The $i$ is essential—it's what makes quantum mechanics fundamentally complex, as we discussed in Lecture 5.

### The Solution: Unitary Evolution

For a time-independent Hamiltonian, the solution is:

$$
|\psi(t)\rangle = e^{-iHt/\hbar}|\psi(0)\rangle = U(t)|\psi(0)\rangle
$$

The operator $U(t) = e^{-iHt/\hbar}$ is called the **time evolution operator**.

### Why Unitary?

$U(t)$ is unitary: $U^\dagger U = I$. Let's verify:

$$
U^\dagger = \left(e^{-iHt/\hbar}\right)^\dagger = e^{+iH^\dagger t/\hbar} = e^{+iHt/\hbar}
$$

(using $H^\dagger = H$ since the Hamiltonian is Hermitian)

$$
U^\dagger U = e^{+iHt/\hbar} e^{-iHt/\hbar} = e^{0} = I \quad \checkmark
$$

Unitarity guarantees **probability conservation**:

$$
\langle\psi(t)|\psi(t)\rangle = \langle\psi(0)|U^\dagger U|\psi(0)\rangle = \langle\psi(0)|\psi(0)\rangle = 1
$$

The total probability stays 1 for all time.

```{admonition} The Deep Connection
:class: note

**Hermitian operators** (observables) ↔ **Unitary operators** (evolution)

If $H$ is Hermitian, then $e^{-iHt}$ is unitary. Conversely, any unitary can be written as $e^{iG}$ for some Hermitian $G$.

Observables generate evolution. Energy generates time evolution. Momentum generates spatial translation. Angular momentum generates rotations.
```

---

## Energy Eigenstates: States That Don't Change

What states have simple time evolution?

### Definition

An **eigenstate** of $H$ with eigenvalue $E$ satisfies:

$$
H|E\rangle = E|E\rangle
$$

### Time Evolution of Eigenstates

If $|\psi(0)\rangle = |E\rangle$ is an energy eigenstate:

$$
|\psi(t)\rangle = e^{-iHt/\hbar}|E\rangle = e^{-iEt/\hbar}|E\rangle
$$

The state just acquires a phase $e^{-iEt/\hbar}$.

But global phase doesn't matter! So energy eigenstates are **stationary**—they don't change physically with time.

This is why energy eigenstates are also called **stationary states**.

### Superpositions Do Evolve

What if the initial state is a superposition of energy eigenstates?

$$
|\psi(0)\rangle = c_1|E_1\rangle + c_2|E_2\rangle
$$

Then:

$$
|\psi(t)\rangle = c_1 e^{-iE_1 t/\hbar}|E_1\rangle + c_2 e^{-iE_2 t/\hbar}|E_2\rangle
$$

The **relative phase** between components changes:

$$
|\psi(t)\rangle = e^{-iE_1 t/\hbar}\left(c_1|E_1\rangle + c_2 e^{-i(E_2-E_1)t/\hbar}|E_2\rangle\right)
$$

The relative phase oscillates at frequency:

$$
\omega = \frac{E_2 - E_1}{\hbar}
$$

This is real, physical evolution—the state moves on the Bloch sphere.

---

## The Simplest Qubit Hamiltonian

For a two-level system, what's the most general Hamiltonian?

### General 2×2 Hermitian Matrix

Any $2 \times 2$ Hermitian matrix can be written as:

$$
H = \begin{pmatrix} a & c \\ c^* & b \end{pmatrix}
$$

where $a, b \in \mathbb{R}$ and $c \in \mathbb{C}$.

This has 4 real parameters. We can decompose it as:

$$
H = \frac{a+b}{2}I + \frac{a-b}{2}\sigma_z + \text{Re}(c)\sigma_x + \text{Im}(c)\sigma_y
$$

where we've introduced the **Pauli matrices**:

$$
\sigma_x = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, \quad
\sigma_y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}, \quad
\sigma_z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}
$$

### The Pauli Matrices Form a Basis

Any $2 \times 2$ Hermitian matrix can be written as:

$$
\boxed{H = h_0 I + h_x \sigma_x + h_y \sigma_y + h_z \sigma_z = h_0 I + \vec{h} \cdot \vec{\sigma}}
$$

where $h_0, h_x, h_y, h_z \in \mathbb{R}$ and $\vec{\sigma} = (\sigma_x, \sigma_y, \sigma_z)$.

The identity part $h_0 I$ just contributes a global phase to evolution (unobservable). The physics is in $\vec{h} \cdot \vec{\sigma}$.

---

## The Pauli Matrices: Properties

The Pauli matrices are fundamental. Let's understand them.

### They Are Hermitian

$$
\sigma_x^\dagger = \sigma_x, \quad \sigma_y^\dagger = \sigma_y, \quad \sigma_z^\dagger = \sigma_z
$$

So they can represent observables.

### They Are Traceless

$$
\text{Tr}(\sigma_x) = \text{Tr}(\sigma_y) = \text{Tr}(\sigma_z) = 0
$$

### They Square to Identity

$$
\sigma_x^2 = \sigma_y^2 = \sigma_z^2 = I
$$

This means their eigenvalues are $\pm 1$.

### Their Eigenstates

**$\sigma_z$ eigenstates:** (the computational basis)

$$
\sigma_z |0\rangle = +|0\rangle, \quad \sigma_z |1\rangle = -|1\rangle
$$

**$\sigma_x$ eigenstates:**

$$
\sigma_x |+\rangle = +|+\rangle, \quad \sigma_x |-\rangle = -|-\rangle
$$

where $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ and $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$.

**$\sigma_y$ eigenstates:**

$$
\sigma_y |{+i}\rangle = +|{+i}\rangle, \quad \sigma_y |{-i}\rangle = -|{-i}\rangle
$$

where $|{+i}\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ and $|{-i}\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$.

### The Eigenstates Are Our Old Friends!

| Pauli matrix | +1 eigenstate | −1 eigenstate | Bloch axis |
|--------------|---------------|---------------|------------|
| $\sigma_z$ | $\|0\rangle = \|H\rangle$ | $\|1\rangle = \|V\rangle$ | z-axis |
| $\sigma_x$ | $\|+\rangle = \|D\rangle$ | $\|-\rangle = \|A\rangle$ | x-axis |
| $\sigma_y$ | $\|{+i}\rangle = \|R\rangle$ | $\|{-i}\rangle = \|L\rangle$ | y-axis |

The three Pauli matrices correspond to the three axes of the Bloch sphere!

---

## Commutation Relations: Why Non-Commuting Matters

### The Pauli Matrices Don't Commute

$$
\sigma_x \sigma_y = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}\begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix} = \begin{pmatrix} i & 0 \\ 0 & -i \end{pmatrix} = i\sigma_z
$$

$$
\sigma_y \sigma_x = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} = \begin{pmatrix} -i & 0 \\ 0 & i \end{pmatrix} = -i\sigma_z
$$

So:
$$
\sigma_x \sigma_y - \sigma_y \sigma_x = 2i\sigma_z
$$

### The Commutation Relations

The Pauli matrices satisfy:

$$
\boxed{[\sigma_i, \sigma_j] = 2i\epsilon_{ijk}\sigma_k}
$$

where $\epsilon_{ijk}$ is the Levi-Civita symbol (+1 for cyclic permutations, −1 for anti-cyclic, 0 if any indices repeat).

Explicitly:
$$
[\sigma_x, \sigma_y] = 2i\sigma_z, \quad [\sigma_y, \sigma_z] = 2i\sigma_x, \quad [\sigma_z, \sigma_x] = 2i\sigma_y
$$

### Why This Matters: The Uncertainty Principle

If two operators don't commute, they can't be simultaneously diagonalized—they can't have simultaneous eigenstates.

Since $[\sigma_x, \sigma_z] \neq 0$:
- A state can't be an eigenstate of both $\sigma_x$ and $\sigma_z$
- If you know the z-component of spin precisely, the x-component is uncertain
- This is the **uncertainty principle** for spin

This is why $|0\rangle$ (definite $\sigma_z = +1$) gives 50/50 outcomes for $\sigma_x$ measurements.

---

## Connection to Rotations: SO(3) and SU(2)

Here's the deep structure. The commutation relations:

$$
[\sigma_i, \sigma_j] = 2i\epsilon_{ijk}\sigma_k
$$

are (up to a factor of 2) the same as the commutation relations of angular momentum in 3D!

### The Rotation Group SO(3)

Rotations in 3D space satisfy:

$$
[J_i, J_j] = i\hbar\epsilon_{ijk}J_k
$$

where $J_x, J_y, J_z$ are the generators of rotations about the x, y, z axes.

### Spin as Angular Momentum

For a spin-1/2 particle, the spin angular momentum operators are:

$$
S_i = \frac{\hbar}{2}\sigma_i
$$

Then:
$$
[S_i, S_j] = \frac{\hbar^2}{4}[\sigma_i, \sigma_j] = \frac{\hbar^2}{4}(2i\epsilon_{ijk}\sigma_k) = i\hbar \cdot \frac{\hbar}{2}\epsilon_{ijk}\sigma_k = i\hbar\epsilon_{ijk}S_k
$$

The spin operators satisfy the same algebra as angular momentum!

### Why The Pauli Matrices Look The Way They Do

The Pauli matrices aren't arbitrary—they're **forced** by the requirement that they:
1. Are $2 \times 2$ Hermitian matrices
2. Have eigenvalues $\pm 1$ (or $\pm\hbar/2$ for spin)
3. Satisfy the angular momentum commutation relations

Any other choice would just be a rotated basis.

```{admonition} The Punchline
:class: important

The Pauli matrices are the **generators of SU(2)**—the group of rotations on the Bloch sphere.

Just as $G$ generated SO(2) rotations in Lecture 5 (Problem 6), the Pauli matrices generate SU(2) rotations in the qubit state space.
```

---

## Time Evolution on the Bloch Sphere

Now we can understand what time evolution does geometrically.

### Hamiltonian $H = \frac{\hbar\omega}{2}\sigma_z$

This describes a qubit with energy splitting $\hbar\omega$ (e.g., spin in a magnetic field along z).

The time evolution operator is:

$$
U(t) = e^{-iHt/\hbar} = e^{-i\omega t \sigma_z/2}
$$

Using $\sigma_z^2 = I$ and the Taylor series:

$$
e^{-i\theta\sigma_z/2} = \cos(\theta/2)I - i\sin(\theta/2)\sigma_z = \begin{pmatrix} e^{-i\theta/2} & 0 \\ 0 & e^{i\theta/2} \end{pmatrix}
$$

With $\theta = \omega t$:

$$
U(t) = \begin{pmatrix} e^{-i\omega t/2} & 0 \\ 0 & e^{i\omega t/2} \end{pmatrix}
$$

### What This Does to States

**On $|0\rangle$:**
$$
U(t)|0\rangle = e^{-i\omega t/2}|0\rangle
$$
Just a global phase—$|0\rangle$ is stationary. (It's an eigenstate of $H$.)

**On $|1\rangle$:**
$$
U(t)|1\rangle = e^{+i\omega t/2}|1\rangle
$$
Just a global phase—$|1\rangle$ is also stationary.

**On $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$:**
$$
U(t)|+\rangle = \frac{1}{\sqrt{2}}(e^{-i\omega t/2}|0\rangle + e^{i\omega t/2}|1\rangle) = \frac{e^{-i\omega t/2}}{\sqrt{2}}(|0\rangle + e^{i\omega t}|1\rangle)
$$

Ignoring global phase:
$$
|+\rangle \to \frac{1}{\sqrt{2}}(|0\rangle + e^{i\omega t}|1\rangle)
$$

This is a state on the equator of the Bloch sphere, with $\phi = \omega t$.

**The state rotates around the z-axis at angular frequency $\omega$!**

### Precession

This is called **precession**: a spin in a magnetic field rotates around the field axis.

At $t = 0$: state is at +x ($|+\rangle$)
At $t = \pi/(2\omega)$: state is at +y ($|{+i}\rangle$)
At $t = \pi/\omega$: state is at −x ($|-\rangle$)
At $t = 3\pi/(2\omega)$: state is at −y ($|{-i}\rangle$)
At $t = 2\pi/\omega$: state returns to +x ($|+\rangle$)

**Figure:** [Draw Bloch sphere with circular trajectory around z-axis on the equator]

---

## General Rotation Formula

For a Hamiltonian $H = \frac{\hbar\omega}{2}\hat{n}\cdot\vec{\sigma}$, where $\hat{n}$ is a unit vector:

$$
U(t) = e^{-i\omega t \hat{n}\cdot\vec{\sigma}/2} = \cos(\omega t/2)I - i\sin(\omega t/2)\hat{n}\cdot\vec{\sigma}
$$

This is a **rotation by angle $\omega t$ around axis $\hat{n}$** on the Bloch sphere.

### Standard Rotations

$$
R_x(\theta) = e^{-i\theta\sigma_x/2} = \begin{pmatrix} \cos(\theta/2) & -i\sin(\theta/2) \\ -i\sin(\theta/2) & \cos(\theta/2) \end{pmatrix}
$$

$$
R_y(\theta) = e^{-i\theta\sigma_y/2} = \begin{pmatrix} \cos(\theta/2) & -\sin(\theta/2) \\ \sin(\theta/2) & \cos(\theta/2) \end{pmatrix}
$$

$$
R_z(\theta) = e^{-i\theta\sigma_z/2} = \begin{pmatrix} e^{-i\theta/2} & 0 \\ 0 & e^{i\theta/2} \end{pmatrix}
$$

These are the fundamental single-qubit gates.

---

## Rabi Oscillations

### Hamiltonian $H = \frac{\hbar\Omega}{2}\sigma_x$

This describes a qubit being driven by a resonant field (e.g., microwave pulses on a superconducting qubit, or laser on an atom).

The evolution is rotation around the x-axis:

$$
U(t) = e^{-i\Omega t\sigma_x/2} = R_x(\Omega t)
$$

### Evolution of $|0\rangle$

Starting in $|0\rangle$ (north pole):

$$
|\psi(t)\rangle = R_x(\Omega t)|0\rangle = \cos(\Omega t/2)|0\rangle - i\sin(\Omega t/2)|1\rangle
$$

The probabilities are:

$$
P_0(t) = \cos^2(\Omega t/2), \quad P_1(t) = \sin^2(\Omega t/2)
$$

These oscillate! The population sloshes back and forth between $|0\rangle$ and $|1\rangle$.

**Figure:** [Plot $P_0(t)$ and $P_1(t)$ vs time, showing sinusoidal oscillation]

### Special Times

**$\pi$-pulse:** $\Omega t = \pi$
$$
R_x(\pi)|0\rangle = -i|1\rangle \sim |1\rangle
$$
Complete population inversion: $|0\rangle \to |1\rangle$.

**$\pi/2$-pulse:** $\Omega t = \pi/2$
$$
R_x(\pi/2)|0\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)
$$
Creates equal superposition (up to phase).

These are the building blocks of quantum control.

---

## Quantum Gates as Rotations

Every single-qubit unitary is a rotation on the Bloch sphere (up to global phase).

| Gate | Matrix | Rotation |
|------|--------|----------|
| $X$ | $\sigma_x$ | $\pi$ around x-axis |
| $Y$ | $\sigma_y$ | $\pi$ around y-axis |
| $Z$ | $\sigma_z$ | $\pi$ around z-axis |
| $H$ | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$ | $\pi$ around (x+z)/√2 axis |
| $S$ | $\begin{pmatrix} 1 & 0 \\ 0 & i \end{pmatrix}$ | $\pi/2$ around z-axis |
| $T$ | $\begin{pmatrix} 1 & 0 \\ 0 & e^{i\pi/4} \end{pmatrix}$ | $\pi/4$ around z-axis |

Any single-qubit gate can be decomposed as:

$$
U = e^{i\alpha} R_z(\beta) R_y(\gamma) R_z(\delta)
$$

for some angles $\alpha, \beta, \gamma, \delta$.

---

## Physical Example: Spin in a Magnetic Field

A spin-1/2 particle (like an electron) in a magnetic field $\vec{B}$ has Hamiltonian:

$$
H = -\gamma \vec{S} \cdot \vec{B} = -\frac{\gamma\hbar}{2}\vec{\sigma}\cdot\vec{B}
$$

where $\gamma$ is the gyromagnetic ratio.

### For $\vec{B} = B_0\hat{z}$:

$$
H = -\frac{\gamma\hbar B_0}{2}\sigma_z = \frac{\hbar\omega_L}{2}\sigma_z
$$

where $\omega_L = -\gamma B_0$ is the **Larmor frequency**.

The spin precesses around the z-axis at frequency $\omega_L$. This is the basis of:
- NMR (nuclear magnetic resonance)
- MRI (magnetic resonance imaging)
- ESR (electron spin resonance)
- Atomic clocks

### For $\vec{B} = B_1\hat{x}\cos(\omega t)$ (oscillating field):

This drives Rabi oscillations when $\omega \approx \omega_L$ (resonance).

The oscillating field tips the spin away from the z-axis. By controlling the pulse duration, we can create any desired rotation.

---

## Summary

1. **Schrödinger equation:** $i\hbar\frac{d}{dt}|\psi\rangle = H|\psi\rangle$

2. **Time evolution is unitary:** $|\psi(t)\rangle = e^{-iHt/\hbar}|\psi(0)\rangle$

3. **Energy eigenstates are stationary:** They only acquire a global phase

4. **Superpositions evolve:** Relative phases oscillate at frequency $(E_2 - E_1)/\hbar$

5. **Any qubit Hamiltonian:** $H = h_0 I + \vec{h}\cdot\vec{\sigma}$

6. **Pauli matrices:** 
   - Hermitian, traceless, square to $I$
   - Eigenstates are the Bloch sphere axes
   - Satisfy $[\sigma_i, \sigma_j] = 2i\epsilon_{ijk}\sigma_k$

7. **Non-commuting observables:** Can't have simultaneous eigenstates → uncertainty principle

8. **Pauli matrices generate rotations:** $e^{-i\theta\hat{n}\cdot\vec{\sigma}/2}$ = rotation by $\theta$ around $\hat{n}$

9. **Precession:** $H \propto \sigma_z$ causes rotation around z-axis

10. **Rabi oscillations:** $H \propto \sigma_x$ causes rotation around x-axis, population transfer

---

## Homework

### Problem 1: Pauli Matrix Algebra

**(a)** Verify that $\sigma_x^2 = \sigma_y^2 = \sigma_z^2 = I$.

**(b)** Compute $\sigma_x\sigma_y$, $\sigma_y\sigma_z$, and $\sigma_z\sigma_x$. Verify the pattern.

**(c)** Compute the anticommutator $\{\sigma_x, \sigma_y\} = \sigma_x\sigma_y + \sigma_y\sigma_x$. What do you get?

**(d)** Show that $\sigma_i\sigma_j = \delta_{ij}I + i\epsilon_{ijk}\sigma_k$ (sum over $k$).

---

### Problem 2: Eigenstates

**(a)** Verify that $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ is an eigenstate of $\sigma_x$ with eigenvalue +1.

**(b)** Find the eigenstates of $\sigma_y$. Verify they have eigenvalues $\pm 1$.

**(c)** What are the eigenstates of $\sigma_x + \sigma_z$? What are the eigenvalues?

---

### Problem 3: Time Evolution

A qubit has Hamiltonian $H = \frac{\hbar\omega}{2}\sigma_z$.

**(a)** Starting in $|+\rangle$, find $|\psi(t)\rangle$.

**(b)** Compute $P_0(t) = |\langle 0|\psi(t)\rangle|^2$ and $P_1(t)$. Do they change with time?

**(c)** Compute $P_+(t) = |\langle +|\psi(t)\rangle|^2$. How does this evolve?

**(d)** Compute $\langle\sigma_x\rangle(t)$, $\langle\sigma_y\rangle(t)$, and $\langle\sigma_z\rangle(t)$. Describe the motion.

---

### Problem 4: Rabi Oscillations

A qubit has Hamiltonian $H = \frac{\hbar\Omega}{2}\sigma_x$, starting in $|0\rangle$.

**(a)** Show that $|\psi(t)\rangle = \cos(\Omega t/2)|0\rangle - i\sin(\Omega t/2)|1\rangle$.

**(b)** What is $P_1(t)$? Sketch it.

**(c)** At what time does the qubit first reach $|1\rangle$ (with certainty)? This is a **$\pi$-pulse**.

**(d)** At what time does the qubit first reach an equal superposition? This is a **$\pi/2$-pulse**.

**(e)** If $\Omega = 2\pi \times 10$ MHz, how long is a $\pi$-pulse in nanoseconds?

---

### Problem 5: Rotation Matrices

**(a)** Derive $R_x(\theta) = e^{-i\theta\sigma_x/2}$ by expanding the exponential and using $\sigma_x^2 = I$.

**(b)** Verify that $R_x(\pi) = -i\sigma_x$ (the X gate, up to global phase).

**(c)** Compute $R_y(\pi/2)|0\rangle$. Where is this state on the Bloch sphere?

**(d)** Show that $R_z(\phi)|+\rangle$ is a state on the equator at azimuthal angle $\phi$.

---

### Problem 6: Commutators and Uncertainty

**(a)** Compute $[\sigma_x, \sigma_z]$.

**(b)** The generalized uncertainty principle states:
$$
\Delta A \cdot \Delta B \geq \frac{1}{2}|\langle[A, B]\rangle|
$$

For the state $|0\rangle$, compute $\langle[\sigma_x, \sigma_z]\rangle$. What does this tell you about $\Delta\sigma_x \cdot \Delta\sigma_z$?

**(c)** For the state $|+\rangle$, compute $\langle[\sigma_x, \sigma_z]\rangle$. Is the uncertainty bound different?

---

### Problem 7: Decomposing Hamiltonians

Any $2\times 2$ Hermitian matrix can be written as $H = h_0 I + h_x\sigma_x + h_y\sigma_y + h_z\sigma_z$.

**(a)** Find $(h_0, h_x, h_y, h_z)$ for:
$$
H = \begin{pmatrix} 3 & 1-i \\ 1+i & 1 \end{pmatrix}
$$

**(b)** What are the eigenvalues of this $H$?

**(c)** What axis does time evolution rotate around for this Hamiltonian?

---

### Problem 8: The Hadamard Gate

The Hadamard gate is $H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$.

**(a)** Show that $H^2 = I$.

**(b)** Find the eigenvalues and eigenstates of $H$.

**(c)** The Hadamard can be written as $H = e^{i\pi/2} e^{-i\pi\hat{n}\cdot\vec{\sigma}/2}$ for some axis $\hat{n}$. Find $\hat{n}$. (Hint: $H = \frac{1}{\sqrt{2}}(\sigma_x + \sigma_z)$.)

**(d)** What rotation on the Bloch sphere does $H$ correspond to?

---

### Problem 9: Physical Units

An electron has gyromagnetic ratio $\gamma_e = 2\pi \times 28$ GHz/T.

**(a)** In a magnetic field $B = 1$ T along z, what is the Larmor frequency $\omega_L$ in GHz?

**(b)** What is the precession period?

**(c)** If you want a $\pi$-pulse using a resonant oscillating field, and your Rabi frequency is $\Omega = 2\pi \times 100$ MHz, how long should the pulse be?

**(d)** How many precession cycles occur during one $\pi$-pulse?

---

### Problem 10: Bloch Sphere Trajectories (Optional Coding)

Modify the Bloch sphere code from Lecture 7 to animate time evolution.

**(a)** For $H = \frac{\hbar\omega}{2}\sigma_z$, starting in $|+\rangle$, plot the trajectory. Verify it's a circle around the z-axis.

**(b)** For $H = \frac{\hbar\Omega}{2}\sigma_x$, starting in $|0\rangle$, plot the trajectory. Verify it goes from north pole to south pole and back.

**(c)** For $H = \frac{\hbar\omega}{2}(\sigma_x + \sigma_z)/\sqrt{2}$, starting in $|0\rangle$, plot the trajectory. What shape is it?

---

## Looking Ahead

We now have the complete single-qubit picture:
- **States:** Points on the Bloch sphere
- **Observables:** Pauli matrices (axes of the sphere)
- **Evolution:** Rotations generated by Hamiltonians
- **Measurement:** Projection onto an axis

Next chapter: **two qubits and entanglement**. When we combine two qubits, something new emerges—correlations that can't be explained by any classical model. This is where quantum computing gets its power.




---
---
---


