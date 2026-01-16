











## Plan for today
1. Quick debrief: what made the wedding problem “hard”
2. How the wedding problem maps to a physics “ground state” problem (Ising spins)
3. Where quantum computers actually are right now (and what “logical qubit” means)

##  Why the wedding problem is hard (even though it’s classical)

The wedding seating problem has:
- a huge number of configurations (different seatings)
- a cost function (total “drama energy”)
- a goal: find the minimum

This is the same computational shape as many physics problems:
- many possible microstates
- an energy function $E(\cdot)$
- a ground state (minimum-energy configuration)

The key scaling idea:
- “Try all possibilities” typically scales like a factorial (or worse)
- Factorials grow faster than exponentials like $2^N$ for large $N$
- Either way, once the growth is super-polynomial, brute force eventually loses

## From wedding seating to a “spin ground state” problem

The wedding problem was actually a trick to get you to run your first quantum simulation of spins.  The problem directly maps to an important problem in quantum physics - find the ground state of an array of spins.  As we will learn in next week,  spins can either be up or down, which we will call up or down.  If two spins are both up and next then they repel, increasing energy.  So for two spins lowest energy is |up down>.   up up and down down have higher energy. 

The cost function is the energy, related to the Hamiltonian below: spins $s_i \in \{-1,+1\}$
$$
H(s) = \sum_{i<j} J_{ij}\, s_i s_j + \sum_i h_i\, s_i.
$$
Finding the ground state means: choose the $s_i$ values that minimize $H(s)$.
Physics version of “find the best configuration”:
- you define an energy function
- the system’s ground state is the configuration with minimal energy



## What are classical computers bad at?

Let’s start with a concrete question. **Suppose you want to compute the lowest-energy configuration of a molecule consisting of nuclei and electrons.**

This is not an abstract problem.  Nature solves is constantly solving it. If molecules are excited, they emit light, loosing energy until it relaxes to the ground state. 

That ground state determines chemical and material properties.  So the question is not _whether_ a solution exists.   The question is whether a classical computer can find it.

First a question for you...

> [!iClicker]
> # Complexity Threshold
> We want to simulate the positions of the nuclei and electrons of a molecule in the lowest energy configuration?  **At what number of atoms do you think classical computation breaks down?**
>
> - $2$
> - $10$
> - $100$
> - $10^4$
> - $10^8$
> - $10^{12}$
> - $10^{15}$
> - $10^{18}$
> - $10^{20}$

The answer is about 10.  Even with as few as ten nuclei, exact calculations become impossible and approximations are unavoidable. Welcome to the field of **computational chemistry**. The central question becomes: _what are the most accurate approximations we can make?_

At first glance, it seems like classical computers _should_ be able to do this.  After all:
- We simulate trillions of trajectories in weather models 
- We simulate the motion of stars in a galaxy
- For goodness sake, we have semi-accurate simulations of black holes!

So why not molecules? Why not just 
- try many configurations
- compute their energies
- pick the lowest one?

This sounds reasonable.  Let’s examine why it fails.

>[!Question]
>> **If classical computers can simulate trillions of trajectories, why can’t they find the ground state of a moderately large molecule?**


## Why is quantum hard? 

The difficulty is that quantum systems do not have a single configuration.  They exist in **superposition**.  Classical simulations follow definite trajectories - each particle has a definite position at any given time.  

Nature however is not so simple.  **Nature explores all possible configurations simultaneously!**  

To understand this let's go back to the beginning of quantum mechanics and see what quantum theory forced us to do.
### Quantum superposition
![[Pasted image 20260108112253.png|300]]
Recall the Schrödinger picture for a single particle.  A quantum particle does not occupy one position at a time.  It occupies many positions simultaneously, each with:
- an amplitude
- a phase
We will use a slightly different language for this to prepare ourselves for what is coming.  We could say that every position is a different possible configuration for the particle.   Each of these positions has an amplitude and phase associated with it, as encoded in the wavefunction:  
$$\psi(x,t)  $$
At every position $x$,  there is a complex number whose magnitude and phase evolve in time, as governed by a wave equation, in this case the Schrodinger equation.



