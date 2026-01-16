## How a qubit evolves in time: the Schrödinger equation

So far, a qubit is a state vector
$$
|\psi\rangle=\alpha|0\rangle+\beta|1\rangle,
\qquad |\alpha|^2+|\beta|^2=1,
$$
where $\alpha$ and $\beta$ are complex amplitudes.

Now we need the rule that tells us how those amplitudes change in time.

That rule is the Schrödinger equation.

---

### The time-dependent Schrödinger equation

The fundamental law of (nonrelativistic) quantum dynamics is:
$$
i\hbar \frac{d}{dt}|\psi(t)\rangle = H|\psi(t)\rangle.
$$

- $|\psi(t)\rangle$ is the state of the system at time $t$
- $H$ is the **Hamiltonian** (the energy operator)
- $\hbar$ is Planck’s constant divided by $2\pi$

For a closed system, the key property is that $H$ is **Hermitian**. This is what guarantees that total probability stays normalized as the state evolves.

Conceptually:
> $H$ is the thing that generates time evolution, the way “velocity generates motion.”

(We will soon make this idea geometric using the Bloch sphere.)

---

### What does $H$ look like for a qubit?

A qubit has a two-dimensional state space. So in the basis $\{|0\rangle,|1\rangle\}$ the Hamiltonian is a $2\times 2$ matrix:
$$
H=\begin{pmatrix}
H_{00} & H_{01}\\
H_{10} & H_{11}
\end{pmatrix}.
$$

Hermitian means
$$
H_{00},H_{11}\in\mathbb{R},
\qquad
H_{10}=H_{01}^*.
$$

And the state is a two-component column vector:
$$
|\psi(t)\rangle=\begin{pmatrix}\alpha(t)\\ \beta(t)\end{pmatrix}.
$$

Plugging into the Schrödinger equation gives coupled differential equations:
$$
i\hbar \frac{d}{dt}\begin{pmatrix}\alpha\\ \beta\end{pmatrix}
=
\begin{pmatrix}
H_{00} & H_{01}\\
H_{10} & H_{11}
\end{pmatrix}
\begin{pmatrix}\alpha\\ \beta\end{pmatrix}.
$$

This is a “wave equation” for amplitudes. The components $\alpha(t)$ and $\beta(t)$ can change magnitude and phase in time.

---

### The solution has the form “unitary evolution”

For time-independent $H$, the solution can be written:
$$
|\psi(t)\rangle = U(t)|\psi(0)\rangle,
\qquad
U(t)=e^{-iHt/\hbar}.
$$

The operator $U(t)$ is called the **time-evolution operator**.

A crucial property of quantum mechanics is that $U(t)$ is **unitary**:
$$
U^\dagger U = I.
$$

Unitary evolution means:

- normalization is preserved (vector length stays 1)
- time evolution is reversible (before measurement)
- probabilities stay normalized automatically

This is one of the cleanest ways to say what quantum dynamics is:

> Closed quantum systems evolve by unitary transformations.

Later, quantum “gates” will just be particular unitary matrices we know how to implement quickly.

**Figure idea (later):** show a Bloch sphere with a state vector moving along a smooth path, labeled “unitary evolution preserves length.”

---

### Warm-up example: energy eigenstates only pick up phase

Suppose $|0\rangle$ and $|1\rangle$ are energy eigenstates with energies $E_0$ and $E_1$. Then the Hamiltonian is diagonal:
$$
H=\begin{pmatrix}
E_0 & 0\\
0 & E_1
\end{pmatrix}.
$$

The Schrödinger equation separates into:
$$
i\hbar \frac{d\alpha}{dt}=E_0\alpha,
\qquad
i\hbar \frac{d\beta}{dt}=E_1\beta.
$$

The solutions are:
$$
\alpha(t)=\alpha(0)e^{-iE_0 t/\hbar},
\qquad
\beta(t)=\beta(0)e^{-iE_1 t/\hbar}.
$$

So the probabilities $|\alpha|^2$ and $|\beta|^2$ do not change at all.
Only the phases evolve.

What matters physically is the **relative phase**. The relative phase factor is
$$
\frac{\beta(t)}{\alpha(t)}=\frac{\beta(0)}{\alpha(0)}\,e^{-i(E_1-E_0)t/\hbar}.
$$

This is our first concrete reminder:
> Energy causes phase to accumulate.

