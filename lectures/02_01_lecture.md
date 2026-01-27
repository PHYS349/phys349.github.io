# Lecture 2.1: Waves, Complex Numbers, and Interference

## Chapter 2 Overview

This chapter answers one big question: **what is a qubit, physically and mathematically?**

We’ll build the idea in a few steps:

- **Interference:** amplitudes add, then we square to get what we observe  
- **Complex numbers and phase:** phase acts like a rotation, and complex numbers keep track of it cleanly  
- **The qubit picture:** a qubit is a 2D complex vector, often visualized on the **Bloch sphere**  
- **Real examples:** photon polarization and spin-½ are our two main physical qubits  
- **Operations:** single-qubit gates are rotations (and interference is how they show up in experiments)

By the end of the chapter, “qubit” should feel like a concrete wave/interference object—not a slogan.

---

## Today's Focus

Today we focus on the mathematical foundations:
- **The wave equation** and its solutions
- **Complex exponentials** as a powerful tool for wave physics
- **Interference** — what happens when waves combine

Why does this matter for quantum computing? Quantum algorithms work by manipulating amplitudes so that wrong answers interfere destructively and right answers interfere constructively. Understanding interference is understanding how quantum computers compute.

Let's start with waves.

---

## The Wave Equation

Waves are everywhere in physics—light, sound, water, quantum mechanics. They all share a common mathematical structure.

### Waves in Space

Consider a wave that varies in space at a fixed moment in time. The simplest wave equation is:

$$
\frac{d^2}{dx^2} E(x) = -k^2 E(x)
$$

This says: "the second derivative of $E$ is proportional to $-E$ itself." Functions that equal (minus) their own second derivative are oscillatory—exactly what we expect for waves.

This is a second-order ordinary differential equation (ODE). From ODE theory, we know there must be two linearly independent solutions. Any solution can be written as a linear combination of these two.

One natural choice are the sines and cosines. The general real solution is:
$$
E(x) = A\cos(kx) + B\sin(kx)
$$

Verify for yourself that both of these are valid solutions. We say these are two linearly independent solutions because there are no constants $A$ and $B$ such that $A\cos(kx) + B\sin(kx) = 0$ for all $x$, except for $A=B=0$. 

The constant $k$ is called the **wavenumber**:
$$
k = \frac{2\pi}{\lambda}
$$

where $\lambda$ is the wavelength. The wave repeats every time $x$ increases by $\lambda$:
$$
\cos(kx) = \cos\left(\frac{2\pi}{\lambda}x\right)
$$

```{figure} placeholder
:name: wave-spatial

A sinusoidal wave showing amplitude $A$ and wavelength $\lambda$.
```

However, another equally valid choice are the complex exponentials:
$$
E(x) = Ce^{ikx} + De^{-ikx}
$$

Verify for yourself that these are also linearly independent. Both forms are correct. They're connected by Euler's formula:
$$
e^{i\theta} = \cos\theta + i\sin\theta
$$

From this we can derive:
$$
e^{-i\theta} = \cos\theta - i\sin\theta
$$

And solving these two for the trig functions gives:
$$
\cos\theta = \frac{e^{i\theta} + e^{-i\theta}}{2}, \qquad \sin\theta = \frac{e^{i\theta} - e^{-i\theta}}{2i}
$$

So $\{\cos(kx), \sin(kx)\}$ and $\{e^{ikx}, e^{-ikx}\}$ span the same solution space—they're just different bases. We'll use whichever is more convenient.

### Waves in Time

Now consider oscillation in time at a fixed position. The equation has the same form:
$$
\frac{d^2}{dt^2} E(t) = -\omega^2 E(t)
$$

Same math, different physical meaning. Solutions include $\cos(\omega t)$, $\sin(\omega t)$, and $e^{\pm i\omega t}$.

The constants:
- **Period** $T$: time for one complete oscillation
- **Frequency** $f = 1/T$: oscillations per second (Hz)
- **Angular frequency** $\omega = 2\pi/T = 2\pi f$: radians per second

### Traveling Waves: Space and Time Together

For light and other propagating waves, we need both space and time. If we look at the field at a fixed time it will look like $\cos(kx)$. If we look at it at a fixed position it will look like $\cos(\omega t)$.  

The full wave equation is:
$$
\frac{\partial^2}{\partial x^2} E(x,t) = \frac{1}{c^2} \frac{\partial^2}{\partial t^2} E(x,t)
$$

