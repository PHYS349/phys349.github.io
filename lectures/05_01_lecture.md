---
kernelspec:
  name: python3
  display_name: Python 3
---

# Lecture 5.1 — Quantum Hardware: From Physics to Processors

## Where We Left Off

In Lectures 4.5 and 4.6, we built quantum algorithms — Deutsch-Jozsa, Bernstein-Vazirani, and Grover's — using abstract gates: $H$, $X$, $Z$, CNOT. We treated these as perfect mathematical operations on ideal qubits. But a real quantum computer isn't a math problem — it's a physical machine. Every $H$ gate is a microwave pulse or a laser beam. Every CNOT is a carefully timed interaction between two physical objects. And every qubit is fighting decoherence from the moment it's initialized.

Back in Lecture 1.4, we surveyed the hardware landscape at a high level. Now you know the physics: Rabi oscillations (Lecture 2.6), Ramsey interferometry (Lecture 2.6), the Bloch sphere (Lecture 2.4), decoherence channels $T_1$ and $T_2$ (Lecture 2.7), and entangling gates (Chapter 3). Today we revisit each hardware platform and see *exactly* how the abstract gate model maps onto physical reality. The punchline: every single-qubit gate is a Rabi rotation, every gate calibration is a Ramsey experiment, and every hardware limitation traces back to decoherence.

------------------------------------------------------------------------

## Part 1: Superconducting Qubits

The story of the transmon is one of the cleanest examples of "build a qubit from scratch out of circuit elements." We are going to build it the way an experimentalist would: start with the simplest possible resonator, see what's wrong with it as a qubit, and then fix what's wrong by adding one nonlinear circuit element.

### Step 1: An LC Circuit is a Cavity for Microwave Photons

You already know the LC circuit from introductory E&M. An inductor $L$ in parallel with a capacitor $C$:

```
       ┌─────────────┐
       │             │
      ─┴─            ⌇
       ─    C        ⌇  L
      ─┬─            ⌇
       │             │
       └─────────────┘
```

Energy sloshes back and forth between the electric field stored on the capacitor and the magnetic field stored in the inductor. Solving $L\ddot{Q} + Q/C = 0$ gives oscillations at the resonant frequency

$$
\omega_0 \;=\; \frac{1}{\sqrt{LC}}.
$$

For typical superconducting circuit values ($L \sim 10\text{ nH}$, $C \sim 100\text{ fF}$), this lands around $\omega_0/2\pi \sim 5\text{ GHz}$. That is the microwave band.

Here is the way to think about this: **an LC circuit is the lumped-element version of an optical cavity.** A Fabry–Pérot cavity stores standing-wave modes of light between two mirrors. An LC circuit stores standing-wave modes of voltage and current between two circuit elements. In the optical case the mode quanta are visible photons. In the LC case the mode quanta are microwave photons. Same physics, different frequency.

To make this work as a quantum system we need two things:
1. **Low loss**, so the photon doesn't leak out before we use it. We use **superconductors** (aluminum or niobium) so the wires have zero resistance.
2. **Low temperature**, so thermal noise doesn't fill the cavity with photons. We put the chip in a dilution refrigerator at $T \approx 15\text{ mK}$, where $k_B T / h \approx 0.3\text{ GHz} \ll \omega_0/2\pi$, so the cavity sits in its ground state.

### Step 2: Quantize the Circuit, Get a Harmonic Oscillator

Now treat the LC circuit quantum mechanically. The two natural variables are the charge $Q$ on the capacitor and the flux $\Phi$ through the inductor. The classical Hamiltonian is

$$
H \;=\; \frac{Q^2}{2C} \;+\; \frac{\Phi^2}{2L}.
$$

Compare this to the mechanical harmonic oscillator $H = p^2/(2m) + \tfrac{1}{2} m \omega^2 x^2$. The mapping is exact, with $Q \leftrightarrow p$, $\Phi \leftrightarrow x$, $C \leftrightarrow m$, and $1/L \leftrightarrow m\omega^2$. So the LC circuit is *literally* a harmonic oscillator, with conjugate variables $[\hat\Phi, \hat Q] = i\hbar$ and energy levels

$$
E_n \;=\; \hbar \omega_0 \left(n + \tfrac{1}{2}\right), \qquad n = 0, 1, 2, \dots
$$

The eigenstate $|n\rangle$ is the state with exactly $n$ microwave photons stored in the resonator. Same Fock-state ladder you would write down for an optical cavity, just at GHz instead of THz.

### Step 3: Why a Plain LC Circuit Cannot Be a Qubit

Now the punchline. Look at the level diagram:

```
   ──── n=3       E_3 = 3.5 ħω_0
            ↕  ω_0
   ──── n=2       E_2 = 2.5 ħω_0
            ↕  ω_0
   ──── n=1       E_1 = 1.5 ħω_0
            ↕  ω_0
   ──── n=0       E_0 = 0.5 ħω_0
```

The levels are **equally spaced** by $\hbar \omega_0$. That is the defining feature of a harmonic oscillator. It is also a disaster for a qubit. If we drive the circuit with a microwave tone at $\omega_0$ to rotate $|0\rangle \to |1\rangle$, that *same tone* is exactly resonant with $|1\rangle \to |2\rangle$, and $|2\rangle \to |3\rangle$, and so on. The drive cannot tell the qubit subspace apart from the rest of the ladder. Population leaks up the staircase.

```{admonition} The Requirement
:class: important
A qubit needs an **anharmonic oscillator**: a system whose level spacings are *unequal*, so that one transition can be addressed without touching the others. The first two levels then form an isolated two-level system.
```

So the question becomes: how do we keep our nice low-loss superconducting LC resonator, but make the spectrum unevenly spaced?

