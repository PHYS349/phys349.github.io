# Lecture 4: Quantum Hardware — Where Are We?

## Recap: What Is a Quantum Computer?

Last lecture, we established the core idea of quantum computing. Let's review.

A quantum computer is a machine that:

1. **Prepares an initial state** — typically all qubits in $|0\rangle$:
$$
|00\cdots0\rangle
$$

2. **Evolves through quantum gates** — unitary operations that cause the wavefunction to spread across configurations. After many gates, the system is in a superposition of all $2^N$ configurations:
$$
|\psi\rangle = c_{000}|000\rangle + c_{001}|001\rangle + c_{010}|010\rangle + \cdots
$$

3. **Exploits interference** — amplitudes flowing through different paths can interfere:
   - Same phase → constructive interference → high probability
   - Opposite phase → destructive interference → low probability

4. **Measures at the end** — the superposition collapses to one classical outcome, a string of 0s and 1s.

**The goal:** Design the gates so that wrong answers interfere destructively and the right answer interferes constructively. When you measure, you're likely to get what you want.

This all sounds great in principle. But there's a catch.

---

## Where Are We? The NISQ Era

In 2018, physicist John Preskill coined the term **NISQ**:

> **Noisy Intermediate-Scale Quantum**

This describes the current era of quantum computing:

- **Intermediate-scale:** We have tens to hundreds of qubits. Enough to do *something*, but not enough for the big algorithms like Shor's factoring.

- **Noisy:** The qubits are imperfect. Gate error rates are still too high for error correction to help—adding more qubits for redundancy just adds more noise.

We're past the "proof of concept" stage—quantum computers exist and work. But we're not yet at fault-tolerant, error-corrected quantum computing.

The central question of this lecture: **Why is it so hard, and what are we doing about it?**

---

## Classical Error Correction

Before we tackle quantum errors, let's remember how classical computers handle noise.

### Classical Computers Have Errors Too

You might think classical computers are perfect, but they're not:
- Cosmic rays can flip bits in memory
- Voltage fluctuations can cause errors
- Hard drives and SSDs have read/write errors

The difference is **where** errors occur and how often:

| Component | Error rate | Error correction? |
|-----------|------------|-------------------|
| CPU logic gates | $\sim 10^{-18}$ per operation | No — too rare to bother |
| RAM (DRAM) | $\sim 10^{-15}$ per bit per hour | Sometimes (ECC memory in servers) |
| Hard drives / SSDs | $\sim 10^{-12}$ per bit | Yes — heavy error correction |
| Data transmission | Varies | Yes — checksums detect errors, then retransmit |

The key insight: **CPU logic is essentially perfect**. We only need error correction for storage and transmission, where bits sit idle or travel through noisy channels.

### The Classical Solution: Redundancy

The simplest error correction is repetition. Instead of storing one bit, store three copies:

$$
0 \rightarrow 000, \qquad 1 \rightarrow 111
$$

If one bit flips due to noise:
$$
000 \xrightarrow{\text{error}} 010
$$

You detect the error (one bit disagrees with the others) and correct it by majority vote: two 0s and one 1 → the answer is 0.

This works because classical information can be:
1. **Copied** freely
2. **Measured** without changing it
3. **Compared** to find disagreements

These three properties let us detect and fix errors. But quantum mechanics breaks all of them.

---

## Why Quantum Is Different: The Phase Matters

In a classical computer, information is stored in definite states: the bit is 0 or 1.

In a quantum computer, information is stored in **superpositions**, and the **phase** between amplitudes is crucial.

### One Qubit

A single qubit can be in a superposition:
$$
|\psi\rangle = \frac{1}{\sqrt{2}}\left(|0\rangle + e^{i\theta}|1\rangle\right)
$$

