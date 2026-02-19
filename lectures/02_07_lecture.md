# Lecture 2.7: Atomic Clocks and Noisy Qubits

## Part 1: What is a Second?

Let's start with a simple question: **How is the second defined?**

The answer has changed throughout history, and it's a fascinating story of humanity trying to pin down time more and more precisely.

### Ancient Times: The Day Divided

For most of human civilization, time was defined astronomically. The Babylonians divided the day into 24 hours, each hour into 60 minutes, and each minute into 60 seconds. This gives us:

$$1 \text{ day} = 24 \times 60 \times 60 = 86,400 \text{ seconds}$$

So a second was simply **1/86,400 of a mean solar day** — the average time between two successive noons.

This worked fine for thousands of years. But there was a problem lurking.

### The Problem: Earth is a Lousy Clock

By the 20th century, astronomers could measure Earth's rotation precisely enough to notice something troubling: **it's not constant**.

- **Tidal friction** from the Moon is gradually slowing Earth's rotation — days are getting longer by about 2 milliseconds per century
- **Seasonal variations** in atmospheric winds and ocean currents cause wobbles
- **Earthquakes** can suddenly shift mass around and change the rotation rate
- **Glacial melting** redistributes mass toward the equator

The "day" was drifting! A second defined as 1/86,400 of a day would itself be changing over time. Not good for precision science.

### 1956: The Ephemeris Second

The scientific community's first fix was to base the second on something more stable: **Earth's orbit around the Sun**.

In 1956, the second was redefined as:

$$1 \text{ second} = \frac{1}{31,556,925.9747} \text{ of the tropical year 1900}$$

The "tropical year" is the time from one spring equinox to the next. By pegging it to the year 1900, they avoided the problem of the year length changing over time.

This was more stable than the rotating Earth, but it had a practical problem: you couldn't actually *measure* it easily. The year 1900 was in the past! Astronomers had to work backwards from observations, which was slow and cumbersome.

### 1967: The Atomic Revolution

Then came the breakthrough. Physicists realized that **atoms** could provide a far better standard than any astronomical motion.

In 1967, the General Conference on Weights and Measures made a radical decision: **define time using atoms, not astronomy**. The second was redefined as:

$$\boxed{1 \text{ second} = 9,192,631,770 \text{ oscillations of cesium-133}}$$

Specifically, the oscillations of the radiation corresponding to the transition between two hyperfine levels of the cesium-133 ground state.

This number (9,192,631,770) wasn't arbitrary — it was chosen to match the previous ephemeris second as closely as possible. But now it *became* the definition.

### A Profound Shift

Think about what happened here:

- **Before 1967:** We measured atomic frequencies and compared them to the "true" second defined by astronomy
- **After 1967:** The atomic frequency *is* the second, and astronomical time is measured against it

Today, when you ask "how long is a second?", the answer is: however long it takes a cesium atom to oscillate 9,192,631,770 times. Atomic clocks don't measure time; they **define** it.

This is why atomic clocks are so fundamental — they're not approximating some external standard, they *are* the standard.

---

## Part 2: The Cesium Atom

Why cesium? And what is this "particular transition"?

### Electron Transitions

Atoms have electrons in different energy levels. When an electron jumps from a higher level to a lower one, it emits a photon. These are the **optical transitions** — the ones that produce visible light. The energy difference between the ground state $|g\rangle$ and excited state $|e\rangle$ corresponds to optical frequencies (~$10^{14}$ Hz).

### But There's More: Spin!

From last lecture, we learned that electrons have **spin** — an intrinsic angular momentum that makes them act like tiny bar magnets.

But here's something new: the **nucleus** of the atom also has a magnetic dipole moment! In cesium-133, the nucleus has spin $I = 7/2$, which means it's also a tiny bar magnet.

### Hyperfine Splitting

Now we have two bar magnets in the same atom: the electron spin and the nuclear spin. What happens when you put two bar magnets near each other?

- If they're **aligned** (pointing the same direction): one energy
- If they're **anti-aligned** (pointing opposite directions): different energy

This energy difference is called the **hyperfine splitting**. It's much smaller than optical transition energies (which is why it's called "fine" — it's a fine detail), but it's incredibly precise and stable.

For cesium-133, this hyperfine splitting corresponds to a frequency:

$$\boxed{\nu_0 = 9,192,631,770 \text{ Hz}}$$

This is not a measurement — this IS the definition of the second.

### Why Atomic Clocks Are So Good

Think about this frequency: roughly **10 billion oscillations per second**. 

If you're trying to measure time precisely, you want a fast "tick." A grandfather clock ticks once per second — not very precise. A quartz watch oscillates at 32,768 Hz — better. But cesium oscillates at 9.2 GHz — almost 10 billion ticks per second!

