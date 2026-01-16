##  Measurement: turning a qubit into a classical bit

So far, everything we’ve done has been reversible.  
A gate is a unitary matrix $U$, and in principle we can always undo it with $U^\dagger$.

Measurement is different. Measurement is the step where we get a definite classical outcome (a bit we can write down).

---

### Measurements come with a basis

A measurement is not one universal action. It depends on what question you ask.

For a qubit, the most common question is:

> Are you in $|0\rangle$ or $|1\rangle$?

That is a measurement in the **computational (Z) basis**:
$$
\{|0\rangle,\;|1\rangle\}.
$$

But we can ask different questions by choosing a different basis, like
$$
\{|+\rangle,\;|-\rangle\}
\quad\text{or}\quad
\{|+i\rangle,\;|-i\rangle\}.
$$

This is the key idea:

> A measurement is “choose a basis, then ask which basis state you are in.”

---

### The Born rule

Suppose the qubit is in the state
$$
|\psi\rangle=\alpha|0\rangle+\beta|1\rangle,
\qquad |\alpha|^2+|\beta|^2=1.
$$

If we measure in the $\{|0\rangle,|1\rangle\}$ basis, then the probabilities are:
$$
P(0)=|\alpha|^2,
\qquad
P(1)=|\beta|^2.
$$

After the measurement, the state is no longer a superposition in that basis:

- if the outcome is $0$, the post-measurement state becomes $|0\rangle$
- if the outcome is $1$, the post-measurement state becomes $|1\rangle$

This is the content of the measurement postulate in its simplest form.

---

### Projectors: a clean way to write measurements

A measurement in the $Z$ basis can be written using **projection operators**:
$$
P_0 = |0\rangle\langle 0|,
\qquad
P_1 = |1\rangle\langle 1|.
$$

These satisfy:
$$
P_0^2=P_0,\quad P_1^2=P_1,
\qquad
P_0+P_1=I.
$$

The Born rule can be written compactly as:
$$
P(0)=\langle\psi|P_0|\psi\rangle,
\qquad
P(1)=\langle\psi|P_1|\psi\rangle.
$$

And the state update rule can be written as a normalized projection. For example, if outcome $0$ occurs:
$$
|\psi\rangle \rightarrow \frac{P_0|\psi\rangle}{\sqrt{\langle\psi|P_0|\psi\rangle}}.
$$

You do not need to memorize this form. The point is the picture:

> Measuring in a basis means projecting the state onto one of the basis directions.

---

### Measuring in a different basis is still the Born rule

Define the $X$-basis states:
$$
|+\rangle=\frac{|0\rangle+|1\rangle}{\sqrt{2}},
\qquad
|-\rangle=\frac{|0\rangle-|1\rangle}{\sqrt{2}}.
$$

If we measure in the $X$ basis, the amplitudes for outcomes $+$ and $-$ are inner products:
$$
\langle +|\psi\rangle=\frac{\alpha+\beta}{\sqrt{2}},
\qquad
\langle -|\psi\rangle=\frac{\alpha-\beta}{\sqrt{2}}.
$$

So the probabilities are:
$$
P(+)=\left|\frac{\alpha+\beta}{\sqrt{2}}\right|^2,
\qquad
P(-)=\left|\frac{\alpha-\beta}{\sqrt{2}}\right|^2.
$$

Notice what appears:

- $\alpha+\beta$ and $\alpha-\beta$ are interference sums
- the relative phase between $\alpha$ and $\beta$ changes whether these add or cancel

This is the operational meaning of “phase matters.”

**Figure idea:** Bloch sphere with the $Z$ axis labeled $|0\rangle,|1\rangle$ and the $X$ axis labeled $|+\rangle,|-\rangle$. Emphasize that “measuring in $X$” means projecting onto the $X$ axis.

---

### Hardware viewpoint: rotate, then measure $Z$

Many quantum devices effectively measure in the $Z$ basis by default.  
To measure in another basis, you rotate the state first.

To measure in the $X$ basis:
$$
\text{(measure in $X$)} \equiv \text{apply }H\text{, then measure in }Z.
$$

