# Lecture 8: Ramsey Interferometry and Quantum Sensing

## The Payoff: From Abstract to Technology

Over the past three lectures, we've built up a complete toolkit:
- Complex amplitudes and interference (Lecture 5)
- Measurement and state collapse (Lecture 6)
- Time evolution and rotations on the Bloch sphere (Lecture 7)

Today, we put it all together. We'll see how the abstract one-qubit interferometer becomes a real device: the **Ramsey interferometer**. This is the heart of atomic clocks, the most precise instruments ever built.

And we'll see the broader picture: a qubit is a **sensor**. Anything that affects its energy levels can be measured through interference.

---

## Review: The One-Qubit Interferometer

In Lecture 5, we analyzed the Mach-Zehnder interferometer for light:
- Beam splitter (split)
- Phase accumulation in one arm
- Beam splitter (recombine)
- Detection

The quantum circuit version is:

$$
|0\rangle \xrightarrow{H} \xrightarrow{R_z(\phi)} \xrightarrow{H} \xrightarrow{\text{measure}}
$$

We derived:
$$
P(0) = \cos^2(\phi/2), \qquad P(1) = \sin^2(\phi/2)
$$

The phase $\phi$ becomes encoded in the measurement probability. But where does this phase come from physically?

---

## The Ramsey Sequence

In atomic physics, the same interferometer appears with a clear physical interpretation.

### The Setup

1. **Atom starts in ground state:** $|0\rangle$ (or $|\downarrow\rangle$)

2. **First π/2-pulse:** Apply a resonant driving field for time $t_{\pi/2} = \pi/(2\Omega)$

   This rotates around x by 90°, creating superposition:
   $$|0\rangle \to \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$$
   
   On the Bloch sphere: north pole → equator

3. **Free evolution for time $T$:** The atom evolves under its natural Hamiltonian $H = \frac{\hbar\omega_0}{2}\sigma_z$

   The state precesses around z, accumulating phase:
   $$\frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle) \to \frac{1}{\sqrt{2}}(e^{-i\omega_0 T/2}|0\rangle - i e^{i\omega_0 T/2}|1\rangle)$$
   
   The relative phase is $\phi = \omega_0 T$

4. **Second π/2-pulse:** Another rotation around x by 90°

   The two "paths" (being in $|0\rangle$ vs $|1\rangle$) recombine and interfere

5. **Measure:** The interference determines $P(0)$ and $P(1)$

```{figure} ./02_04_lecture_files/ramsey_sequence.svg
:width: 500px
:name: ramsey-sequence

The Ramsey sequence: π/2 pulse - free evolution - π/2 pulse - measure.
```

### The Calculation

Let's work through this carefully.

**After first π/2-pulse** ($R_x(\pi/2)$ on $|0\rangle$):

Using $R_x(\pi/2) = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & -i \\ -i & 1 \end{pmatrix}$:

$$
|\psi_1\rangle = R_x(\pi/2)|0\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)
$$

**After free evolution for time $T$** (under $H = \frac{\hbar\omega_0}{2}\sigma_z$):

$$
|\psi_2\rangle = R_z(\omega_0 T)|\psi_1\rangle = \frac{1}{\sqrt{2}}(e^{-i\omega_0 T/2}|0\rangle - i e^{i\omega_0 T/2}|1\rangle)
$$

Pull out a global phase $e^{-i\omega_0 T/2}$:

$$
|\psi_2\rangle = \frac{e^{-i\omega_0 T/2}}{\sqrt{2}}(|0\rangle - i e^{i\omega_0 T}|1\rangle)
$$

**After second π/2-pulse:**

$$
|\psi_3\rangle = R_x(\pi/2)|\psi_2\rangle
$$

After some algebra (or using the general rotation formula):

$$
|\psi_3\rangle = \frac{e^{-i\omega_0 T/2}}{\sqrt{2}}\left[\frac{1-e^{i\omega_0 T}}{2}|0\rangle + \frac{-i(1+e^{i\omega_0 T})}{2}|1\rangle\right]
$$

The probabilities are:

$$
P(0) = \sin^2\left(\frac{\omega_0 T}{2}\right), \qquad P(1) = \cos^2\left(\frac{\omega_0 T}{2}\right)
$$