```{figure} ../figures/lc_circuit_vs_transmon_levels.gif
:alt: Side-by-side LC circuit and LC+Josephson-junction circuit, with corresponding harmonic and anharmonic energy ladders
:width: 90%
:align: center

The whole story in one picture. **Left:** A plain LC circuit is a quantum harmonic oscillator. The energy levels are equally spaced, so any microwave drive resonant with $|0\rangle \to |1\rangle$ also drives every other transition on the ladder. There is no isolated two-level system. **Right:** Replace the linear inductor with a Josephson junction (the cross symbol). The cosine potential is no longer a perfect parabola; level spacings shrink as you go up the ladder. The lowest two levels become a true qubit subspace because $\omega_{12}$ is detuned from $\omega_{01}$ by the anharmonicity. [Source: TBD — please fill in.]
```

### Step 4: Replace the Inductor with a Josephson Junction

The trick is to replace the linear inductor with a **nonlinear** one. The element that does this is the **Josephson junction**: two superconductors separated by a thin (~1 nm) insulating barrier through which Cooper pairs tunnel coherently.

A Josephson junction obeys two relations, set by the superconducting phase difference $\phi$ across it:

$$
I \;=\; I_c \sin\phi, \qquad V \;=\; \frac{\hbar}{2e}\,\dot\phi.
$$

An ordinary inductor satisfies $V = L \dot I$, i.e. $V \propto \dot I$ with a *constant* $L$. The Josephson element satisfies the same kind of relation, but with an *effective* inductance

$$
L_J(\phi) \;=\; \frac{\hbar}{2 e I_c \cos\phi}
$$

that depends on $\phi$ itself. So yes: the "inductor" is now amplitude-dependent. That's exactly what makes the oscillator anharmonic.

Equivalently, the energy stored in the junction is

$$
U_J(\phi) \;=\; E_J\bigl(1 - \cos\phi\bigr) \;\approx\; \frac{E_J}{2}\,\phi^2 \;-\; \frac{E_J}{24}\,\phi^4 \;+\; \cdots,
$$

where $E_J = \hbar I_c / (2e)$ is the Josephson energy. The first term is the quadratic potential of an ordinary inductor (it gives back the harmonic oscillator). The $\phi^4$ correction is the new ingredient. It bends the parabola and squeezes the upper levels closer together.

### Step 5: The Transmon Hamiltonian and Anharmonicity

Putting the Josephson junction in parallel with a capacitor gives the transmon. The Hamiltonian is

$$
\hat H \;=\; 4 E_C\,\hat n^2 \;-\; E_J \cos\hat\phi,
$$

where $\hat n$ is the number of Cooper pairs that have crossed the junction and $E_C = e^2/(2 C_\Sigma)$ is the charging energy. Transmons live in the regime $E_J / E_C \gg 1$ (typically $\sim 50$), where the phase $\phi$ is well-localized near the bottom of the cosine well and the standard perturbative result is

$$
E_n \;\approx\; \sqrt{8 E_J E_C}\,\bigl(n + \tfrac{1}{2}\bigr) \;-\; \frac{E_C}{12}\bigl(6 n^2 + 6 n + 3\bigr).
$$

The first term looks like a harmonic oscillator with frequency $\hbar \omega_0 = \sqrt{8 E_J E_C}$. The second term is the anharmonic correction from the $\phi^4$ piece. Two transition frequencies pop out:

$$
\hbar\omega_{01} \;=\; \sqrt{8 E_J E_C} - E_C, \qquad \hbar\omega_{12} \;=\; \sqrt{8 E_J E_C} - 2 E_C.
$$

The **anharmonicity** is the difference:

$$
\alpha \;\equiv\; \omega_{12} - \omega_{01} \;\approx\; -\,E_C / \hbar.
$$

For a typical transmon, $\omega_{01}/2\pi \approx 5\text{ GHz}$ and $\alpha/2\pi \approx -250\text{ MHz}$. The $|1\rangle \to |2\rangle$ transition is detuned from the qubit transition by a quarter of a GHz. A microwave pulse shaped to be narrow enough in frequency (a few tens of MHz wide, so a few-nanosecond pulse) drives $|0\rangle \to |1\rangle$ and barely touches $|1\rangle \to |2\rangle$. We have isolated a two-level subspace inside the ladder.

```
   ──── n=3
             ω_01 + 2α   ← getting smaller (squished)
   ──── n=2
             ω_01 + α    ← detuned by α ≈ −250 MHz
   ──── n=1
             ω_01 ≈ 5 GHz
   ──── n=0
```

### Step 6: What the Two Qubit States Actually Are

The qubit states $|0\rangle$ and $|1\rangle$ are the ground and first excited states of the transmon Hamiltonian above. They are *not* spin states, and they are not the polarizations of a real atom. They are the **zero-photon and one-photon states of an anharmonic microwave resonator on a chip.** A transmon in $|1\rangle$ contains literally one quantum of GHz electromagnetic excitation, oscillating between charge on the capacitor and current through the junction. A transmon in $|0\rangle$ contains zero such quanta. A transmon in $\frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ is in a quantum superposition of containing zero and one microwave photons.

This is why we call it an **artificial atom**: not because it is made of atoms (it is made of aluminum on silicon), but because it has an isolated, addressable, anharmonic energy spectrum, exactly like a real atom. The transition frequency $\omega_{01}$ is set by us, by choosing $L$ (i.e. $E_J$, set by the junction area) and $C$ (set by the pad geometry). We have engineered an atom whose level structure is whatever we want.

### Operating Conditions

Because the qubit transition is at $\omega_{01}/2\pi \approx 5\text{ GHz}$, we need $k_B T \ll \hbar \omega_{01}$. That sets the temperature requirement of $T \sim 15\text{ mK}$, achieved with a **dilution refrigerator**. At that temperature the thermal occupation of $|1\rangle$ is below $10^{-3}$, so the qubit reliably initializes into $|0\rangle$ just by waiting. The same low temperature also keeps the aluminum superconducting (its critical temperature is about 1.2 K, so 15 mK is comfortably below).

