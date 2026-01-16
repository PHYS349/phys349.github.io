# Lecture 5: Waves, Complex Numbers, and Interference

## Why Complex Numbers?

In the last chapter, we saw that quantum mechanics is a "wave over configurations." Each configuration gets a complex amplitude, and these amplitudes can interfere.

But why complex numbers? Why not just regular numbers?

Today we'll answer that question. We'll see that complex numbers are the natural language of waves—they package amplitude and phase into a single object. And interference, the phenomenon at the heart of quantum computing, emerges naturally from how complex numbers add.

---

## Starting with a Real Wave

Let's begin with something familiar: a wave.

Think of a vibrating string, a sound wave, or light. At any point in space and time, the wave has some displacement or field value. 

For light, Maxwell's equations reduce to the **wave equation**. In one dimension:

$$
\frac{\partial^2 E}{\partial x^2} = \frac{1}{c^2} \frac{\partial^2 E}{\partial t^2}
$$

This equation says: the curvature in space is proportional to the curvature in time. We'll see equations like this again when we study the Schrödinger equation.

A simple traveling wave that solves this equation looks like:

$$
E(x,t) = A \cos(kx - \omega t + \phi)
$$

where:
- $A$ is the **amplitude** (how big the wave is)
- $k$ is the wave number ($k = 2\pi/\lambda$)
- $\omega$ is the angular frequency ($\omega = 2\pi f$)
- $\phi$ is the **phase** (where you are in the cycle)

This wave has two independent pieces of information that matter for interference:
- The **amplitude** $A$
- The **phase** $\phi$

These two numbers—amplitude and phase—will follow us throughout quantum mechanics.

---

## The Complex Trick

Here's a mathematical trick that physicists use constantly.

Euler's formula says:

$$
e^{i\theta} = \cos\theta + i\sin\theta
$$

This means we can write our wave as:

$$
\tilde{E}(x,t) = A e^{i(kx - \omega t + \phi)}
$$

The tilde reminds us this is a complex quantity. The original real wave is recovered by taking the real part:

$$
E(x,t) = \text{Re}\left[\tilde{E}(x,t)\right] = A\cos(kx - \omega t + \phi)
$$

**Why bother?**

At first this seems like extra work. But here's the payoff: in the complex representation, amplitude and phase are unified into a single complex number.

The complex amplitude is:

$$
\tilde{A} = A e^{i\phi}
$$

This one object encodes both:
- **Magnitude** $|\tilde{A}| = A$ (the amplitude)
- **Angle** $\arg(\tilde{A}) = \phi$ (the phase)

And crucially: when we add waves, the complex numbers add. When we multiply waves (or propagate them), the phases add. Everything becomes algebra.

---

## Visualizing Complex Numbers

A complex number $z = x + iy$ can be visualized as an arrow in the complex plane:

```{figure} ./02_01_lecture_files/complex_plane.svg
:width: 300px
:name: complex-plane

A complex number as an arrow in the complex plane.
```

The same number can be written in polar form:

$$
z = r e^{i\theta}
$$

where:
- $r = |z| = \sqrt{x^2 + y^2}$ is the length of the arrow
- $\theta = \arg(z)$ is the angle from the positive real axis

**Key operations:**

| Operation | What it does |
|-----------|--------------|
| $z_1 + z_2$ | Vector addition (tip-to-tail) |
| $z_1 \cdot z_2$ | Multiply magnitudes, add phases |
| $\|z\|^2 = z^* z$ | Square of the length |

The last one is especially important. If $z = re^{i\theta}$, then:

$$
|z|^2 = z^* z = (re^{-i\theta})(re^{i\theta}) = r^2
$$

The phase disappears when we take $|z|^2$. This will connect directly to probabilities in quantum mechanics.

---

## Interference: Why Complex Numbers Matter

Now let's see why this formalism is so powerful.

### The Double Slit

Consider the classic double-slit experiment — first performed by Thomas Young in 1801, demonstrating that light is a wave. A wave approaches a barrier with two slits, and we want to know the intensity at a point P on a screen behind the barrier.

```{figure} ./02_01_lecture_files/double_slit.svg
:width: 400px
:name: double-slit

The double-slit experiment: a wave passes through two slits and interferes.
```

The wave can reach P by two paths:
- Path 1: through the top slit (distance $r_1$)
- Path 2: through the bottom slit (distance $r_2$)

If the source emits a wave $e^{i(kx - \omega t)}$, then the amplitude arriving at P from each path is:

$$
\psi_1 = \frac{1}{\sqrt{2}} e^{ikr_1}, \qquad \psi_2 = \frac{1}{\sqrt{2}} e^{ikr_2}
$$