```{admonition} The Ramsey Result
:class: important

For a Ramsey sequence with free evolution time $T$:

$$
P(0) = \sin^2\left(\frac{\omega_0 T}{2}\right) = \frac{1 - \cos(\omega_0 T)}{2}
$$

The atomic transition frequency $\omega_0$ is directly encoded in the oscillation of the measurement probability.
```

---

## Ramsey Fringes

If we vary the free evolution time $T$ (or equivalently, the detuning), the probability oscillates. These oscillations are called **Ramsey fringes**.

```{figure} ./02_04_lecture_files/ramsey_fringes.svg
:width: 500px
:name: ramsey-fringes

Ramsey fringes: probability oscillates with free evolution time or detuning.
```

### With Detuning

In practice, we often work in a frame rotating at some reference frequency $\omega_{\text{ref}}$. The free evolution phase then depends on the **detuning**:

$$
\delta = \omega_0 - \omega_{\text{ref}}
$$

The accumulated phase is $\phi = \delta \cdot T$, and:

$$
P(0) = \sin^2\left(\frac{\delta T}{2}\right)
$$

Scanning $\delta$ (by changing the reference frequency) produces fringes with period:

$$
\Delta\delta = \frac{2\pi}{T}
$$

**Key insight:** Longer free evolution time $T$ means narrower fringes — higher frequency resolution.

---

## Why Ramsey Beats Direct Measurement

Why use this complicated sequence? Why not just measure the atomic frequency directly?

### The Linewidth Problem

If you try to measure $\omega_0$ by driving the transition and looking for maximum absorption, the spectral line has a **width** determined by how long you probe:

$$
\Delta\omega \sim \frac{1}{t_{\text{probe}}}
$$

This is the time-energy uncertainty relation.

### Ramsey's Trick

Ramsey's insight: **separate the "probing" (π/2-pulses) from the "measuring" (free evolution).**

The π/2-pulses can be short (microseconds). The free evolution can be long (seconds). The resolution is set by the free evolution time:

$$
\Delta\omega \sim \frac{1}{T}
$$

You get the resolution of a long measurement with the control of short pulses.

```{admonition} Norman Ramsey's Nobel Prize
:class: note

Norman Ramsey received the 1989 Nobel Prize in Physics for developing this technique. His "separated oscillatory fields" method is the basis of all modern precision spectroscopy and atomic clocks.
```

---

## Atomic Clocks: The Most Precise Instruments

An atomic clock is a Ramsey interferometer where the phase comes from the energy difference between two atomic states.

### The Principle

Atoms have precise, universal energy levels. The transition frequency between two levels is a constant of nature:

**Cesium-133:** The ground state hyperfine transition defines the second.

$$
\omega_{\text{Cs}} = 2\pi \times 9,192,631,770 \text{ Hz (exactly)}
$$

This isn't an approximate measurement — it's the **definition** of the second since 1967.

### How It Works

1. **Prepare atoms in state $|0\rangle$** (ground hyperfine level)

2. **Apply π/2-pulse** from a local oscillator at frequency $\omega_{\text{LO}}$

3. **Free evolution for time $T$**

4. **Apply second π/2-pulse**

5. **Measure population in $|0\rangle$**

6. **Feedback:** Adjust $\omega_{\text{LO}}$ to lock to the atomic frequency

```{figure} ./02_04_lecture_files/atomic_clock_schematic.svg
:width: 450px
:name: atomic-clock

Schematic of an atomic clock: Ramsey sequence + feedback loop.
```

The feedback loop keeps the local oscillator exactly on resonance with the atomic transition. The oscillator's ticks become "atomic ticks."

### Performance

Modern cesium clocks achieve:
- **Fractional accuracy:** $\sim 10^{-16}$
- **Would gain or lose 1 second in:** 300 million years

Optical atomic clocks (using visible light transitions) do even better:
- **Fractional accuracy:** $\sim 10^{-18}$
- **Would gain or lose 1 second in:** longer than the age of the universe

