# Lecture 2.3: Polarization and Measurement

## Polarization as a Qubit

Light is an electromagnetic wave. For light traveling along the $z$-axis, the electric field oscillates in the transverse $x$-$y$ plane:

$$\vec{E}(x,t) = \text{Re}\left[(E_x \hat{x} + E_y e^{i\phi} \hat{y})\, e^{i(kx - \omega t)}\right]$$

The amplitudes $E_x$ and $E_y$ and the relative phase $\phi$ together determine the **polarization** — the pattern traced by the electric field vector as the wave passes. Linear polarization, circular polarization, and everything in between are all encoded in these parameters.

### The Jones Vector

We can factor out the propagation $e^{i(kx - \omega t)}$ and focus on what makes different polarizations different. What remains is the **Jones vector**:

$$\vec{J} = \begin{pmatrix} E_x \\ E_y e^{i\phi} \end{pmatrix}$$

This is a 2D complex vector — exactly like our qubit state vectors! For normalized states (total intensity = 1):

$$|E_x|^2 + |E_y|^2 = 1$$

### The Six Cardinal Polarization States

We identify horizontal and vertical polarization with our computational basis:

$$|H\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}, \qquad |V\rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$$

From these we can build the diagonal and anti-diagonal linear polarizations:

$$|D\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ 1 \end{pmatrix} = \frac{|H\rangle + |V\rangle}{\sqrt{2}}, \qquad |A\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ -1 \end{pmatrix} = \frac{|H\rangle - |V\rangle}{\sqrt{2}}$$

And the circular polarizations, where the relative phase between $x$ and $y$ components is $\pm \pi/2$:

$$|R\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ i \end{pmatrix} = \frac{|H\rangle + i|V\rangle}{\sqrt{2}}, \qquad |L\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ -i \end{pmatrix} = \frac{|H\rangle - i|V\rangle}{\sqrt{2}}$$

For circular polarization, the $E_x$ and $E_y$ components have equal amplitude but are 90° out of phase. The electric field vector traces out a circle as the wave propagates — rotating clockwise (R) or counterclockwise (L) when viewed head-on.

### The Polarization–Qubit Dictionary

These six states map directly onto the Bloch sphere:

| Polarization | Jones Vector | Qubit | Bloch Position |
|------------------|-----------------|----------------|---------------------|
| Horizontal $\|H\rangle$ | $(1, 0)^T$ | $\|0\rangle$ | North pole (+z) |
| Vertical $\|V\rangle$ | $(0, 1)^T$ | $\|1\rangle$ | South pole (−z) |
| Diagonal $\|D\rangle$ | $(1, 1)^T/\sqrt{2}$ | $\|+\rangle$ | +x axis |
| Anti-diagonal $\|A\rangle$ | $(1, -1)^T/\sqrt{2}$ | $\|-\rangle$ | −x axis |
| Right circular $\|R\rangle$ | $(1, i)^T/\sqrt{2}$ | $\|+i\rangle$ | +y axis |
| Left circular $\|L\rangle$ | $(1, -i)^T/\sqrt{2}$ | $\|-i\rangle$ | −y axis |

Notice what distinguishes these states. $|D\rangle$ and $|A\rangle$ have the same amplitudes — both have $|E_x| = |E_y| = 1/\sqrt{2}$ — but differ by a relative phase of $\pi$. Similarly, $|R\rangle$ and $|L\rangle$ have the same amplitudes but differ by a relative phase of $\pi$. Relative phase is physical: it produces completely different polarizations.

The six states form three pairs of orthogonal states, corresponding to three measurement bases:

| Axis | Basis | Physical Measurement |
|----------------|----------------|-----------------------------------------|
| z | $\|H\rangle$, $\|V\rangle$ | Horizontal/Vertical polarizer |
| x | $\|D\rangle$, $\|A\rangle$ | Polarizer at ±45° |
| y | $\|R\rangle$, $\|L\rangle$ | Circular polarizer (quarter-wave plate + linear polarizer) |

------------------------------------------------------------------------

## Polarizers and Projective Measurement

A polarizer transmits light polarized along one direction and blocks the orthogonal component. Let's figure out two things: how much power comes through, and what is the state of the light afterward.

### How Much Power Passes?

Consider a general input state $|\psi\rangle = c_0|H\rangle + c_1|V\rangle$ hitting a vertical polarizer. The polarizer only passes the $|V\rangle$ component.