```{figure} ../figures/dilution_fridge_pnnl.jpg
:alt: Gold-colored dilution refrigerator hanging in a laboratory, with cascading thermal stages and microwave wiring
:width: 70%
:align: center

A superconducting qubit testbed mounted in a dilution refrigerator at Pacific Northwest National Laboratory. The cascading gold plates are successively colder thermal stages: from top to bottom roughly 50 K, 4 K, 800 mK, 100 mK, and the bottom mixing chamber stage at ~15 mK, where the qubit chip is mounted. The white and silver lines snaking down through every stage are coaxial microwave cables: each one carries a control or readout signal from room-temperature electronics down to the chip, with attenuators at each stage to thermalize the noise. Photo by Andrea Starr, [Pacific Northwest National Laboratory](https://www.pnnl.gov/news-media/new-superconducting-qubit-testbed-benefits-quantum-information-science-development).
```


The frequency $\omega_{01}$ is now what we will call $\omega_0$ for the rest of this part. It is the frequency that appeared in our Rabi and Ramsey discussions in Lecture 2.6.

### Single-Qubit Gates: Rabi Oscillations with Microwaves

Here's where your physics training pays off. How do you perform an $X$ rotation on a transmon? You apply a **microwave pulse** at the qubit's resonant frequency $\omega_0$. This is *exactly* the Rabi problem from Lecture 2.6:

$$
|\psi(t)\rangle = \cos\left(\frac{\Omega t}{2}\right)|0\rangle - i\sin\left(\frac{\Omega t}{2}\right)|1\rangle
$$

where $\Omega$ is the Rabi frequency, set by the pulse amplitude. To perform a $\pi$-pulse ($X$ gate), you apply the drive for time $t = \pi/\Omega$. For a $\pi/2$-pulse (the Hadamard-like rotation), $t = \pi/(2\Omega)$.

The phase of the microwave pulse determines the rotation axis on the Bloch sphere:
- In-phase pulse ($\cos(\omega_0 t)$) → rotation about $X$
- Quadrature pulse ($\sin(\omega_0 t)$) → rotation about $Y$
- The $Z$ rotation comes for free: just wait, and the qubit precesses around $Z$ at a rate $\omega_0$ in the rotating frame (this is the **virtual Z gate**, which costs zero time)

```{admonition} The Hadamard Gate on a Transmon
:class: tip
The Hadamard gate $H$ isn't a "natural" rotation — it's a rotation about the axis $(X + Z)/\sqrt{2}$, which is at 45° between $X$ and $Z$ on the Bloch sphere. On hardware, this is implemented as a sequence: a virtual $Z(\pi/2)$, then an $R_X(\pi/2)$ pulse, then another virtual $Z(\pi/2)$. The two $Z$ rotations are free (phase shifts), so the Hadamard costs only one physical microwave pulse.
```

### Gate Calibration: Ramsey Experiments

How do IBM engineers know the exact frequency $\omega_0$ and pulse duration for each qubit? They run **Ramsey experiments** — the same interferometry from Lecture 2.6.

1. Apply a $\pi/2$ pulse (puts the qubit on the equator of the Bloch sphere).
2. Wait a time $\tau$.
3. Apply a second $\pi/2$ pulse.
4. Measure.

If the drive frequency matches $\omega_0$ perfectly, the qubit returns to $|0\rangle$ regardless of $\tau$. If there's a detuning $\delta = \omega_{\text{drive}} - \omega_0$, the qubit oscillates with frequency $\delta$ during the wait. By scanning $\tau$ and fitting the oscillation, you extract $\delta$ and correct the drive frequency.

This is done *daily* on IBM's processors. Qubit frequencies drift due to two-level systems in the Josephson junction — tiny defects that couple to the qubit and shift its frequency by a few kHz. The Ramsey calibration tracks these drifts.

### Two-Qubit Gates: The Cross-Resonance Gate

To entangle two transmons, IBM uses the **cross-resonance (CR) gate**. The idea: drive qubit A at the *frequency of qubit B*. Because the qubits are coupled through a shared resonator, this off-resonant drive on A creates a conditional rotation on B — the rotation direction depends on whether A is in $|0\rangle$ or $|1\rangle$.

Mathematically, the CR interaction generates a $ZX$ Hamiltonian: $H_{\text{CR}} \propto Z_A \otimes X_B$. Evolving under this Hamiltonian for the right time gives a CNOT gate (up to single-qubit corrections).

This is IBM's native two-qubit gate. It works only between qubits that are physically coupled on the chip — which is why qubit connectivity matters and SWAP gates are sometimes needed during transpilation.

### Decoherence: $T_1$ and $T_2$ on the Transmon

From Lecture 2.7:
- $T_1$ (energy relaxation): the qubit spontaneously decays from $|1\rangle$ to $|0\rangle$ by emitting a microwave photon into the environment. Typical: 100–600 μs.
- $T_2$ (dephasing): the qubit's phase wanders due to fluctuations in $\omega_0$. Typical: 50–300 μs.

Every gate takes time (~20 ns for single-qubit, ~300 ns for CNOT). A circuit with depth $d$ takes roughly $d \times 300$ ns. If $d \times 300\text{ ns} \gtrsim T_2$, the computation is overwhelmed by dephasing. For $T_2 = 200\,\mu$s, that's a maximum depth of about 600 CNOT gates before the signal is lost.

```{admonition} Why Circuit Depth Matters
:class: warning
This is the core hardware constraint you'll encounter in your final project. When you transpile Grover's algorithm for 5 qubits on IBM hardware, the circuit depth can reach hundreds of gates. If that exceeds the coherence budget $T_2 / t_{\text{gate}}$, the algorithm fails — not because the algorithm is wrong, but because the qubits have decohered before it finishes.
```

------------------------------------------------------------------------

## Part 2: Trapped Ions

The transmon was an artificial atom we *built*. Trapped-ion qubits are the opposite philosophy: take an atom that nature has already perfected, hold it still, and use its built-in quantum structure as the qubit. Every $^{171}\text{Yb}^+$ ion in the universe is identical to every other one. There is no fabrication variation, no aging, no calibration drift between two qubits of the same species. If you can find a level structure inside the atom that does the job, the hardware comes for free.