```{admonition} Why So Precise?
:class: tip

1. **Universal:** All cesium atoms are identical. The frequency is a constant of nature.

2. **Long coherence:** Atoms can be isolated from perturbations, allowing long free evolution times.

3. **Quantum interference:** The Ramsey fringe pattern allows exquisite frequency discrimination.

4. **Feedback averaging:** Running continuously and averaging reduces statistical noise.
```

---

## GPS: Atomic Clocks in Your Pocket

The Global Positioning System relies on atomic clocks.

### The Principle

Each GPS satellite carries atomic clocks and broadcasts timing signals. Your phone receives signals from multiple satellites, each with a different travel time.

The time delay tells you the distance: $d = c \cdot \Delta t$

From multiple distances, you triangulate your position.

### The Numbers

- Light travels ~30 cm in 1 nanosecond
- To get 1-meter position accuracy, you need ~3 ns timing accuracy
- Over one day, a clock must not drift more than ~1 µs
- This requires fractional accuracy of $\sim 10^{-14}$

Without atomic clocks, GPS would accumulate errors of kilometers per day.

```{admonition} Relativity Correction
:class: note

GPS satellites orbit at high altitude and high speed. General relativity (gravity) and special relativity (velocity) both affect the clock rates:
- Gravitational time dilation: clocks run faster by ~45 µs/day
- Velocity time dilation: clocks run slower by ~7 µs/day
- Net effect: clocks run ~38 µs/day fast

This is corrected for. GPS is a real-world test of Einstein's theories.
```

---

## Quantum Sensing: The Qubit as a Probe

Atomic clocks are the most famous application, but the principle is much broader.

**Core idea:** A qubit is sensitive to anything that shifts its energy levels. The Ramsey sequence converts that energy shift into a measurable phase.

### The General Framework

Any perturbation that causes a Hamiltonian $H = \frac{\hbar\omega}{2}\sigma_z$ leads to phase accumulation:

$$
\phi = \omega \cdot T
$$

If $\omega$ depends on some physical quantity $X$ (magnetic field, electric field, gravity, etc.), then measuring $\phi$ measures $X$:

$$
X \to \omega(X) \to \phi = \omega T \to P(0) = \sin^2(\phi/2)
$$

### Example: Magnetometry

An electron spin in a magnetic field $B$ has:

$$
\omega = \gamma_e B
$$

where $\gamma_e = 2\pi \times 28 \text{ GHz/T}$ is the electron gyromagnetic ratio.

A Ramsey sequence with time $T$ gives phase $\phi = \gamma_e B T$.

**Sensitivity:** The minimum detectable field change corresponds to the minimum detectable phase change:

$$
\delta B = \frac{\delta\phi}{\gamma_e T}
$$

Longer $T$ → better sensitivity (until decoherence sets in).

### Example: Gravimetry

Atom interferometers use the gravitational potential energy:

$$
\phi = \frac{m g \Delta h \cdot T}{\hbar}
$$

where $\Delta h$ is the height difference between paths in a matter-wave interferometer.

These devices can measure:
- Gravitational acceleration $g$ to 9 decimal places
- Gravitational gradients (useful for underground mapping)
- Tests of the equivalence principle

### The Quantum Advantage

Classical sensors measure a field by its direct effect (deflection, voltage, etc.). Quantum sensors measure through **interference** — they're sensitive to phase, which can be accumulated over time.

The precision scales as:
- **Classical averaging:** $\delta X \propto 1/\sqrt{T}$ (central limit theorem)
- **Quantum (Ramsey):** $\delta X \propto 1/T$ (phase accumulation)

This is called the **Standard Quantum Limit**. With entanglement, you can do even better (**Heisenberg Limit**), but that's for a later lecture.

---

## Decoherence: The Enemy of Coherence

There's a catch. We've been assuming the qubit evolves purely under the desired Hamiltonian. In reality, the qubit also interacts with its environment.

### What Goes Wrong

Random interactions with the environment cause:
1. **Dephasing:** Random phase kicks scramble the accumulated phase
2. **Relaxation:** The qubit decays from $|1\rangle$ to $|0\rangle$

Both effects destroy the interference pattern.

```{figure} ./02_04_lecture_files/decoherence.svg
:width: 400px
:name: decoherence

Decoherence: Ramsey fringes decay as free evolution time increases.
```

### Coherence Times