And relative phase is exactly what controls interference.

**Figure idea (later):** Bloch sphere showing “precession around $z$” (constant latitude, changing azimuth angle).

---

### From time-dependent to time-independent Schrödinger equation (quick connection)

If the Hamiltonian is time-independent, we can look for special solutions of the form
$$
|\psi(t)\rangle = e^{-iEt/\hbar}|E\rangle,
$$
where $|E\rangle$ is a time-independent state vector.

Plugging into
$$
i\hbar \frac{d}{dt}|\psi(t)\rangle = H|\psi(t)\rangle
$$
gives:
$$
H|E\rangle = E|E\rangle.
$$

This is the **time-independent Schrödinger equation**: it is just an eigenvalue equation for the Hamiltonian.

So the familiar story from “particle in a box” is a special case:

- solve $H|E\rangle=E|E\rangle$
- expand any initial state in the energy eigenbasis
- each component picks up a phase $e^{-iEt/\hbar}$

That strategy works for qubits too.

---

### Why this matters for quantum computing

A classical bit “evolves” only when you flip it.

A qubit evolves continuously, because its amplitudes are waves.

Quantum control is the art of choosing a Hamiltonian $H(t)$ so that the evolution implements the gate you want.

You don’t need the full general formula yet. The intuition is enough:

> Gates are not magic. They are controlled time evolution.

In the next section we will introduce the simplest and most important Hamiltonians for a qubit, and see how they produce rotations and **Rabi oscillations**.


## The “knobs” we can turn: Pauli matrices, rotations, and Rabi oscillations

In the last section we learned the rule of motion:
$$
i\hbar \frac{d}{dt}|\psi(t)\rangle = H|\psi(t)\rangle,
\qquad
|\psi(t)\rangle = e^{-iHt/\hbar}|\psi(0)\rangle.
$$

Now we want to make this *practical* for a qubit.

What kinds of Hamiltonians actually show up in the lab?  
And what do they *do* to the state?

---

### Any qubit Hamiltonian can be written using the Pauli matrices

A qubit lives in a two-dimensional space, so $H$ is a $2\times 2$ Hermitian matrix.

A powerful fact (that we will use constantly) is:

> Any $2\times 2$ Hermitian matrix can be written as a linear combination of  
> the identity $I$ and the three Pauli matrices $\sigma_x,\sigma_y,\sigma_z$.

The Pauli matrices are
$$
\sigma_x=
\begin{pmatrix}
0 & 1\\
1 & 0
\end{pmatrix},
\qquad
\sigma_y=
\begin{pmatrix}
0 & -i\\
i & 0
\end{pmatrix},
\qquad
\sigma_z=
\begin{pmatrix}
1 & 0\\
0 & -1
\end{pmatrix}.
$$

So the most general qubit Hamiltonian can be written as
$$
H = a\,I + b_x\sigma_x + b_y\sigma_y + b_z\sigma_z.
$$

It is also common (and physically intuitive) to package those coefficients as a vector:
$$
H = a\,I + \vec{b}\cdot\vec{\sigma},
\qquad
\vec{\sigma}=(\sigma_x,\sigma_y,\sigma_z).
$$

**Figure idea (later):** Bloch sphere with a vector $\vec{b}$ drawn as the rotation axis, labeled “$H\propto \vec{b}\cdot\vec{\sigma}$ rotates about $\vec{b}$.”

---

### The identity term only adds a global phase

The term $aI$ produces evolution
$$
e^{-iat/\hbar}I,
$$
which multiplies the whole state by the same phase. That has no measurable effect.

So for most single-qubit dynamics, we can ignore $aI$ and focus on
$$
H = \vec{b}\cdot\vec{\sigma}.
$$

This is the first place you see something deep but simple:

> The physically relevant part of a qubit Hamiltonian is a 3-number object $\vec{b}$.

Those three numbers will become the three rotation axes of the Bloch sphere.

---

### Example 1: A “$z$-Hamiltonian” changes relative phase

Take
$$
H = \frac{\hbar\omega}{2}\sigma_z
=
\frac{\hbar\omega}{2}
\begin{pmatrix}
1 & 0\\
0 & -1
\end{pmatrix}.
$$

