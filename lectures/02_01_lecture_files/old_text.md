


























































# OLD 


# Lecture: The Qubit  

## Complex numbers are the natural language of waves

In the last lecture we hit the “exponential wall”: quantum systems are hard to simulate because they don’t sit in one configuration at a time. They exist in superposition—many configurations at once—each carrying an amplitude and a phase. Today we start building the formalism that makes that sentence precise.

Before we can talk about qubits, we need one tool: complex numbers. Complex numbers are the cleanest way to represent waves, and quantum states behave like waves.

In many parts of classical physics, we use complex numbers as a convenient representation of real waves. In quantum mechanics, complex amplitudes are not just a bookkeeping trick: the state is naturally described by complex numbers, and measurable outcomes depend on how those complex numbers add.

### Real waves to complex waves

A simple oscillation can be written as
$$
A\cos(\omega t + \phi).
$$

This has two independent pieces of information:

- amplitude $A$: how big the wave is  
- phase $\phi$: where you are in the cycle

Using Euler’s formula,
$$
e^{i\theta}=\cos\theta+i\sin\theta,
$$
we can represent the same oscillation using a complex exponential:
$$
\psi(t)=A e^{i(\omega t+\phi)}.
$$

A complex number is naturally represented as an arrow in the complex plane:

- the length of the arrow is the magnitude $|\psi|$ (here $|\psi| = A$)  
- the angle is the phase $\arg(\psi)$ (here $\arg(\psi)=\omega t+\phi$)

If you want the usual real wave, you can take the real part:
$$
A\cos(\omega t+\phi)=\mathrm{Re}\!\left(Ae^{i(\omega t+\phi)}\right).
$$

### Interference with complex waves

%![[Pasted image 20260108130250.png]]

%![[Pasted image 20260108131311.png]]

Interference happens when multiple waves contribute to the same outcome, and the contributions add before you square.

If two contributions arrive at the same place, the total is
$$
\psi_{\text{total}} = \psi_1 + \psi_2.
$$

For a prototypical interference setup (like the double slit), suppose an amplitude can arrive by two routes (two slits, two paths, two computational “branches”). Each route contributes a complex phase.

Define the total amplitude
$$
\Psi = \frac{e^{i\theta_1} + e^{i\theta_2}}{2}.
$$

Then the observed probability is
$$
P = |\Psi|^2.
$$

Compute:
$$
|\Psi|^2
= \frac{1}{4}\left|e^{i\theta_1} + e^{i\theta_2}\right|^2
= \frac{1}{4}(e^{i\theta_1}+e^{i\theta_2})(e^{-i\theta_1}+e^{-i\theta_2}).
$$

Multiply out:
$$
|\Psi|^2
= \frac{1}{4}\left(2 + e^{i(\theta_1-\theta_2)} + e^{-i(\theta_1-\theta_2)}\right).
$$

Using $e^{ix}+e^{-ix}=2\cos x$:
$$
P = |\Psi|^2 = \frac{1}{2}\big(1+\cos(\theta_1-\theta_2)\big)
= \cos^2\!\left(\frac{\theta_1-\theta_2}{2}\right).
$$

So everything depends only on the phase difference
$$
\Delta\theta = \theta_1-\theta_2.
$$

- $\Delta\theta = 0$ gives $P=1$ (constructive interference)  
- $\Delta\theta = \pi$ gives $P=0$ (destructive interference)

Geometrically: each term is an arrow in the complex plane. Two arrows pointing the same direction add to a bigger arrow. Two arrows pointing opposite directions cancel.

Interference is vector addition in the complex plane.

What do we actually observe? For light we observe intensity; in quantum mechanics we observe probabilities. In both cases the rule has the same structure:
$$
I \propto |\Psi|^2
\qquad \text{or} \qquad
P = |\Psi|^2 \ \ (\text{after normalization}).
$$

Notice what happens for a single contribution:
$$
|A e^{i\theta}|^2 = A^2.
$$

So the phase of a single isolated term is not directly observable. Phase becomes observable only through comparisons—when multiple contributions can add and interfere.