(We've dropped the time dependence since it's the same for both paths, and the $1/\sqrt{2}$ accounts for the wave splitting between two slits.)

### The Key Step: Amplitudes Add

The total amplitude at P is the **sum** of the contributions:

$$
\psi_{\text{total}} = \psi_1 + \psi_2 = \frac{1}{\sqrt{2}}\left(e^{ikr_1} + e^{ikr_2}\right)
$$

This is where the magic happens. We're adding complex numbers—arrows in the complex plane.

### What We Observe: Intensity

The intensity (or probability, in quantum mechanics) is the magnitude squared:

$$
I = |\psi_{\text{total}}|^2 = \psi_{\text{total}}^* \psi_{\text{total}}
$$

Let's compute this. Define the phase difference:

$$
\Delta\phi = k(r_1 - r_2) = \frac{2\pi}{\lambda}(r_1 - r_2)
$$

Then:

$$
\psi_{\text{total}} = \frac{1}{\sqrt{2}} e^{ikr_1}\left(1 + e^{-i\Delta\phi}\right)
$$

The intensity is:

$$
I = |\psi_{\text{total}}|^2 = \frac{1}{2}\left|1 + e^{-i\Delta\phi}\right|^2
$$

Let's expand this:

$$
\left|1 + e^{-i\Delta\phi}\right|^2 = (1 + e^{i\Delta\phi})(1 + e^{-i\Delta\phi})
$$

$$
= 1 + e^{i\Delta\phi} + e^{-i\Delta\phi} + 1 = 2 + 2\cos(\Delta\phi)
$$

So:

$$
\boxed{I = 1 + \cos(\Delta\phi) = 2\cos^2\left(\frac{\Delta\phi}{2}\right)}
$$

### The Physics

This result contains everything:

| Path difference | Phase difference $\Delta\phi$ | Intensity $I$ | What happens |
|-----------------|------------------------------|---------------|--------------|
| $0, \lambda, 2\lambda, ...$ | $0, 2\pi, 4\pi, ...$ | 2 (maximum) | Constructive interference |
| $\lambda/2, 3\lambda/2, ...$ | $\pi, 3\pi, ...$ | 0 (minimum) | Destructive interference |

When the two waves arrive **in phase** ($\Delta\phi = 0$), they add constructively—the arrows point the same direction.

When they arrive **out of phase** ($\Delta\phi = \pi$), they cancel—the arrows point opposite directions.

```{figure} ./02_01_lecture_files/interference_arrows.svg
:width: 400px
:name: interference-arrows

Interference as vector addition: in-phase arrows add, out-of-phase arrows cancel.
```

This is interference, and it emerges directly from the addition of complex numbers.

```{admonition} The Quantum Mystery
:class: note

Remarkably, this interference pattern appears even when we send photons through one at a time. Each individual photon lands at a single spot on the screen — but after thousands of photons, the spots build up the interference pattern. 

Each photon interferes with *itself* — it travels through both slits simultaneously. This is the quantum mystery we'll explore in coming lectures.
```

---

## The Cross Term: Where Interference Lives

Let's look at the intensity calculation another way. Write:

$$
I = |\psi_1 + \psi_2|^2 = (\psi_1^* + \psi_2^*)(\psi_1 + \psi_2)
$$

Expanding:

$$
I = |\psi_1|^2 + |\psi_2|^2 + \psi_1^*\psi_2 + \psi_2^*\psi_1
$$

The first two terms are just the individual intensities. The last two terms are the **cross terms**:

$$
I = I_1 + I_2 + 2\text{Re}(\psi_1^*\psi_2)
$$

If we just had $I = I_1 + I_2$, there would be no interference—just two independent waves adding their intensities.

The cross term $2\text{Re}(\psi_1^*\psi_2)$ is where the phases meet. It's positive when the phases align (constructive) and negative when they oppose (destructive).

```{admonition} Key Insight
:class: important

**Interference is the cross term.**

It appears because we add amplitudes first, then square.
```

```{admonition} The Fundamental Difference
:class: warning

**Classical particles:** Probabilities add. If two paths lead to the same outcome, $P = P_1 + P_2$.

**Waves and quantum systems:** Amplitudes add, then square. $P = |c_1 + c_2|^2 \neq |c_1|^2 + |c_2|^2$.

The cross term $2\text{Re}(c_1^* c_2)$ is what makes quantum mechanics different from classical probability.
```

This is profoundly different from classical probability, where you would add probabilities (intensities) directly.

---