Complex numbers play an important role in nature, which we’ll learn about soon. For now, we’ll represent a complex number as an arrow in the complex plane:

%![[Pasted image 20260108162726.png|200]]

The angle of the arrow is the phase, and the radius is the amplitude.

Now imagine we discretize space into a set of points $x_0, x_1, \ldots, x_{N-1}$. Instead of drawing a continuous wavefunction, we draw a little phasor (unit-circle arrow) at each position:

%![[Pasted image 20260108163329.png|400]]

In our language, each position is a “configuration,” and it carries a complex number
$$
c_i = \psi(x_i).
$$

We can bundle all of these complex amplitudes into a single state vector:
$$
\vec{\psi} =
\begin{pmatrix}
c_0 \\
c_1 \\
\vdots \\
c_{N-1}
\end{pmatrix}.
$$

The interpretation is:

- $|c_i|^2$ is the probability of finding the particle at position $x_i$ if we measure position (more about measurement later).
- To guarantee the particle is somewhere, the probabilities must add to 1:
$$
\sum_i |c_i|^2 = 1.
$$

### Time evolution is linear

The Schrödinger equation tells us how the state changes in time. The key structural point is that quantum evolution is **linear**: the new vector is obtained by applying a linear operator (a matrix) to the old vector.

So we can write, for a small time step $\Delta t$,
$$
\underbrace{
\begin{pmatrix}
c_0(t+\Delta t) \\
c_1(t+\Delta t) \\
\vdots \\
c_{N-1}(t+\Delta t)
\end{pmatrix}
}_{\psi(t+\Delta t)}
=
\underbrace{
\begin{pmatrix}
H_{00} & H_{01} & \cdots & H_{0,N-1} \\
H_{10} & H_{11} & \cdots & H_{1,N-1} \\
\vdots & \vdots & \ddots & \vdots \\
H_{N-1,0} & H_{N-1,1} & \cdots & H_{N-1,N-1}
\end{pmatrix}
}_{N\times N}
\underbrace{
\begin{pmatrix}
c_0(t) \\
c_1(t) \\
\vdots \\
c_{N-1}(t)
\end{pmatrix}
}_{\psi(t)}.
$$

If this matrix is dense, that update takes about $O(N^2)$ operations per time step.

We are still okay!!! This is still **polynomial** scaling. GPUs got this.

And we can take this all the way down to the simplest case, where the particle can only be in **two** locations…

### Two-position system

Suppose the particle can only be in two positions: Left or Right. We draw a unit circle at each site to represent phase and amplitude.

![[Pasted image 20260108130320.png]]

In Dirac (bra–ket) notation, the state is:
$$
|\Psi\rangle = c_0|L\rangle + c_1|R\rangle.
$$

Normalization becomes:
$$
|c_0|^2 + |c_1|^2 = 1.
$$

### Time evolution is linear

A key feature of quantum mechanics is linearity: time evolution acts linearly on the state vector. Over a short time step $\Delta t$, we can write the update as a $2\times 2$ matrix acting on the coefficient vector:

$$
\underbrace{
\begin{pmatrix}
c_0(t+\Delta t) \\
c_1(t+\Delta t)
\end{pmatrix}
}_{\psi(t+\Delta t)}
=
\underbrace{
\begin{pmatrix}
H_{00} & H_{01} \\
H_{10} & H_{11}
\end{pmatrix}
}_{2\times 2}
\underbrace{
\begin{pmatrix}
c_0(t) \\
c_1(t)
\end{pmatrix}
}_{\psi(t)}.
$$

