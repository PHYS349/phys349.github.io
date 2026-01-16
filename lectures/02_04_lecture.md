##  Programmable interference: a one-qubit “algorithm”

By now you’ve seen the slogan:

> A quantum computer is a programmable interferometer.

Let’s make that concrete with the smallest possible example.

The structure is always:

1. **Split** the wave into multiple paths (create a superposition)  
2. **Imprint phase** differences between paths  
3. **Recombine**  
4. **Measure** and see interference

For a single qubit, the “paths” are the basis configurations $|0\rangle$ and $|1\rangle$.

---

### The one-qubit interferometer circuit

Start in $|0\rangle$ and apply:
$$
|0\rangle \xrightarrow{H} \xrightarrow{R_z(\phi)} \xrightarrow{H} \text{measure in }Z.
$$

Here
$$
R_z(\phi)=e^{-i\phi\sigma_z/2}
=
\begin{pmatrix}
e^{-i\phi/2} & 0\\
0 & e^{+i\phi/2}
\end{pmatrix}.
$$

This gate does not change $|0\rangle$ vs $|1\rangle$ probabilities by itself.  
It only changes relative phase.

---

### Step 1: split with $H$

After the first Hadamard:
$$
H|0\rangle = |+\rangle = \frac{|0\rangle+|1\rangle}{\sqrt{2}}.
$$

Interpretation: the wave is now equally “in both paths.”

---

### Step 2: add a relative phase

Apply $R_z(\phi)$:
$$
R_z(\phi)\frac{|0\rangle+|1\rangle}{\sqrt{2}}
=
\frac{e^{-i\phi/2}|0\rangle+e^{+i\phi/2}|1\rangle}{\sqrt{2}}.
$$

Same probabilities, different relative phase.

---

### Step 3: recombine with $H$

Use:
$$
H|0\rangle=\frac{|0\rangle+|1\rangle}{\sqrt{2}},
\qquad
H|1\rangle=\frac{|0\rangle-|1\rangle}{\sqrt{2}}.
$$

Then
$$
H\left(\frac{e^{-i\phi/2}|0\rangle+e^{+i\phi/2}|1\rangle}{\sqrt{2}}\right)
=
\frac{1}{2}\left(
(e^{-i\phi/2}+e^{+i\phi/2})|0\rangle
+
(e^{-i\phi/2}-e^{+i\phi/2})|1\rangle
\right).
$$

Using
$$
e^{-i\phi/2}+e^{+i\phi/2}=2\cos(\phi/2),
\qquad
e^{-i\phi/2}-e^{+i\phi/2}=-2i\sin(\phi/2),
$$
the output state becomes
$$
|\psi_{\text{out}}\rangle
=
\cos(\phi/2)\,|0\rangle
-
i\sin(\phi/2)\,|1\rangle.
$$

So the measurement probabilities are
$$
P(0)=\cos^2(\phi/2),
\qquad
P(1)=\sin^2(\phi/2).
$$

This is interference in the cleanest possible form.

---

### What this means

- If $\phi=0$, then $P(0)=1$ and $P(1)=0$ (perfect constructive interference into $|0\rangle$).
- If $\phi=\pi$, then $P(0)=P(1)=1/2$.
- If $\phi=2\pi$, it returns to $P(0)=1$.

The circuit converts an invisible phase into a visible probability.

This is the move behind many quantum algorithms:

> Engineer phases so that when paths recombine, wrong answers cancel and right answers add.

**Figure idea:** Bloch sphere showing the sequence
- start at $|0\rangle$ (north pole)
- $H$ moves to $|+\rangle$ (+x direction)
- $R_z(\phi)$ rotates around the $z$ axis by $\phi$ along the equator
- final $H$ rotates back so the $Z$ measurement probability depends on $\phi$

---

### Polarization version (optional anchor)

This is the same logic as a Mach–Zehnder interferometer:

- first beamsplitter $\leftrightarrow$ first $H$
- phase shifter in one arm $\leftrightarrow$ $R_z(\phi)$
- second beamsplitter $\leftrightarrow$ second $H$
- detectors $\leftrightarrow$ $Z$ measurement

If you prefer polarization: split into $H/V$ components, add a phase delay (waveplate), then rotate basis and measure.

---

## 3.8 Teaser: two qubits is where “exponential” becomes real

One qubit has two basis configurations:
$$
|0\rangle,\;|1\rangle,
$$
so its most general state needs two complex amplitudes:
$$
|\psi\rangle=\alpha|0\rangle+\beta|1\rangle.
$$

Two qubits have four basis configurations:
$$
|00\rangle,\;|01\rangle,\;|10\rangle,\;|11\rangle,
$$
so the most general state needs four complex amplitudes:
$$
|\Psi\rangle
=
c_{00}|00\rangle+c_{01}|01\rangle+c_{10}|10\rangle+c_{11}|11\rangle,
\qquad
\sum_{ab\in\{0,1\}^2}|c_{ab}|^2=1.
$$

This is the first place the “configuration wave” idea scales up directly:

> Combining systems means every configuration pairs with every configuration.

Some two-qubit states are just combinations of independent qubits (product states).  
But some are not. For example,
$$
|\Phi^+\rangle=\frac{|00\rangle+|11\rangle}{\sqrt{2}}
$$
cannot be written as “qubit A times qubit B.”

That new phenomenon is **entanglement**.

Next lecture we will make the tensor-product rule concrete and see how entanglement appears naturally once we can do two-qubit gates.