A preview of an important distinction we will return to:
- changing all phases together (a global phase) does not change probabilities
- changing phases relative to each other (a relative phase) does

## Quantum computing is interference of configurations

This is the first time we can say, precisely, what “quantum computation” is doing:

- A quantum state assigns a complex number to each configuration.
- Gates change those complex numbers (especially their relative phases).
- At the end we measure probabilities using $|\cdot|^2$.
- Algorithms work by engineering interference:
  - wrong paths cancel  
  - right paths add  

In a wave on a string, the label might be position $x$: $\psi(x)$.  
In quantum mechanics, the label might be a configuration: $|0\rangle$ or $|1\rangle$, and later $|00\rangle,|01\rangle,\ldots$

The big idea we carry forward is:

Every configuration gets a complex amplitude (a magnitude and a phase). Those amplitudes add and interfere.

In the next section we will take the simplest possible configuration space—two configurations—and define the qubit.

A general two-configuration quantum state will look like
$$
|\psi\rangle = c_0|0\rangle + c_1|1\rangle,
$$
where $c_0$ and $c_1$ are complex numbers.



### States are vectors (Dirac notation)

We choose a basis and label it
$$
|0\rangle,\quad |1\rangle.
$$

A general qubit state is a superposition:
$$
|\psi\rangle = \alpha|0\rangle + \beta|1\rangle,
$$
where $\alpha$ and $\beta$ are complex amplitudes.

If we represent the state as a column vector in the $\{|0\rangle,|1\rangle\}$ basis,
$$
|0\rangle = \begin{pmatrix}1\\0\end{pmatrix},\quad
|1\rangle = \begin{pmatrix}0\\1\end{pmatrix},
$$
then
$$
|\psi\rangle = \begin{pmatrix}\alpha\\\beta\end{pmatrix}.
$$

Dirac notation is useful because it reminds us that the state is abstract; the column vector is just its coordinates in a chosen basis.

### Born rule (how probabilities appear)

If we measure in the $\{|0\rangle,|1\rangle\}$ basis, the outcomes are $0$ or $1$.

The Born rule says:
$$
P(0)=|\alpha|^2,\qquad P(1)=|\beta|^2.
$$

So the complex numbers $\alpha$ and $\beta$ are not probabilities. They are amplitudes. Probabilities come from magnitudes squared.

This is also where we have to separate two ideas:

- A classical probability distribution means the system is really in one state or the other, but we do not know which.
- A quantum superposition means the state has both components at once, with a definite relative phase.

Whether that relative phase is well defined (and remains stable) is what we call coherence. When the relative phase becomes scrambled by the environment, interference disappears.

### Rule 1: normalization

A physical quantum state must assign total probability 1:
$$
|\alpha|^2 + |\beta|^2 = 1.
$$

### Rule 2: global phase does not matter

Consider two states
$$
|\psi\rangle \quad \text{and} \quad e^{i\gamma}|\psi\rangle.
$$

Multiplying the entire state by the same complex phase rotates every amplitude arrow together. It never changes any measurement probabilities, because
$$
|e^{i\gamma}\alpha|^2 = |\alpha|^2,\qquad |e^{i\gamma}\beta|^2 = |\beta|^2.
$$

So global phase is not physically observable.

Even though overall phase is irrelevant, the relative phase between $\alpha$ and $\beta$ is real physics.

A common way to write a general qubit state (after removing global phase) is:
$$
|\psi\rangle = \cos\frac{\theta}{2}\,|0\rangle + e^{i\phi}\sin\frac{\theta}{2}\,|1\rangle.
$$
Here:

- $\theta$ sets the relative weights of $|0\rangle$ and $|1\rangle$
- $\phi$ is the relative phase between the two components

### Relative phase matters: $|+\rangle$ vs $|-\rangle$

Consider these two states:
$$
|+\rangle = \frac{|0\rangle+|1\rangle}{\sqrt{2}},
\qquad
|-\rangle = \frac{|0\rangle-|1\rangle}{\sqrt{2}}.
$$