```{figure} ../figures/single_ion_nadlinger.webp
:alt: Long-exposure photograph of a single trapped strontium ion glowing as a tiny bright dot between two electrode blades inside a vacuum chamber
:width: 60%
:align: center

A single trapped ion. The faint blue dot in the center, between the two electrode blades, is one $^{88}\text{Sr}^+$ ion held in a Paul trap and lit up by a resonant laser. Yes — you can take an ordinary photograph of a single atom. This image (David Nadlinger, University of Oxford, 2018) won the EPSRC "Single Atom in an Ion Trap" science photography prize. [Source: TBD — please confirm credit.]
```

### Step 1: Where the Qubit Lives Inside the Atom

To pick out the qubit we need to remember the layered structure of an atom. Hydrogen is the cleanest example.

**Layer 1: Electronic states.** The big energy structure of hydrogen comes from the electron's orbital around the nucleus. The transitions $1s \to 2p \to 3p \to \cdots$ are separated by electron-volts, corresponding to optical or UV frequencies (hundreds of THz). These are the lines you see in the Balmer series.

**Layer 2: Electron spin.** Inside any given electronic level the electron's spin can be up or down, $|{\uparrow}\rangle$ or $|{\downarrow}\rangle$. With no other interactions this is a 2-fold degeneracy.

**Layer 3: Nuclear spin.** The nucleus also has a spin. For hydrogen the proton has spin $1/2$, so the nuclear spin states are $|{\Uparrow}\rangle$ and $|{\Downarrow}\rangle$.

**Layer 4: Hyperfine coupling.** The electron and the nucleus both have magnetic moments, and they talk to each other. This is the **hyperfine interaction**. It splits the otherwise-degenerate electron-spin / nuclear-spin combinations into a small set of levels separated by typically a few GHz. For hydrogen this is the famous 21 cm line at 1.42 GHz, used by radio astronomers to map neutral hydrogen in the galaxy.

So inside the *same* electronic ground state of the atom, hyperfine coupling carves out a few levels separated by microwave frequencies. **That is where we put the qubit.**

For $^{171}\text{Yb}^+$, the same picture holds. The ground electronic state is ${}^2S_{1/2}$, and the nuclear spin is $1/2$. Hyperfine coupling splits this into two levels labeled by total angular momentum $F=0$ and $F=1$, separated by

$$
\omega_{\text{hf}} / 2\pi \;\approx\; 12.6\text{ GHz}.
$$

The qubit is

$$
|0\rangle \;=\; |F=0, m_F=0\rangle, \qquad |1\rangle \;=\; |F=1, m_F=0\rangle.
$$

Both qubit states sit in the *electronic ground state* of the ion. They are spin-like configurations of the electron and nucleus, not different orbitals. Because both states are in the ground electronic level, neither one can radiate by spontaneous emission to anything lower. Coherence times stretch into seconds, sometimes minutes. This is why trapped-ion qubits are often called the "gold standard" for coherence.

```{admonition} Why the Excited Electronic State Still Matters
:class: tip
We do not store the qubit in the excited electronic level (${}^2P_{1/2}$, optical transition at 369 nm), because that level decays in nanoseconds. But the optical transition is essential for **state readout**. A laser tuned to ${}^2S_{1/2}, F=1 \to {}^2P_{1/2}, F=0$ will scatter many photons if the ion is in $|1\rangle$ (a "bright" state), and zero photons if the ion is in $|0\rangle$ ($F=0$ is off-resonant and "dark"). A sensitive camera collects the fluorescence and assigns each ion a bright/dark label. This optical detection is destructive in the sense that it projects the qubit, but it is essentially **perfect**: trapped-ion readout fidelities exceed 99.9 percent, the highest of any platform.
```

```{figure} ../figures/ion_trap_schematic_readout.webp
:alt: Schematic of a linear Paul trap with a row of ions, laser beams crossing the trap, and a CCD camera detecting fluorescence; inset shows a real fluorescence image of a chain of bright spots
:width: 90%
:align: center

The full readout picture. A linear chain of ions (blue spheres) sits inside a Paul trap, addressed from the side by laser beams that drive both the gates and the cycling fluorescence transition. A CCD camera images the scattered photons. The inset is the actual fluorescence image: each bright spot is a single ion. Bright spots are projected to $|1\rangle$, dark spots to $|0\rangle$. [Source: TBD — please fill in.]
```

### Step 2: Holding the Ion Still

Now we need to keep the atom in one place. Earnshaw's theorem forbids stable confinement of a charged particle by purely static electric fields, so we use a **Paul trap**: an RF electric field at $\sim$ 10–100 MHz that creates a time-averaged "ponderomotive" potential. A single ion sits at the field zero in ultra-high vacuum ($10^{-11}$ torr, less than 1 stray molecule per cm$^3$). A row of 10 to 100 ions forms a one-dimensional crystal, spaced about 5 µm apart, held in line by the balance of the trap and their mutual Coulomb repulsion.

```{figure} ../figures/ion_trap_chip.jpg
:alt: Macro photograph of a microfabricated surface-electrode ion-trap chip mounted on a gold carrier with bond wires
:width: 80%
:align: center

A modern microfabricated **surface-electrode ion trap**. The chip is the small silver rectangle at the center; the gold pads and bond wires deliver the RF and DC voltages that shape the trap. Ions float a few tens of microns above the chip surface, held by the RF field generated by the patterned electrodes. This is the Sandia HOA-style trap used in many academic and commercial ion-trap quantum processors. [Source: TBD — please fill in.]
```

### Single-Qubit Gates: Rabi with Lasers

Single-qubit gates are Rabi oscillations, just like on a transmon — but driven by **laser pulses** instead of microwaves. A focused laser beam addresses a single ion and drives the $|0\rangle \leftrightarrow |1\rangle$ transition. The Rabi frequency $\Omega$ is set by the laser intensity. A $\pi$-pulse takes ~1–10 μs (about 100× slower than a superconducting gate, but the coherence times are also 1000× longer, so the ratio is favorable).

