# Lecture 1: Why Quantum? Why Now?

## The Two Quantum Revolutions

If you are taking this course, you likely already know that "quantum" is a buzzword in industry and academia. But beyond the hype, there is a fundamental shift happening in how we interact with the physical world.

For the last seventy years, we have lived in the era of the **First Quantum Revolution**. This was the era where we discovered the laws of quantum mechanics and used them to build the modern world. The transistor in your phone, the laser in your fiber-optic cables, and the MRI machines in our hospitals all rely on quantum effects. However, in all of these technologies, we use quantum mechanics in bulk. A transistor works because of the collective behavior of trillions of electrons; a laser works because of the collective behavior of trillions of photons. We use quantum mechanics to understand the properties of materials, but the information itself—the "1" or "0"—is still stored in a very classical way: lots of charge versus no charge.

We are now entering the **Second Quantum Revolution**.

In this era, we are moving from "understanding" to "control." We have stopped just observing quantum effects in bulk and started engineering them in individual particles. We can now trap a single ion in a vacuum, isolate a single photon, or control a single superconducting circuit. When you work with a single quantum particle, you can define two of its states—like an electron's spin being "up" or "down"—and call that a **qubit**.

This is where it gets interesting. In a classical bit, you are either in state 0 or state 1. In a qubit, because it is a single quantum particle, you can exist in a superposition. But more importantly, you can store information in the relative phase between those states. Imagine the "0" and "1" are like two waves. If they are in sync, they mean one thing; if they are out of sync, they mean something else. This gives us a new "knob" to turn that classical computers simply do not have.

In this section we will cover the history of quantum theory, how it lead to the first quantum revolution, and how we are entering a new age.
## The First Quantum Revolution

### End of the 19th Century: The Failures of Classical Theory

By the end of the 19th century, classical physics was extraordinarily successful. Maxwell’s equations described light, Newton’s laws described motion, and thermodynamics described heat. Many physicists believed the subject was nearly complete.

They were wrong.

At the turn of the 20th century, a series of experiments revealed fundamental failures of classical physics—failures that could not be fixed by better measurements or improved engineering. These failures forced a radical rethinking of what matter and light are.

This rethinking became **quantum theory**.

### 1900–1910: Energy Is Quantized

Classical physics treated energy as continuous. There was no smallest unit of energy—systems could exchange arbitrarily small amounts.  This assumption led to direct contradictions with experiment.

Two famous examples:
- **The ultraviolet catastrophe**: classical theory predicted that hot objects should emit infinite energy at high frequencies.   
- **The photoelectric effect**: shining light on a metal ejects electrons, but only if the light’s frequency is above a threshold—intensity alone is not enough.

Both results were impossible to explain classically.
Between 1900 and 1905, Max Planck and Albert Einstein independently introduced a revolutionary idea:
> **Energy is not continuous. It comes in discrete packets.**
Planck proposed that electromagnetic radiation can only exchange energy in multiples of  
$$
E = \hbar \omega,  
$$
where $\omega$ is the light’s frequency.
Einstein took this idea seriously as a statement about light itself. He proposed that light consists of particles, later called **photons**, each carrying this energy.   This immediately explained the photoelectric effect:
- Light below a threshold frequency cannot eject electrons, no matter how intense it is.
   - Above the threshold, increasing intensity increases the _number_ of ejected electrons, not their energy.

### 1920-1930: Matter Is a Wave

If light—long thought to be a wave—sometimes behaves like a particle, then a natural question arises:
> **Could matter—long thought to be particles—behave like a wave?**
The answer, unexpectedly, was yes.

Louis de Broglie proposed that particles have an associated wavelength:  
$$  
p = \frac{h}{\lambda},  
$$
where $p$ is momentum and $\lambda$ is wavelength. This was not philosophical speculation. It was experimentally confirmed through electron diffraction, showing that electrons produce interference patterns just like waves.

In 1926, Erwin Schrödinger introduced a wave equation describing how matter waves evolve in time:  
$$
i\hbar \frac{\partial}{\partial t}\Psi(x,t) = - \frac{\hbar^2}{2m} \frac{d^2}{dx^2} \Psi(x,t).  
$$
This equation does not describe a particle following a trajectory. Instead, it describes a **wavefunction** $\Psi(x,t)$, which encodes the amplitudes for every possible position or outcomes.


Key consequences:
- Particles do not have definite positions and momenta simultaneously.
- Bound systems (atoms, molecules) have discrete energy levels.
- Interference and superposition are fundamental features of matter.


Werner Heisenberg showed that wave-like behavior implies a strict limit on what can be known:  
$$
\Delta x \Delta p \ge \hbar.  
$$

This is not a technological limitation.   It is not about imperfect instruments. It is a statement about nature:

> **Certain pairs of physical properties cannot simultaneously have precise values.**

Classical determinism fails at a fundamental level.