If you measure in the computational basis $\{|0\rangle,|1\rangle\}$, both give $50/50$ outcomes:
$$
P(0)=P(1)=\frac{1}{2}.
$$

The only difference is a relative minus sign (a relative phase shift of $\pi$). But they are not the same state. In fact, they are orthogonal:
$$
\langle +|-\rangle = 0.
$$

So there exists a measurement basis that distinguishes them perfectly. (In fact, $\{|+\rangle,|-\rangle\}$ is itself a measurement basis.)

This is the first hint of how quantum information works:

>Quantum information is stored not just in probabilities, but in phase relationships that show up through interference.



### The Bloch sphere (pure states of one qubit)

We now have two key facts:

1. The state must be normalized:
$$
|\alpha|^2+|\beta|^2=1.
$$

2. Global phase does not matter:
$$
|\psi\rangle \sim e^{i\gamma}|\psi\rangle.
$$

Together, these imply something powerful:

A pure qubit state has only two physically meaningful continuous parameters.

A convenient way to write any qubit state (after removing global phase) is
$$
|\psi\rangle = \cos\frac{\theta}{2}\,|0\rangle + e^{i\phi}\sin\frac{\theta}{2}\,|1\rangle,
$$
where
- $\theta \in [0,\pi]$
- $\phi \in [0,2\pi)$

These are exactly the coordinates you would use to specify a point on a sphere.

So the set of all pure qubit states can be represented as points on the surface of a sphere: the Bloch sphere.

---

#### Mapping a qubit to a point

Given
$$
|\psi\rangle = \cos\frac{\theta}{2}\,|0\rangle + e^{i\phi}\sin\frac{\theta}{2}\,|1\rangle,
$$
we associate it with the point on the unit sphere with coordinates
$$
x=\sin\theta\cos\phi,\qquad
y=\sin\theta\sin\phi,\qquad
z=\cos\theta.
$$

Interpretation:

- $\theta$ controls the relative weights of $|0\rangle$ and $|1\rangle$
- $\phi$ is the relative phase between the two components

Global phase is not shown on the sphere; it has been quotiented out.

---

#### Landmarks on the sphere

The computational basis states live at the poles:

- north pole: $|0\rangle$
- south pole: $|1\rangle$

The equator contains equal-weight superpositions. For example:
$$
|+\rangle = \frac{|0\rangle+|1\rangle}{\sqrt2},
\qquad
|-\rangle = \frac{|0\rangle-|1\rangle}{\sqrt2}.
$$
These differ only by a relative phase of $\pi$, so they appear as opposite points on the equator.

Another pair on the equator is
$$
|i+\rangle = \frac{|0\rangle+i|1\rangle}{\sqrt2},
\qquad
|i-\rangle = \frac{|0\rangle-i|1\rangle}{\sqrt2}.
$$

---

#### Measurement along an axis

The Bloch sphere also helps you visualize measurement.

Measuring in the computational basis $\{|0\rangle,|1\rangle\}$ is “measuring along the $z$ axis.”

If
$$
|\psi\rangle = \cos\frac{\theta}{2}\,|0\rangle + e^{i\phi}\sin\frac{\theta}{2}\,|1\rangle,
$$
then
$$
P(0)=\cos^2\frac{\theta}{2},\qquad
P(1)=\sin^2\frac{\theta}{2}.
$$

Notice that these probabilities depend only on $\theta$ (how far north/south you are), not on $\phi$.
The phase $\phi$ becomes visible when you measure in a different basis (for example the $\{|+\rangle,|-\rangle\}$ basis, which corresponds to the $x$ axis).

This connects directly to the earlier lesson:
- phase is not directly observable in a single basis
- phase shows up through interference when you choose the right measurement


## Connection to polarization (a very concrete qubit)

A photon’s polarization is one of the cleanest “real life” qubits because the complex amplitudes are literally the two components of the electric field.

### Polarization as a two-configuration wave