And here's the key: **every cesium atom in the universe is exactly identical**. Not approximately the same — exactly the same. This is quantum mechanics: atoms don't have manufacturing tolerances. A cesium atom in Boulder, Colorado has exactly the same hyperfine splitting as a cesium atom in Paris, or on the Moon, or in a galaxy a billion light-years away.

This is why atomic clocks are the most precise instruments ever built.

---

## Part 3: How Does a Clock Work?

We already know the answer from last lecture: the **Ramsey interferometer**.

### The Ramsey Sequence

1. **Initialize** in $|0\rangle$ (one hyperfine state — say, spins aligned)

2. **π/2-pulse** ($R_x(\pi/2)$): Create superposition
   $$|0\rangle \to \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$$

3. **Wait time $T$**: The state precesses (accumulates phase)

4. **π/2-pulse** ($R_x(\pi/2)$): Convert phase to population

5. **Measure**: Count atoms in $|0\rangle$ vs $|1\rangle$

The measurement probability oscillates: $P_0 = \sin^2(\phi/2)$ where $\phi$ is the accumulated phase.

### The Key Question

Where does the **precession** come from in an atomic clock?

In the Stern-Gerlach picture, we applied an external magnetic field. But in an atomic clock, we're not applying a DC magnetic field — we're applying **microwave radiation**.

To understand this, we need to look at atom-light interactions.

---

## Part 4: Atom-Light Interactions

### The Energy Hamiltonian

From last lecture, we know that a spin in a magnetic field has Hamiltonian:

$$H = -\boldsymbol{\mu} \cdot \mathbf{B} = -\gamma \mathbf{S} \cdot \mathbf{B}$$

This gives us rotations on the Bloch sphere.

For our two hyperfine states with energy splitting $\hbar\omega_0$, we can write:

$$H_0 = \frac{\hbar\omega_0}{2}\sigma_z = \frac{\hbar\omega_0}{2}(|{\uparrow}\rangle\langle{\uparrow}| - |{\downarrow}\rangle\langle{\downarrow}|)$$

where $|{\uparrow}\rangle$ has energy $+\hbar\omega_0/2$ and $|{\downarrow}\rangle$ has energy $-\hbar\omega_0/2$.

### Time Evolution

From last lecture, the time evolution operator is $U(t) = e^{-iH_0 t/\hbar}$.

Acting on our energy eigenstates:
$$|{\uparrow}\rangle \to e^{-i\omega_0 t/2}|{\uparrow}\rangle$$
$$|{\downarrow}\rangle \to e^{+i\omega_0 t/2}|{\downarrow}\rangle$$

A superposition picks up a relative phase:
$$\frac{1}{\sqrt{2}}(|{\uparrow}\rangle + |{\downarrow}\rangle) \to \frac{1}{\sqrt{2}}(e^{-i\omega_0 t/2}|{\uparrow}\rangle + e^{+i\omega_0 t/2}|{\downarrow}\rangle)$$

This is **precession** around the z-axis at frequency $\omega_0$ — ten billion times per second!

But this is just free evolution. We still need a way to **couple** the two states — to drive transitions between them.

---

## Part 5: Coupling with Light

### Light is an Oscillating Field

How do we drive transitions between $|{\uparrow}\rangle$ and $|{\downarrow}\rangle$?

We use **light** — specifically, microwave radiation at frequency near $\omega_0$.

Light is an oscillating electromagnetic field. The electric field oscillates as $\mathbf{E}(t) = \mathbf{E}_0 \cos(\omega_L t)$, and similarly for the magnetic field. The subscript $L$ stands for "laser" (or "light") — this is the frequency of our applied radiation.

### The Interaction Picture

When a photon has energy matching the transition ($\hbar\omega_L \approx \hbar\omega_0$), it can excite the atom from $|{\downarrow}\rangle$ to $|{\uparrow}\rangle$ or stimulate emission from $|{\uparrow}\rangle$ to $|{\downarrow}\rangle$.

We model this coupling with an interaction term:

$$H_{int} = \hbar\Omega\cos(\omega_L t)(|{\uparrow}\rangle\langle{\downarrow}| + |{\downarrow}\rangle\langle{\uparrow}|)$$

where $\Omega$ is the **Rabi frequency** — it tells us how strongly the light couples to the atom (proportional to the light intensity).

### The Full Hamiltonian

The total Hamiltonian is:

$$H = \frac{\hbar\omega_0}{2}\sigma_z + \hbar\Omega\cos(\omega_L t)\sigma_x$$

Uh oh. This Hamiltonian is **time-dependent**. The Schrödinger equation becomes:

$$i\hbar\frac{\partial}{\partial t}|\psi\rangle = H(t)|\psi\rangle$$

This is much harder to solve than the time-independent case!

---

## Part 6: The Rotating Frame Trick

Here's a beautiful trick that transforms our hard, time-dependent problem into an easy, time-independent one.

### The Idea