Some trapped-ion systems (like Quantinuum's) use **Raman transitions**: two laser beams whose frequency difference matches the qubit splitting. This avoids the need for a single laser at the exact qubit frequency and gives better control.

### Two-Qubit Gates: The Mølmer-Sørensen Gate

This is where trapped ions get beautiful. The ions don't interact directly — they're separated by micrometers. But they share **collective vibrational modes** (phonons) because they're all charged and confined in the same trap. Push one ion, and the whole crystal responds.

The **Mølmer-Sørensen (MS) gate** uses laser beams to couple each ion's internal state to the collective motion:

1. The laser entangles ion A's spin with the phonon mode.
2. The phonon mode transfers the entanglement to ion B.
3. A carefully shaped pulse ensures the phonons disentangle at the end, leaving only spin-spin entanglement.

The result is a native $XX$ gate: $e^{-i\theta X_A X_B}$. For $\theta = \pi/4$, this is equivalent to a CNOT (up to single-qubit rotations).

The key advantage: because the phonon mode couples *all* ions in the trap, any ion can interact with any other ion. This **all-to-all connectivity** means no SWAP gates are needed — unlike superconducting processors, where you can only entangle nearest neighbors.

### Coherence: Why Atoms Win

Trapped ions have extraordinarily long coherence times:
- $T_1$: effectively infinite (the hyperfine states are both ground states — there's nowhere to decay to)
- $T_2$: seconds to minutes, limited by magnetic field fluctuations

The ratio of coherence time to gate time is about $10^6$ for trapped ions vs. $10^3$ for superconducting qubits. This is why trapped ions hold the record for gate fidelity: **IonQ** reported 99.99% two-qubit gates in October 2025, and **Quantinuum** demonstrated 99.914% ("three-nines") two-qubit gates on its commercial H-series machines in 2024.

The tradeoff: gate speeds are ~1000× slower than superconducting, and scaling beyond ~100 ions in a single trap is difficult because the phonon modes become crowded and hard to control.

------------------------------------------------------------------------

## Part 3: Neutral Atoms

Neutral atoms inherit all the good features of trapped ions: identical from-the-factory atoms, hyperfine qubits with second-long coherence, optical readout. The difference is that they are **uncharged**. That fact cuts both ways. On the bad side, you cannot grab them with electric fields the way a Paul trap grabs an ion, so they are much harder to hold still. On the good side, they do not feel the long-range Coulomb interaction, so they sit quietly next to each other without talking unless you tell them to. There is no shared phonon bus that you have to manage. You get a clean array of independent qubits and you turn the interactions on only when you want them.

```{figure} ../figures/tweezer_experiment.jpg
:alt: Photograph of a neutral-atom experiment showing blue cooling laser beams crossing the science chamber, with an inset image of a regular grid of glowing single atoms held in optical tweezers
:width: 90%
:align: center

A neutral-atom quantum processor in the lab. The blue glow is the cooling laser scatter from a magneto-optical trap (MOT) that loads single atoms into the experiment. The inset on the right is a fluorescence image of the actual qubit array: a regular grid of single atoms, each held in its own focused-laser optical tweezer. Each spot is one atom; each atom is one qubit. [Source: TBD — please fill in.]
```

### How Light Traps a Neutral Atom

Without a charge to push on, the only handle we have is the atom's **electric dipole moment**. An isolated alkali atom has no permanent dipole, but if you put it in an oscillating electric field $E(t) = E_0 \cos(\omega t)$ from a laser, the field pushes the electron cloud back and forth and *induces* an oscillating dipole $p(t)$. The interaction energy averaged over one optical cycle is

$$
U \;=\; -\tfrac{1}{2}\,\alpha(\omega)\, E_0^2,
$$

where $\alpha(\omega)$ is the atom's frequency-dependent polarizability. The sign of $\alpha(\omega)$ is what matters, and it depends on the **phase relationship** between the induced dipole and the driving field.

Think of the atom as a driven oscillator (electron on a spring, with resonance at the atomic transition frequency $\omega_0$). For a sinusoidal drive at frequency $\omega$:

- **Below resonance** ($\omega < \omega_0$, called *red-detuned*): the dipole oscillates **in phase** with the field. $\alpha > 0$, so $U < 0$. The atom **lowers** its energy by sitting where the light intensity $E_0^2$ is *highest*. The atom is **attracted to bright regions**.
- **Above resonance** ($\omega > \omega_0$, *blue-detuned*): the dipole oscillates **out of phase** ($180^\circ$ behind). $\alpha < 0$, so $U > 0$. The atom is **pushed away from bright regions**.

This is the **optical dipole force**, and it is the entire idea behind an **optical tweezer**: take a laser, focus it tightly with a lens to a spot a few hundred nanometers across, detune it red of the atomic resonance. A nearby atom rolls down into the bright focus and sits there in a potential well a few mK deep. Use a spatial light modulator (a programmable holographic phase mask) to split the beam into hundreds or thousands of focused spots simultaneously, and you have a 2D or 3D array of single-atom traps.

```{figure} ../figures/tweezer_slm_setup.png
:alt: Optical schematic of a neutral-atom tweezer experiment with a spatial light modulator that produces 2D and 1D arrays of focused spots, plus camera fluorescence images of the atom arrays
:width: 95%
:align: center

How the array is made. **(a)** A single laser is reflected off a **spatial light modulator (SLM)**, which imprints a programmable phase pattern, then sent through a high-numerical-aperture lens onto a vacuum cell containing the atoms. The same lens collects fluorescence onto an EMCCD camera. **(b–c)** Single-shot fluorescence images of large 2D atom arrays produced by the SLM; each bright dot is one atom in one tweezer. **(d–e)** 1D chains for comparison. The geometry of the array is whatever pattern you write to the SLM. [Source: TBD — please fill in (looks like an MDPI Photonics paper, likely CC-BY).]
```

```{admonition} The Same Physics as the Transmon Drive
:class: tip
The dipole-field coupling here is exactly the dipole approximation $H_\text{int} = -\hat{p}\cdot E(t)$ that you saw in the Rabi problem in Lecture 2.6. There the *resonant* drive flipped the qubit ($|0\rangle \leftrightarrow |1\rangle$). Here a *far-detuned* drive doesn't flip anything. It just shifts the energy of the atomic levels, producing a position-dependent potential that traps the atom's center-of-mass motion. Same Hamiltonian, two limits.
```

### The Qubit and Why Rearrangeable Arrays Are Special

The qubit itself uses the same trick as trapped ions: two hyperfine ground states of an alkali atom (commonly $^{87}$Rb or $^{133}$Cs), separated by a microwave frequency, with second-long coherence times limited mostly by photon scattering from the trapping light. Single-qubit gates are Rabi oscillations driven by microwaves or by a pair of Raman lasers, just like the ion case.

What is genuinely new is that **the trap is just a beam of light, and you can move the beam.** By steering the tweezers with acousto-optic deflectors, you can pick up an atom from one site, walk it across the array, and drop it next to any other atom you choose. Connectivity becomes *reconfigurable*: the qubit graph is whatever you draw with the tweezer paths. There is no fixed nearest-neighbor lattice to design around.

```{figure} ../figures/tweezer_load_rearrange_readout.png
:alt: Three-step sequence showing initial random loading of atoms in a tweezer array, rearrangement into a defect-free target pattern, and final readout
:width: 95%
:align: center

The standard neutral-atom protocol: **(1) Load** atoms stochastically from a MOT into the tweezer array; each site fills with probability ~50%, leaving random defects. **(2) Rearrange** the array by moving the tweezers, hopping single atoms from where they happened to land into the geometry the algorithm needs. The result is a defect-free, programmable qubit pattern. **(3) Run gates and read out** by collecting fluorescence. This load–rearrange–readout cycle is what makes neutral-atom processors so flexible: the qubit graph is rebuilt every shot. [Source: TBD — please fill in (likely from a Nature/Lukin/Endres-style paper).]
```

### Two-Qubit Gates: Rydberg Blockade

This is the physics that makes neutral atoms special. In their ground states, neutral atoms at typical spacings (~5 μm) barely interact. To create entanglement, you temporarily excite the atoms to **Rydberg states** — states with very high principal quantum number $n \sim 50$–$100$.

A Rydberg atom is enormous: the electron orbits at radius $\sim n^2 a_0 \sim 0.25\,\mu$m, giving the atom a huge electric dipole moment $\sim n^2 e a_0$. Two nearby Rydberg atoms interact through the van der Waals potential:

$$
V(r) = -\frac{C_6}{r^6}
$$

where $C_6 \propto n^{11}$ scales extremely steeply with principal quantum number. For $n = 70$ and $r = 5\,\mu$m, this interaction is ~100 MHz — much larger than the Rydberg excitation linewidth (~1 MHz).

This creates the **Rydberg blockade**: if atom A is excited to the Rydberg state, the interaction shifts atom B's Rydberg level out of resonance with the excitation laser. Atom B *cannot* be excited to the Rydberg state while atom A is there. This conditional excitation is a natural controlled operation — the basis for a CZ gate.

```{code-cell} python
:tags: [hide-input]
import numpy as np
import matplotlib.pyplot as plt

# Rydberg blockade visualization
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))

# Left panel: energy levels with and without blockade
# No blockade (atoms far apart)
levels_far = [0, 1, 2, 3]  # |gg⟩, |gr⟩, |rg⟩, |rr⟩
labels_far = [r'$|gg\rangle$', r'$|gr\rangle$', r'$|rg\rangle$', r'$|rr\rangle$']
energies_far = [0, 1, 1, 2]

ax1.set_title("No blockade (atoms far apart)", fontsize=12)
for e, l in zip(energies_far, labels_far):
    ax1.hlines(e, 0.2, 0.8, colors='#2563eb', linewidth=2)
    ax1.text(0.85, e - 0.05, l, fontsize=11, va='center')
ax1.annotate('', xy=(0.5, 1), xytext=(0.5, 0),
            arrowprops=dict(arrowstyle='->', color='red', lw=1.5))
ax1.annotate('', xy=(0.5, 2), xytext=(0.5, 1),
            arrowprops=dict(arrowstyle='->', color='red', lw=1.5))
ax1.text(0.05, 0.5, r'$\omega_r$', fontsize=12, color='red')
ax1.text(0.05, 1.5, r'$\omega_r$', fontsize=12, color='red')
ax1.set_xlim(-0.1, 1.3)
ax1.set_ylim(-0.3, 2.5)
ax1.set_ylabel("Energy", fontsize=12)
ax1.set_xticks([])

# Blockade (atoms close)
ax2.set_title("Rydberg blockade (atoms close)", fontsize=12)
energies_close = [0, 1, 1, 2.8]  # |rr⟩ shifted up by V
labels_close = [r'$|gg\rangle$', r'$|gr\rangle$', r'$|rg\rangle$', r'$|rr\rangle + V$']

for i, (e, l) in enumerate(zip(energies_close, labels_close)):
    color = '#dc2626' if i == 3 else '#2563eb'
    style = '--' if i == 3 else '-'
    ax2.hlines(e, 0.2, 0.8, colors=color, linewidth=2, linestyle=style)
    ax2.text(0.85, e - 0.05, l, fontsize=11, va='center', color=color)

ax2.annotate('', xy=(0.5, 1), xytext=(0.5, 0),
            arrowprops=dict(arrowstyle='->', color='red', lw=1.5))
ax2.annotate('', xy=(0.5, 2.8), xytext=(0.5, 1),
            arrowprops=dict(arrowstyle='->', color='gray', lw=1.5, linestyle='dashed'))
ax2.text(0.05, 0.5, r'$\omega_r$', fontsize=12, color='red')
ax2.text(0.05, 1.8, r'$\omega_r$', fontsize=12, color='gray')
ax2.annotate('', xy=(1.15, 2.8), xytext=(1.15, 2.0),
            arrowprops=dict(arrowstyle='<->', color='#dc2626', lw=1.5))
ax2.text(1.2, 2.35, r'$V = C_6/r^6$', fontsize=11, color='#dc2626')

# X through the blocked transition
ax2.plot([0.35, 0.65], [1.7, 2.3], 'r-', linewidth=2)
ax2.plot([0.35, 0.65], [2.3, 1.7], 'r-', linewidth=2)

ax2.set_xlim(-0.1, 1.6)
ax2.set_ylim(-0.3, 3.3)
ax2.set_ylabel("Energy", fontsize=12)
ax2.set_xticks([])

plt.tight_layout()
plt.show()
```

The blockade radius — the distance within which the interaction is strong enough to prevent double excitation — is typically 5–10 μm for $n \sim 70$. This naturally defines the range of two-qubit gates.

### Scaling: Why Neutral Atoms Lead in Qubit Count

The scaling argument is simple: optical tweezers are created by a spatial light modulator (essentially a programmable hologram), and you can make as many spots as you want. The Caltech group (Endres lab) demonstrated **6,100 neutral atoms** in a single array in September 2025, with single-atom coherence times of 12.6 seconds. This is far beyond what any other platform has achieved.

The challenge is maintaining high fidelity as you scale. With more atoms, the Rydberg interactions become more complex, and stray interactions between non-target atoms can introduce errors. Two-qubit fidelities are in the **99.4–99.6%** range across recent experiments (Harvard/MIT/QuEra, Atom Computing, Caltech, 2023–2025), behind trapped ions but improving rapidly.

------------------------------------------------------------------------

## Part 4: Photonics — Full Circle to the Beam Splitter

### The Mach-Zehnder Interferometer *Is* a Quantum Computer

We've come full circle. In Lecture 2.2, we introduced the Mach-Zehnder interferometer as a way to understand quantum interference: a photon enters, a beam splitter puts it in superposition, a phase shifter creates a phase difference, and a second beam splitter converts phase into amplitude.

That beam splitter? It's a **Hadamard gate**. The phase shifter? It's a **$Z$ rotation**. The whole interferometer is a single-qubit quantum circuit:

$$
H \cdot R_z(\phi) \cdot H = R_x(\phi)
$$

The qubit is the **path** of the photon: $|0\rangle$ = upper arm, $|1\rangle$ = lower arm. This is called a **dual-rail encoding**.

```{admonition} Physical Intuition
:class: tip
Every algorithm we've built this semester — Deutsch-Jozsa, Grover's, everything — can be implemented with beam splitters, phase shifters, and mirrors. The math is identical. The challenge is making the photons interact with each other for two-qubit gates.
```

### The Two-Qubit Gate Problem

Single-qubit gates with photons are easy and nearly perfect — beam splitters and phase shifters can be manufactured with extremely low loss. The problem is **two-qubit gates**: photons don't interact with each other under normal conditions. Two photons pass right through each other.

The **KLM protocol** (Knill, Laflamme, Milburn, 2001) showed that you *can* build a universal quantum computer with just beam splitters, phase shifters, single-photon sources, and photon detectors — no direct photon-photon interaction needed. The trick is **measurement-induced nonlinearity**: by measuring some photons and conditioning on the outcome, you can effectively create an interaction between the remaining photons.

The cost: KLM gates are **probabilistic**. A CNOT gate succeeds only a fraction of the time. You need extra photons (ancillas), and you have to repeat until you get the right measurement outcome. This can be mitigated with clever schemes, but it makes scaling difficult.

### Current State: PsiQuantum and Xanadu

**PsiQuantum** is building a photonic quantum computer using silicon photonic chips — waveguides etched into silicon that guide photons the same way fiber optics do. Their bet: silicon photonics can be manufactured at scale using existing semiconductor fabs, and photons can operate at room temperature (no dilution refrigerators).

```{figure} ../figures/silicon_photonic_chip.png
:alt: Photograph of a silicon photonic quantum chip showing a dense network of waveguides connecting many on-chip beam splitters and phase shifters
:width: 90%
:align: center

A silicon photonic quantum processor. Each thin line is a waveguide etched into the silicon; each junction is a directional coupler (beam splitter) and each straight segment with a control pad is a programmable phase shifter. The whole chip is essentially a giant reconfigurable Mach-Zehnder network — every algorithm in the course, written in glass and silicon. Manufacturing happens in the same CMOS fabs that make commercial computer chips. [Source: TBD — please fill in.]
```

**Xanadu** uses a different approach: **squeezed light** (continuous-variable quantum computing) rather than single photons. Squeezed states are Gaussian states of light with reduced noise in one quadrature, and they can be entangled and measured to perform computations. This avoids the single-photon detection problem.

### Advantages and Limitations

**Advantages:**
- Room temperature operation (no cryogenics)
- Extremely low single-qubit gate errors (beam splitters are very precise)
- Photons travel at the speed of light — potential for quantum networking
- Leverages mature fiber optic and silicon photonic technology

**Limitations:**
- Two-qubit gates are probabilistic and resource-intensive
- Photon loss is cumulative — each optical element loses a small fraction of photons
- Single-photon sources and detectors are imperfect
- No quantum memory (photons can't be stored easily — they move at the speed of light)

------------------------------------------------------------------------

## Part 5: Comparing the Platforms

### Updated Comparison Table (2025–2026)

| | Superconducting | Trapped Ions | Neutral Atoms | Photonic |
|:--|:----------------|:-------------|:--------------|:---------|
| **Qubit** | Transmon (circuit) | Hyperfine levels | Hyperfine levels | Photon path |
| **$\|0\rangle \leftrightarrow \|1\rangle$** | Microwave (~5 GHz) | Microwave/optical | Microwave/laser | Beam splitter |
| **1Q gate** | μwave pulse (~20 ns) | Laser pulse (~1 μs) | Laser pulse (~1 μs) | Beam splitter (~ps) |
| **2Q gate** | CZ / cross-resonance (~100–300 ns) | Mølmer-Sørensen (~100 μs) | Rydberg blockade (~1 μs) | KLM (probabilistic) |
| **$T_2$** | ~150–500 μs (median, IBM Eagle/Heron) | seconds (record: 12.6 s, Caltech 2025 — single-atom hyperfine) | 1–10 s (single atom) | N/A (no memory) |
| **2Q fidelity** | ~99.7% (IBM Heron, 2024–25) | 99.914% commercial (Quantinuum, 2024); **99.99%** record (IonQ, 2025) | 99.4–99.6% (Harvard/QuEra, Atom Computing, 2024–25) | ~99% (heralded, KLM) |
| **Max qubits** | 1,121 (IBM Condor, 2023); 156 perf-focused (IBM Heron, 2024–25) | 98 (Quantinuum Helios, 2025) | **6,100** (Caltech, 2025) | 12 networked (Xanadu Aurora, 2025); PsiQuantum target ~10⁶ |
| **Connectivity** | Nearest-neighbor | All-to-all | Reconfigurable | All-to-all (optical) |
| **Temperature** | 15 mK | Room temp + laser cooling | Room temp + laser cooling | **Room temp** |
| **Key advantage** | Speed, fabrication | Fidelity, coherence | Scale, flexibility | No cryogenics, networking |
| **Key challenge** | Decoherence, connectivity | Scaling, speed | Rydberg fidelity | Probabilistic gates, loss |
| **Companies** | IBM, Google, Rigetti | Quantinuum, IonQ | QuEra, Pasqal, Atom Computing | PsiQuantum, Xanadu |

### The Common Thread: Every Gate Is a Rabi Rotation

Despite the enormous physical differences between these platforms, the abstract gate model is the same:

| Abstract operation | Superconducting | Trapped ions | Neutral atoms | Photonic |
|:-------------------|:----------------|:-------------|:--------------|:---------|
| $R_x(\theta)$ | μwave pulse, duration $\theta/\Omega$ | Laser pulse, duration $\theta/\Omega$ | Laser pulse, duration $\theta/\Omega$ | Phase shifter + BS |
| $R_z(\phi)$ | Virtual Z (frame change) | Virtual Z (frame change) | Virtual Z (frame change) | Phase shifter |
| CNOT | Cross-resonance + 1Q corrections | MS gate + 1Q corrections | CZ (Rydberg) + H | KLM + postselection |

The universality of the gate model is what lets us write algorithms without specifying the hardware. Qiskit's transpiler handles the translation from abstract gates to the native gate set of whatever backend you're using.

------------------------------------------------------------------------

## Part 6: What Limits Quantum Computers Today?

### The Error Budget

For each platform, the key question is: how many gates can you run before decoherence overwhelms the computation?

$$
\text{Maximum useful depth} \approx \frac{T_2}{t_{\text{gate}}}
$$

| Platform | $T_2$ | $t_{\text{2Q gate}}$ | Max depth |
|:---------|:-------|:---------------------|:----------|
| Superconducting | 200 μs | 300 ns | ~600 |
| Trapped ions | 1 s | 100 μs | ~10,000 |
| Neutral atoms | 1 s | 1 μs | ~1,000,000 |

These are rough upper bounds — in practice, gate errors accumulate faster than decoherence alone would predict, because each gate has a finite error rate even before decoherence kicks in.

```{admonition} Check Yourself
:class: tip
Grover's algorithm on $n = 10$ qubits ($N = 1024$) needs about $\frac{\pi}{4}\sqrt{1024} \approx 25$ iterations, each containing an oracle + diffusion. If each iteration requires ~50 two-qubit gates after transpilation, the total circuit depth is ~1,250 two-qubit gates. Which platform(s) could handle this? (Compare to the max depth table above.)
```

### The Path to Fault Tolerance

None of today's hardware can run Shor's algorithm on meaningful problem sizes. The circuit is too deep, and the qubits are too noisy. The path forward is **quantum error correction** — which we'll cover in the next lecture.

The key metric is: can your hardware's error rate beat the error correction threshold (~1% for surface codes)? Current status:

- **Trapped ions:** ~0.01% two-qubit error (IonQ, 2025) — well below threshold
- **Superconducting:** ~0.3% (IBM Heron, 2024–25) — at threshold
- **Neutral atoms:** ~0.4–0.6% (Harvard/QuEra, Atom Computing, 2024–25) — at threshold
- **Photonic:** ~1% (heralded) — at threshold

Google demonstrated in December 2024 that a distance-5 surface code on their Willow processor outperformed a distance-3 code — the first time error correction was shown to improve with code distance. This is the beginning of the fault-tolerant era.

------------------------------------------------------------------------

## Summary

- **Every single-qubit gate is a Rabi rotation.** Whether driven by microwaves (transmons), lasers (ions, atoms), or beam splitters (photons), the physics is the same: oscillation between $|0\rangle$ and $|1\rangle$ at a rate set by the drive strength.

- **Gate calibration is a Ramsey experiment.** Qubit frequencies drift, and the Ramsey protocol (from Lecture 2.6) is used daily to track and correct these drifts on real processors.

- **Two-qubit gates use different physics on each platform:** cross-resonance coupling (superconducting), phonon-mediated MS gates (trapped ions), Rydberg blockade (neutral atoms), and measurement-induced nonlinearity (photonic). All produce the same abstract CNOT.

- **Decoherence sets the circuit depth limit.** The ratio $T_2 / t_{\text{gate}}$ determines how many gates you can run before noise overwhelms the signal.

- **The platforms are converging on error correction thresholds.** Trapped ions are already below threshold. Superconducting and neutral atoms are approaching it. The next lecture covers how error correction works and why it's the key to useful quantum computing.

------------------------------------------------------------------------

## What's Next

We've now seen how physical qubits work, what limits them, and where each platform stands. But even the best qubits make errors. In the next lecture, we tackle **quantum error correction** — how to use redundancy and entanglement to protect quantum information, why the no-cloning theorem doesn't prevent it, and what it will take to build a fault-tolerant quantum computer.