## Interferometers: Precision Instruments from Interference

The double-slit experiment spreads interference across many points in space. But we can also engineer interference to happen at a single point, with all the "which path" information encoded in a controlled phase difference.

These devices are called **interferometers**, and they're some of the most precise instruments ever built.

### The Michelson Interferometer

The Michelson interferometer splits a beam into two perpendicular arms, reflects them back with mirrors, and recombines them.

```{figure} ./02_01_lecture_files/michelson.svg
:width: 350px
:name: michelson

The Michelson interferometer: a beam splitter, two mirrors, and a detector.
```

Here's how it works:
1. Light hits a 50/50 beam splitter at 45°
2. Half goes to mirror 1 (distance $L_1$), half to mirror 2 (distance $L_2$)
3. Both beams return and recombine at the beam splitter
4. The detector sees the interference

The phase difference comes from the path length difference:

$$
\Delta\phi = \frac{2\pi}{\lambda} \cdot 2(L_1 - L_2)
$$

(The factor of 2 is because light travels to the mirror and back.)

The intensity at the detector:

$$
I = I_0 \cos^2\left(\frac{\Delta\phi}{2}\right) = I_0 \cos^2\left(\frac{2\pi (L_1 - L_2)}{\lambda}\right)
$$

**Why this matters:** The Michelson interferometer can detect length changes smaller than the wavelength of light. Move one mirror by $\lambda/4 \approx 125$ nm and the output swings from maximum to zero.

```{admonition} LIGO: Gravitational Wave Detection
:class: note

The LIGO detectors that discovered gravitational waves in 2015 are Michelson interferometers with 4 km arms. They detected length changes of $10^{-19}$ meters—about 1/10,000 the diameter of a proton. This is interference as a precision measurement tool.
```

### The Mach-Zehnder Interferometer

The Mach-Zehnder interferometer is similar but uses two separate beam splitters—one to split, one to recombine. The beams travel in the same direction rather than bouncing back.

```{figure} ./02_01_lecture_files/mach_zehnder.svg
:width: 400px
:name: mach-zehnder

The Mach-Zehnder interferometer: split, propagate, phase shift, recombine.
```

The setup:
1. First beam splitter splits the input into upper and lower paths
2. Light propagates through both arms
3. A phase shifter adds controlled phase $\phi$ to one arm
4. Second beam splitter recombines the beams
5. Two detectors measure the outputs D1 and D2

Let's trace the amplitudes. After the first beam splitter:

$$
|in\rangle \to \frac{1}{\sqrt{2}}|upper\rangle + \frac{1}{\sqrt{2}}|lower\rangle
$$

After the phase shifter (on the lower arm):

$$
\frac{1}{\sqrt{2}}|upper\rangle + \frac{e^{i\phi}}{\sqrt{2}}|lower\rangle
$$

At the second beam splitter, each path splits again. Importantly, reflection at a beam splitter adds a phase of $\pi$ (a minus sign). The transformation is:

$$
|upper\rangle \to \frac{1}{\sqrt{2}}(|D1\rangle + |D2\rangle)
$$

$$
|lower\rangle \to \frac{1}{\sqrt{2}}(|D1\rangle - |D2\rangle)
$$

Combining everything:

$$
|\psi_{out}\rangle = \frac{1}{2}\left[(1 + e^{i\phi})|D1\rangle + (1 - e^{i\phi})|D2\rangle\right]
$$

The probabilities are:

$$
P(D1) = \left|\frac{1 + e^{i\phi}}{2}\right|^2 = \cos^2\left(\frac{\phi}{2}\right)
$$

$$
P(D2) = \left|\frac{1 - e^{i\phi}}{2}\right|^2 = \sin^2\left(\frac{\phi}{2}\right)
$$

Notice: $P(D1) + P(D2) = 1$. All the light goes somewhere—it's just a question of which detector.

| Phase $\phi$ | $P(D1)$ | $P(D2)$ | What happens |
|--------------|---------|---------|--------------|
| $0$ | 1 | 0 | All light to D1 |
| $\pi/2$ | 0.5 | 0.5 | Equal split |
| $\pi$ | 0 | 1 | All light to D2 |
| $2\pi$ | 1 | 0 | Back to D1 |

The phase shift controls where the light goes. This is interference as a **switch**.

### The MZI as a Quantum Circuit

Here's something remarkable. The Mach-Zehnder interferometer has the same structure as a quantum circuit:

| MZI Component | Quantum Circuit |
|---------------|-----------------|
| First beam splitter | Hadamard gate $H$ |
| Phase shifter | $R_z(\phi)$ gate |
| Second beam splitter | Hadamard gate $H$ |
| Detectors | Measurement in Z basis |

