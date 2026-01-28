

# Lecture 2.2: The Mach-Zehnder Interferometer and the Bloch Sphere

## Review of Last Lecture

In Lecture 2.1, we established the key ideas about waves and interference:

- **Traveling waves** in complex form: $E(x,t) = E_0 e^{i(kx - \omega t)}$
- **Intensity** is the magnitude squared: $I \propto |E|^2$
- **Interference** comes from adding amplitudes before squaring:
$$|E_1 + E_2|^2 = |E_1|^2 + |E_2|^2 + 2\text{Re}(E_1^* E_2)$$
- For two sources with phases $\phi_1$ and $\phi_2$:
$$I = |e^{i\phi_1} + e^{i\phi_2}|^2 = 2(1 + \cos(\phi_1 - \phi_2))$$
- **Only the relative phase matters** — the intensity depends on $\phi_1 - \phi_2$, not on the individual phases.

Today we'll put these ideas to work in a concrete device: the Mach-Zehnder interferometer. Then we'll discover that this interferometer is mathematically identical to a quantum computing circuit, and develop the Bloch sphere as a way to visualize qubit states.

---

## iClicker: Review Question

**Two waves interfere with intensity $I = |e^{i \phi_1}  + e^{i \phi_2} |^2$. If $\Delta\phi = \pi$, the interference is:**

- (A) Constructive ($I = 4$)
- (B) Destructive ($I = 0$) ✓
- (C) Partially constructive ($I = 2$)
- (D) Depends on the wavelength


---

## The Mach-Zehnder Interferometer

The Mach-Zehnder interferometer (MZI) is one of the cleanest demonstrations of interference. Unlike the two-source setup from last lecture, here a *single* beam of light is split into two paths that later recombine.

### The Setup

```{figure} mzi_diagram.svg
:name: mzi-setup
:width: 80%

The Mach-Zehnder interferometer. Light enters from the left, is split by BS1 into two arms with lengths $L_1$ and $L_2$, and recombines at BS2. Detectors D1 and D2 measure the output intensities.
```

**Components:**
- **BS1**: First 50/50 beam splitter — splits incoming light equally into two arms
- **Arm 1 and Arm 2**: The two paths, with lengths $L_1$ and $L_2$
- **M1, M2**: Mirrors that redirect the beams toward the second beam splitter
- **BS2**: Second 50/50 beam splitter — recombines the two beams
- **D1, D2**: Two detectors that measure the output intensity

### Calculation Using Wave Amplitudes

Let's analyze this using the complex amplitude formalism from last lecture. We'll track the wave $e^{ikx}$ as it propagates through the interferometer.

**Step 1: Input and first beam splitter**

An incident wave arrives at BS1:
$$E_{in} = E_0 e^{ikx}$$