The time dependence comes from the $\cos(\omega_L t)$ oscillating at frequency $\omega_L$. 

What if we **rotate our reference frame** at the same frequency? Then the oscillation would appear stationary!

This is like watching a spinning merry-go-round. If you stand on the ground, you see horses going up and down, around and around. But if you **ride on the merry-go-round**, the horses appear stationary (just bobbing up and down).

### The Transformation

We define a rotating frame by the unitary transformation:

$$|\psi_R\rangle = U_R^\dagger|\psi\rangle \quad \text{where} \quad U_R = e^{-i\omega_L t \sigma_z/2}$$

This rotates our state around the z-axis at frequency $\omega_L$ — we're "riding along" with the light field.

### The Rotating Frame Hamiltonian

After some algebra (see box below), the Hamiltonian in the rotating frame becomes:

$$\boxed{H_R = \frac{\hbar\Delta}{2}\sigma_z + \frac{\hbar\Omega}{2}\sigma_x}$$

where $\Delta = \omega_0 - \omega_L$ is the **detuning** — the difference between the atomic frequency and the light frequency.

```{admonition} Derivation: Rotating Frame Hamiltonian
:class: dropdown

Starting from the Schrödinger equation $i\hbar\partial_t|\psi\rangle = H|\psi\rangle$ with $|\psi\rangle = U_R|\psi_R\rangle$:

$$i\hbar\partial_t(U_R|\psi_R\rangle) = H U_R|\psi_R\rangle$$

$$i\hbar(\partial_t U_R)|\psi_R\rangle + i\hbar U_R\partial_t|\psi_R\rangle = H U_R|\psi_R\rangle$$

$$i\hbar\partial_t|\psi_R\rangle = U_R^\dagger H U_R|\psi_R\rangle - i\hbar U_R^\dagger(\partial_t U_R)|\psi_R\rangle$$

The effective Hamiltonian in the rotating frame is:
$$H_R = U_R^\dagger H U_R - i\hbar U_R^\dagger(\partial_t U_R)$$

For $U_R = e^{-i\omega_L t\sigma_z/2}$:
- $U_R^\dagger(\partial_t U_R) = -i\omega_L\sigma_z/2$, giving a term $+\frac{\hbar\omega_L}{2}\sigma_z$
- The $\sigma_z$ term transforms as: $U_R^\dagger \sigma_z U_R = \sigma_z$ (unchanged)
- The $\sigma_x\cos(\omega_L t)$ term: Using $U_R^\dagger \sigma_x U_R = \sigma_x\cos(\omega_L t) + \sigma_y\sin(\omega_L t)$

After the rotating wave approximation (dropping fast-oscillating terms at $2\omega_L$):

$$H_R = \frac{\hbar\omega_0}{2}\sigma_z - \frac{\hbar\omega_L}{2}\sigma_z + \frac{\hbar\Omega}{2}\sigma_x = \frac{\hbar\Delta}{2}\sigma_z + \frac{\hbar\Omega}{2}\sigma_x$$
```

### What We've Achieved

**Before:** Time-dependent Hamiltonian with oscillating terms — hard!

**After:** Time-independent Hamiltonian with constant coefficients — easy!

$$H_R = \frac{\hbar\Delta}{2}\sigma_z + \frac{\hbar\Omega}{2}\sigma_x$$

This is just a constant "magnetic field" pointing in a direction determined by $\Delta$ and $\Omega$!

---

## Part 7: Understanding the Rotating Frame

Let's build intuition for what $H_R = \frac{\hbar\Delta}{2}\sigma_z + \frac{\hbar\Omega}{2}\sigma_x$ means.

### Visualizing the Rotating Frame

Imagine you're on the Bloch sphere, but the whole sphere is rotating around the z-axis at frequency $\omega_L$ (the light frequency).

- **In the lab frame:** The state precesses at $\omega_0$ (ten billion times per second!).
- **In the rotating frame:** We subtract off the $\omega_L$ rotation, so the state only precesses at $\Delta = \omega_0 - \omega_L$.

If $\omega_L$ is close to $\omega_0$, then $\Delta$ is small — the precession appears slow in the rotating frame.

### On Resonance ($\Delta = 0$)

When $\omega_L = \omega_0$ (the light is exactly on resonance):

$$H_R = \frac{\hbar\Omega}{2}\sigma_x$$

This is just rotation around the **x-axis** at the Rabi frequency $\Omega$. 

The state oscillates between $|{\uparrow}\rangle$ and $|{\downarrow}\rangle$ — these are Rabi oscillations!

### Off Resonance ($\Delta \neq 0$)

When $\omega_L \neq \omega_0$:

$$H_R = \frac{\hbar\Delta}{2}\sigma_z + \frac{\hbar\Omega}{2}\sigma_x$$

The effective "field" points in a direction between z and x. The state precesses around this tilted axis.