The quantum circuit $H \to R_z(\phi) \to H$ is mathematically identical to an MZI.

This isn't a coincidence. Both are doing the same thing:
1. **Split** into a superposition of two paths/states
2. **Accumulate phase** between the paths
3. **Recombine** and let interference determine the outcome

This is the template for quantum algorithms: split, phase, recombine, measure.

---

## From Classical Waves to Quantum States

We've seen that complex numbers are *convenient* for describing classical waves like light. But in quantum mechanics, they're not just convenient — they're *fundamental*. Quantum amplitudes are irreducibly complex.

In the double-slit experiment, the "configurations" were the two paths — through slit 1 or through slit 2. Each configuration got a complex amplitude, and we summed them before squaring.

In quantum mechanics, we'll have configurations like:
- A spin pointing up or down
- A photon polarized horizontally or vertically
- A particle on the left or right

Each configuration gets a complex amplitude:

$$
|\psi\rangle = c_1 |\text{config 1}\rangle + c_2 |\text{config 2}\rangle
$$

The same rule applies: add amplitudes first, then square to get probabilities. The cross term — the interference — is what makes quantum mechanics powerful.

```{admonition} The Core Idea of Quantum Computing
:class: important

**Quantum computing is the art of engineering the cross term.**

We arrange for wrong answers to interfere destructively (cancel) and right answers to interfere constructively (amplify).
```

---

## Summary

Today we established the mathematical foundation for everything that follows:

1. **Waves satisfy a wave equation** — and have two key properties: amplitude and phase

2. **Complex numbers package both** — $z = Ae^{i\phi}$ encodes amplitude $A$ and phase $\phi$

3. **Interference is addition of complex numbers** — arrows add in the complex plane

4. **What we observe is $|z|^2$** — the phase disappears, but only after the cross term has done its work

5. **The cross term is where interference lives** — it's positive or negative depending on relative phase

6. **Interferometers harness interference for precision** — Michelson (LIGO), Mach-Zehnder (quantum circuits)

Next lecture, we'll see these ideas in action with polarized light—a qubit you can hold in your hands.

---

## Homework

### Problem 1: Complex Number Warm-Up

For each of the following, express in both Cartesian ($x + iy$) and polar ($re^{i\theta}$) form:

**(a)** $z = 1 + i$

**(b)** $z = -2$

**(c)** $z = 3e^{i\pi/3}$

**(d)** Compute $|z|^2$ for each.

---

### Problem 2: Adding Waves

Two waves with equal amplitude arrive at a point:

$$
\psi_1 = e^{i\phi_1}, \qquad \psi_2 = e^{i\phi_2}
$$

**(a)** Show that the total intensity is:

$$
I = |\psi_1 + \psi_2|^2 = 2(1 + \cos(\phi_1 - \phi_2))
$$

**(b)** What is $I$ when $\phi_1 - \phi_2 = 0$? When $\phi_1 - \phi_2 = \pi$? When $\phi_1 - \phi_2 = \pi/2$?

**(c)** Sketch $I$ as a function of $\phi_1 - \phi_2$ from $0$ to $4\pi$.

---

### Problem 3: Three Waves

Now suppose three waves arrive at a point:

$$
\psi_{\text{total}} = e^{i\phi_1} + e^{i\phi_2} + e^{i\phi_3}
$$

**(a)** For the symmetric case $\phi_1 = 0$, $\phi_2 = 2\pi/3$, $\phi_3 = 4\pi/3$, draw the three arrows in the complex plane. What is $\psi_{\text{total}}$? What is the intensity $I$?

**(b)** For $\phi_1 = \phi_2 = \phi_3 = 0$, what is the intensity?

**(c)** In general, what phase relationship maximizes the intensity? What minimizes it?

---

### Problem 4: Mach-Zehnder Variations

In lecture, we derived the MZI output probabilities:

$$
P(D1) = \cos^2(\phi/2), \qquad P(D2) = \sin^2(\phi/2)
$$

**(a)** **Verify the algebra:** Starting from the state after the phase shifter,

$$
|\psi\rangle = \frac{1}{\sqrt{2}}|upper\rangle + \frac{e^{i\phi}}{\sqrt{2}}|lower\rangle
$$

apply the second beam splitter transformation and show that you get $P(D1) = \cos^2(\phi/2)$.

**(b)** **Unequal beam splitter:** Suppose the first beam splitter sends fraction $r$ of the amplitude to the upper arm and $t = \sqrt{1-r^2}$ to the lower arm:

$$
|in\rangle \to r|upper\rangle + t|lower\rangle
$$

Keep the second beam splitter 50/50. Derive the new expression for $P(D1)$ as a function of $\phi$ and $r$. 

**(c)** For what value of $r$ do you get the maximum contrast (difference between max and min of $P(D1)$)?

**(d)** **Phase in both arms:** Now suppose both arms have phase shifters: $\phi_1$ in the upper arm and $\phi_2$ in the lower arm. Show that the output only depends on the phase *difference* $\phi_1 - \phi_2$, not the individual phases.

**(e)** **Connection to quantum circuits:** The MZI is equivalent to the circuit H → Rz(φ) → H. In this analogy:
- What does the "upper path" correspond to in the qubit?
- What does "all light goes to D1" mean in terms of the qubit state?

---

### Problem 5: Double-Slit Simulation (Coding)

Use the provided Python code to simulate the double-slit experiment.

```{code-block} python
import numpy as np
import matplotlib.pyplot as plt

def double_slit_intensity(y, d, L, wavelength):
    """
    Calculate intensity pattern for double slit.
    
    Parameters:
    y : array of positions on screen
    d : slit separation
    L : distance to screen
    wavelength : wavelength of light
    
    Returns:
    intensity at each position y
    """
    # Path difference (small angle approximation)
    delta_r = d * y / L
    
    # Phase difference
    delta_phi = 2 * np.pi * delta_r / wavelength
    
    # Intensity (normalized)
    intensity = (np.cos(delta_phi / 2))**2
    
    return intensity

# Parameters
L = 1.0  # distance to screen (meters)
d = 0.1e-3  # slit separation (0.1 mm)
wavelength = 500e-9  # green light (500 nm)

# Screen positions
y = np.linspace(-0.02, 0.02, 1000)  # +/- 2 cm

# Calculate and plot
I = double_slit_intensity(y, d, L, wavelength)

plt.figure(figsize=(10, 5))
plt.plot(y * 1000, I, 'b-', linewidth=1.5)
plt.xlabel('Position on screen (mm)', fontsize=12)
plt.ylabel('Intensity (normalized)', fontsize=12)
plt.title('Double-Slit Interference Pattern', fontsize=14)
plt.grid(True, alpha=0.3)
plt.show()
```

**(a)** Run the code with the default parameters. How many bright fringes do you see in the central 20 mm?

**(b)** Double the wavelength to 1000 nm (infrared). What happens to the fringe spacing?

**(c)** Double the slit separation to 0.2 mm. What happens?

**(d)** The fringe spacing is approximately $\Delta y = \lambda L / d$. Verify this formula using your simulations.

**(e)** What happens if you make the wavelength very large compared to the slit separation? What does the pattern look like? Explain physically.

---

### Problem 6: Why Not Real Numbers? (Conceptual)

Someone argues: "I can describe interference using real numbers. Just track the amplitude and phase separately. Why do we need complex numbers?"

Write 1-2 paragraphs explaining the advantage of complex numbers. Your answer should address:
- What happens when you multiply two waves (e.g., propagation)
- What happens when you add two waves (e.g., interference)
- Why a single complex number is more natural than two real numbers

---

### Problem 7: The Michelson Interferometer and LIGO

A Michelson interferometer has arms of length $L_1$ and $L_2$.

**(a)** The output intensity is $I = I_0 \cos^2\left(\frac{2\pi (L_1 - L_2)}{\lambda}\right)$. Starting from $L_1 = L_2$, how much do you need to change $L_1$ to go from maximum intensity to zero intensity? Express your answer in terms of $\lambda$.

**(b)** For green light ($\lambda = 500$ nm), what is this distance in nanometers?

**(c)** LIGO uses infrared light with $\lambda = 1064$ nm. The detector can measure intensity changes corresponding to a phase shift of $\delta\phi \sim 10^{-10}$ radians. What length change $\delta L$ does this correspond to?

**(d)** LIGO's arms are $L = 4$ km long. A gravitational wave causes a fractional length change (strain) of $h = \delta L / L$. Using your answer from (c), what is the smallest strain LIGO can detect?

**(e)** The gravitational wave event GW150914 (the first detection) had a strain of about $h \sim 10^{-21}$. Is this detectable according to your answer in (d)? What does this tell you about the additional techniques LIGO must use?

---

## Looking Ahead

Next lecture, we'll see interference in action with polarized light. A polarizer is a measurement device—it projects onto a basis and gives a definite outcome. And we'll see the surprising three-polarizer effect, where adding a measurement can *increase* transmission.

This will be our first real qubit.