The amplitude to pass is the overlap with $|V\rangle$:

$$\text{amplitude} = \langle V|\psi\rangle = \langle V|(c_0|H\rangle + c_1|V\rangle) = c_1$$

The power (intensity) detected is the magnitude squared of the amplitude:

$$\text{power detected} = |c_1|^2 = |\langle V|\psi\rangle|^2$$

This is Malus's Law in Dirac notation: the fraction of power transmitted through a polarizer $|\chi\rangle$ is $|\langle \chi|\psi\rangle|^2$.

### What Is the State Afterward?

After the vertical polarizer, the output state is:

$$|\psi_{\text{out}}\rangle = |V\rangle\langle V|\psi\rangle$$

The polarizer doesn't just filter — it **projects** the state onto $|V\rangle$. Whatever the input was, what comes out is always proportional to $|V\rangle$.

### The Projection Operator

We can write this as the action of a **projection operator**:

$$P_V = |V\rangle\langle V| = \begin{pmatrix} 0 \\ 1 \end{pmatrix}\begin{pmatrix} 0 & 1 \end{pmatrix} = \begin{pmatrix} 0 & 0 \\ 0 & 1 \end{pmatrix}$$

Acting on an arbitrary state:

$$P_V |\psi\rangle = \begin{pmatrix} 0 & 0 \\ 0 & 1 \end{pmatrix}\begin{pmatrix} c_0 \\ c_1 \end{pmatrix} = \begin{pmatrix} 0 \\ c_1 \end{pmatrix}$$

The $H$ component is gone; only the $V$ component survives.

Similarly, the horizontal projector is:

$$P_H = |H\rangle\langle H| = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}$$

### Projectors Are Idempotent

A key property of projectors: applying the same projection twice gives the same result as applying it once.

$$P_V P_V = |V\rangle\langle V|V\rangle\langle V| = |V\rangle(1)\langle V| = |V\rangle\langle V| = P_V$$

$$P_V^2 = P_V$$

And more generally:

$$P_V^N = P_V$$

This makes physical sense: once you've projected onto $|V\rangle$, the state is already $|V\rangle$. Running it through the same polarizer again changes nothing.

------------------------------------------------------------------------

## From Waves to Single Photons

Everything so far has been classical wave optics. Malus's Law tells us what fraction of the **intensity** passes through a polarizer. Now let's ask: what happens when we turn the power way down?

### Photons as Discrete Energy Packets

From the blackbody radiation problem (one of the founding puzzles of quantum mechanics), we know that the electromagnetic field carries energy in discrete packets — **photons** — each with energy:

$$E_{\text{photon}} = h\nu$$

where $h = 6.626 \times 10^{-34}$ J·s is Planck's constant and $\nu$ is the frequency of the light.

Let's calculate what this energy actually is. For a helium-neon (HeNe) laser at $\lambda = 633$ nm:

$$\nu = \frac{c}{\lambda} = \frac{3 \times 10^8 \text{ m/s}}{633 \times 10^{-9} \text{ m}} \approx 4.7 \times 10^{14} \text{ Hz}$$

$$E_{\text{photon}} = h\nu \approx (6.6 \times 10^{-34})(4.7 \times 10^{14}) \approx 3.1 \times 10^{-19} \text{ J}$$

That is an incredibly small amount of energy.

### How Many Photons in a Laser Pointer?

A typical laser pointer has a power of about $P = 1$ mW $= 10^{-3}$ W. Power is energy per unit time, so the number of photons emitted per second is:

$$\dot{N} = \frac{P}{E_{\text{photon}}} = \frac{10^{-3} \text{ W}}{3.1 \times 10^{-19} \text{ J}} \approx 3.2 \times 10^{15} \text{ photons/s}$$

That's about 3 quadrillion photons per second! At this power level, the photons arrive so densely packed that the light appears perfectly continuous. Your detector sees a smooth signal proportional to $|E|^2$.

### Turning Down the Power

Now imagine we gradually reduce the power. What do we expect to see on a detector?

At 1 mW, we have $\sim 10^{15}$ photons/s — a smooth, continuous signal.

At 1 nW ($10^{-9}$ W), we have $\sim 10^{9}$ photons/s — still essentially continuous.

But at extremely low power — say, a few photons per second — something qualitatively different happens.