where $c$ is the wave speed. This is a partial differential equation. It contains derivatives with respect to both position and time, which is why we write $\partial$ instead of $d$.

The traveling wave solution:
$$
E(x,t) = E_0 \cos(kx - \omega t)
$$

describes a sinusoidal pattern moving to the right. To see this, rewrite it:
$$
E(x,t) = E_0 \cos\left(\frac{2\pi}{\lambda}\left(x - \frac{\omega}{k} t\right)\right) = E_0 \cos\left(\frac{2\pi}{\lambda}(x - ct)\right)
$$

This has the form $f(x - ct)$—a shape that shifts to the right with speed $c$. Similarly, $\cos(kx + \omega t)$ has the form $f(x + ct)$ and travels to the *left*.

For electromagnetic waves:
$$
\boxed{c = f\lambda = \frac{\omega}{k}}
$$

The two linearly independent real solutions are $\cos(kx - \omega t)$ and $\sin(kx - \omega t)$ (or equivalently, right-traveling and left-traveling waves).

Just like before, there is also a complex form of the traveling wave: $e^{i(kx - \omega t)}$ and $e^{-i(kx - \omega t)}$. Verify for yourself that $E(x,t) = E_0 e^{i(kx - \omega t)}$ satisfies the wave equation.
In classical electromagnetism, the electric field $E$ is real. But calculations are often easier using complex exponentials. Why?

**Derivatives are simpler.** They just bring down a factor of $ik$ or $-i\omega$—no sign changes or switching between sin and cos.

**Adding waves is easier.** Suppose we want to add two waves with different phases:
$$
E_1 = \cos(kx), \qquad E_2 = \cos(kx + \phi)
$$

Using trig identities, this requires expanding $\cos(kx + \phi) = \cos(kx)\cos\phi - \sin(kx)\sin\phi$ and collecting terms. Tedious.

Using complex exponentials:
$$
E_1 = e^{ikx}, \qquad E_2 = e^{i(kx + \phi)} = e^{ikx} e^{i\phi}
$$
$$
E_1 + E_2 = e^{ikx}(1 + e^{i\phi})
$$

We just factor out the common piece. The phases combine by multiplication, which is much simpler than trig identities.

When we talk about waves, we will generally assume the complex form:
$$
E(x,t) = E_0 e^{i(kx - \omega t)}
$$

If we need the physical (real) field, we take the real part:
$$
E_{\text{physical}}(x,t) = \text{Re}\left[E(x,t)\right]
$$

Often, we only need the intensity $|E|^2$, and we never need to take the real part at all.

---

## Intensity: What Detectors Measure

Detectors (your eye, a camera, a photodiode) don't measure the electric field directly. They measure **intensity**, which is proportional to the square of the field:
$$
\text{Intensity} \propto E^2
$$

Why squared? Energy in electromagnetic waves goes as $E^2$ (and $B^2$), and detectors respond to energy.

### Time Averaging for Real Waves

The field oscillates rapidly—hundreds of THz for visible light. That's $\sim 10^{14}$ oscillations per second. No detector can follow oscillations that fast; even the fastest photodetectors have response times of picoseconds ($10^{-12}$ s), which is still thousands of optical cycles.

So detectors respond to the **time-averaged** intensity.

At a fixed position, let $E(t) = E_0\cos(\omega t)$. Then:
$$
E^2(t) = E_0^2 \cos^2(\omega t)
$$

The average of $\cos^2$ over one period is $1/2$:
$$
I = \langle E^2 \rangle = \frac{1}{T}\int_0^T E_0^2 \cos^2(\omega t)\, dt = \frac{E_0^2}{2}
$$

```{figure} placeholder
:name: intensity-averaging

Top: $E(t) = \cos(\omega t)$ oscillates between $\pm 1$.
Bottom: $E^2(t) = \cos^2(\omega t)$ oscillates between 0 and 1, with average 1/2.
```

### The Complex Shortcut

Here's the beautiful part: the complex representation gives us intensity without time averaging. For the complex field $E = E_0 e^{i(kx - \omega t)}$:
$$
|E|^2 = E \cdot E^* = \left(E_0 e^{i(kx-\omega t)}\right)\left(E_0^* e^{-i(kx-\omega t)}\right) = |E_0|^2
$$

We used the key fact:
$$
|e^{i\theta}|^2 = e^{i\theta} \cdot e^{-i\theta} = 1
$$

The time dependence cancels! The magnitude squared of the complex amplitude directly gives us something proportional to intensity, with no integral required.