At the beam splitter (let's call this position $x = 0$), the wave is split equally into two arms. The 50/50 beam splitter sends equal amplitude into each path:

$$E_{arm1} = \frac{E_0}{\sqrt{2}}, \qquad E_{arm2} = \frac{E_0}{\sqrt{2}}$$

The factor of $1/\sqrt{2}$ ensures that the total intensity is conserved: $|E_{arm1}|^2 + |E_{arm2}|^2 = |E_0|^2$.

**Step 2: Phase accumulation**

As light travels through each arm, it accumulates phase according to $e^{ikx}$. After traveling a distance $L_j$ in arm $j$:

$$E_{arm1} = \frac{E_0}{\sqrt{2}} e^{ikL_1} = \frac{E_0}{\sqrt{2}} e^{i\phi_1}$$

$$E_{arm2} = \frac{E_0}{\sqrt{2}} e^{ikL_2} = \frac{E_0}{\sqrt{2}} e^{i\phi_2}$$

where we define the phases:
$$\phi_1 = kL_1 = \frac{2\pi}{\lambda} L_1, \qquad \phi_2 = kL_2 = \frac{2\pi}{\lambda} L_2$$

This is exactly the $e^{ikx}$ propagation we studied — each arm just accumulates phase proportional to its length!

**Step 3: Second beam splitter and detection**

The second beam splitter recombines the beams. A 50/50 beam splitter does the following:
- **Detector D1** receives the sum of the two amplitudes (both transmitted or both reflected)
- **Detector D2** receives the difference of the two amplitudes (one transmitted, one reflected)

The sign difference comes from the physics of beam splitters: reflection from one side picks up a phase shift of $\pi$ relative to reflection from the other side. This ensures energy conservation.

$$E_{D1} = \frac{1}{\sqrt{2}}\left(\frac{E_0}{\sqrt{2}} e^{i\phi_1} + \frac{E_0}{\sqrt{2}} e^{i\phi_2}\right) = \frac{E_0}{2}(e^{i\phi_1} + e^{i\phi_2})$$

$$E_{D2} = \frac{1}{\sqrt{2}}\left(\frac{E_0}{\sqrt{2}} e^{i\phi_1} - \frac{E_0}{\sqrt{2}} e^{i\phi_2}\right) = \frac{E_0}{2}(e^{i\phi_1} - e^{i\phi_2})$$

**Step 4: Calculate intensities**

For detector D1:
$$I_{D1} = |E_{D1}|^2 = \frac{|E_0|^2}{4}|e^{i\phi_1} + e^{i\phi_2}|^2$$

Using our result from last lecture:
$$|e^{i\phi_1} + e^{i\phi_2}|^2 = 2(1 + \cos(\phi_1 - \phi_2))$$

So (normalizing by setting $|E_0|^2 = 2$):
$$I_{D1} = \frac{1}{2}(1 + \cos\Delta\phi)$$

where $\Delta\phi = \phi_1 - \phi_2 = k(L_1 - L_2)$ is the phase difference.

Similarly for D2:
$$I_{D2} = |E_{D2}|^2 = \frac{|E_0|^2}{4}|e^{i\phi_1} - e^{i\phi_2}|^2 = \frac{1}{2}(1 - \cos\Delta\phi)$$

### Key Results

$$\boxed{I_{D1} = \frac{1}{2}(1 + \cos\Delta\phi), \qquad I_{D2} = \frac{1}{2}(1 - \cos\Delta\phi)}$$

**Important observations:**

1. **Conservation of energy:** $I_{D1} + I_{D2} = 1$. All the light goes somewhere!

2. **Complementary outputs:** When D1 is bright, D2 is dark, and vice versa.

3. **Only the phase difference matters:** The intensities depend on $\Delta\phi = \phi_1 - \phi_2$, not on $\phi_1$ and $\phi_2$ individually.

| Phase difference $\Delta\phi$ | $I_{D1}$ | $I_{D2}$ | Result |
|------------------------------|----------|----------|--------|
| $0$ | $1$ | $0$ | All light to D1 |
| $\pi/2$ | $1/2$ | $1/2$ | Split equally |
| $\pi$ | $0$ | $1$ | All light to D2 |
| $3\pi/2$ | $1/2$ | $1/2$ | Split equally |
| $2\pi$ | $1$ | $0$ | All light to D1 |

### Global Phase Doesn't Matter

Let's see explicitly why only the *relative* phase matters.

Suppose we add a common phase $\gamma$ to both arms (for example, by inserting glass in both paths):

$$E_{arm1} \to e^{i\gamma} E_{arm1}, \qquad E_{arm2} \to e^{i\gamma} E_{arm2}$$

Then:
$$E_{D1} = \frac{1}{2}(e^{i\gamma}e^{i\phi_1} + e^{i\gamma}e^{i\phi_2}) = \frac{e^{i\gamma}}{2}(e^{i\phi_1} + e^{i\phi_2})$$

The intensity:
$$I_{D1} = |E_{D1}|^2 = \left|\frac{e^{i\gamma}}{2}\right|^2 |e^{i\phi_1} + e^{i\phi_2}|^2 = \frac{1}{4}|e^{i\phi_1} + e^{i\phi_2}|^2$$

The global phase $e^{i\gamma}$ has magnitude 1, so it drops out! 

```{admonition} Key Principle
:class: important

**Global phase is unobservable.** Multiplying all amplitudes by the same phase factor $e^{i\gamma}$ does not change any measurement outcome. Only *relative* phases between different paths are physically meaningful.
```

This principle will be crucial when we construct the Bloch sphere.

---

## The MZI as a Quantum Circuit

Now let's rewrite the same calculation using matrix notation. This will reveal that the MZI is mathematically identical to a quantum computing circuit.

### State Vector Notation

Instead of tracking separate amplitudes $E_{arm1}$ and $E_{arm2}$, we combine them into a **state vector**:

$$|\psi\rangle = \begin{pmatrix} c_1 \\ c_2 \end{pmatrix}$$

where $c_1$ is the amplitude in arm 1 (or path $|0\rangle$) and $c_2$ is the amplitude in arm 2 (or path $|1\rangle$).

The input light enters arm 1 only:
$$|\psi_{in}\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix} = |0\rangle$$

### The Hadamard Matrix (Beam Splitter)

A 50/50 beam splitter is represented by the **Hadamard matrix**:

$$H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$$

Let's verify this does what we expect. Applying $H$ to the input:

$$H|0\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}\begin{pmatrix} 1 \\ 0 \end{pmatrix} = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix}$$

This is exactly what we had: equal amplitude $1/\sqrt{2}$ in each arm.

### The Phase Gate

The phase accumulated in each arm is represented by a diagonal matrix:

$$P(\phi_1, \phi_2) = \begin{pmatrix} e^{i\phi_1} & 0 \\ 0 & e^{i\phi_2} \end{pmatrix}$$

Often we factor out the global phase and write this as:

$$P(\phi) = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\phi} \end{pmatrix}$$

where $\phi = \phi_2 - \phi_1$ is the relative phase. (The global phase doesn't matter anyway!)

### The Full Calculation

The MZI corresponds to the sequence:

$$|\psi_{out}\rangle = H \cdot P(\phi) \cdot H \cdot |\psi_{in}\rangle$$

Let's compute this step by step.

**Step 1:** Apply first Hadamard
$$H|0\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix}$$

**Step 2:** Apply phase gate
$$P(\phi) \cdot \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix} = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ e^{i\phi} \end{pmatrix}$$

**Step 3:** Apply second Hadamard
$$H \cdot \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ e^{i\phi} \end{pmatrix} = \frac{1}{2}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}\begin{pmatrix} 1 \\ e^{i\phi} \end{pmatrix} = \frac{1}{2}\begin{pmatrix} 1 + e^{i\phi} \\ 1 - e^{i\phi} \end{pmatrix}$$

**Result:**
$$|\psi_{out}\rangle = \begin{pmatrix} \frac{1}{2}(1 + e^{i\phi}) \\ \frac{1}{2}(1 - e^{i\phi}) \end{pmatrix}$$

The detection probabilities are:
$$P(D1) = \left|\frac{1}{2}(1 + e^{i\phi})\right|^2 = \frac{1}{2}(1 + \cos\phi)$$
$$P(D2) = \left|\frac{1}{2}(1 - e^{i\phi})\right|^2 = \frac{1}{2}(1 - \cos\phi)$$

**This is exactly what we got before!** The matrix calculation and the wave calculation give identical results.

### The Quantum Circuit

We can draw the MZI as a quantum circuit:

$$|0\rangle \xrightarrow{\quad H \quad} \xrightarrow{\quad P(\phi) \quad} \xrightarrow{\quad H \quad} \text{measure}$$

This is a **single-qubit interferometer**. The sequence $H \to P(\phi) \to H$ is one of the most fundamental operations in quantum computing.

| Interferometer Component | Quantum Gate |
|-------------------------|--------------|
| Beam splitter (split) | Hadamard $H$ |
| Path length difference | Phase gate $P(\phi)$ |
| Beam splitter (recombine) | Hadamard $H$ |
| Detector | Measurement |

The MZI isn't just *like* a quantum circuit — it *is* a quantum circuit, implemented with light.

---

## The Bloch Sphere

We've seen that a qubit state can be written as:

$$|\psi\rangle = c_0|0\rangle + c_1|1\rangle = \begin{pmatrix} c_0 \\ c_1 \end{pmatrix}$$

where $c_0$ and $c_1$ are complex numbers. How do we visualize this?

### Counting Degrees of Freedom

Let's count how many real parameters we need to specify a qubit state.

**Start:** Two complex numbers $c_0$ and $c_1$
$$\text{2 complex numbers} = \text{4 real parameters}$$

**Constraint 1: Normalization**

We require $|c_0|^2 + |c_1|^2 = 1$ (probabilities must sum to 1).

This is one real equation, so:
$$\text{4 real parameters} - \text{1 constraint} = \text{3 real parameters}$$

**Constraint 2: Global phase doesn't matter**

We just saw in the MZI that multiplying all amplitudes by $e^{i\gamma}$ doesn't change any measurement. So the states $|\psi\rangle$ and $e^{i\gamma}|\psi\rangle$ are physically identical.

We can use this freedom to make $c_0$ real and non-negative. This removes one more parameter:
$$\text{3 real parameters} - \text{1 (global phase)} = \text{2 real parameters}$$

### Two Parameters = A Sphere!

A state with 2 real parameters can be represented as a point on a 2-dimensional surface. The natural choice is a sphere.

### The Bloch Parametrization

With $c_0$ real and non-negative, and $|c_0|^2 + |c_1|^2 = 1$, we can write:

$$c_0 = \cos\frac{\theta}{2}, \qquad c_1 = e^{i\phi}\sin\frac{\theta}{2}$$

where:
- $\theta \in [0, \pi]$ is the **polar angle** (from the north pole)
- $\phi \in [0, 2\pi)$ is the **azimuthal angle** (around the equator)