### How Do You Actually Detect Single Photons?

A standard photodetector works like this: an incoming photon hits a semiconductor and promotes an electron from the valence band to the conduction band. That electron flows as a current. The current passes through a resistor, and by Ohm's law ($V = IR$), you measure a voltage drop.

The problem is that one photon produces one electron, which is a fantastically tiny current — far too small to measure directly.

The solution is an **Avalanche Photodetector (APD)**. In an APD, a strong reverse bias voltage accelerates the initial photoelectron so that it knocks out additional electrons through impact ionization. Those electrons are also accelerated, knock out more, and so on — creating a cascade, an *avalanche*. A single absorbed photon triggers a macroscopic current pulse that you can easily measure.

A more modern alternative is the **Superconducting Nanowire Single-Photon Detector (SNSPD)**. An SNSPD consists of a thin superconducting nanowire, cooled well below its critical temperature and biased with a current just below the critical current. When a single photon is absorbed, it breaks Cooper pairs in a small region of the wire, creating a resistive hotspot. The bias current is forced to divert around this hotspot, which pushes the current density in the remaining superconducting cross-section above the critical value. The entire wire cross-section goes normal (resistive), and the sudden appearance of resistance in the circuit produces a measurable voltage pulse. The wire then cools back to superconducting on a timescale of nanoseconds, ready for the next photon.

Regardless of the detection method, the end result is the same: a voltage spike on our oscilloscope, roughly 1 ps to 1 ns wide, that tells us exactly one photon has arrived.

------------------------------------------------------------------------

## Single Photons in the Mach-Zehnder Interferometer

Now put a single-photon detector at each output port of a Mach-Zehnder interferometer, and send in photons one at a time — on average, about one per second.

Classically, we found that the output intensities depend on the phase difference $\Delta\phi$ between the two arms:

$$I_{D1} = \frac{1}{2}(1 + \cos\Delta\phi), \qquad I_{D2} = \frac{1}{2}(1 - \cos\Delta\phi)$$

With single photons, you don't see continuous intensities. Instead, each photon produces a click at exactly one detector — never both, never neither. The photon is indivisible.

But if you accumulate many clicks, the statistics reproduce the classical interference pattern. If $\Delta\phi = 0$ (constructive interference at D1), every photon clicks at D1. If $\Delta\phi = \pi/2$, each photon has a 50/50 chance of clicking at either detector.

### Which Path Did the Photon Take?

This is the central question — the MZI version of the double-slit argument.

After the first beam splitter, the quantum state of the photon is:

$$|\psi\rangle = \frac{1}{\sqrt{2}}\big(|\text{arm 1}\rangle + |\text{arm 2}\rangle\big)$$

The photon is in a superposition of being in both arms. It accumulates phase in each arm, arrives at the second beam splitter, and interference determines which detector clicks.

How can we be confident the photon really went both ways? Because we see interference. If we block one arm, the interference disappears — both detectors click with equal probability regardless of $\Delta\phi$. The fact that $\Delta\phi$ controls the outcome means both arms must be contributing amplitudes. The only consistent picture is that the single photon traveled both paths simultaneously, as a wave with complex amplitudes along each arm. This is exactly the quantum description we've been building: multiple configurations, each carrying a complex amplitude.

### The Wave Picture Works — Until Detection

So let's trace the photon through the interferometer:

-   At the first beam splitter: the photon is a wave, split into two amplitudes. Fine.
-   Traveling through the arms: still a wave, accumulating phase. Fine.
-   At the second beam splitter: still a wave, interfering. Fine.
-   Right before the detectors: still a wave, with amplitudes at both output ports. Fine.

$$|\psi\rangle = c_1|\text{D1}\rangle + c_2|\text{D2}\rangle$$

At every stage, the wave picture is perfectly logical and consistent. But then the photon hits a detector — and we get a click at exactly one location. The wave, which had amplitude at both detectors, seems to "choose" one.

What happened?

------------------------------------------------------------------------

## The Measurement Problem

This has been a source of a great deal of headache in physics. It's called the **measurement problem**: how does a quantum superposition become a definite classical outcome?

Our goal here is not to resolve the full philosophical debate, but to understand the physical process that occurs during detection. The key framework is called **open quantum system theory** — a formalism that describes what happens to a quantum system when information or energy leaks out into an environment and can never be recovered. We won't develop the full formalism here, but we can understand the essential physics by following the quantum state step by step through the detection process.