Fix a propagation direction (say the photon travels along $z$). Then the electric field lives in the transverse plane and can be decomposed into two basis directions. A common choice is horizontal/vertical:
$$
|0\rangle \equiv |H\rangle,\qquad |1\rangle \equiv |V\rangle.
$$

A general polarization state is
$$
|\psi\rangle = \alpha|H\rangle + \beta|V\rangle,
\qquad |\alpha|^2+|\beta|^2=1,
$$
where $\alpha$ and $\beta$ are complex.

If you like a more “waves first” picture, you can think of a classical polarization (Jones vector) as
$$
\mathbf{E}(t) = \mathrm{Re}\!\left[
\begin{pmatrix}\alpha\\ \beta\end{pmatrix}
e^{-i\omega t}
\right].
$$
The key point for us: the *relative phase* between $\alpha$ and $\beta$ controls the shape of the polarization (linear vs elliptical vs circular). This makes phase feel physically real.

### Special qubit states become familiar polarizations

The equal-weight superpositions with different relative phases correspond to standard polarization states.

Diagonal/anti-diagonal (the “plus/minus” states):
$$
|D\rangle = \frac{|H\rangle+|V\rangle}{\sqrt{2}},
\qquad
|A\rangle = \frac{|H\rangle-|V\rangle}{\sqrt{2}}.
$$

Right/left circular (the “$\pm i$” states):
$$
|R\rangle = \frac{|H\rangle+i|V\rangle}{\sqrt{2}},
\qquad
|L\rangle = \frac{|H\rangle-i|V\rangle}{\sqrt{2}}.
$$

Notice what this says:
- $|D\rangle$ and $|A\rangle$ differ only by a relative phase of $\pi$ (a minus sign).
- $|R\rangle$ and $|L\rangle$ differ by a relative phase of $\pm\pi/2$.

So polarization is already teaching you the core lesson of qubits:
relative phase changes the physics, even when $|\alpha|^2$ and $|\beta|^2$ stay the same.

### A single-photon measurement makes the probabilities feel real

Here is the cleanest experiment.

Prepare a photon in the state $|H\rangle$. Now measure in the diagonal basis $\{|D\rangle,|A\rangle\}$.  
A diagonal polarizer is exactly this measurement: it transmits the $|D\rangle$ component and blocks the orthogonal $|A\rangle$ component.

To see the probabilities, rewrite $|H\rangle$ in the $\{|D\rangle,|A\rangle\}$ basis. Using
$$
|D\rangle = \frac{|H\rangle+|V\rangle}{\sqrt{2}},
\qquad
|A\rangle = \frac{|H\rangle-|V\rangle}{\sqrt{2}},
$$
we can invert to get
$$
|H\rangle = \frac{|D\rangle+|A\rangle}{\sqrt{2}},
\qquad
|V\rangle = \frac{|D\rangle-|A\rangle}{\sqrt{2}}.
$$

So if the photon starts in $|H\rangle$, its amplitudes in the diagonal basis are
$$
\alpha_D=\frac{1}{\sqrt{2}},\qquad \alpha_A=\frac{1}{\sqrt{2}}.
$$
The Born rule then says
$$
P(D)=|\alpha_D|^2=\frac{1}{2},\qquad
P(A)=|\alpha_A|^2=\frac{1}{2}.
$$

This is the moment where the “wave picture” becomes unavoidable:

- For a bright laser beam, you see about half the intensity transmitted.
- For a *single photon*, you do not get “half a photon.”

You get a definite outcome:
- the photon is transmitted (a $D$ outcome), or
- it is absorbed/blocked (an $A$ outcome),

and which one happens is fundamentally probabilistic, with probabilities set by $|\cdot|^2$.

After the measurement, the state is no longer $|H\rangle$. If the photon is transmitted, its post-measurement state is
$$
|\psi\rangle \rightarrow |D\rangle.
$$
If it is blocked, the experiment ends for that photon.

This is a good way to say it in one line:

> A polarizer is a measurement device: it produces a classical outcome and it prepares the post-measurement state.