The amplitudes have equal magnitude ($1/\sqrt{2}$ each), but the phase $\theta$ encodes information. Different values of $\theta$ represent different quantum states:
- $\theta = 0$: $|0\rangle + |1\rangle$
- $\theta = \pi$: $|0\rangle - |1\rangle$
- $\theta = \pi/2$: $|0\rangle + i|1\rangle$

These are all different states that behave differently under quantum gates. **The phase is information**—and it's exactly what enables interference. The phase determines whether amplitudes add (constructive) or cancel (destructive).

### N Qubits

For $N$ qubits, the state is a superposition over all $2^N$ configurations:
$$
|\psi\rangle = c_{000\cdots0}|000\cdots0\rangle + c_{000\cdots1}|000\cdots1\rangle + \cdots + c_{111\cdots1}|111\cdots1\rangle
$$

Each coefficient $c_i$ is a complex number with a magnitude and a phase. For interference to work correctly, **all $2^N$ phases must remain coherent**—they must maintain their precise relationships with each other.

If anything scrambles these phases, the interference patterns are destroyed, and the quantum computer becomes useless.

---

## Decoherence: The Enemy

Here is the central challenge of quantum computing:

> **Quantum systems are extremely sensitive to their environment.**

Any interaction with the outside world—a stray photon, a vibrating atom, a fluctuating magnetic field—can disturb the delicate phase relationships in a quantum state.

This is called **decoherence**: the loss of definite phase relationships between configurations.

### The Double-Slit Analogy

Remember the double-slit experiment. A particle goes through both slits simultaneously, and the two paths interfere to create a pattern on the screen.

```
                    │░░░░│
       ════╗        │    │        ░░░░░
           ║  slit  │    │      ░░    ░░
  source   ║========│    │    ░░        ░░
     •     ║        │    │    ░░        ░░
           ║========│    │      ░░    ░░
           ║  slit  │    │        ░░░░░
       ════╝        │░░░░│
                   barrier      interference
                               pattern on screen
```

The interference pattern exists because the particle takes both paths, and the amplitudes from each path combine. Where they're in phase, you get bright bands; where they're out of phase, you get dark bands.

But if you put a detector at one slit to measure "which path" the particle took:

```
                    │░░░░│
       ════╗        │    │        
           ║  slit  │    │      ░░░░░░░░
  source   ║===●====│    │      ░░░░░░░░  ← no pattern!
     •     ║   ↑    │    │      ░░░░░░░░    just blur
           ║detector│    │      ░░░░░░░░
           ║========│    │        
       ════╝  slit  │░░░░│
```

**The interference pattern vanishes.** The act of measurement destroys the superposition.

A photon scatters off your qubit and flies away into the room. That photon now carries information about your qubit's state. The universe has "measured" your qubit, even if no human ever looks at that photon.

The same thing happens in a quantum computer. If the environment "measures" your qubit—if any information about the qubit's state leaks out—the superposition is destroyed.

### Three Sources of Error

There are three main ways the environment can ruin your quantum computation:

**1. Measurement by the environment**

Something interacts with your qubit and carries away "which state" information.

Example: A photon scatters off your qubit. The photon's wavelength or direction now depends on the qubit's state. Even if no human looks at that photon, the information has leaked into the environment. The qubit is now entangled with the outside world, and from the qubit's perspective, the superposition has collapsed.

**2. Phase scrambling (pure dephasing)**

The relative phase between $|0\rangle$ and $|1\rangle$ gets randomized, but no information actually leaves the system.

Example: Your qubit sits in a magnetic field that fluctuates slightly over time. The energy difference between $|0\rangle$ and $|1\rangle$ shifts unpredictably, causing the phase to accumulate at a jittery rate. After a while, the phase has drifted to a random value.

No photon escaped. No "measurement" happened. But the phase is now garbage.

**3. Decay / relaxation**

The qubit loses energy to the environment and physically transitions from $|1\rangle$ to $|0\rangle$ (or equilibrates thermally).

