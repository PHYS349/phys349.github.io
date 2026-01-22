# Lecture 2.1: Interference

## Why Are We Here?

In Chapter 1, we saw that quantum mechanics assigns complex amplitudes to configurations, and these amplitudes can interfere. But we haven't yet asked the deeper question:

> **Why complex numbers?**

Today we'll see that complex numbers aren't just a convenient trick—they're the natural language for systems with amplitude and phase. And we'll see that quantum mechanics is *fundamentally* complex in a way that classical physics is not.

{ i want ot make this intro more about the interference the interfoermeter - see an interfereometer as a simple qubit operation. }

---

## Two Wave Equations: A Tale of Real vs Complex

### The Classical Wave Equation

In classical physics—light, sound, water waves—the wave equation is:

$$
\frac{\partial^2 E}{\partial x^2} = \frac{1}{c^2} \frac{\partial^2 E}{\partial t^2}
$$

This is a *real* equation. The solutions are real functions like $E(x,t) = A\cos(kx - \omega t)$.

When we use complex exponentials to solve this equation:

$$
\tilde{E}(x,t) = A e^{i(kx - \omega t)}
$$

this is a **trick**. We always take the real part at the end to get the physical field. The complex numbers are bookkeeping.

### The Schrödinger Equation

Now compare the Schrödinger equation:

$$
i\hbar \frac{\partial \psi}{\partial t} = -\frac{\hbar^2}{2m}\frac{\partial^2 \psi}{\partial x^2} + V\psi
$$

Notice the $i$ on the left side. This is not a trick—it's built into the equation. The wavefunction $\psi$ is *irreducibly complex*. If you try to use only real numbers, the equation doesn't work.

```{admonition} The Key Difference
:class: important

**Classical waves:** Complex numbers are a convenient trick. Real part is physical.

**Quantum mechanics:** Complex numbers are fundamental. The $i$ in Schrödinger's equation is essential.
```

Why does nature use complex numbers at the quantum level? We'll build toward an answer over the next few lectures. For now, let's understand what complex numbers *do*.

---

## Complex Numbers: Amplitude and Phase in One Package

A complex number $z$ can be written two ways:

**Cartesian form:** $z = x + iy$ (real and imaginary parts)

**Polar form:** $z = re^{i\theta}$ (magnitude and angle)

The connection is Euler's formula:

$$
e^{i\theta} = \cos\theta + i\sin\theta
$$

So a complex number encodes two pieces of information:
- **Magnitude:** $r = |z| = \sqrt{x^2 + y^2}$
- **Phase:** $\theta = \arg(z)$

**Figure:** [Draw complex plane with arrow from origin to point $z$, showing $r$ as length and $\theta$ as angle from real axis]

### Why This Packaging Matters

The beautiful thing about polar form is how operations work:

| Operation | Result |
|-----------|--------|
| $z_1 \cdot z_2$ | Multiply magnitudes, **add phases**: $r_1 r_2 e^{i(\theta_1 + \theta_2)}$ |
| $z_1 + z_2$ | Vector addition in the complex plane |
| $\|z\|^2 = z^* z$ | $r^2$ (phase disappears!) |

The last one is crucial: when we compute $|z|^2$, the phase vanishes. But if we *add* two complex numbers first, their phases interact before we square.

This is interference.

---

## Aside: Complex Numbers as Rotations (Group Theory Preview)

Here's a deeper way to think about phase.

Multiplying by $e^{i\theta}$ rotates a complex number by angle $\theta$:

$$
z \mapsto e^{i\theta} z
$$

The set of all such rotations forms a **group**—specifically, the group $SO(2)$ of rotations in 2D:

$$
R(\theta_1) R(\theta_2) = R(\theta_1 + \theta_2)
$$

$$
R(\theta) R(-\theta) = R(0) = I
$$

Every rotation has an inverse, and rotations compose by adding angles.

```{admonition} Group Theory Preview
:class: note

A **group** is a set with an operation that:
1. Is associative: $(ab)c = a(bc)$
2. Has an identity: $ae = ea = a$
3. Has inverses: $aa^{-1} = e$

Rotations in 2D form the group $SO(2)$. Rotations in 3D form $SO(3)$. These will become important when we study spin.
```