### Step 1: Superposition at the Detectors

Just before detection, the photon is in a superposition of arriving at the two detectors:

$$|\Psi\rangle = \frac{1}{\sqrt{2}}|\gamma_{\text{D1}}\rangle + \frac{1}{\sqrt{2}}|\gamma_{\text{D2}}\rangle$$

where $|\gamma_{\text{D1}}\rangle$ means "photon heading toward detector D1" and likewise for D2.

### Step 2: The Photon Interacts with One Electron

The photon hits the semiconductor in one of the detectors and is absorbed, promoting an electron from the valence band to the conduction band. But the photon was in a superposition of *which* detector it was at, so now the superposition transfers to the electron:

$$|\Psi\rangle = \frac{1}{\sqrt{2}}|\text{photon absorbed, } e^-\text{ excited at D1}\rangle + \frac{1}{\sqrt{2}}|\text{photon absorbed, } e^-\text{ excited at D2}\rangle$$

This is still a perfectly valid quantum superposition. The photon is gone, but its "which-detector" superposition has been handed off to the electron. In principle, if we could carefully isolate just these two electrons — one at D1, one at D2 — we could interfere them and still find evidence that the photon went both ways.

### Step 3: The Avalanche

But we don't isolate the electron. In an APD, that electron is accelerated and collides with other atoms, liberating more electrons. Those electrons hit more atoms. Within nanoseconds, the initial single-electron excitation has spread into an avalanche involving billions of electrons, atoms, and phonons.

Now the state looks something like:

$$|\Psi\rangle = \frac{1}{\sqrt{2}}|\text{avalanche}_{\text{D1}},\; \text{quiet}_{\text{D2}}\rangle + \frac{1}{\sqrt{2}}|\text{quiet}_{\text{D1}},\; \text{avalanche}_{\text{D2}}\rangle$$

The superposition hasn't disappeared — it has *spread* to an enormous number of particles.

### Step 4: The Two Branches Become Orthogonal

Here is the crucial point. As the entanglement spreads to more and more particles, the two terms in the superposition **diverge**. By "diverge" we mean they become orthogonal:

$$\langle \text{avalanche}_{\text{D1}},\, \text{quiet}_{\text{D2}} \;|\; \text{quiet}_{\text{D1}},\, \text{avalanche}_{\text{D2}}\rangle \;\approx\; 0$$

When the overlap between two branches of a superposition reaches zero, they can no longer interfere with each other. For all practical purposes, they have become two completely independent macroscopic events — unaware of each other, unable to ever communicate or recombine. You might picture it as two independent simulations running in parallel: one universe where the photon clicked at D1, and another where it clicked at D2.

### The Schrödinger Equation Is Enough

This process is called **decoherence**, and it is described entirely by the Schrödinger equation. We did not need to add any special "measurement postulate" or "wavefunction collapse" rule to quantum mechanics. The Schrödinger equation, applied to the photon *and* the detector *and* the environment together, naturally produces the branching into orthogonal, non-interfering outcomes.

What the Schrödinger equation does *not* do is select one branch over the other. From the perspective of the equation, both branches continue to exist. But from our perspective — trapped inside one branch — it appears that nature "chose" one outcome. We experience a click at D1 *or* D2, never both.

This is admittedly unsatisfying. But it is the current state of the art: the Schrödinger equation fully describes the measurement process, including the apparent randomness, without requiring any additional postulates. The "randomness" we experience emerges from our inability to access the other branch once decoherence has made the two branches orthogonal.

------------------------------------------------------------------------

## Homework 2.3

### Problem 1: Changing Basis

A photon is prepared in the state $|H\rangle$.

**(a)** Write $|H\rangle$ in the circular polarization basis $\{|R\rangle, |L\rangle\}$. That is, find coefficients $\alpha$ and $\beta$ such that $|H\rangle = \alpha|R\rangle + \beta|L\rangle$.

*Hint:* Use the definitions $|R\rangle = \frac{1}{\sqrt{2}}(|H\rangle + i|V\rangle)$ and $|L\rangle = \frac{1}{\sqrt{2}}(|H\rangle - i|V\rangle)$, and solve for $|H\rangle$.