The general qubit state is:

$$\boxed{|\psi\rangle = \cos\frac{\theta}{2}|0\rangle + e^{i\phi}\sin\frac{\theta}{2}|1\rangle}$$

These are exactly spherical coordinates! Every qubit state corresponds to a point on the unit sphere, called the **Bloch sphere**.

```{figure} bloch_sphere.svg
:name: bloch-sphere
:width: 70%

The Bloch sphere. The state $|0\rangle$ is at the north pole, $|1\rangle$ at the south pole. Equal superposition states lie on the equator, with the relative phase $\phi$ determining the position around the equator. A general state $|\psi\rangle$ is specified by the polar angle $\theta$ and azimuthal angle $\phi$.
```

```{admonition} Why θ/2?
:class: note

The factor of $1/2$ in $\theta/2$ is a convention that makes the geometry work out nicely. It ensures that orthogonal states are on opposite sides of the sphere (antipodal points). We'll see this shortly.
```

### Key Points on the Bloch Sphere

Let's identify some important states.

**North Pole: $|0\rangle$**

Set $\theta = 0$:
$$|\psi\rangle = \cos(0)|0\rangle + e^{i\phi}\sin(0)|1\rangle = |0\rangle$$

The north pole is $|0\rangle$, regardless of $\phi$ (since the coefficient of $|1\rangle$ is zero).

**South Pole: $|1\rangle$**

Set $\theta = \pi$:
$$|\psi\rangle = \cos(\pi/2)|0\rangle + e^{i\phi}\sin(\pi/2)|1\rangle = e^{i\phi}|1\rangle$$

Since global phase doesn't matter, this is equivalent to $|1\rangle$. The south pole is $|1\rangle$.

**Equator: Equal Superpositions**

On the equator, $\theta = \pi/2$, so $\cos(\theta/2) = \sin(\theta/2) = 1/\sqrt{2}$:

$$|\psi\rangle = \frac{1}{\sqrt{2}}(|0\rangle + e^{i\phi}|1\rangle)$$

Different points on the equator correspond to different relative phases:

| $\phi$ | State | Name | Position |
|--------|-------|------|----------|
| $0$ | $\frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ | $|+\rangle$ | +x axis |
| $\pi$ | $\frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$ | $|-\rangle$ | −x axis |
| $\pi/2$ | $\frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ | $|+i\rangle$ | +y axis |
| $3\pi/2$ | $\frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$ | $|-i\rangle$ | −y axis |

### Summary of Bloch Sphere Positions

| State | $\theta$ | $\phi$ | Position |
|-------|----------|--------|----------|
| $\|0\rangle$ | $0$ | — | North pole (+z) |
| $\|1\rangle$ | $\pi$ | — | South pole (−z) |
| $\|+\rangle = \frac{1}{\sqrt{2}}(\|0\rangle + \|1\rangle)$ | $\pi/2$ | $0$ | +x axis |
| $\|-\rangle = \frac{1}{\sqrt{2}}(\|0\rangle - \|1\rangle)$ | $\pi/2$ | $\pi$ | −x axis |
| $\|+i\rangle = \frac{1}{\sqrt{2}}(\|0\rangle + i\|1\rangle)$ | $\pi/2$ | $\pi/2$ | +y axis |
| $\|-i\rangle = \frac{1}{\sqrt{2}}(\|0\rangle - i\|1\rangle)$ | $\pi/2$ | $3\pi/2$ | −y axis |

### Orthogonal States are Antipodal

A beautiful fact: **orthogonal quantum states are on opposite sides of the Bloch sphere**.

- $|0\rangle$ (north pole) and $|1\rangle$ (south pole): orthogonal ✓
- $|+\rangle$ (+x) and $|-\rangle$ (−x): orthogonal ✓
- $|+i\rangle$ (+y) and $|-i\rangle$ (−y): orthogonal ✓

This is why we use $\theta/2$ in the parametrization — it makes orthogonal states antipodal.

---

## iClicker Questions

**iClicker 1:** A state has Bloch coordinates $\theta = \pi/2$, $\phi = \pi/2$. What is the state?

- (A) $|0\rangle$
- (B) $|1\rangle$  
- (C) $\frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$
- (D) $\frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ ✓

**Solution:** With $\theta = \pi/2$: $\cos(\theta/2) = \cos(\pi/4) = 1/\sqrt{2}$ and $\sin(\theta/2) = 1/\sqrt{2}$.
With $\phi = \pi/2$: $e^{i\phi} = e^{i\pi/2} = i$.
So $|\psi\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle) = |+i\rangle$.