So complex numbers naturally encode $SO(2)$—the group of 2D rotations. The phase of a complex amplitude is a rotation angle.

Why does quantum mechanics care about rotations? That's a question we'll answer when we study spin and the Pauli matrices. For now, file this away: **phase is rotation**.

---

## Interference: Where Phase Becomes Observable

If phase is like an internal rotation angle, how do we ever see its effects? After all, when we measure $|z|^2$, the phase disappears.

The answer: **we compare two phases**.

### The Michelson Interferometer

{I think we want to change this back to MZI. Michelson is too cmpact. Michelson would be a fun HW problem though.}


The Michelson interferometer is the cleanest demonstration of interference. Light takes two paths and recombines. The *relative* phase between the paths determines the output.

**Figure:** [Draw Michelson interferometer: source → beam splitter → two perpendicular arms with mirrors → beams return and recombine → detector]

**Setup:**
1. A beam splitter sends light into two perpendicular arms
2. Mirrors reflect light back along each arm
3. The beams recombine at the beam splitter
4. A detector measures the output intensity

Let the two arms have lengths $L_1$ and $L_2$. Light travels to each mirror and back, so the path lengths are $2L_1$ and $2L_2$.

The phase accumulated along each path:

$$
\phi_1 = k \cdot 2L_1 = \frac{2\pi}{\lambda} \cdot 2L_1
$$

$$
\phi_2 = k \cdot 2L_2 = \frac{2\pi}{\lambda} \cdot 2L_2
$$

The amplitudes from each arm recombine at the detector. Let's call them $\psi_1$ and $\psi_2$, each with unit magnitude:

$$
\psi_1 = \frac{1}{\sqrt{2}} e^{i\phi_1}, \qquad \psi_2 = \frac{1}{\sqrt{2}} e^{i\phi_2}
$$

### The Calculation

The total amplitude at the detector:

$$
\psi_{\text{total}} = \psi_1 + \psi_2 = \frac{1}{\sqrt{2}}\left(e^{i\phi_1} + e^{i\phi_2}\right)
$$

The intensity (what we measure) is $I = |\psi_{\text{total}}|^2$:

$$
I = \left|\psi_1 + \psi_2\right|^2 = |\psi_1|^2 + |\psi_2|^2 + \psi_1^* \psi_2 + \psi_2^* \psi_1
$$

$$
I = \frac{1}{2} + \frac{1}{2} + 2 \cdot \frac{1}{2}\text{Re}\left(e^{i(\phi_2 - \phi_1)}\right)
$$

$$
I = 1 + \cos(\phi_2 - \phi_1) = 1 + \cos(\Delta\phi)
$$

where $\Delta\phi = \phi_2 - \phi_1 = \frac{4\pi}{\lambda}(L_2 - L_1)$ is the **phase difference**.

### The Cross Term

Let's pause and look at what happened. We can write:

$$
I = \underbrace{|\psi_1|^2 + |\psi_2|^2}_{\text{individual intensities}} + \underbrace{2\text{Re}(\psi_1^* \psi_2)}_{\text{cross term}}
$$

The cross term $2\text{Re}(\psi_1^* \psi_2) = \cos(\Delta\phi)$ is where the phases meet. 

- When $\Delta\phi = 0$: cross term $= +1$, constructive interference, $I = 2$
- When $\Delta\phi = \pi$: cross term $= -1$, destructive interference, $I = 0$

```{admonition} The Fundamental Principle
:class: important

**Interference is the cross term.**

It exists because we add amplitudes first, then square:

$$
|c_1 + c_2|^2 = |c_1|^2 + |c_2|^2 + 2\text{Re}(c_1^* c_2)
$$

The cross term depends on the *relative* phase $\arg(c_1) - \arg(c_2)$.
```

### Global Phase Doesn't Matter

Notice something: if we multiply *both* amplitudes by the same phase $e^{i\gamma}$:

$$
\psi_1 \to e^{i\gamma}\psi_1, \qquad \psi_2 \to e^{i\gamma}\psi_2
$$

The intensity is unchanged:

$$
|e^{i\gamma}\psi_1 + e^{i\gamma}\psi_2|^2 = |e^{i\gamma}|^2 |\psi_1 + \psi_2|^2 = |\psi_1 + \psi_2|^2
$$

This is profound: **global phase is unobservable**. Nature doesn't care about the overall phase of a wave—only relative phases between different paths or components.

We'll return to this fact repeatedly. It's one of the defining features of quantum mechanics.

---

## The Interferometer as a Qubit Circuit

Let's reframe the Michelson interferometer in the language we'll use for quantum computing.

**The two arms are two "configurations" or "states":**

$$
|\psi\rangle = c_1 |\text{arm 1}\rangle + c_2 |\text{arm 2}\rangle
$$

**The beam splitter creates superposition:**

A 50/50 beam splitter transforms input light into an equal superposition of the two arms:

$$
|\text{input}\rangle \to \frac{1}{\sqrt{2}}|\text{arm 1}\rangle + \frac{1}{\sqrt{2}}|\text{arm 2}\rangle
$$

This is analogous to the Hadamard gate $H$ in quantum computing.

**Phase accumulation:**

Each arm accumulates phase based on its length:

$$
|\text{arm } j\rangle \to e^{i\phi_j}|\text{arm } j\rangle
$$

**Recombination and measurement:**

When the beams recombine, we measure the intensity. The interference determines the outcome.

| Interferometer | Quantum Circuit |
|----------------|-----------------|
| Beam splitter (split) | Hadamard gate $H$ |
| Path length difference | Phase gate $R_z(\phi)$ |
| Recombination | Hadamard gate $H$ |
| Detector intensity | Measurement probability |

This is not just an analogy—it's the same mathematics. Interferometry *is* quantum computation with light.

---

## Historical Note: Michelson and the Speed of Light

Albert Michelson built the first precision interferometer in the 1880s. His goal: detect Earth's motion through the "luminiferous ether."

The famous Michelson-Morley experiment (1887) found no ether drift—a null result that helped inspire special relativity. But the interferometer itself became a revolutionary tool for precision measurement.

**Modern application:** The LIGO detectors that discovered gravitational waves in 2015 are Michelson interferometers with 4 km arms. They detect length changes of $\sim 10^{-19}$ meters—about 1/10,000 the width of a proton.

Interference converts tiny phase shifts into measurable intensity changes. This is quantum sensing, and it works because of the cross term.

---

## Summary

1. **Classical vs quantum waves:** In classical physics, complex numbers are a trick (take Re at the end). In quantum mechanics, complex numbers are fundamental—the $i$ in Schrödinger's equation is essential.

2. **Complex numbers package amplitude and phase:** $z = re^{i\theta}$ encodes both magnitude and angle. Multiplication adds phases; the magnitude squared loses the phase.

3. **Phase is like rotation:** Multiplying by $e^{i\theta}$ rotates in the complex plane. This is the group $SO(2)$. (Foreshadowing: similar structure will give us spin.)

4. **Interference is the cross term:** When we add amplitudes then square, we get $|c_1 + c_2|^2 = |c_1|^2 + |c_2|^2 + 2\text{Re}(c_1^* c_2)$. The cross term depends on relative phase.

5. **Global phase is unobservable:** Only relative phases matter. Nature doesn't care about the overall phase.

6. **Interferometers are quantum circuits:** Split → phase → recombine → measure. This pattern will appear throughout quantum computing.

---

## Looking Ahead

We've seen that complex amplitudes and interference are fundamental to quantum mechanics. Next lecture, we'll see this in action with polarized light—our first concrete qubit. 

A single photon's polarization lives in a 2D complex vector space, exactly like the two-path interferometer. But now we'll confront measurement directly: what happens when a photon hits a polarizer?

---

## Homework

### Problem 1: Michelson Interferometer

A Michelson interferometer uses light with wavelength $\lambda = 633$ nm (red HeNe laser).

**(a)** If the two arms have equal length, what is the phase difference $\Delta\phi$? What is the output intensity (taking $I_{\max} = 2$)?

**(b)** One mirror is moved by distance $d$. The light travels to the mirror and back, so the path length changes by $2d$. Show that the phase difference changes by:
$$\Delta\phi = \frac{4\pi d}{\lambda}$$

