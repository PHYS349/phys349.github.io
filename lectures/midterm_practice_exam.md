# Midterm Practice Exam

The practice exam will be released shortly. I'm finishing it sunday afternoon. I'm still adding problems. It will be largely based on homework problems — study those to prepare.
-3/8/2026


# Midterm Practice Exam

The practice exam will be released shortly. I'm finishing it Sunday afternoon. It will be largely based on homework problems — study those to prepare.

-3/8/2026

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