Example: An excited atom spontaneously emits a photon and drops to the ground state. The qubit that was in a superposition is now definitely in $|0\rangle$.

### All Three Destroy Interference

| Error type | Information leaves? | Population changes? | Effect |
|------------|--------------------|--------------------|--------|
| Environment measurement | Yes | Maybe | Collapse |
| Phase scrambling | No | No | Random phase |
| Decay | Yes | Yes | State resets |

Despite their differences, all three have the same consequence: **the interference patterns are scrambled, and the quantum computation fails.**

### Timescales

Physicists characterize these errors with two timescales:

- **$T_1$** (relaxation time): How long before $|1\rangle$ decays to $|0\rangle$
- **$T_2$** (coherence time): How long before the phase becomes randomized

Typically $T_2 \leq 2T_1$ (you can't maintain phase coherence if the state has already decayed).

For superconducting qubits, $T_2 \sim 100$ microseconds. A single gate takes ~20 nanoseconds. That means you can do roughly **1000–5000 gates** before decoherence kills your computation.

The race is on: **you must finish your computation before decoherence destroys your quantum state.**

---

## The Quantum Error Correction Problem

Given that errors are inevitable, can we correct them like we do classically?

### Why Classical Strategies Fail

The classical approach—copy the data, check for errors, fix by majority vote—fails for two fundamental reasons:

**1. The No-Cloning Theorem**

In quantum mechanics, you cannot copy an unknown quantum state. There is no machine that takes $|\psi\rangle$ as input and produces $|\psi\rangle \otimes |\psi\rangle$ as output.

This isn't a technological limitation—it's a theorem. If you could clone quantum states, you could violate fundamental principles like the uncertainty principle and no faster-than-light communication.

So we can't just make three copies of our qubit and do majority vote.

**2. Measurement Destroys Superposition**

To check if a classical bit has flipped, you just look at it. Looking at a bit doesn't change it.

But measuring a qubit in superposition collapses it to $|0\rangle$ or $|1\rangle$. The very act of "checking for errors" destroys the quantum information you're trying to protect.

### The Error Rate Problem

Current quantum gates have error rates of about **1% per operation** (or $\sim 10^{-2}$ to $10^{-3}$).

A useful quantum algorithm might require millions of gates. If each gate has a 1% error rate, errors compound rapidly and you have essentially no chance of getting the right answer.

### The Solution: Logical Qubits

Despite these obstacles, quantum error correction is possible. The breakthrough, developed in the 1990s:

Encode one **logical qubit** across many **physical qubits** in such a way that you can detect errors without measuring the encoded information.

> **Physical qubit:** A single hardware device—one trapped ion, one superconducting circuit, one atom.

> **Logical qubit:** An error-corrected qubit encoded across many physical qubits.

How does this avoid the measurement problem? The trick is to measure *correlations* between physical qubits (called "syndromes") rather than the qubits themselves. These measurements reveal whether an error occurred and where, without revealing the encoded quantum information.

The overhead is substantial. Current estimates suggest:

$$
\sim 1000 \text{ physical qubits per logical qubit}
$$

There's a crucial threshold: if your physical qubit error rate is below ~1%, adding more physical qubits makes the logical qubit *better*, not worse. Above threshold, more qubits just means more noise. Below threshold, more qubits means more protection.

We're currently right at this threshold—which is why improving gate fidelity is the central challenge.

---



### The Solution: Logical Qubits

Despite these obstacles, quantum error correction is possible. The breakthrough, developed in the 1990s:

Encode one **logical qubit** across many **physical qubits** in such a way that you can detect errors without measuring the encoded information.

> **Physical qubit:** A single hardware device—one trapped ion, one superconducting circuit, one atom.

> **Logical qubit:** An error-corrected qubit encoded across many physical qubits.

How does this avoid the measurement problem? The trick is to measure *correlations* between physical qubits (called "syndromes") rather than the qubits themselves. These measurements reveal whether an error occurred and where, without revealing the encoded quantum information.

The overhead is substantial. Current estimates suggest:

$$
\sim 1000 \text{ physical qubits per logical qubit}
$$

There's a crucial threshold: if your physical qubit error rate is below ~1%, adding more physical qubits makes the logical qubit *better*, not worse. Above threshold, more qubits just means more noise. Below threshold, more qubits means more protection.

We're currently right at this threshold—which is why improving gate fidelity is the central challenge.

---

## Quantum Hardware Platforms

There are several competing approaches to building physical qubits. Each has different strengths and weaknesses. No one knows which will "win"—they may coexist for different applications.

### Superconducting Qubits

**Companies:** IBM, Google, Rigetti, IQM

**How it works:**
- Tiny circuits made of superconducting metal (usually aluminum) on a silicon chip
- The **transmon** qubit: a capacitor in parallel with a **Josephson junction** (a thin insulating barrier between superconductors)
- The Josephson junction provides nonlinearity, creating an **anharmonic oscillator**—the energy levels are not evenly spaced, so you can address just the lowest two levels as $|0\rangle$ and $|1\rangle$
- The qubit states correspond to different numbers of Cooper pairs (paired electrons) oscillating across the junction
- Cooled to ~15 millikelvin in a dilution refrigerator (colder than outer space!)

**Gates:**
- **Single-qubit gates:** Microwave pulses at the qubit's resonant frequency (~5 GHz) rotate the state on the Bloch sphere. Gate time ~20 ns.
- **Two-qubit gates:** Neighboring qubits are coupled through a resonator or tunable coupler. Turning on the coupling allows energy exchange, creating entanglement. Gate time ~50-200 ns.

**State of the art (2025-2026):**
- Single-qubit gate fidelity: ~99.9%
- Two-qubit gate fidelity: ~99.5% (median), up to ~99.9% on best qubit pairs
- Coherence time $T_2$: ~100-600 μs
- Largest systems: IBM has 1000+ physical qubits; Google Willow has 105 high-quality qubits
- Google demonstrated error correction below threshold (logical qubits improve as code distance increases)

**Advantages:**
- Very fast gates (~20-100 ns)
- Fabricated using standard lithography—leverages semiconductor industry
- Flexible chip design

**Disadvantages:**
- Requires extreme cooling (dilution refrigerators are expensive and complex)
- Each qubit is slightly different due to fabrication variation
- Relatively short coherence times
- Nearest-neighbor connectivity on chip (long-range coupling is harder)

---

### Trapped Ions

**Companies:** IonQ, Quantinuum, Oxford Ionics (now part of IonQ)

**How it works:**
- Individual ions (charged atoms, typically ytterbium Yb⁺ or barium Ba⁺) suspended in vacuum using oscillating electric fields (Paul trap or linear trap)
- Each ion is a qubit—two internal energy levels (hyperfine or optical states) encode $|0\rangle$ and $|1\rangle$
- Ions are laser-cooled to near absolute zero
- All ions of the same species are **identical**—no fabrication variation

**Gates:**
- **Single-qubit gates:** Focused laser or microwave pulses drive transitions between $|0\rangle$ and $|1\rangle$. Fidelity >99.99%.
- **Two-qubit gates:** Ions interact through their **shared motion** (phonons). A laser pulse entangles the ions' internal states with the collective vibrational mode of the ion chain, then disentangles the motion, leaving the spins entangled (Mølmer-Sørensen gate). Gate time ~10-100 μs.

**State of the art (2025-2026):**
- Single-qubit gate fidelity: >99.99%
- Two-qubit gate fidelity: **99.99%** (IonQ, October 2025—world record)
- Quantinuum Helios: 98 qubits, 99.92% two-qubit fidelity
- Coherence times: seconds to minutes (!)
- All-to-all connectivity (any qubit can interact with any other by shuttling ions)

**Advantages:**
- Highest gate fidelities of any platform
- Very long coherence times
- Every qubit is identical
- Flexible connectivity (can rearrange ions)

**Disadvantages:**
- Slow gate times compared to superconducting (~1000× slower)
- Difficult to scale beyond ~100 ions in a single trap
- Complex laser systems and vacuum requirements
- Ion shuttling takes time

---

### Neutral Atoms

**Companies:** QuEra, Pasqal, Atom Computing

**How it works:**
- Individual neutral atoms (rubidium, cesium, or ytterbium) held in place by tightly focused laser beams called **optical tweezers**
- Qubits encoded in hyperfine ground states (long-lived) or nuclear spin states
- Atoms can be arranged in arbitrary 2D or 3D arrays and **rearranged dynamically**
- Atoms don't interact in their ground states—they're independent until you want them to interact

**Gates:**
- **Single-qubit gates:** Raman laser pulses or microwave pulses drive transitions. Fidelity ~99.97%.
- **Two-qubit gates:** Excite atoms to **Rydberg states**—highly excited states where the electron is far from the nucleus, giving the atom a huge electric dipole moment. Two nearby Rydberg atoms interact strongly via van der Waals forces. The **Rydberg blockade** prevents both atoms from being excited simultaneously, enabling controlled-Z gates. Gate time ~1 μs.

**State of the art (2025-2026):**
- Single-qubit gate fidelity: >99.9%
- Two-qubit gate fidelity: ~99.5% (demonstrated on 60 atoms in parallel)
- **Largest arrays: 6,100 qubits** (Caltech, September 2025)—far more than any other platform
- Coherence times: ~1-10 seconds in ground states; Rydberg states are short-lived (~100 μs)
- Demonstrated error correction and logical qubit operations

**Advantages:**
- Massive scalability—can trap thousands of atoms with optical tweezers
- Flexible, reconfigurable arrays (atoms can be moved)
- All atoms are identical
- No cryogenics required (operates at room temperature with laser cooling)
- Native multi-qubit gates possible via Rydberg blockade

**Disadvantages:**
- Rydberg states are short-lived—must do gates quickly
- Atom loss during computation (atoms can escape tweezers)
- Two-qubit fidelity still behind trapped ions
- Technology is newer and less mature

---

### Summary Table

| Platform | Qubits (2025) | Gate speed | $T_2$ | 2Q fidelity | Key advantage |
|----------|---------------|------------|-------|-------------|---------------|
| Superconducting | ~100-1000 | ~50 ns | ~100-600 μs | ~99.5% | Fast gates, scalable fab |
| Trapped ions | ~50-100 | ~10-100 μs | seconds-minutes | **99.99%** | Highest fidelity |
| Neutral atoms | **~1000-6000** | ~1 μs | ~1-10 s | ~99.5% | Massive scale, flexibility |

---

## What's Next?

We are in a pivotal moment. The key milestones:

**Where we are now:**
- NISQ era: tens to hundreds of noisy qubits
- Error correction demonstrated but not yet practical
- Gate fidelities approaching the ~99.9% threshold needed for useful error correction

**Near-term (2026-2028):**
- Systems with hundreds of high-quality physical qubits
- First demonstrations of useful logical qubits with real error suppression
- Hybrid classical-quantum algorithms for chemistry, optimization, machine learning

**Medium-term (2029-2035):**
- Fault-tolerant quantum computers with thousands of logical qubits
- Breaking RSA encryption becomes possible (with ~1 million physical qubits at current error rates)
- Quantum simulation of complex molecules for drug discovery and materials science

**The race:**
- Superconducting systems are betting on fast iteration and semiconductor-style scaling
- Trapped ions are betting on quality over quantity—fewer qubits but near-perfect operations
- Neutral atoms are betting on massive parallelism and flexibility

All three approaches have crossed important thresholds in the past two years. The question is no longer "can we build a quantum computer?" but "how soon can we build a *useful* one?"