If $|\Delta| \gg \Omega$: The field is mostly along z, so mostly precession (phase accumulation).

If $\Omega \gg |\Delta|$: The field is mostly along x, so mostly Rabi oscillations.

---

## iClicker Question 1

**In the rotating frame, what does the detuning $\Delta = \omega_0 - \omega_L$ represent?**

- (A) The Rabi frequency
- (B) The precession frequency around the z-axis ✓
- (C) The total energy of the atom
- (D) The intensity of the light

**Answer:** (B). In the rotating frame, $H_R = \frac{\hbar\Delta}{2}\sigma_z + \frac{\hbar\Omega}{2}\sigma_x$. The $\sigma_z$ term causes precession around z at frequency $\Delta$. When on resonance ($\Delta = 0$), there's no precession — only Rabi oscillations from the $\sigma_x$ term.

---

## Part 8: Ramsey Interferometry for Atomic Clocks

Now we can understand exactly how an atomic clock works!

### The Goal

We have a local oscillator (a microwave source) at frequency $\omega_L$. We want to lock $\omega_L$ to the atomic frequency $\omega_0$.

### The Ramsey Sequence in the Rotating Frame

1. **Initialize** in $|{\downarrow}\rangle$ (ground state)

2. **π/2-pulse** (short, strong pulse with $\Omega \gg \Delta$):
   - During the pulse, $H_R \approx \frac{\hbar\Omega}{2}\sigma_x$
   - Apply for time $t_\pi = \pi/(2\Omega)$
   - Result: $|{\downarrow}\rangle \to \frac{1}{\sqrt{2}}(|{\downarrow}\rangle - i|{\uparrow}\rangle)$
   - State is now on the equator of the Bloch sphere

3. **Free evolution for time $T$** (light off, so $\Omega = 0$):
   - $H_R = \frac{\hbar\Delta}{2}\sigma_z$
   - The state precesses around z at frequency $\Delta$
   - Accumulates phase $\phi = \Delta \cdot T$

4. **Second π/2-pulse**:
   - Converts the accumulated phase into population difference

5. **Measure**:
   - $P_{\downarrow} = \sin^2(\Delta T/2)$

### The Feedback Loop