**(c)** How far must the mirror move to go from maximum intensity to zero intensity (one quarter of a fringe)?

**(d)** How far must the mirror move to shift by one complete fringe (intensity maximum → maximum)?

**(e)** If you can detect a 1% change in intensity, approximately what mirror displacement can you measure? Express your answer in nanometers.

---

### Problem 2: Fringe Counting

Light with wavelength $\lambda$ enters a Michelson interferometer. One arm is fixed; the other arm's mirror is mounted on a translation stage. As you slowly move the mirror, you observe the output intensity oscillating between bright and dark.

**(a)** Starting from the intensity formula $I = I_0(1 + \cos\Delta\phi)$ with $\Delta\phi = 4\pi d/\lambda$, make a plot of $I/I_0$ vs. mirror displacement $d$ for $d \in [0, 3\lambda]$. Label the positions of the maxima and minima.

**(b)** You observe 100 complete oscillations (bright → dark → bright counts as one oscillation) as you move the mirror by total distance $D$. What is $D$ in terms of $\lambda$?

**(c)** This technique was used historically to measure the wavelength of light. If you count 100 fringes while moving the mirror by $D = 31.65 \, \mu\text{m}$, what is $\lambda$?

**(d)** Modern interferometers can count millions of fringes. If you can count $10^6$ fringes with $\lambda = 633$ nm, what total displacement can you measure? What precision does this give you (assuming you can resolve to $\pm 1$ fringe)?

---

### Problem 3: LIGO Sensitivity

LIGO (Laser Interferometer Gravitational-Wave Observatory) uses laser light with $\lambda = 1064$ nm and has arms of length $L = 4$ km.

**(a)** A gravitational wave passing through LIGO causes a fractional length change (called "strain") $h = \Delta L / L$ in the arms. For the first detected gravitational wave (GW150914), the strain was approximately $h = 10^{-21}$. What is the physical length change $\Delta L$ in meters? Compare this to the size of a proton ($\sim 10^{-15}$ m).

**(b)** What phase shift $\Delta\phi$ does this length change produce? (Remember, light travels down the arm and back.)

**(c)** LIGO can detect phase shifts of approximately $\Delta\phi \sim 10^{-10}$ radians. What strain sensitivity does this correspond to?

**(d)** Your answer to (c) should be much larger than $10^{-21}$—LIGO couldn't detect gravitational waves with a simple Michelson interferometer! LIGO uses "Fabry-Pérot cavities" that make the light bounce back and forth ~300 times in each arm, effectively increasing the arm length. If the effective arm length is $L_{\text{eff}} = 300 \times 4$ km, recalculate the strain sensitivity. Is this close to $10^{-21}$?

---

### Problem 4: The Mach-Zehnder Interferometer as a Quantum Circuit

The Mach-Zehnder interferometer (MZI) uses two beam splitters: one to split the light, one to recombine it.

**Setup:** 
- Light enters one input port
- First beam splitter creates superposition of "upper" and "lower" paths
- A phase shifter adds phase $\phi$ to the lower path  
- Second beam splitter recombines the paths
- Two detectors D1 and D2 measure the outputs

**(a)** A 50/50 beam splitter can be modeled by the matrix:
$$B = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$$

This is the **Hadamard matrix** $H$. If light enters in state $|u\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$ (upper path only), compute $B|u\rangle$. Interpret the result physically.

**(b)** The phase shifter on the lower path is represented by:
$$P(\phi) = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\phi} \end{pmatrix}$$

Starting from your answer in (a), apply $P(\phi)$. Write the resulting state.

**(c)** Now apply the second beam splitter $B$ to recombine. Compute the final state $|\psi_{\text{out}}\rangle = B \cdot P(\phi) \cdot B |u\rangle$.

**(d)** The two components of $|\psi_{\text{out}}\rangle$ give the amplitudes for detectors D1 and D2. Find the probabilities $P(D1) = |c_1|^2$ and $P(D2) = |c_2|^2$. Verify that $P(D1) + P(D2) = 1$.

**(e)** Fill in this table:

| Phase $\phi$ | $P(D1)$ | $P(D2)$ | Where does the light go? |
|--------------|---------|---------|--------------------------|
| $0$ | | | |
| $\pi/2$ | | | |
| $\pi$ | | | |
| $2\pi$ | | | |

**(f)** The MZI is equivalent to the quantum circuit: $H \to R_z(\phi) \to H$, where $H$ is the Hadamard gate and $R_z(\phi) = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\phi} \end{pmatrix}$ is a phase gate. 

In quantum computing language:
- The "upper path" corresponds to qubit state $|0\rangle$
- The "lower path" corresponds to qubit state $|1\rangle$
- "All light goes to D1" means measuring $|0\rangle$

What does "all light goes to D2" correspond to in qubit language?

---

### Problem 5: Complex Numbers and Rotation Matrices

This problem connects complex multiplication to rotations, preparing for the group theory of spin.

A rotation by angle $\theta$ in 2D is represented by the matrix:
$$R(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}$$

**(a)** Compute the matrix product $R(\theta_1) R(\theta_2)$. Show that:
$$R(\theta_1) R(\theta_2) = R(\theta_1 + \theta_2)$$
This is the key property: rotations combine by adding angles.

**(b)** What is $R(\theta) R(-\theta)$? What is $R(0)$? Interpret these results: what is the "inverse" of a rotation?

**(c)** A complex number $z = x + iy$ can be thought of as a vector $\begin{pmatrix} x \\ y \end{pmatrix}$. When we multiply $z$ by $e^{i\theta}$, we get a new complex number $z' = x' + iy'$.

Show that:
$$\begin{pmatrix} x' \\ y' \end{pmatrix} = R(\theta) \begin{pmatrix} x \\ y \end{pmatrix}$$

*Hint:* Use $e^{i\theta} = \cos\theta + i\sin\theta$ and multiply out $(\cos\theta + i\sin\theta)(x + iy)$.

**(d)** This proves that **multiplication by $e^{i\theta}$ is the same as rotation by angle $\theta$**. 

In quantum mechanics, we'll see that phase factors $e^{i\theta}$ appear everywhere. This problem shows they are rotations in disguise. 

The set of all 2D rotations forms a group called $SO(2)$. What makes it a "group" is:
- Combining two rotations gives another rotation (closure)
- There's an identity rotation $R(0)$  
- Every rotation has an inverse $R(-\theta)$

Does the order of rotations matter for $SO(2)$? That is, does $R(\theta_1)R(\theta_2) = R(\theta_2)R(\theta_1)$?

---

### Problem 6: Generators of Rotations (Challenge)

This problem introduces the concept of a **generator**, which is central to understanding spin and the Pauli matrices.

**(a)** For a small angle $\delta\theta \ll 1$, expand $R(\delta\theta)$ to first order:
$$R(\delta\theta) \approx I + \delta\theta \cdot G$$
where $I$ is the identity matrix. Find the matrix $G$ (called the **generator** of rotations).

**(b)** Compute $G^2$. What simple matrix do you get?

**(c)** Using the Taylor series $e^x = 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \cdots$ and your result from (b), show that:
$$e^{\theta G} = \cos\theta \cdot I + \sin\theta \cdot G$$
*Hint:* Group the even and odd powers of $\theta G$ separately, and use the fact that $G^2 = -I$ to simplify.

**(d)** Verify that $e^{\theta G} = R(\theta)$. This is a profound result: **finite rotations are exponentials of the generator**.

**(e)** For complex numbers, the analogous statement is $e^{i\theta} = \cos\theta + i\sin\theta$ (Euler's formula). Compare this to your result in (c). What plays the role of $i$ in the matrix world?

**(f)** (Preview) In quantum mechanics, rotations of spin-1/2 particles are generated by the **Pauli matrices** $\sigma_x, \sigma_y, \sigma_z$. Unlike the single generator $G$ for 2D rotations, there are three generators for 3D rotations. And crucially, they don't commute: $\sigma_x \sigma_y \neq \sigma_y \sigma_x$. 

Based on Problem 5(d), 2D rotations commute. What does the non-commutativity of the Pauli matrices tell you about 3D rotations?