If we multiply it out, this is just:
$$
\begin{pmatrix}
c_0(t+\Delta t) \\
c_1(t+\Delta t)
\end{pmatrix}
=
\begin{pmatrix}
H_{00}c_0(t) + H_{01}c_1(t) \\
H_{10}c_0(t) + H_{11}c_1(t)
\end{pmatrix}.
$$

(We will later connect this matrix more precisely to the Hamiltonian and the Schrödinger equation.)

---

### A physical two-configuration system: spin

There is a real physical system with two configurations: the spin of an electron. An electron has spin $S=1/2$, and the spin along a chosen axis (usually $z$) can be either up or down.

The two basis states are:
- $|\uparrow\rangle$
- $|\downarrow\rangle$

We know this experimentally because if we send a spin-$1/2$ particle through a Stern–Gerlach apparatus, it separates into only two beams.

%![[Pasted image 20260108130345.png]]

A general state is:
$$
|\Psi\rangle = c_0|\uparrow\rangle + c_1|\downarrow\rangle,
$$
with normalization:
$$
|c_0|^2 + |c_1|^2 = 1.
$$

If $|\uparrow\rangle$ and $|\downarrow\rangle$ are energy eigenstates with energies $E_\uparrow$ and $E_\downarrow$, then the time evolution is:
$$
|\Psi(t)\rangle =
c_0\,e^{-iE_\uparrow t/\hbar}|\uparrow\rangle +
c_1\,e^{-iE_\downarrow t/\hbar}|\downarrow\rangle.
$$

This doesn’t look so bad: it’s just two complex numbers. Are you ready for things to get crazy?


### The exponential wall

> **What if we have two spins?**

Each spin can be up or down, so the system has four configurations:
$$
|\uparrow\uparrow\rangle,\;
|\uparrow\downarrow\rangle,\;
|\downarrow\uparrow\rangle,\;
|\downarrow\downarrow\rangle.
$$

Each configuration has an associated complex amplitude (magnitude + phase). The most general state is:
$$
|\Psi\rangle =
c_{00}|\uparrow\uparrow\rangle +
c_{01}|\uparrow\downarrow\rangle +
c_{10}|\downarrow\uparrow\rangle +
c_{11}|\downarrow\downarrow\rangle,
$$
with normalization
$$
|c_{00}|^2 + |c_{01}|^2 + |c_{10}|^2 + |c_{11}|^2 = 1.
$$

Already, the number of complex amplitudes has doubled—from $2$ to $4$.

Now let’s do three spins.

Each spin is up/down, so there are $2^3 = 8$ configurations:
$$
|\uparrow\uparrow\uparrow\rangle,\;
|\uparrow\uparrow\downarrow\rangle,\;
|\uparrow\downarrow\uparrow\rangle,\;
|\uparrow\downarrow\downarrow\rangle,\;
|\downarrow\uparrow\uparrow\rangle,\;
|\downarrow\uparrow\downarrow\rangle,\;
|\downarrow\downarrow\uparrow\rangle,\;
|\downarrow\downarrow\downarrow\rangle.
$$

The general state is:
$$
|\Psi\rangle =
c_{000}|\uparrow\uparrow\uparrow\rangle +
c_{001}|\uparrow\uparrow\downarrow\rangle +
c_{010}|\uparrow\downarrow\uparrow\rangle +
c_{011}|\uparrow\downarrow\downarrow\rangle +
c_{100}|\downarrow\uparrow\uparrow\rangle +
c_{101}|\downarrow\uparrow\downarrow\rangle +
c_{110}|\downarrow\downarrow\uparrow\rangle +
c_{111}|\downarrow\downarrow\downarrow\rangle,
$$
with normalization
$$
\sum_{b_1,b_2,b_3\in\{0,1\}} |c_{b_1b_2b_3}|^2 = 1.
$$

### Question

> **If one spin requires 2 amplitudes, how many amplitudes are required for:**
>
> - 2 spins?
> - 10 spins?
> - 100 spins?

### The scaling problem