**(b)** If this $|H\rangle$-polarized photon passes through a right-circular polarizer, what is the probability it is transmitted? What about a left-circular polarizer?

**(c)** Do your answers to (b) sum to 1? Why must this be the case?

**(d)** Now write $|D\rangle$ in the circular basis $\{|R\rangle, |L\rangle\}$. What is the probability that a $|D\rangle$-polarized photon passes through an $|R\rangle$ polarizer?

------------------------------------------------------------------------

### Problem 2: Sequential Polarizers

**(a)** A photon is prepared in the state $|H\rangle$ and sent through a $|V\rangle$ polarizer. What fraction of the light is transmitted?

**(b)** Now insert a $|D\rangle$ polarizer between the $|H\rangle$ source and the $|V\rangle$ polarizer. Walk through the sequence step by step: what is the state after each polarizer, and what is the total fraction of light transmitted?

**(c)** Instead of a $|D\rangle$ polarizer, insert a polarizer oriented at angle $\theta$ from horizontal. The corresponding state is $|\theta\rangle = \cos\theta\,|H\rangle + \sin\theta\,|V\rangle$. Show that the total transmission through the sequence $|H\rangle \to |\theta\rangle\text{ polarizer} \to |V\rangle\text{ polarizer}$ is:

$$T(\theta) = \cos^2\theta\,\sin^2\theta = \frac{1}{4}\sin^2(2\theta)$$

What angle $\theta$ maximizes the transmission, and what is the maximum?

**(d)** Now insert $n$ polarizers evenly spaced in angle between 0° and 90°, at angles $\theta_k = k \cdot \frac{90°}{n+1}$ for $k = 1, 2, \ldots, n$. Each consecutive pair of polarizers differs by $\Delta\theta = \frac{90°}{n+1}$. Show that the total transmission is:

$$T(n) = \cos^{2(n+1)}\!\left(\frac{\pi}{2(n+1)}\right)$$

**(e)** Compute $T(n)$ numerically for $n = 1, 2, 3, 5, 10, 50, 100$. What happens as $n \to \infty$?

**(f)** Explain physically why adding more polarizers — each of which can only absorb light — *increases* the total transmission. What does this have to do with measurement changing the state?

------------------------------------------------------------------------

### Problem 3: Polarizer Round Trips