By the end of the 1920s, classical physics had been replaced at a fundamental level.

The new picture was radical:

- Energy is **quantized**
- Light can behave like **particles**
- Matter can behave like **waves**
- Physical systems exist in **superpositions**
- Measurement outcomes are **probabilistic**
- Certain quantities are **fundamentally uncertain**
    
Quantum mechanics was not a correction to classical physics.  
It was a new framework for describing reality.

What remained was the question:

> **How do these strange rules explain the ordinary, solid, classical world we live in?**

That question would drive the next phase of the First Quantum Revolution and lead to a technological revolution: the transistor and classical computing. 

### 1930s–40s: From single particles to chemistry and materials

By the early 1930s, quantum theory was no longer a speculative framework—it was a working theory.  
Particles are waves. States are quantized. Measurement is probabilistic.

Now came the natural question:

> **What can we actually _do_ with this theory?**

The answer, discovered rapidly over the next two decades, was: almost everything around us.

During the 1930s and 1940s, physicists learned how to scale quantum mechanics upward—from single particles to atoms, molecules, and eventually to bulk materials. We had scaled down to understand electrons and photons; now we began to build back up, asking:

> **What happens when ~10²³ quantum particles interact?**

The result was a profound shift: quantum mechanics became the foundation of chemistry, materials science, and solid-state physics.
#### From atoms to molecules
One of the first successes of quantum theory was a complete understanding of atomic structure.
Quantum mechanics explained:
- Atomic orbitals as standing-wave solutions of the Schrödinger equation
- The structure of the periodic table
- Why atoms have distinct chemical properties
- The Pauli exclusion principle, which governs how electrons fill orbitals
    
Crucially, these ideas extended naturally to molecules. Chemical bonds were no longer empirical rules—they emerged from:
- Wavefunction overlap
- Energy minimization
- Electron statistics

Chemistry became, at its core, an application of quantum mechanics.
#### From molecules to materials
The next step was to apply quantum mechanics not just to a few atoms, but to large collections arranged in regular patterns.

This led to the development of band theory, which describes electrons moving in periodic lattices.
Key ideas:
- Electrons experience periodic potentials in crystals
- Discrete atomic energy levels broaden into energy bands
- Allowed and forbidden energies naturally emerge

From this single framework came a classification of materials:
- **Conductors**: partially filled bands
- **Insulators**: filled valence band with a large energy gap
- **Semiconductors**: small band gap that can be engineered

Properties that once seemed unrelated—electrical conductivity, optical response, heat capacity—were unified under the same quantum description.

Quantum mechanics also introduced a fundamentally new degree of freedom: **spin**.
Spin, combined with the Pauli exclusion principle and electron interactions, explained:
- Magnetism
- Ferromagnetic and antiferromagnetic order
- Magnetic materials and spin-dependent effects 

Once again, collective quantum behavior—not individual particles—was the key.

![[Pasted image 20260108094107.png]]
### Quantum-enabled technologies (1940s–1980s)
With the foundations in place, the mid-20th century marks a decisive shift:  
quantum mechanics moves from explanation to engineering.

> **Quantum mechanics becomes a practical tool for designing materials, devices, and electronics.**

Crucially, these technologies exploit quantum effects collectively.  
They are quantum at the microscopic level—but the information they process is classical.

#### 1940s–50s: Semiconductors and the transistor

The invention of the transistor (1947) marks the beginning of the modern technological era.
The transistor is inherently quantum:
- Band structure determines conductivity
- Tunneling and carrier statistics enable switching
- Doping engineers energy landscapes at the atomic scale

And yet, the information it stores is classical:
- High voltage vs low voltage
- Current on vs current off
-  or 1, encoded redundantly across trillions of electrons

This distinction matters.  The computing revolution—and Moore’s Law—was driven not by new physics, but by the miniaturization of quantum-engineered devices:

- Early transistors: centimeter scale
- Modern transistors: a few nanometers

Similarly, magnetic memory exploits quantum spin:
- Individual electron spins are quantum
- But memory states are encoded in macroscopic alignment of many spins
- Again: quantum physics, classical information

Figure for transitor

#### 1950s–60s: Superconductivity and macroscopic quantum states

Superconductivity revealed a striking new possibility:  
**quantum mechanics operating coherently at macroscopic scales**.

At sufficiently low temperatures:
- Electrons form correlated pairs (Cooper pairs)
- Electrical resistance vanishes
- Magnetic fields are expelled

The system behaves as if it were a single quantum state—yet it still involves an enormous number of particles.  
This is a recurring theme of the First Quantum Revolution:

> Collective quantum behavior without individual quantum control.

Superconductors would later become essential to:
- MRI
- Precision sensors
- And eventually, quantum computing hardware itself

Figure for superconducting. 

#### 1960s: Lasers and quantum coherence