```{admonition} Key Result
:class: important

**Real wave (requires time averaging):**
$$I = \langle E^2 \rangle = \frac{1}{T}\int_0^T E^2(t)\, dt$$

**Complex wave (no averaging needed):**
$$I \propto |E|^2$$

The complex method is cleaner: no time averaging needed, and all the phase information is preserved until we're ready to compute the final intensity.
```

---

## Interference

Now the payoff: what happens when two waves combine?

### Setup: Two Sources

Imagine two point sources emitting waves. At a detector, the waves have traveled different distances $x_1$ and $x_2$.

```{figure} placeholder
:name: two-source

Two point sources at different distances from a detector. The waves arrive with different phases.
```

Each source contributes a complex amplitude at the detector:
$$
E_1 = e^{i(kx_1 - \omega t)}, \qquad E_2 = e^{i(kx_2 - \omega t)}
$$

The total field is the sum of the amplitudes:
$$
E_{\text{total}} = E_1 + E_2
$$

### The Cross Term: Where Interference Lives

Before doing the full calculation, let's see the structure. For any two complex amplitudes:
$$
|E_1 + E_2|^2 = (E_1 + E_2)(E_1^* + E_2^*)
$$
$$
= |E_1|^2 + |E_2|^2 + E_1 E_2^* + E_1^* E_2
$$
$$
= |E_1|^2 + |E_2|^2 + 2\text{Re}(E_1^* E_2)
$$

The last term is the **cross term** (or **interference term**). It depends on the relative phase between $E_1$ and $E_2$.

```{admonition} Waves vs. Particles
:class: note

This is fundamentally different from classical particles!

**Classical particles:** Probabilities add. If source 1 sends particles with probability $P_1$ and source 2 sends with probability $P_2$, the detector sees $P_1 + P_2$. No cross term.

**Waves (and quantum mechanics):** Amplitudes add, then we square. The cross term $2\text{Re}(E_1^* E_2)$ creates interference—the total can be more than $|E_1|^2 + |E_2|^2$ (constructive) or less (destructive).

This will be the heart of quantum computing: manipulating amplitudes so cross terms work in our favor.
```

### The Intensity Calculation

Now let's work it out explicitly. The intensity is:
$$
I = |E_{\text{total}}|^2 = |e^{i(kx_1 - \omega t)} + e^{i(kx_2 - \omega t)}|^2
$$

**Step 1:** Factor out common terms.
$$
I = |e^{-i\omega t}|^2 \cdot |e^{ikx_1} + e^{ikx_2}|^2
$$

Since $|e^{-i\omega t}|^2 = 1$:
$$
I = |e^{ikx_1} + e^{ikx_2}|^2
$$

**Step 2:** Factor out $e^{ikx_1}$.
$$
I = |e^{ikx_1}|^2 \cdot |1 + e^{ik(x_2 - x_1)}|^2 = |1 + e^{ik\Delta x}|^2
$$

where $\Delta x = x_2 - x_1$ is the **path difference**.

**Step 3:** Expand the magnitude squared.
$$
|1 + e^{ik\Delta x}|^2 = (1 + e^{ik\Delta x})(1 + e^{-ik\Delta x})
$$
$$
= 1 + e^{ik\Delta x} + e^{-ik\Delta x} + 1
$$
$$
= 2 + 2\cos(k\Delta x)
$$

### The Interference Formula

$$
I = 2(1 + \cos(k\Delta x))
$$

Using the identity $1 + \cos\theta = 2\cos^2(\theta/2)$, we can also write this as:

$$
\boxed{I = 4\cos^2\left(\frac{k\Delta x}{2}\right)}
$$

This form makes the oscillation between 0 and 4 more explicit. The intensity depends only on the **path difference** $\Delta x$.

### Global Phase Doesn't Matter

Notice what happened in Step 1: the overall time-dependent phase $e^{-i\omega t}$ dropped out completely. And in Step 2, the overall spatial phase $e^{ikx_1}$ also dropped out.

Only the **relative phase** $k\Delta x = k(x_2 - x_1)$ matters.

This is a general principle: if we multiply all amplitudes by the same phase factor $e^{i\gamma}$, the intensity doesn't change:
$$
|e^{i\gamma}E_1 + e^{i\gamma}E_2|^2 = |e^{i\gamma}|^2|E_1 + E_2|^2 = |E_1 + E_2|^2
$$

**Global phase is unobservable.** Only relative phases between different paths or components affect measurements. This will be important throughout quantum mechanics.