---

**iClicker 2:** I start with $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ on the +x axis and apply the phase gate $P(\pi) = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}$. Where do I end up on the Bloch sphere?

- (A) +x axis (same place)
- (B) −x axis ✓
- (C) +y axis
- (D) North pole

**Solution:** 
$$P(\pi)|+\rangle = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix} \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix} = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ -1 \end{pmatrix} = |-\rangle$$

The phase gate rotated us from +x to −x, which is a rotation by $\pi$ around the z-axis!

---

## Visualizing the MZI on the Bloch Sphere

Now let's see how the Mach-Zehnder interferometer looks on the Bloch sphere.

**The MZI sequence:** $|0\rangle \xrightarrow{H} \xrightarrow{P(\phi)} \xrightarrow{H}$

**Step 1: Start at the north pole**

Initial state: $|0\rangle$ — this is the north pole of the Bloch sphere.

**Step 2: Apply Hadamard**

$$H|0\rangle = |+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$$

This moves us from the north pole to the +x axis (on the equator).

Geometrically, the Hadamard gate is a rotation that takes the z-axis to the x-axis.

**Step 3: Apply phase gate $P(\phi)$**

$$P(\phi)|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + e^{i\phi}|1\rangle)$$

This rotates us around the z-axis by angle $\phi$, staying on the equator.

- $\phi = 0$: stay at +x axis ($|+\rangle$)
- $\phi = \pi/2$: move to +y axis ($|+i\rangle$)
- $\phi = \pi$: move to −x axis ($|-\rangle$)
- $\phi = 3\pi/2$: move to −y axis ($|-i\rangle$)

**Step 4: Apply second Hadamard**

The second Hadamard rotates again. Where we end up depends on where we were on the equator:

- Starting at +x ($\phi = 0$): $H|+\rangle = |0\rangle$ → back to north pole
- Starting at −x ($\phi = \pi$): $H|-\rangle = |1\rangle$ → south pole
- Starting at +y or −y: end up somewhere in between

**The result:**

| Phase $\phi$ | After $P(\phi)$ | Final state | $P(D1) = P(|0\rangle)$ |
|--------------|-----------------|-------------|------------------------|
| $0$ | $|+\rangle$ (+x) | $|0\rangle$ | $1$ |
| $\pi/2$ | $|+i\rangle$ (+y) | $\frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ | $1/2$ |
| $\pi$ | $|-\rangle$ (−x) | $|1\rangle$ | $0$ |
| $3\pi/2$ | $|-i\rangle$ (−y) | $\frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$ | $1/2$ |

The phase $\phi$ controls where we end up — this is the essence of quantum interference!

---

## Qiskit Visualization

Here's Python code using Qiskit to visualize states on the Bloch sphere:

```python
import numpy as np
from qiskit.visualization import plot_bloch_multivector
from qiskit.quantum_info import Statevector

def bloch_state(theta, phi):
    """
    Create qubit state: cos(theta/2)|0> + e^(i*phi)*sin(theta/2)|1>
    
    Parameters:
        theta: polar angle from north pole (0 to pi)
        phi: azimuthal angle around equator (0 to 2*pi)
    
    Returns:
        Qiskit Statevector object
    """
    c0 = np.cos(theta / 2)
    c1 = np.exp(1j * phi) * np.sin(theta / 2)
    return Statevector([c0, c1])

# Examples: the six cardinal states
print("North pole |0⟩:")
plot_bloch_multivector(bloch_state(0, 0))

print("|+⟩ state (+x axis):")
plot_bloch_multivector(bloch_state(np.pi/2, 0))

print("|+i⟩ state (+y axis):")
plot_bloch_multivector(bloch_state(np.pi/2, np.pi/2))
```

### Visualizing the MZI Sequence

```python
from qiskit.quantum_info import Operator

# Define gates
H = Operator([[1, 1], [1, -1]]) / np.sqrt(2)  # Hadamard

def phase_gate(phi):
    """Phase gate P(phi) = diag(1, e^(i*phi))"""
    return Operator([[1, 0], [0, np.exp(1j * phi)]])

# MZI with phase = pi
psi = Statevector([1, 0])       # Start at |0⟩
print("Initial |0⟩:"); plot_bloch_multivector(psi)

psi = psi.evolve(H)             # After first Hadamard
print("After H:"); plot_bloch_multivector(psi)

psi = psi.evolve(phase_gate(np.pi))  # After phase gate
print("After P(π):"); plot_bloch_multivector(psi)

psi = psi.evolve(H)             # After second Hadamard  
print("Final state:"); plot_bloch_multivector(psi)
```