The characteristic timescales are:
- **$T_1$ (relaxation time):** How long until the qubit decays
- **$T_2$ (dephasing time):** How long until phase coherence is lost

For quantum sensing or computing, you need:
$$
T_{\text{operation}} \ll T_2
$$

| System | Typical $T_2$ |
|--------|---------------|
| Trapped ions | seconds to minutes |
| Neutral atoms | ~1 second |
| Superconducting qubits | ~100 µs |
| NV centers | ~ms |
| Electron spin (solid state) | ~µs to ms |

The quest for longer coherence times drives much of modern quantum hardware research.

---

## Lab: Ramsey Fringes in Qiskit

Let's simulate a Ramsey sequence.

```{code-block} python
import numpy as np
import matplotlib.pyplot as plt
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Ramsey sequence: Rx(pi/2) - Rz(phi) - Rx(pi/2) - measure
# The Rz represents free evolution with accumulated phase phi

phases = np.linspace(0, 4*np.pi, 100)
prob_0 = []

for phi in phases:
    qc = QuantumCircuit(1)
    
    # First pi/2 pulse (around x)
    qc.rx(np.pi/2, 0)
    
    # Free evolution (phase accumulation around z)
    qc.rz(phi, 0)
    
    # Second pi/2 pulse (around x)
    qc.rx(np.pi/2, 0)
    
    # Get probabilities
    state = Statevector(qc)
    probs = state.probabilities()
    prob_0.append(probs[0])

# Plot Ramsey fringes
plt.figure(figsize=(10, 5))
plt.plot(phases/np.pi, prob_0, 'b-', linewidth=2)
plt.xlabel('Phase φ/π (proportional to detuning × time)', fontsize=12)
plt.ylabel('P(0)', fontsize=12)
plt.title('Ramsey Fringes', fontsize=14)
plt.grid(True, alpha=0.3)
plt.axhline(y=0.5, color='gray', linestyle='--', alpha=0.5)
plt.show()

# Verify: P(0) should follow sin²(φ/2)
theory = np.sin(phases/2)**2
plt.figure(figsize=(10, 5))
plt.plot(phases/np.pi, prob_0, 'b-', linewidth=2, label='Qiskit simulation')
plt.plot(phases/np.pi, theory, 'r--', linewidth=2, label='Theory: sin²(φ/2)')
plt.xlabel('Phase φ/π', fontsize=12)
plt.ylabel('P(0)', fontsize=12)
plt.title('Ramsey Fringes: Simulation vs Theory', fontsize=14)
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

### Adding Decoherence (Conceptual)

Real decoherence requires simulating open quantum systems, which is beyond basic Qiskit. But we can simulate the effect:

```{code-block} python
# Simulating the effect of decoherence on Ramsey fringes
# As T increases, the fringe contrast decays

T2 = 10  # arbitrary units

evolution_times = np.linspace(0, 30, 100)
prob_0_with_decoherence = []

for T in evolution_times:
    # Phase accumulates
    phi = T  # Assume detuning = 1 in arbitrary units
    
    # Ideal probability
    P0_ideal = np.sin(phi/2)**2
    
    # Decoherence reduces fringe contrast
    contrast = np.exp(-T/T2)
    P0_real = 0.5 + contrast * (P0_ideal - 0.5)
    
    prob_0_with_decoherence.append(P0_real)

plt.figure(figsize=(10, 5))
plt.plot(evolution_times, prob_0_with_decoherence, 'b-', linewidth=2)
plt.xlabel('Free evolution time T (arb. units)', fontsize=12)
plt.ylabel('P(0)', fontsize=12)
plt.title(f'Ramsey Fringes with Decoherence (T₂ = {T2})', fontsize=14)
plt.axhline(y=0.5, color='gray', linestyle='--', alpha=0.5)
plt.grid(True, alpha=0.3)
plt.show()
```

---

## The Big Picture: Interference as Information

Let's step back and see the unified view.

| System | "Split" | Phase Source | "Recombine" | Observable |
|--------|---------|--------------|-------------|------------|
| Double slit | Two slits | Path length difference | Screen | Intensity pattern |
| Mach-Zehnder | Beam splitter | Arm length or medium | Beam splitter | Detector signal |
| Polarization | Waveplate | Birefringence | Waveplate | Polarization |
| Ramsey | π/2 pulse | Energy splitting × time | π/2 pulse | Population |
| Atom interferometer | Light pulse | Gravity × path × time | Light pulse | Momentum state |

Every quantum sensor follows this pattern:
1. **Create superposition** (split into paths/states)
2. **Accumulate phase** from whatever you want to measure
3. **Interfere** (recombine)
4. **Measure** (convert phase to probability)

```{admonition} The Central Idea
:class: important