If $\Delta = 0$ (we're on resonance): No precession during free evolution → $P_{\downarrow} = 0$ (all atoms return to ground state).

If $\Delta \neq 0$ (we're off resonance): Precession causes $P_{\downarrow} \neq 0$.

**The clock algorithm:**
1. Run Ramsey sequence
2. Measure $P_{\downarrow}$
3. If $P_{\downarrow} > 0$, adjust $\omega_L$ to reduce $|\Delta|$
4. Repeat

The feedback loop locks $\omega_L$ to $\omega_0$. Now our microwave oscillator ticks at exactly the cesium frequency!

### Why This Works So Well

- We're matching our microwave to cesium atoms
- Every cesium atom is **exactly identical** (quantum mechanics guarantees this)
- The longer the free evolution time $T$, the more sensitive we are to small detunings
- This is a universal, reproducible standard

---

## iClicker Question 2

**In a Ramsey sequence for an atomic clock, if the measured probability $P_{\downarrow} = 0$, this means:**

- (A) The local oscillator frequency $\omega_L$ equals the atomic frequency $\omega_0$ ✓
- (B) The atoms have all decayed
- (C) The π/2-pulses failed
- (D) The free evolution time was too short

**Answer:** (A). When $\omega_L = \omega_0$, the detuning $\Delta = 0$, so there's no precession during free evolution. The state stays where the first π/2-pulse put it, and the second π/2-pulse returns it exactly to $|{\downarrow}\rangle$. Zero population in the excited state means we're on resonance.

---

## Part 9: Clock Precision

### How Accurate Can This Be?

The frequency resolution of the Ramsey sequence is:

$$\delta\omega \sim \frac{1}{T}$$

Longer free evolution time $T$ → better precision.

In a cesium fountain clock:
- Atoms are laser-cooled to microkelvin temperatures
- Launched upward in a "fountain"
- Free-fall time $T \approx 0.5$ seconds
- This gives $\delta\nu \sim 2$ Hz

For the cesium transition at $\nu_0 = 9.2$ GHz:
$$\frac{\delta\nu}{\nu_0} \sim \frac{2}{9.2 \times 10^9} \sim 2 \times 10^{-10}$$

After averaging many measurements:

**NIST-F2 (Boulder, Colorado):** Fractional accuracy $\sim 10^{-16}$

This means: gains or loses **1 second in 300 million years**.

### Optical Clocks: The Future

The fractional precision scales as $1/(\nu_0 T)$. Higher frequency → better precision!

**Optical clocks** use transitions in the visible/UV range:
- Strontium: $\nu_0 = 429$ THz (vs 9.2 GHz for cesium)
- Factor of ~50,000 higher frequency!

Current optical clock precision: $\sim 10^{-18}$

This means: gains or loses **1 second in the age of the universe**.

These clocks are so precise they can detect:
- Gravitational redshift from 1 cm height difference
- Your clock runs faster when you stand up!

**In your lifetime**, the definition of the second will probably switch from cesium to an optical transition.

---

## Part 10: Decoherence — The Enemy

So far, we've assumed perfect, isolated atoms. Reality is messier.

### What Goes Wrong?

As the atom precesses during free evolution, the environment can randomly kick its phase.

Imagine running the Ramsey experiment many times:
- Ideally: every run accumulates the same phase $\phi = \Delta T$
- Reality: each run sees a slightly different phase $\phi + \delta\phi_{random}$

When we average over many runs:
- If all runs agree: clear oscillation pattern
- If runs have random phases: oscillations wash out

### Watching Decoherence Happen

Consider the Ramsey fringes as a function of free evolution time $T$:

At short $T$: Clear oscillations with full contrast.

At long $T$: Random phase kicks accumulate → contrast decreases → eventually just flat at $P = 0.5$.

This decay of oscillation amplitude is **dephasing** or **decoherence**.

```python
# Simulating decoherence in Ramsey fringes
import numpy as np
import matplotlib.pyplot as plt

def ramsey_with_dephasing(phi, noise_strength, n_trials=200):
    """Average Ramsey signal with random phase noise."""
    results = []
    for _ in range(n_trials):
        random_phase = np.random.normal(0, noise_strength)
        results.append(np.sin((phi + random_phase)/2)**2)
    return np.mean(results)

# Plot for different noise strengths
phi = np.linspace(0, 4*np.pi, 100)
for noise in [0, 0.5, 1.0, 2.0]:
    P = [ramsey_with_dephasing(p, noise) for p in phi]
    plt.plot(phi/np.pi, P, label=f'noise = {noise}')
plt.xlabel('Phase φ/π')
plt.ylabel('P₀')
plt.legend()
plt.title('Ramsey Fringes with Dephasing')
plt.show()
```

---

## Part 11: Physical Sources of Decoherence

Where does this random phase noise come from?

### Example: NV Centers in Diamond

An **NV center** (nitrogen-vacancy center) is a defect in diamond that acts as a qubit. It's an electron spin that we can control with microwaves and read out optically.

Sounds great! But there's a problem...

**The diamond crystal contains other spins:**
- Carbon-13 nuclei (1% natural abundance) have nuclear spin
- Other nitrogen impurities have electron spins
- These are all tiny bar magnets, randomly oriented, fluctuating

The NV center spin feels the magnetic field from all these neighbors. As the neighbors fluctuate, the field at the NV center changes randomly.

Remember: $H = -\gamma \mathbf{S} \cdot \mathbf{B}$

Random field → random precession frequency → random phase accumulation.

This is exactly the dephasing we described!

### Other Systems

| System | Main noise source |
|--------|-------------------|
| Neutral atoms | Stray magnetic fields |
| Superconducting qubits | Flux noise, charge noise |
| Trapped ions | Magnetic field fluctuations |
| Quantum dots | Nuclear spin bath, charge fluctuations |

Every qubit platform has its own dominant noise sources, but the effect is the same: **random phase kicks**.

---

## Part 12: T₁ and T₂ Times

We characterize decoherence with two timescales:

### T₂: Dephasing Time

**What it measures:** How long until the phase information is lost.

**Physical picture:** Random rotations around the z-axis scramble the phase.

**Bloch sphere:** States on the equator spread out into a ring, then average to the center.

$$\text{Fringe contrast} \sim e^{-T/T_2}$$

### T₁: Relaxation Time

**What it measures:** How long until the population decays.

**Physical picture:** The qubit randomly jumps between states, usually decaying to the ground state (lower energy).

**Bloch sphere:** All states drift toward the north pole (ground state).

**Physical origin:** Energy exchange with environment — emit a photon, create a phonon, etc.

### The Relationship

$$T_2 \leq 2T_1$$

Why? $T_1$ processes also cause dephasing (if the state changes, the phase information is lost). But you can have pure dephasing without energy decay.

### Typical Values

| System | $T_1$ | $T_2$ |
|--------|-------|-------|
| Trapped ions | minutes | seconds |
| Neutral atoms | seconds | ~1 second |
| Superconducting qubits | ~100 μs | ~100 μs |
| NV centers | ms | μs – ms |
| Quantum dots | ns – μs | ns |

---

## iClicker Question 3

**A qubit has $T_1 = 1$ ms and $T_2 = 100$ μs. Which statement is correct?**

- (A) This is impossible
- (B) Dephasing is the dominant source of error ✓
- (C) Energy relaxation is the dominant source of error
- (D) The qubit is perfectly isolated

**Answer:** (B). Having $T_2 \ll T_1$ (but still $T_2 < 2T_1$) means pure dephasing dominates over relaxation. The qubit loses its phase information long before it loses its energy. This is common in solid-state systems where magnetic field fluctuations cause rapid dephasing.

---

## Part 13: Why Quantum Computing is Hard

Now we get to the heart of the matter.

### Quantum Computing = Interference Machine

A quantum computer works by:
1. Preparing a superposition of many computational states
2. Letting them evolve and **interfere** with each other
3. Measuring the result

The interference is controlled by **phases**. Different computational paths accumulate different phases, and they add up (constructive interference) or cancel out (destructive interference).

**Phase is everything!**

### The Problem

Any interaction with the environment scrambles the phases.

Even the tiniest coupling to the outside world — a stray magnetic field, a thermal photon, a vibrating atom — can kick the phase randomly.

Once the phases are scrambled, the interference pattern is destroyed. The quantum computation fails.

**Dephasing is the enemy of quantum computing.**

### The Fundamental Dichotomy

Here's what makes quantum computing so hard:

**We need:** Qubits that are **perfectly isolated** from their environment, so their phases stay coherent.

**But we also need:** Qubits that **interact strongly** with each other, so we can do two-qubit gates and create entanglement.

These requirements seem contradictory!

We want: 
- No coupling to the environment (thermal bath, stray fields, etc.)
- Strong coupling to other qubits
- Strong coupling to our control fields (so we can do gates)

**Perfectly isolated... except for precisely controlled interactions.**

This is incredibly difficult to achieve. It's why, despite decades of effort, we still don't have large-scale fault-tolerant quantum computers.

### The Engineering Challenge

Every quantum computing platform is trying to solve this puzzle:

- **Superconducting qubits:** Operate at 10 mK to freeze out thermal noise, use microwave shielding, but still suffer from materials defects
- **Trapped ions:** Levitate ions in vacuum to isolate them, but electrode noise still causes problems
- **Neutral atoms:** Use optical tweezers in ultra-high vacuum, but stray fields still cause dephasing
- **NV centers:** Embedded in solid diamond, surrounded by fluctuating spins

Each platform makes different tradeoffs between isolation and control.

---

## Summary

### Atomic Clocks

- The second is defined by the cesium hyperfine transition: $\nu_0 = 9,192,631,770$ Hz
- Atomic clocks use Ramsey interferometry to lock a local oscillator to this frequency
- Every cesium atom is identical → universal, reproducible standard
- Current precision: $10^{-16}$ (cesium), $10^{-18}$ (optical)

### Atom-Light Interactions

- Full Hamiltonian: $H = \frac{\hbar\omega_0}{2}\sigma_z + \hbar\Omega\cos(\omega_L t)\sigma_x$
- Rotating frame trick: $H_R = \frac{\hbar\Delta}{2}\sigma_z + \frac{\hbar\Omega}{2}\sigma_x$
- Time-dependent → time-independent!
- Detuning $\Delta = \omega_0 - \omega_L$ controls precession rate

### Decoherence

- Environment causes random phase kicks → fringes wash out
- $T_2$: dephasing time (phase information lost)
- $T_1$: relaxation time (population decays)
- Dephasing is the enemy of quantum computing!

### The Quantum Computing Challenge

$$\text{Need: } \underbrace{\text{perfect isolation}}_{\text{from environment}} + \underbrace{\text{strong coupling}}_{\text{between qubits}}$$

This dichotomy is why quantum computing is so hard.

---

## Looking Ahead

We've completed the single-qubit story. We understand:
- States (Bloch sphere)
- Gates (rotations)
- Evolution (Hamiltonians, rotating frame)
- Measurement
- Noise ($T_1$, $T_2$)

**Next lecture:** We expand our quantum computer to **two qubits**.

This is where things get really interesting:
- **Entanglement:** "Spooky action at a distance"
- **Two-qubit gates:** How do we create controlled interactions?
- **Bell states:** Maximally entangled qubit pairs
- **Why quantum computers can outperform classical ones**

How do we create the precisely controlled interactions we need, while maintaining isolation from the environment? That's the central challenge we'll explore.

---

## Homework 2.7

### Problem 1: The Rotating Frame Transformation

This problem guides you through the rotating frame transformation — the key trick that turns a time-dependent Hamiltonian into a time-independent one.

**Setup:** We have a two-level atom with energy splitting $\hbar\omega_0$ driven by a light field oscillating at frequency $\omega_L$. The lab-frame Hamiltonian is:

$$H = \frac{\hbar\omega_0}{2}\sigma_z + \hbar\Omega\cos(\omega_L t)\sigma_x$$

Our goal is to transform to a rotating frame where this becomes time-independent.

---

**(a)** First, let's understand the problem. Write $\cos(\omega_L t)$ using Euler's formula:
$$\cos(\omega_L t) = \frac{1}{2}(e^{i\omega_L t} + e^{-i\omega_L t})$$

The $e^{+i\omega_L t}$ and $e^{-i\omega_L t}$ terms oscillate at frequency $\omega_L$. This time dependence makes the Schrödinger equation hard to solve.

---

**(b)** We define the **rotating frame transformation**:
$$U_R(t) = e^{i\omega_L t \sigma_z/2}$$

This is a rotation around the z-axis that accumulates angle $\omega_L t$ over time — it "spins" at the light frequency.

Compute $U_R(t)$ explicitly as a $2\times 2$ matrix. 

*Hint:* Use $e^{i\theta\sigma_z/2} = \cos(\theta/2)I + i\sin(\theta/2)\sigma_z = \begin{pmatrix} e^{i\theta/2} & 0 \\ 0 & e^{-i\theta/2} \end{pmatrix}$

---

**(c)** We define the rotating frame state as:
$$|\tilde{\psi}(t)\rangle = U_R(t)|\psi(t)\rangle$$

Take the time derivative of both sides. Show that:
$$i\hbar\frac{d|\tilde{\psi}\rangle}{dt} = \left[U_R H U_R^\dagger - i\hbar U_R \frac{dU_R^\dagger}{dt}\right]|\tilde{\psi}\rangle$$

*Hint:* Use the product rule: $\frac{d}{dt}(U_R|\psi\rangle) = \frac{dU_R}{dt}|\psi\rangle + U_R\frac{d|\psi\rangle}{dt}$, and substitute the Schrödinger equation for $\frac{d|\psi\rangle}{dt}$.

---

**(d)** Compute the second term. Show that:
$$-i\hbar U_R \frac{dU_R^\dagger}{dt} = -\frac{\hbar\omega_L}{2}\sigma_z$$

*Hint:* First find $\frac{dU_R}{dt}$, then $\frac{dU_R^\dagger}{dt}$, then put it together.

---

**(e)** Now we need to transform $H$ to the rotating frame: $U_R H U_R^\dagger$.

The $\sigma_z$ term is easy since $\sigma_z$ commutes with $U_R = e^{i\omega_L t \sigma_z/2}$:
$$U_R \sigma_z U_R^\dagger = \sigma_z$$

For the $\sigma_x$ term, use the identity:
$$e^{i\theta\sigma_z/2}\sigma_x e^{-i\theta\sigma_z/2} = \cos(\theta)\sigma_x + \sin(\theta)\sigma_y$$

Show that:
$$U_R \sigma_x U_R^\dagger = \cos(\omega_L t)\sigma_x + \sin(\omega_L t)\sigma_y$$

---

**(f)** Put it together. The driving term $\hbar\Omega\cos(\omega_L t)\sigma_x$ transforms to:
$$\hbar\Omega\cos(\omega_L t)\left[\cos(\omega_L t)\sigma_x + \sin(\omega_L t)\sigma_y\right]$$

Expand this using $\cos^2(\omega_L t) = \frac{1}{2}(1 + \cos(2\omega_L t))$ and $\cos(\omega_L t)\sin(\omega_L t) = \frac{1}{2}\sin(2\omega_L t)$.

You should get terms that are constant plus terms oscillating at $2\omega_L$.

---

**(g)** The **Rotating Wave Approximation (RWA)**: When $\omega_L$ is large (optical or microwave frequencies), the $2\omega_L$ terms oscillate so fast that they average to zero. We drop them.

After applying the RWA, show that the effective rotating-frame Hamiltonian is:
$$\boxed{\tilde{H} = \frac{\hbar\Delta}{2}\sigma_z + \frac{\hbar\Omega}{2}\sigma_x}$$

where $\Delta = \omega_0 - \omega_L$ is the **detuning**.

---

**(h)** Interpret your result:
- What happens when $\omega_L = \omega_0$ (on resonance)?
- What happens when $\Omega = 0$ (no driving)?
- What happens when $|\Delta| \gg \Omega$ (far off resonance)?

---

### Problem 2: Clock Precision

**(a)** A cesium fountain clock has free evolution time $T = 0.5$ s. What is the approximate frequency resolution $\delta\nu \sim 1/T$?

**(b)** The cesium frequency is $\nu_0 = 9.192631770$ GHz. What is the fractional precision $\delta\nu/\nu_0$ per single measurement?

**(c)** By averaging $N$ measurements, the precision improves by $\sqrt{N}$. How many measurements are needed to reach $10^{-16}$ fractional precision?

**(d)** An optical clock uses a transition at $\nu_0 = 429$ THz. For the same $T = 0.5$ s and same measurement noise, what fractional precision can it achieve?

**(e)** By what factor is the optical clock better than the cesium clock?

---

### Problem 3: Simulating Dephasing

This problem explores how random phase noise destroys coherent oscillations — the essence of $T_2$ decoherence.

---

**(a) Ideal Rabi Oscillations**

First, let's simulate ideal precession with no noise. A qubit precesses at frequency $f = 1$ Hz (so $\omega = 2\pi$ rad/s). 

Implement the sequence:
1. Start in $|0\rangle$
2. Apply $R_x(\pi/2)$ to create a superposition
3. Precess for time $t$ (this is $R_z(\omega t) = R_z(2\pi f t)$)
4. Apply $R_x(\pi/2)$ to convert phase to population
5. Compute $P_0 = |\langle 0|\psi\rangle|^2$

```python
import numpy as np
import matplotlib.pyplot as plt
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

def ramsey(t, freq=1.0):
    """
    Ramsey sequence: π/2 - precess - π/2
    t: total precession time (seconds)
    freq: precession frequency (Hz)
    """
    phase = 2 * np.pi * freq * t
    
    qc = QuantumCircuit(1)
    qc.rx(np.pi/2, 0)    # First π/2
    qc.rz(phase, 0)       # Precession
    qc.rx(np.pi/2, 0)    # Second π/2
    
    state = Statevector(qc)
    return state.probabilities()[0]  # P_0
```

Plot $P_0$ vs $t$ for $t \in [0, 10]$ seconds with ~200 points. You should see 10 complete oscillations. Verify that the result matches $P_0 = \sin^2(\pi f t)$.

---

**(b) Single Realization with Noise**

Now add dephasing. Every $\Delta t = 1$ second, the qubit receives a random phase kick drawn from a Gaussian distribution with standard deviation $\delta\phi$.

```python
def ramsey_noisy(t, freq=1.0, delta_phi=0.3, dt=1.0):
    """
    Ramsey sequence with random phase kicks.
    t: total precession time (seconds)
    freq: precession frequency (Hz)
    delta_phi: std dev of random phase kicks (radians)
    dt: time between kicks (seconds)
    """
    qc = QuantumCircuit(1)
    qc.rx(np.pi/2, 0)    # First π/2
    
    # Break precession into intervals, add noise after each
    n_intervals = int(t / dt)
    remaining_time = t - n_intervals * dt
    
    for _ in range(n_intervals):
        # Ideal precession for dt
        qc.rz(2 * np.pi * freq * dt, 0)
        # Random phase kick
        qc.rz(np.random.normal(0, delta_phi), 0)
    
    # Any remaining time (no kick at the end)
    if remaining_time > 0:
        qc.rz(2 * np.pi * freq * remaining_time, 0)
    
    qc.rx(np.pi/2, 0)    # Second π/2
    
    state = Statevector(qc)
    return state.probabilities()[0]
```

For $\delta\phi = 0.5$ rad, plot $P_0$ vs $t$ for **10 different realizations** on the same graph (use different colors or transparency). 

What do you observe? How do the different realizations compare to the ideal case?

---

**(c) Ensemble Average**

In a real experiment, we repeat the measurement many times and average. The random kicks are different each time, so we're averaging over an ensemble of noise realizations.

```python
def ramsey_averaged(t, freq=1.0, delta_phi=0.3, dt=1.0, n_trials=200):
    """Average P_0 over many noise realizations."""
    return np.mean([ramsey_noisy(t, freq, delta_phi, dt) for _ in range(n_trials)])
```

Plot the averaged $\langle P_0 \rangle$ vs $t$ for:
- `delta_phi = 0` (no noise — should match part a)
- `delta_phi = 0.3`
- `delta_phi = 0.5`
- `delta_phi = 1.0`

Use `n_trials = 300` for smooth curves.

---

**(d) Observing Decoherence**

You should see that:
- The oscillations persist, but their **amplitude decays** over time
- Larger $\delta\phi$ causes faster decay
- The average approaches $P_0 = 0.5$ (complete dephasing)

Describe what you observe. At roughly what time does the oscillation amplitude drop to half its initial value for each $\delta\phi$?

---

**(e) Connecting to $T_2$**

The coherence decay you're seeing is characterized by the **dephasing time $T_2$**. For our model, the contrast decays roughly as:

$$\text{Amplitude} \propto e^{-t/T_2}$$

where $T_2$ depends on the noise strength $\delta\phi$ and the kick interval $dt$.

From your plots in part (c), estimate $T_2$ for each value of $\delta\phi$ by finding the time when the oscillation amplitude drops to $\sim 37\%$ ($1/e$) of its initial value.

How does $T_2$ depend on $\delta\phi$? (You should find that stronger noise → shorter $T_2$.)

---

**(f) The Physics**

Explain in your own words:
1. Why do individual realizations (part b) show oscillations that drift out of phase with each other?
2. Why does averaging over many realizations (part c) cause the oscillation amplitude to decay?
3. Why is this called "dephasing" — what is being lost?
4. In a real atomic clock or qubit, what physical processes play the role of the random phase kicks?