Why this works:
$$
H|+\rangle=|0\rangle,
\qquad
H|-\rangle=|1\rangle.
$$

So an $X$-basis question becomes a $Z$-basis question after a basis change.

Similarly, measuring in the $Y$ basis can be done by a different rotation (we’ll return to this once we introduce the $S$ gate and $R_y$ rotations).

**Figure idea:** simple circuit diagram:
- “measure $X$” drawn as $H$ followed by a measurement meter in the $Z$ basis.

---

##  Stern–Gerlach: measurement basis is a physical choice

A Stern–Gerlach apparatus is the classic way to measure spin.

If you send spin-$1/2$ particles through a Stern–Gerlach magnet aligned along $z$:

- you get two spots on the screen
- the outcomes correspond to the two eigenstates
$$
|\uparrow_z\rangle \equiv |0\rangle,
\qquad
|\downarrow_z\rangle \equiv |1\rangle.
$$

If you rotate the apparatus to align along $x$, you are measuring in a different basis:
$$
|\uparrow_x\rangle \equiv |+\rangle,
\qquad
|\downarrow_x\rangle \equiv |-\rangle.
$$

This is the key bridge:

> “Measure spin along different axes” means “measure in different bases.”

**Figure idea:** two Stern–Gerlach setups: one labeled $z$, one labeled $x$, with corresponding Bloch-sphere axes.

---

### A simple sequence that shows quantum randomness

Start with particles prepared in $|\uparrow_z\rangle$.

1) Measure in $Z$: you always get $\uparrow_z$.

2) Now measure the same beam in $X$: you get $\uparrow_x$ or $\downarrow_x$ with equal probability:
$$
P(\uparrow_x)=P(\downarrow_x)=\frac{1}{2}.
$$

3) Now measure again in $Z$: you again get $\uparrow_z$ or $\downarrow_z$ with equal probability.

The middle measurement changed the state. This is not “we disturbed it with bad equipment.”  
It is the rule: the act of measuring in a basis prepares an eigenstate of that basis.

---

##  Polarization: a one-photon measurement you can picture

Polarization is a perfect qubit because it is easy to visualize measurement.

Let
$$
|H\rangle \equiv |0\rangle,\qquad |V\rangle \equiv |1\rangle.
$$

A polarizing beam splitter (PBS) performs a measurement in the $\{|H\rangle,|V\rangle\}$ basis.

If a single photon is in state
$$
|\psi\rangle=\alpha|H\rangle+\beta|V\rangle,
$$
then:

- with probability $|\alpha|^2$ it exits the $H$ port
- with probability $|\beta|^2$ it exits the $V$ port

For a single photon, you do not see “half intensity.”  
You get one detector click or the other. The probabilities appear only after many repeated trials.

---

### The three-polarizer effect (state update in action)

Put two polarizers at $90^\circ$ relative angle: ideally, no light gets through.

Now insert a third polarizer at $45^\circ$ between them. Some light gets through.

What is happening in qubit language?

- The first polarizer measures in one basis and prepares the photon in that polarization state.
- The middle polarizer measures in a rotated basis and prepares a new state with a component along the final polarizer.
- The final polarizer measures again.

The middle measurement changes the state, so “blocked forever” is no longer true.

This is a clean, hands-on example of:

> Measurements are basis-dependent projections, and projections change the state.

**Figure idea:** three polarizers drawn as axes on the Poincaré/Bloch sphere: first at $0^\circ$, middle at $45^\circ$, last at $90^\circ$. Show the step-by-step projection.

---

### Key takeaways

- Measurement chooses a basis and returns a definite classical outcome.
- Probabilities come from squared magnitudes (Born rule).
- Projectors give a compact way to write the probability and state update rules.
- Measuring in different bases is real physics (Stern–Gerlach, polarization optics).
- Phase may be invisible in one basis, but it becomes visible when you rotate basis and interfere amplitudes.

Next section: we’ll use a short circuit (a one-qubit interferometer) to show how a phase shift becomes a measurable probability difference, and why this is the core move in quantum algorithms.