The invention of the laser (1960) introduced a new kind of quantum technology.
Lasers rely on:
- Stimulated emission (predicted by Einstein in 1917)
- Population inversion
- Phase coherence across many photons

A laser produces a macroscopic quantum state
- Massive numbers of photons
- Occupying the same quantum mode
- Acting in perfect synchrony
    
Yet once again:
- The output is used classically
- Information is encoded in intensity, frequency, or modulation—not in fragile quantum superpositions
    
Figure for laser. 

### The End of the First Quantum Revolution
By the late 20th century, quantum mechanics underpinned essentially all modern technology.  
Electronics, computing, communication, sensing, and materials science all relied on quantum principles.

And yet, quantum mechanics had largely disappeared from view.
Its effects were hidden beneath bulk behavior:
- Many particles acting together
- Strong averaging over microscopic details
- Robust classical signals

Quantum mechanics did not vanish—it became infrastructure.

By the 1980s and beyond, this trend only intensified.
- Transistors shrank to the nanometer scale, with billions—now trillions—on a single chip
- Data transmission increasingly relied on light, encoding information in classical properties such as intensity, frequency, and phase
- Computing power exploded, enabling modern machine learning and large-scale neural networks

These advances were extraordinary—but conceptually, they introduced nothing fundamentally new.
### The Second Quantum Revolution: Why—and when?

At this point, new questions began to emerge.

Not questions about materials or miniaturization—but about information itself.

- What are classical computers fundamentally bad at?  
- What would it mean to process information quantum mechanically?
- Can quantum effects be used directly, rather than averaged away?

To answer these questions, we must return to the most basic feature of quantum mechanics: superposition.

Let's go back to single-particle quantum physics that we started this lecture with.  Consider a single quantum system.
A classical bit is either:  
$$0 \quad \text{or} \quad 1.   $$
But a quantum system can exist in a superposition: 


$$|\psi\rangle = |0\rangle + |1\rangle.  $$ 

This could a particle in a superposition of the ground and excited state of the particle box.  Or a hydrogen atom in a superposition of n=0 and n=1. 

Or, in terms of a single electron spin, it can be in a superposition of up and down:  

$$|\Psi\rangle = |\uparrow\rangle + |\downarrow\rangle.  $$

This is not uncertainty about the state.  
It _is_ the state.

What if this object—not a voltage level, not a macroscopic current—were the basic unit of information?

What would computation built from such objects look like?

Would it simply be faster?

Or would it be fundamentally different?

We will begin answering these questions in the next lecture.




# Homework

#### Why did classical physics fail?  
Answer in **3–6 sentences each**:

1) What experiments contradicted classical physics?  Describe the experiment needed to conduct each one. 
2) How did quantum theory resolve them?


#### Waves to discrete energies
One of the key observations that pushed physics toward quantum theory was that atoms emit light at discrete frequencies (spectral lines), rather than a continuous rainbow.

De Broglie proposed that matter behaves like a wave with wavelength
$$
\lambda = \frac{h}{p}.
$$
**Question:** Explain how a wave picture of an electron bound in an atom can lead naturally to discrete energy levels, and therefore discrete emission frequencies.

Write roughly 0.5 page. Your answer should:
- briefly mention one historical step we discussed (e.g., Planck, Einstein/photoelectric effect, Bohr model, de Broglie, Schrödinger),
- explain the mechanism in the wave picture (standing waves / boundary conditions / resonance condition),
- and connect it to one quantum system you have seen before (particle-in-a-box, harmonic oscillator, or hydrogen).

(You do not need to do a full derivation, but your explanation should make clear *why* only certain energies are allowed.)

#### The First Quantum Revolution (and the jump to the Second)
Choose two technologies from this list:
- transistor
- laser
- MRI
- superconductors

For each chosen technology, answer the following.

**(a) Bulk quantum vs. individual control (≈ 1 paragraph each).**  
Explain where quantum mechanics enters the story. Is the device best described as:
- **bulk/averaged quantum behavior** (many particles acting collectively; robust classical signals), or
- **control of an individual quantum system** (single atoms/ions/photons/spins kept coherent and manipulated)?

Be specific: name the key quantum idea (e.g., band structure, stimulated emission, spin quantization, Cooper pairing).

**(b) “Single-quantum” thought experiment (≈ 1 paragraph each).**  
Imagine pushing the technology toward the few-quanta limit, where discrete particles and measurement back-action matter. Describe what new effects would become important and what would break about the classical/engineering intuition.

**Optional concrete examples (use if helpful):**
- transistor → single-electron / single-donor / single-atom transistor (tunneling, Coulomb blockade)
- laser → single-photon / single-atom emitter, photon antibunching
- MRI → single-spin readout (NV centers, quantum projection noise)
- superconductor → few-Cooper-pair circuits (Josephson junction qubits, phase as a quantum variable)