### The three-polarizer “paradox” (how inserting a measurement can increase transmission)

Now a classic demonstration that students remember.

Start with light prepared in $|H\rangle$ and send it through two polarizers:
1. a horizontal polarizer ($H$),
2. then a vertical polarizer ($V$).

Since $|H\rangle$ and $|V\rangle$ are orthogonal, essentially no light gets through the second one.

Now insert a third polarizer at $45^\circ$ in between, i.e. a $D$ polarizer:

$$
H \;\rightarrow\; D \;\rightarrow\; V.
$$

Suddenly, some light gets through.

What happened?

The middle polarizer is a measurement in the $\{|D\rangle,|A\rangle\}$ basis. If a photon is transmitted by the $D$ polarizer, its state becomes $|D\rangle$. But $|D\rangle$ is *not* orthogonal to $|V\rangle$:
$$
|D\rangle=\frac{|H\rangle+|V\rangle}{\sqrt{2}}
\quad\Rightarrow\quad
\langle V|D\rangle = \frac{1}{\sqrt{2}}.
$$
So a photon in state $|D\rangle$ has probability
$$
P(V\ \text{after}\ D)=|\langle V|D\rangle|^2=\frac{1}{2}
$$
to pass the final vertical polarizer.

If you want the overall probability (starting from $|H\rangle$):

- $|H\rangle \rightarrow D$ succeeds with probability $|\langle D|H\rangle|^2=1/2$
- then $D \rightarrow |V\rangle$ succeeds with probability $|\langle V|D\rangle|^2=1/2$

So
$$
P(\text{pass all three})=\frac{1}{2}\cdot\frac{1}{2}=\frac{1}{4}.
$$

With crossed polarizers alone it was (ideally) $0$. With the middle polarizer it becomes $1/4$.

The key lesson is not “a trick polarizer lets light sneak through.” The key lesson is:

> Each measurement changes the state. Changing the measurement basis changes what components exist for the next step.

This is exactly the logic we will use in quantum circuits:
rotate the basis (a gate), then measure.

### The Poincaré sphere is the Bloch sphere (for polarization)

Optics has been using the Bloch-sphere geometry for a long time under a different name: the **Poincaré sphere**.

A pure polarization state corresponds to a point on a sphere:
- the north/south poles are $|H\rangle$ and $|V\rangle$ (with our convention)
- points on the equator are equal-weight superpositions (linear polarizations at different angles)
- opposite points correspond to orthogonal polarizations

A useful bridge formula is that the Bloch vector components can be written directly from amplitudes:
$$
x = 2\,\mathrm{Re}(\alpha^*\beta),\qquad
y = 2\,\mathrm{Im}(\alpha^*\beta),\qquad
z = |\alpha|^2-|\beta|^2.
$$
These are (up to naming/sign conventions) exactly the normalized Stokes parameters used in optics.

So the same picture we use for abstract qubits is already a standard picture in polarization.

### Measurements and optical elements (why this is such a good model)

- A polarizing beam splitter (PBS) or polarizer performs a measurement in the $\{|H\rangle,|V\rangle\}$ basis.
- Rotate the polarizer by $45^\circ$ and you measure in the $\{|D\rangle,|A\rangle\}$ basis.
- With waveplates + a PBS you can effectively measure in the circular basis $\{|R\rangle,|L\rangle\}$.

Waveplates are “gates”:
- a quarter-wave plate changes relative phase and can convert linear $\leftrightarrow$ circular polarization
- a half-wave plate rotates linear polarization direction

On the Poincaré/Bloch sphere, these optical elements correspond to rotations of the state vector. That is exactly the picture we will use for single-qubit gates.

**Figure idea (later):** a Poincaré/Bloch sphere labeled both ways:
- Bloch: $|0\rangle,|1\rangle,|+\rangle,|-\rangle,|+i\rangle,|-i\rangle$
- Polarization: $|H\rangle,|V\rangle,|D\rangle,|A\rangle,|R\rangle,|L\rangle$
emphasizing that it is the same sphere with different names.