**(a)** A photon starts in state $|H\rangle$ and passes through an $|H\rangle$ polarizer. What is the state afterward? What fraction of the light is transmitted? (This should be trivial — that's the point.)

**(b)** A photon starts in $|H\rangle$ and passes through a $|D\rangle$ polarizer. What is the state afterward? Now that photon passes through an $|H\rangle$ polarizer. What is the state afterward, and what is the total fraction of light transmitted through both polarizers?

**(c)** Compare parts (a) and (b). In both cases the photon starts as $|H\rangle$ and ends (if it survives) as $|H\rangle$. But in (b) only a fraction of the light makes it through. Where did the "lost" light go? What did the intermediate $|D\rangle$ polarizer actually do to the state?

**(d)** Repeat part (b), but replace the $|D\rangle$ polarizer with an $|R\rangle$ (right circular) polarizer. What is the state after the $|R\rangle$ polarizer? What is the state after the final $|H\rangle$ polarizer? What is the total transmission?

**(e)** In part (d), the photon starts as $|H\rangle$ and ends as $|H\rangle$, passing through $|R\rangle$ along the way. But $|R\rangle$ involves a complex phase ($i$). Did the complex phase have any observable effect on the final transmission probability? Why or why not?

------------------------------------------------------------------------

### Problem 4: Can You Tell the Difference?

Alice prepares single photons in one of two ways, but won't tell Bob which:

-   **Method 1:** She prepares every photon in state $|D\rangle$.
-   **Method 2:** She prepares each photon randomly as either $|H\rangle$ or $|V\rangle$ with equal probability (she flips a coin each time).

**(a)** Bob sends each photon through a $|D\rangle$ polarizer. Under Method 1, what fraction of photons pass? Under Method 2, what fraction pass on average? Can Bob distinguish the two methods this way?

**(b)** Bob instead sends each photon through an $|H\rangle$ polarizer. Under Method 1, what fraction pass? Under Method 2, what fraction pass on average? Can Bob distinguish the two methods this way?

**(c)** Can Bob distinguish the methods using a $|V\rangle$ polarizer?

**(d)** Can Bob distinguish the two methods using *any* single polarizer measurement? Try a general polarizer $|\theta\rangle = \cos\theta\,|H\rangle + \sin\theta\,|V\rangle$ and compare the two methods.

*Hint for Method 2:* Average the transmission probability over the two equally likely preparations.

**(e)** Discuss: the states are clearly different — $|D\rangle$ is a definite quantum state, while Method 2 is a statistical mixture. Yet Bob cannot tell them apart with any single polarizer. What type of measurement *would* reveal the difference?

*Hint:* Think about what basis you would need to measure in.

------------------------------------------------------------------------

### Problem 5: The Projector Algebra

**(a)** Write out the projection operator $P_D = |D\rangle\langle D|$ as a $2\times 2$ matrix in the $\{|H\rangle, |V\rangle\}$ basis.

**(b)** Verify by explicit matrix multiplication that $P_D^2 = P_D$.

**(c)** Compute $P_H + P_V$ where $P_H = |H\rangle\langle H|$ and $P_V = |V\rangle\langle V|$. What matrix do you get? Why does this make physical sense?

**(d)** Compute $P_D + P_A$. What do you get? Is this the same as your answer to (c)? Should it be?

**(e)** Compute the product $P_H P_V$. What matrix do you get? Interpret physically: what does it mean to project onto $|V\rangle$ and then onto $|H\rangle$?

**(f)** Compute $P_H P_D P_V$ as a matrix. This represents the three-polarizer sequence H → D → V. Show that this operator is not zero, even though $P_H P_V = 0$. Relate the nonzero matrix elements to the 25% transmission we found in class.

------------------------------------------------------------------------

### Problem 6: Energy Conservation in Measurement

When a photon is absorbed by a polarizer, its energy doesn't disappear — it is transferred to the polarizer material (as heat, lattice vibrations, etc.).

**(a)** In the three-polarizer setup H → D → V with 25% total transmission: what fraction of the photon energy is absorbed by the D polarizer? What fraction by the V polarizer?

**(b)** A single $|H\rangle$ photon with energy $E = h\nu$ enters the three-polarizer sequence. There are four possible outcomes. List them and give the probability of each:

1.  Photon is absorbed by the D polarizer
2.  Photon passes D but is absorbed by V
3.  Photon passes both
4.  Any other outcome?

Verify your probabilities sum to 1.

**(c)** In each outcome, where did the photon's energy $h\nu$ go? Verify that energy is conserved in every case.

------------------------------------------------------------------------

### Problem 7: States on the Bloch Sphere

Recall the Bloch sphere parametrization:

$$|\psi\rangle = \cos\frac{\theta}{2}|H\rangle + e^{i\phi}\sin\frac{\theta}{2}|V\rangle$$

**(a)** A state has Bloch coordinates $\theta = \pi/3$, $\phi = 0$. Write out the state explicitly as $\alpha|H\rangle + \beta|V\rangle$ with numerical values for $\alpha$ and $\beta$.

**(b)** For this state, calculate the probability of passing each of the following polarizers: $|H\rangle$, $|V\rangle$, $|D\rangle$, $|A\rangle$, $|R\rangle$, $|L\rangle$. Verify that each complementary pair sums to 1.

**(c)** Where on the Bloch sphere is this state? Describe its location in words (e.g., "halfway between the north pole and the equator, on the +x side"). Is it closer to $|H\rangle$ or to $|V\rangle$? Do your probabilities from (b) confirm this?

**(d)** The state orthogonal to $|\psi\rangle$ is located at the antipodal point on the Bloch sphere: $\theta' = \pi - \theta$, $\phi' = \phi + \pi$. Find the orthogonal state for the state in part (a) and verify explicitly that $\langle\psi|\psi_\perp\rangle = 0$.

**(e)** Now consider a state with $\theta = \pi/2$, $\phi = \pi/4$. Write out this state. This state lies on the equator — where exactly? It's not one of the six cardinal states. Calculate $P(H)$, $P(D)$, and $P(R)$. Which of the three measurement bases comes closest to giving a definite (probability $\approx 1$) outcome?

**(f)** A state is prepared such that $P(H) = 3/4$ and $P(D) = 1/2$. What are the Bloch coordinates $\theta$ and $\phi$? Is the state uniquely determined by these two probabilities? If not, what additional measurement would pin it down?