### Constructive and Destructive Interference

| Condition | Phase $k\Delta x$ | $\cos(k\Delta x)$ | Intensity |
|-----------|-------------------|-------------------|-----------|
| **Constructive** | $0, 2\pi, 4\pi, ...$ | $+1$ | $I = 4$ (max) |
| **Destructive** | $\pi, 3\pi, 5\pi, ...$ | $-1$ | $I = 0$ (min) |

```{figure} placeholder
:name: interference-fringes

Top: Two waves in phase add constructively.
Middle: Two waves out of phase by $\pi$ cancel destructively.
Bottom: Intensity vs. path difference shows oscillating "fringes."
```

- **Constructive interference** ($\Delta x = 0, \lambda, 2\lambda, ...$): The waves arrive "in step"—peak meets peak, trough meets trough. They reinforce.

- **Destructive interference** ($\Delta x = \lambda/2, 3\lambda/2, ...$): The waves arrive "out of step"—peak meets trough. They cancel.

The key insight: **we add amplitudes first, then square.** This creates the cross term $2\text{Re}(E_1^* E_2) = 2\cos(k\Delta x)$ that depends on the relative phase.

---

## Summary

1. **Wave equation:** $d^2E/dx^2 = -k^2 E$ has two linearly independent solutions; we can use $\{\cos(kx), \sin(kx)\}$ or $\{e^{ikx}, e^{-ikx}\}$

2. **Euler's formula:** $e^{i\theta} = \cos\theta + i\sin\theta$ connects the two bases

3. **Traveling waves:** $E(x,t) = E_0\cos(kx - \omega t)$ moves right with speed $c = f\lambda = \omega/k$

4. **Complex representation:** Use $E = E_0 e^{i(kx-\omega t)}$—derivatives and addition are simpler

5. **Intensity:** $I \propto |E|^2$ (magnitude squared of complex amplitude)

6. **Interference:** When two waves combine:
$$|E_1 + E_2|^2 = |E_1|^2 + |E_2|^2 + 2\text{Re}(E_1^* E_2)$$
   - The cross term is where interference lives
   - Constructive when $\Delta x = n\lambda$
   - Destructive when $\Delta x = (n + 1/2)\lambda$

7. **Global phase is unobservable:** Only relative phases affect measurements

```{admonition} The Central Message
:class: important

**Interference comes from adding amplitudes before squaring.**

The cross term $2\text{Re}(E_1^* E_2)$ can be positive (constructive) or negative (destructive) depending on relative phase. This will be the engine of quantum computing: we manipulate amplitudes so that wrong answers interfere destructively and right answers interfere constructively.
```

---

## Looking Ahead

Next lecture: We'll see interference in action with the **Mach-Zehnder interferometer**—and discover that it's mathematically identical to a quantum computing circuit. The interferometer will be our first concrete example of a qubit.

---

## Homework

### Problem 1: Separation of Variables for the Wave Equation

The wave equation in one dimension is:
$$\frac{\partial^2 E}{\partial x^2} = \frac{1}{c^2}\frac{\partial^2 E}{\partial t^2}$$

Solve this equation using separation of variables.

**(a)** Assume a solution of the form $E(x,t) = X(x) \cdot T(t)$. Substitute this into the wave equation and show that you can separate it into:
$$\frac{1}{X}\frac{d^2 X}{dx^2} = \frac{1}{c^2 T}\frac{d^2 T}{dt^2}$$

*Hint:* After substituting, divide both sides by $X(x)T(t)$.

**(b)** The left side depends only on $x$, and the right side depends only on $t$. For these to be equal for all $x$ and $t$, both sides must equal the same constant. Call it $-k^2$ (we choose negative so we get oscillatory solutions). Write the two ordinary differential equations for $X(x)$ and $T(t)$.

**(c)** Solve each ODE. What is the general solution $E(x,t) = X(x) \cdot T(t)$?

**(d)** Show that the separation constant $k$ and the resulting frequency $\omega$ in $T(t)$ are related by $\omega = ck$. This is the **dispersion relation** for waves.

---

### Problem 2: Visualizing Traveling Waves

In this problem, you'll create an animation of a traveling wave to build intuition for how $\cos(kx - \omega t)$ represents motion.

**(a)** Using the code below, create an animation of the traveling wave $E(x,t) = \cos(kx - \omega t)$. Run it and observe the wave moving to the right.

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation

# Parameters
wavelength = 1.0  # lambda = 1 meter
frequency = 1.0   # f = 1 Hz
k = 2 * np.pi / wavelength  # wavenumber
omega = 2 * np.pi * frequency  # angular frequency
c = wavelength * frequency  # wave speed (should equal omega/k)

# Spatial grid
x = np.linspace(0, 4 * wavelength, 500)

# Set up the figure
fig, ax = plt.subplots(figsize=(10, 4))
line, = ax.plot(x, np.cos(k * x), 'b-', lw=2)
ax.set_xlim(0, 4 * wavelength)
ax.set_ylim(-1.5, 1.5)
ax.set_xlabel('x (m)')
ax.set_ylabel('E(x,t)')
ax.set_title('Traveling Wave: E(x,t) = cos(kx - ωt)')
ax.axhline(y=0, color='k', linestyle='-', lw=0.5)
ax.grid(True, alpha=0.3)

# Animation function
def animate(frame):
    t = frame / 30  # time in seconds (30 frames per second)
    E = np.cos(k * x - omega * t)
    line.set_ydata(E)
    ax.set_title(f'Traveling Wave: E(x,t) = cos(kx - ωt), t = {t:.2f} s')
    return line,

# Create animation
anim = FuncAnimation(fig, animate, frames=120, interval=33, blit=True)
anim.save('traveling_wave.gif', writer='pillow', fps=30)
plt.show()
```

**(b)** With $\lambda = 1$ m and $f = 1$ Hz, what is the wave speed $c$? How far does the wave travel in one period $T = 1/f$?

**(c)** Modify the code to plot *both* $\cos(kx - \omega t)$ (right-traveling) and $\cos(kx + \omega t)$ (left-traveling) on the same graph. Use different colors for each. Run the animation and observe how they move in opposite directions.

*Hint:* Add a second line object and update both in the animate function:
```python
line1, = ax.plot(x, np.cos(k * x), 'b-', lw=2, label='Right-traveling')
line2, = ax.plot(x, np.cos(k * x), 'r-', lw=2, label='Left-traveling')
ax.legend()
```

**(d)** Explain in words why changing the sign from $(kx - \omega t)$ to $(kx + \omega t)$ reverses the direction of travel. *Hint:* Think about what value of $x$ keeps the phase constant as $t$ increases.

**(e)** Create a new animation showing two waves traveling in the *same* direction: $E_1 = \cos(kx - \omega t)$ and $E_2 = \cos(kx - \omega t + \pi)$ (shifted by $\pi$). Also plot their sum $E_1 + E_2$. What do you observe? Relate this to destructive interference.

---

### Problem 3: Complex Number Practice

Using Euler's formula $e^{i\theta} = \cos\theta + i\sin\theta$:

**(a)** Express $\cos(kx)$ in terms of complex exponentials.

**(b)** Express $\sin(\omega t)$ in terms of complex exponentials.

**(c)** Show that the general real solution $A\cos(kx) + B\sin(kx)$ can be written as $Ce^{ikx} + De^{-ikx}$ by finding $C$ and $D$ in terms of $A$ and $B$.

**(d)** Compute $|e^{i\theta}|^2 = e^{i\theta} \cdot e^{-i\theta}$ and verify it equals 1.

---

### Problem 4: Intensity and Time Averaging

**(a)** Show that $\langle \cos^2(\omega t) \rangle = \frac{1}{2}$ by computing the integral:
$$\frac{1}{T}\int_0^T \cos^2(\omega t)\, dt$$
*Hint:* Use the identity $\cos^2\theta = \frac{1}{2}(1 + \cos 2\theta)$.

**(b)** Similarly, show that $\langle \sin^2(\omega t) \rangle = \frac{1}{2}$.

**(c)** What is $\langle \cos(\omega t)\sin(\omega t) \rangle$? This "cross term" will be important for interference.

**(d)** For a complex wave $E = E_0 e^{i(kx - \omega t)}$, compute $|E|^2$ and show it's constant in time.

---

### Problem 5: Two-Source Interference

Two sources emit waves with wavelength $\lambda = 500$ nm. A detector is positioned so that one source is 10.0 μm away and the other is 10.5 μm away.

**(a)** What is the path difference $\Delta x$ in meters? In wavelengths?

**(b)** Calculate the phase difference $k\Delta x$ in radians.

**(c)** Calculate the intensity $I = 4\cos^2(k\Delta x/2)$. Is this closer to constructive or destructive interference?

**(d)** How much would you need to move the detector (changing one path length) to get from maximum to minimum intensity?