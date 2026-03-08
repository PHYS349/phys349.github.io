# Midterm Practice Exam

The practice exam will be released shortly. I'm finishing it Sunday afternoon. It will be largely based on homework problems — study those to prepare.  NOTE NO HOMEWORK IS DUE THIS WEEK.   But practice the problems from 3.3 and 3.4.  We will go over the problem again on Tuesday that could be on the exam. 



## Lecture 1.1 — Why Quantum? Why Now?

**1.** What property of matter explains why atoms have discrete energy levels rather than a continuous range of energies? Briefly explain the physical mechanism.

**2.** Explain the distinction between the First Quantum Revolution and the Second Quantum Revolution. What is the key difference in how quantum mechanics is used in each?

**3.** Name two technologies from the list below that came out of the First Quantum Revolution and explain what quantum principle each one relies on. Then describe one capability that the Second Quantum Revolution is expected to enable and why it was not possible in the first.

*First Quantum Revolution:* transistor, laser, MRI, superconductors, semiconductors, magnetic memory, LED, solar cells.

*Second Quantum Revolution:* qubits, atomic clocks (Ramsey interferometry), quantum computing, quantum sensing.

## Lecture 1.2 — What Are Classical Computers Bad At?

**4.** A system has $N$ components, each with 2 possible states. How does the total number of configurations scale with $N$? Give the expression and explain why this scaling makes brute-force search impractical for large $N$.

**5.** In the wedding-planning problem, what feature of the problem causes it to become hard as the number of guests increases? Why can't you simply break it into smaller independent pieces?

**6.** For each problem below, state whether the computational cost scales polynomially, exponentially, or super-exponentially with system size. Briefly justify each answer.

- Simulating the trajectory of $N$ particles with no interactions between them
- Simulating the trajectory of $N$ particles with pairwise interactions between every particle
- $N \times N$ matrix multiplications in a large language model
- Finding the ground state configuration of $N$ interacting spins in a magnetic material
- Finding the absolute minimum of the cost function for seating $N$ wedding guests
- Finding the minimum energy of a molecule with $N$ electrons

## Lecture 1.3 — What Makes Quantum Hard?

**7.** The Ising Hamiltonian for a 1D spin chain is

$$H = -J \sum_{i} s_i s_{i+1} - B \sum_{i} s_i$$

where $s_i = \pm 1$. Consider an antiferromagnet ($J < 0$) with an external field $B > 0$.

**(a)** When $B = 0$, what does the ground state look like and why?

**(b)** When $B$ is very large compared to $|J|$, what does the ground state look like and why?

**(c)** Explain in words why there must be a phase transition at some intermediate value of $B$.

**8.** A classical system of $N$ spins is described by specifying one configuration — a list of $N$ values, each $\pm 1$. A quantum system of $N$ spins is described by specifying a complex amplitude for every possible configuration. How many complex amplitudes are needed? Why does this make quantum systems fundamentally harder to simulate on a classical computer than classical systems?

**9.** A common misconception is that a quantum computer "tries all answers at once and picks the best one." Explain why this is wrong. What must actually happen during a quantum computation — before measurement — for the correct answer to be likely? Your answer should use the word *interference*.

## Lecture 1.4 — Quantum Hardware: Where Are We?

**10.** Explain the physical difference between $T_1$ (energy relaxation time) and $T_2$ (dephasing time) for a qubit. Which process can destroy quantum information without the qubit changing its population?

**11.** A single-qubit gate requires isolating one qubit and applying a pulse. A two-qubit gate requires two qubits to interact with each other while remaining isolated from everything else. Explain why this tension makes two-qubit gates fundamentally harder and why two-qubit gate fidelity is typically the more important benchmark.

**12.** In classical computing, you can copy a bit and use majority voting to correct errors. Give two reasons why this same strategy fails for qubits.

## Lecture 2.1 — Waves, Complex Numbers, and Interference

**13.** The spatial wave equation is

$$\frac{d^2}{dx^2} E(x) = -k^2 E(x)$$

**(a)** Give two linearly independent real solutions to this equation.

**(b)** Give two linearly independent complex solutions to this equation.

**14.** A wave is described by $E(t) = E_0 \cos(\omega t)$.

**(a)** Rewrite this wave in complex exponential form.

**(b)** Using the complex form, find the expression for the intensity $|E|^2$. Show that this gives the same result as the time-averaged intensity $\langle E^2 \rangle$ of the real form (up to a factor of 2).

**15.** Two waves $E_1 = e^{ikx}$ and $E_2 = e^{i(kx + \phi)}$ are incident on a beam splitter. One output port gives $E_1 + E_2$ and the other gives $E_1 - E_2$.

**(a)** Using the complex form, compute the intensity $|E_1 + E_2|^2$ at the first output. Show how it depends on $\phi$.

**(b)** Compute the intensity $|E_1 - E_2|^2$ at the second output. How does it depend on $\phi$?

**(c)** For what value of $\phi$ does all the power go to one output? What about the other?

## Lecture 2.2 — Interferometers and the Bloch Sphere

**16.** Light enters one input of a Mach-Zehnder interferometer (gate sequence $H \to P(\phi) \to H$, starting from $|0\rangle$). For each case below, draw the path on the Bloch sphere, labeling the state after each gate.

**(a)** Find the value of $\phi$ such that all the light exits output port $|0\rangle$. Draw the Bloch sphere trajectory.

**(b)** Find the value of $\phi$ such that all the light exits output port $|1\rangle$. Draw the Bloch sphere trajectory.

**18.** On a Bloch sphere, label the locations of the following states:

- $|0\rangle$ and $|1\rangle$
- $|X_+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ and $|X_-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$
- $|Y_+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ and $|Y_-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$

Note: different textbooks may use different sign conventions for $|X_\pm\rangle$ and $|Y_\pm\rangle$. What matters is that the two states in each pair are orthogonal. Verify that orthogonal states appear at antipodal (opposite) points on the Bloch sphere.

**17.** A Mach-Zehnder interferometer consists of: a first beam splitter that splits light into two arms, a relative phase shift $\phi$ accumulated between the arms, and a second beam splitter that recombines the light.

The available quantum gates are the Hadamard gate $H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$ and the phase gate $P(\phi) = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\phi} \end{pmatrix}$.

**(a)** What sequence of gates, applied to input state $|0\rangle$, describes the MZI? Write $|\psi_{out}\rangle$ in terms of $H$, $P(\phi)$, and $|0\rangle$.