This is the heart of the problem. For $N$ two-level quantum systems,
$$
\text{number of complex amplitudes} = 2^N.
$$

Let’s see how fast this explodes:

- $N=10$: $2^{10} = 1024$ amplitudes. Totally fine. (Kilobytes.)
- $N=20$: $2^{20} \approx 10^6$ amplitudes. Still fine. (Megabytes.)
- $N=40$: $2^{40} \approx 10^{12}$ amplitudes. Uh oh. (Terabytes.)
- $N=100$: $2^{100} \approx 10^{30}$ amplitudes. Absolutely not.

No classical computer could ever even store that state—forget trying to time-evolve it. This is not a hardware problem. It’s a scaling problem.

### Why classical simulation fails

Classical computers are good at:

- tracking one configuration
- evolving it forward in time
- repeating this many times

Quantum systems require:

- tracking **many configurations at once**
- including their **relative phases**
- which produce **interference**

This is why brute-force search fails, trajectory sampling fails, and “just use a GPU” fails.

Nature is not sampling configurations.
It is evolving the **entire superposition at once**.

> **Quantum mechanics is hard to simulate classically because classical computers were never designed to store or manipulate exponentially large state spaces.**

This is not a bug. It is the central insight that leads to quantum computing.

---

### The next question

If classical computers fail because they cannot represent quantum states…
and nature evolves quantum states effortlessly…

> **Can we use quantum systems themselves as computers?**

This is essentially Feynman’s famous motivation for quantum computing: if nature is quantum, then simulating nature should be done with a quantum machine.

---

### A moment of respect for nature

With your new appreciation for the complexity of quantum systems, let’s admire what nature is quietly doing all around us.

On the left is a caffeine molecule—this is hard, but with supercomputers and many approximations we can often get decent results.

What about the protein on the right?

![[Pasted image 20260108132754.png]]

At this point you might ask: do we really need to keep track of *all* those amplitudes? What if only a polynomial number are important, and the rest are basically zero?

That’s a great question. But then you have a new problem: **how do you know which ones are important?**

One approach is physics intuition and approximation:
- maybe far-apart atoms don’t strongly affect each other,
- maybe only certain orbitals matter,
- maybe correlations are “local.”

That’s the art (and pain) of computational chemistry. It gets us far—but it also raises a brutal question:

How do we know the approximation is accurate, if the exact calculation is already impossible?





## Where Are We with Quantum Computers?

The fundamental challenge is that quantum states are fragile. A qubit in superposition is a delicate thing. Any interaction with the environment—a stray photon, a vibrating atom in the substrate, a fluctuating magnetic field—can "measure" the qubit and collapse the superposition. This is **decoherence**, and it destroys quantum information.

For decades, this seemed like an insurmountable obstacle. Qubits decohered in microseconds; useful computations would take milliseconds or longer. The math did not work.

### Quantum Error Correction: The Breakthrough

The conceptual breakthrough came in the 1990s with quantum error correction. The idea is counterintuitive: you cannot copy a quantum state (the no-cloning theorem forbids it), so how can you back it up? The answer is to encode a single _logical_ qubit across many _physical_ qubits in such a way that errors can be detected and corrected without ever measuring the encoded information directly.

This is not just theoretical. It has a threshold: if the error rate of your physical qubits is below a certain value (roughly 1%), you can stitch them together into logical qubits that have _arbitrarily low_ error rates. You pay for this with overhead—you need many physical qubits per logical qubit—but in principle, you can compute indefinitely.

As of late 2024, we have crossed this threshold. Google's Willow chip (105 physical qubits) demonstrated that their logical qubits are more reliable than their physical qubits—the error correction is _working_, not just adding noise. This is a phase transition. It means the path to fault-tolerant quantum computing is no longer speculative; it is an engineering problem.

### Physical Qubits vs. Logical Qubits

The distinction matters:

- **Physical qubit:** A single hardware device (one superconducting circuit, one trapped ion, one atom, etc.)
- **Logical qubit:** An error-corrected qubit encoded across many physical qubits

Raw physical qubits are noisy. Error correction can, in principle, make logical qubits arbitrarily reliable—but the cost is overhead: many physical qubits per logical qubit.

So when someone announces "we have $N$ qubits," the right questions are: Physical or logical? What error rates? What circuit depth is realistic?

### What These Machines Look Like

If you ever visit a quantum computing lab, here is what you will see:

**Superconducting systems (IBM, Google):** A large gold-colored cylinder, about the size of a chandelier, hanging from the ceiling. This is a dilution refrigerator. Inside, the temperature is about 15 mK—colder than outer space, colder than anywhere in the natural universe. At the bottom sits a chip, maybe 2 cm on a side, containing the qubits: tiny aluminum circuits that behave like artificial atoms. Microwave pulses, delivered through coaxial cables, manipulate the qubits. Gate times are around 20 ns, with coherence times of tens to hundreds of microseconds. The whole system is exquisitely engineered to keep out noise: special foundations to dampen vibrations, carefully filtered wiring, extensive shielding.

**Trapped ion systems (Quantinuum, IonQ):** A steel vacuum chamber, perhaps the size of a shoebox, sitting on an optical table covered in mirrors and lenses. Inside, a few dozen ions—often ytterbium or barium—float in a line, suspended by oscillating electric fields. Lasers do everything: cooling the ions, manipulating their internal states (the qubits), and reading out the results through fluorescence. Gate times are slower (microseconds), but coherence times are much longer (seconds to minutes), and every qubit is identical because they are actual atoms, not fabricated circuits.

**Neutral atom systems (QuEra):** Similar to trapped ions, but using neutral atoms held by optical tweezers—tightly focused laser beams that act like tiny force fields. Arrays of atoms can be rearranged dynamically, allowing flexible connectivity. Entanglement is generated through Rydberg interactions: exciting atoms to high-$n$ states where they have enormous dipole moments and interact strongly over micron-scale distances. This platform has advanced rapidly in the last few years.

No one knows which platform will "win." They may coexist for different applications. The competition is fierce and the progress is fast.

### The Current Moment

We are in the **NISQ era**—Noisy Intermediate-Scale Quantum—but we are leaving it. The machines of 2020 could do impressive physics experiments but nothing practically useful. The machines of 2025 are beginning to cross into "quantum utility": computations that are genuinely easier to do on quantum hardware than on classical supercomputers, for problems people actually care about (mostly in chemistry and materials science).

The next milestone is fault-tolerant quantum computing at scale: thousands of logical qubits with low enough error rates to run deep circuits. That is probably 5–10 years away. When it arrives, algorithms like Shor's (which breaks RSA encryption) and quantum simulation of complex molecules will become practical.

This is the moment we are in: the transition from "it works in principle" to "it works in practice."

### Summary: Two Worlds

|Feature|Classical Computing|Quantum Computing|
|---|---|---|
|**Basic unit**|Bit (0 or 1)|Qubit (coherent superposition)|
|**Key resource**|Transistor count|Superposition and entanglement|
|**Logic**|Boolean (deterministic)|Unitary (interference-based)|
|**State space scaling**|Linear ($N$ bits → $N$ values)|Exponential ($N$ qubits → $2^N$ amplitudes)|
|**Outcome**|Deterministic|Probabilistic (engineered)|

### What to Take Away Before Next Lecture

1. Some problems are hard because the number of possibilities explodes.
2. Many "hard" problems can be rewritten as "find the minimum of an energy function."
3. That connects directly to physics language: Hamiltonians and ground states.
4. Quantum computers exist, but "useful, error-corrected, scalable" is still a work in progress.
5. Next: we define a qubit cleanly and learn how quantum gates move states around.


## Homework

- Keep working on your wedding python code. 