If we start with
$$
|\psi(0)\rangle=\alpha|0\rangle+\beta|1\rangle,
$$
then time evolution gives
$$
|\psi(t)\rangle=
\alpha e^{-i\omega t/2}|0\rangle
+
\beta e^{+i\omega t/2}|1\rangle.
$$

Probabilities in the $\{|0\rangle,|1\rangle\}$ basis do not change, but the relative phase between the two components rotates in time.

This is exactly the kind of evolution you get for a spin in a static magnetic field: it “precesses.”

**Figure idea (later):** Bloch sphere showing a state vector rotating around the $z$ axis (a circle of constant $z$).

---

### Example 2: A “$x$-Hamiltonian” causes oscillations between $|0\rangle$ and $|1\rangle$

Now take
$$
H = \frac{\hbar\Omega}{2}\sigma_x
=
\frac{\hbar\Omega}{2}
\begin{pmatrix}
0 & 1\\
1 & 0
\end{pmatrix}.
$$

This Hamiltonian *mixes* the two basis states. It doesn’t just add phase — it moves amplitude back and forth.

Start the qubit in $|0\rangle$:
$$
|\psi(0)\rangle=|0\rangle.
$$

The time evolution operator is
$$
U(t)=e^{-iHt/\hbar}=e^{-i(\Omega t/2)\sigma_x}.
$$

Using the identity (you can take this as a known result for now),
$$
e^{-i(\theta/2)\sigma_x}=\cos\frac{\theta}{2}\,I - i\sin\frac{\theta}{2}\,\sigma_x,
$$
we get
$$
|\psi(t)\rangle
=
\left(\cos\frac{\Omega t}{2}\,I - i\sin\frac{\Omega t}{2}\,\sigma_x\right)|0\rangle.
$$

Since $\sigma_x|0\rangle=|1\rangle$, this becomes
$$
|\psi(t)\rangle
=
\cos\frac{\Omega t}{2}\,|0\rangle
-
i\sin\frac{\Omega t}{2}\,|1\rangle.
$$

Now the measurement probabilities are
$$
P(0)=\left|\cos\frac{\Omega t}{2}\right|^2=\cos^2\frac{\Omega t}{2},
\qquad
P(1)=\left|\sin\frac{\Omega t}{2}\right|^2=\sin^2\frac{\Omega t}{2}.
$$

So the qubit oscillates between $|0\rangle$ and $|1\rangle$.

These are Rabi oscillations.

**Figure idea (later):** Bloch sphere showing rotation about the $x$ axis, starting at the north pole ($|0\rangle$) and moving toward the south pole ($|1\rangle$). Next to it: a simple plot sketch of $P(1)=\sin^2(\Omega t/2)$ vs $t$ (optional).

---

### The control idea: a pulse is a rotation by a chosen angle

The formula above contains the key “engineering knob”:

If you apply the $\sigma_x$ Hamiltonian for a time $\tau$, the state rotates by an angle
$$
\theta = \Omega \tau.
$$

So:

- apply for $\tau$ such that $\Omega\tau=\pi$  
  $$|0\rangle \rightarrow |1\rangle$$  
  (a full flip)

- apply for $\tau$ such that $\Omega\tau=\pi/2$  
  $$|0\rangle \rightarrow \frac{|0\rangle - i|1\rangle}{\sqrt{2}}$$  
  (a “half flip” into a superposition)

This is exactly how many quantum computers work in practice:

- the qubit has two energy levels (an effective $|0\rangle$ and $|1\rangle$)
- control fields implement effective Hamiltonians proportional to $\sigma_x$ and $\sigma_y$ (rotations in the equatorial plane)
- frequency detuning or phase accumulation implements effective $\sigma_z$ rotations (relative phase shifts)

**Figure idea (later):** a Bloch sphere showing three labeled rotation axes ($x,y,z$), with arrows indicating what kinds of control typically generate each rotation.

---

### The general rule (one line you will use forever)

Up to an irrelevant global phase, any single-qubit Hamiltonian can be written as
$$
H=\frac{\hbar\Omega}{2}\,\hat{n}\cdot\vec{\sigma},
$$
and the unitary evolution
$$
U(t)=e^{-iHt/\hbar}
$$
rotates the Bloch vector about axis $\hat{n}$ by angle
$$
\theta=\Omega t.
$$

This is why the Bloch sphere becomes the main picture for single-qubit control:
it turns “$2\times 2$ complex matrices” into “rotations you can see.”