**Quantum sensing converts physical quantities into phase differences, which interference converts into measurable probabilities.**

Phase can accumulate continuously, giving sensitivity that improves with time — until decoherence sets in.
```

---

## Summary

Today we completed our single-qubit journey:

1. **The Ramsey sequence** — π/2 pulse, free evolution, π/2 pulse — is a qubit interferometer

2. **Free evolution creates phase** — $\phi = \omega_0 T$ from energy splitting

3. **Ramsey fringes** — probability oscillates with phase, allowing frequency measurement

4. **Atomic clocks** use Ramsey to lock an oscillator to atomic transitions
   - Cesium defines the second: 9,192,631,770 Hz
   - Best clocks: 1 second accuracy over the age of the universe

5. **GPS** relies on atomic clocks for timing signals

6. **Quantum sensing** — any energy shift becomes a measurable phase
   - Magnetometry, gravimetry, electric fields, etc.
   - Sensitivity: $\delta\omega \sim 1/T$

7. **Decoherence** limits the useful evolution time

8. **Universal pattern:** Split → Phase → Recombine → Measure

---

## Homework

### Problem 1: Ramsey Derivation

Work through the Ramsey sequence calculation in detail.

**(a)** Start with $|0\rangle$. Apply $R_x(\pi/2)$. Write the resulting state.

**(b)** Apply $R_z(\phi)$ where $\phi = \omega_0 T$. Write the resulting state.

**(c)** Apply another $R_x(\pi/2)$. Write the final state.

**(d)** Compute $P(0)$ and $P(1)$. Verify they sum to 1.

**(e)** For what values of $\phi$ is $P(0) = 0$? For what values is $P(0) = 1$?

---

### Problem 2: Ramsey Fringes

Consider a Ramsey experiment where the free evolution time is $T$ and the atomic transition frequency is $\omega_0$.

**(a)** If you scan the local oscillator frequency $\omega_{\text{LO}}$ near $\omega_0$, the effective detuning is $\delta = \omega_0 - \omega_{\text{LO}}$. What is the fringe period $\Delta\delta$?

**(b)** If $T = 1$ s, what is the fringe period in Hz?

**(c)** If $T = 1$ ms, what is the fringe period?

**(d)** Why would you want longer $T$? What limits how long $T$ can be?

---

### Problem 3: Atomic Clock Precision

A cesium atomic clock has transition frequency $\omega_{\text{Cs}} = 2\pi \times 9.192631770$ GHz.

**(a)** If the Ramsey time is $T = 10$ ms, what is the linewidth $\Delta\omega$ of the Ramsey fringes?

**(b)** What fractional frequency precision $\Delta\omega/\omega_0$ does this correspond to?

**(c)** If you average for 1 day ($\sim 10^5$ seconds), and the noise averages down as $1/\sqrt{N}$ where $N$ is the number of measurements, by what factor does the precision improve?

**(d)** A modern optical clock uses a transition at $\omega = 2\pi \times 429$ THz (strontium). If it achieves the same absolute frequency uncertainty $\Delta\omega$ as the cesium clock, what fractional precision does this give?

---

### Problem 4: Magnetometry

An electron spin is used as a magnetic field sensor. The Zeeman splitting is $\omega = \gamma_e B$ where $\gamma_e = 2\pi \times 28$ GHz/T.

**(a)** If the Ramsey time is $T = 1$ µs, what is the phase accumulated in a field $B = 1$ µT?

**(b)** If you can resolve phase to $\delta\phi = 0.01$ rad (from counting statistics), what is the minimum detectable field change $\delta B$?

**(c)** If you increase $T$ to 100 µs (still shorter than $T_2$), how does the sensitivity improve?

**(d)** The Earth's magnetic field is about 50 µT. How many Ramsey fringes would you see scanning from 0 to 50 µT with $T = 1$ µs?

---

### Problem 5: Decoherence

A qubit has dephasing time $T_2 = 100$ µs.

**(a)** In a Ramsey experiment, the fringe contrast decays as $C(T) = e^{-T/T_2}$. The probability becomes:

$$
P(0) = \frac{1}{2} + \frac{C(T)}{2}\sin(\omega_0 T)
$$

Plot $P(0)$ vs $T$ for $\omega_0 = 2\pi \times 10$ MHz, from $T = 0$ to $T = 500$ µs.

**(b)** What is the fringe contrast at $T = T_2$? At $T = 2T_2$?

**(c)** For sensing, there's a trade-off: longer $T$ gives better resolution but lower contrast. The signal-to-noise ratio goes as $C(T) \times T$. What value of $T$ maximizes this?

---

### Problem 6: GPS Timing

GPS satellites orbit at altitude $h = 20,200$ km with orbital period $\approx 12$ hours.

**(a)** The speed of light is $c = 3 \times 10^8$ m/s. How long does a signal take to travel from satellite to ground?

**(b)** If you want 1 meter position accuracy, what timing accuracy is needed? (Recall $d = c \cdot \Delta t$.)

**(c)** Over one day, how much can the satellite clock drift while still meeting this requirement?

**(d)** What fractional clock accuracy does this require?

---

### Problem 7: Comparing Classical and Quantum Sensing

Consider measuring a magnetic field using (i) a classical compass, (ii) a Hall sensor, (iii) an atomic magnetometer.

**(a)** A compass needle deflects by an angle proportional to the field. If you can resolve 0.1° deflection, and the sensitivity is 1°/mT, what is the minimum detectable field?

**(b)** A Hall sensor produces voltage $V = k B$ with $k = 1$ mV/mT. If your voltmeter can resolve 1 µV, what is the minimum detectable field?

**(c)** An atomic magnetometer with $\gamma = 2\pi \times 28$ GHz/T and $T = 1$ ms can resolve phase $\delta\phi = 0.01$ rad. What is the minimum detectable field?

**(d)** Compare the three sensitivities. Why is the atomic magnetometer so much better?

---

### Problem 8: The Ramsey Sequence in Qiskit

Implement the Ramsey sequence in Qiskit.

**(a)** Create a function `ramsey(phi)` that returns the probability $P(0)$ for a Ramsey sequence with phase $\phi$. Use the circuit: `rx(π/2)` → `rz(phi)` → `rx(π/2)` → measure.

**(b)** Plot Ramsey fringes: $P(0)$ vs $\phi$ for $\phi$ from 0 to $4\pi$. Verify they match the theoretical curve.

**(c)** Modify the sequence to use `ry(π/2)` pulses instead of `rx(π/2)`. How does the fringe pattern change?

**(d)** Now add a small "error" in the first pulse: use `rx(π/2 + ε)` with $\epsilon = 0.1$. Plot the new fringes. How does the contrast change?

---

### Problem 9: Conceptual Questions

**(a)** In a Ramsey sequence, what physical role does the first π/2-pulse play? What about the second?

**(b)** Why is Ramsey better than a single long pulse for measuring frequency? (Hint: think about the difference between "probe time" and "evolution time.")

**(c)** An atomic clock doesn't directly measure time — it measures frequency. How do you get a "clock" from a frequency measurement?

**(d)** If all cesium atoms have exactly the same transition frequency, why do we need to stabilize a local oscillator to them? Why not just count atomic transitions directly?

**(e)** Decoherence destroys quantum interference. Does this mean a decohered qubit is useless for sensing? Or can you still extract some information?

---

## Looking Ahead

We've now completed our study of single-qubit physics:
- **States:** Points on the Bloch sphere
- **Evolution:** Rotations (from Hamiltonians)
- **Measurement:** Projection onto an axis
- **Interference:** Split-phase-recombine pattern

Next chapter, we enter the truly quantum regime: **two qubits and entanglement**. When you have more than one qubit, something entirely new happens — correlations that have no classical explanation. This is where quantum computing gets its power.
