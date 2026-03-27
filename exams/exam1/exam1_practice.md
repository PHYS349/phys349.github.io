# Midterm Practice Exam

The practice exam will be released shortly. I'm finishing it Sunday afternoon. It will be largely based on homework problems — study those to prepare.  NOTE NO HOMEWORK IS DUE THIS WEEK.  The practice problems from 3.3 and 3.4 are the same as the homeworks problems.  We will go over the problem again on Tuesday that could be on the exam.


## Lecture 1.1 — Why Quantum? Why Now?

**1.** What property of matter explains why atoms have discrete energy levels rather than a continuous range of energies? Briefly explain the physical mechanism.

---

**2.** Explain the distinction between the First Quantum Revolution and the Second Quantum Revolution. What is the key difference in how quantum mechanics is used in each?

---

**3.** Name two technologies from the list below that came out of the First Quantum Revolution and explain what quantum principle each one relies on. Then describe one capability that the Second Quantum Revolution is expected to enable and why it was not possible in the first.

*First Quantum Revolution:* transistor, laser, MRI, superconductors, semiconductors, magnetic memory, LED, solar cells.

*Second Quantum Revolution:* qubits, atomic clocks (Ramsey interferometry), quantum computing, quantum sensing.


## Lecture 1.2 — What Are Classical Computers Bad At?

**4.** A system has $N$ components, each with 2 possible states. How does the total number of configurations scale with $N$? Give the expression and explain why this scaling makes brute-force search impractical for large $N$.

---

**5.** In the wedding-planning problem, what feature of the problem causes it to become hard as the number of guests increases? Why can't you simply break it into smaller independent pieces?

---

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

---

**8.** A classical system of $N$ spins is described by specifying one configuration — a list of $N$ values, each $\pm 1$. A quantum system of $N$ spins is described by specifying a complex amplitude for every possible configuration. How many complex amplitudes are needed? Why does this make quantum systems fundamentally harder to simulate on a classical computer than classical systems?

---

**9.** A common misconception is that a quantum computer "tries all answers at once and picks the best one." Explain why this is wrong. What must actually happen during a quantum computation — before measurement — for the correct answer to be likely? Your answer should use the word *interference*.


## Lecture 1.4 — Quantum Hardware: Where Are We?

**10.** Explain the physical difference between $T_1$ (energy relaxation time) and $T_2$ (dephasing time) for a qubit. Which process can destroy quantum information without the qubit changing its population?

---

**11.** A single-qubit gate requires isolating one qubit and applying a pulse. A two-qubit gate requires two qubits to interact with each other while remaining isolated from everything else. Explain why this tension makes two-qubit gates fundamentally harder and why two-qubit gate fidelity is typically the more important benchmark.

---

**12.** In classical computing, you can copy a bit and use majority voting to correct errors. Give two reasons why this same strategy fails for qubits.


## Lecture 2.1 — Waves, Complex Numbers, and Interference

**13.** The spatial wave equation is

$$\frac{d^2}{dx^2} E(x) = -k^2 E(x)$$

**(a)** Give two linearly independent real solutions to this equation.

**(b)** Give two linearly independent complex solutions to this equation.

---

**14.** A wave is described by $E(t) = E_0 \cos(\omega t)$.

**(a)** Rewrite this wave in complex exponential form.

**(b)** Using the complex form, find the expression for the intensity $|E|^2$. Show that this gives the same result as the time-averaged intensity $\langle E^2 \rangle$ of the real form (up to a factor of 2).

---

**15.** Two waves $E_1 = e^{ikx}$ and $E_2 = e^{i(kx + \phi)}$ are incident on a beam splitter. One output port gives $E_1 + E_2$ and the other gives $E_1 - E_2$.

**(a)** Using the complex form, compute the intensity $|E_1 + E_2|^2$ at the first output. Show how it depends on $\phi$.

**(b)** Compute the intensity $|E_1 - E_2|^2$ at the second output. How does it depend on $\phi$?

**(c)** For what value of $\phi$ does all the power go to one output? What about the other?


## Lecture 2.2 — Interferometers and the Bloch Sphere

**16.** Light enters one input of a Mach-Zehnder interferometer (gate sequence $H \to P(\phi) \to H$, starting from $|0\rangle$). For each case below, draw the path on the Bloch sphere, labeling the state after each gate.

**(a)** Find the value of $\phi$ such that all the light exits output port $|0\rangle$. Draw the Bloch sphere trajectory.

**(b)** Find the value of $\phi$ such that all the light exits output port $|1\rangle$. Draw the Bloch sphere trajectory.

---

**17.** A Mach-Zehnder interferometer consists of: a first beam splitter that splits light into two arms, a relative phase shift $\phi$ accumulated between the arms, and a second beam splitter that recombines the light.

The available quantum gates are the Hadamard gate $H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$ and the phase gate $P(\phi) = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\phi} \end{pmatrix}$.

**(a)** What sequence of gates, applied to input state $|0\rangle$, describes the MZI? Write $|\psi_{out}\rangle$ in terms of $H$, $P(\phi)$, and $|0\rangle$.

---

**18.** On a Bloch sphere, label the locations of the following states:

- $|0\rangle$ and $|1\rangle$
- $|X_+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ and $|X_-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$
- $|Y_+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ and $|Y_-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$

Note: different textbooks may use different sign conventions for $|X_\pm\rangle$ and $|Y_\pm\rangle$. What matters is that the two states in each pair are orthogonal. Verify that orthogonal states appear at antipodal (opposite) points on the Bloch sphere.


## Lecture 2.3 — Polarization and Measurement

**19.** An $|H\rangle$-polarized photon is sent toward a $|V\rangle$ polarizer.

**(a)** What fraction of the light is transmitted?

**(b)** Now a $|D\rangle = \frac{1}{\sqrt{2}}(|H\rangle + |V\rangle)$ polarizer is inserted between the source and the $|V\rangle$ polarizer. Walk through the sequence step by step: what is the state after each polarizer, and what is the total fraction of light transmitted?

**(c)** Explain why inserting an extra polarizer — which can only absorb light — actually *increases* the total transmission. What does the intermediate polarizer do to the quantum state?

---

**20.** A single photon enters a beam splitter and travels through both arms of a Mach-Zehnder interferometer. At the output, detectors D1 and D2 each record clicks — but every photon produces exactly one click at one detector, never both.

**(a)** What experimental evidence tells us the photon traveled through both arms, not just one?

**(b)** At what stage of the experiment does the photon stop behaving like a wave (superposition in both arms, producing interference) and start behaving like a particle (one click at one detector)? Describe what happens physically at that stage.


## Lectures 2.4 & 2.5 — Groups, Pauli Matrices, and SU(2)

**21.** Both SO(3) and SU(2) describe rotations, but they are not the same group. Explain the key differences: what does each group act on, what are the generators of each, and what happens when you rotate by $2\pi$? What is the physical consequence of the difference — how does it distinguish fermions from bosons?

---

**22.** A qubit starts in the state $|0\rangle$. The rotation operator $R_x(\theta) = e^{-i\sigma_x \theta/2}$ is applied.

**(a)** Write out $R_x(\theta)$ as an explicit $2 \times 2$ matrix using the formula $R_{\hat{n}}(\theta) = \cos(\theta/2)\,I - i\sin(\theta/2)\,(\hat{n}\cdot\vec{\sigma})$.

**(b)** Apply $R_x(\theta)$ to $|0\rangle$ and find the output state as a function of $\theta$.

**(c)** Describe the path this state traces on the Bloch sphere as $\theta$ goes from $0$ to $2\pi$. What are the states at $\theta = 0$, $\pi/2$, $\pi$, and $3\pi/2$?

---

**23.** State the four properties a set and operation must satisfy to form a group. Then for each of the following, state whether it is a group and briefly explain why or why not:

- Rotations on a circle (all angles, under composition)
- Rotations on a sphere (all axes and angles, under composition)
- Shifts on a number line (any direction, under addition)
- Shifts on a number line in only one direction (positive shifts only, under addition)

---

**24.** What is the generator of rotations about the $z$-axis in SU(2)? Write the corresponding rotation matrix $R_z(\theta)$ in terms of this generator.


## Lecture 2.6 — Time Evolution and Rabi Oscillations

**25.** Consider the rotation $R_{\hat{n}}(\theta) = e^{-i\theta(\hat{n}\cdot\vec{\sigma})/2}$ with $\hat{n} = \frac{1}{\sqrt{2}}(\hat{x} + \hat{y})$ and $\theta = \pi/2$. Starting from $|0\rangle$, draw on the Bloch sphere the rotation axis and the resulting state after the rotation is applied.

---

**26.** The Schrodinger equation is $H|\psi(t)\rangle = i\hbar \frac{\partial}{\partial t}|\psi(t)\rangle$.

**(a)** Derive the time evolution operator $U(t)$ such that $|\psi(t)\rangle = U(t)|\psi(0)\rangle$. Show that $U(t) = e^{-iHt/\hbar}$.

**(b)** Why must $U(t)$ be unitary? What physical principle does this enforce?

---

**27.** A qubit has Hamiltonian $H = \frac{\hbar\omega}{2}\sigma_z$ and starts in the state $|\psi(0)\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$.

**(a)** Find $|\psi(t)\rangle$.

**(b)** Describe the motion on the Bloch sphere. What kind of motion is this?

**(c)** Do the populations $P_0(t)$ and $P_1(t)$ change with time? Why or why not?

---

**28.** A qubit has Hamiltonian $H = \frac{\hbar\Omega}{2}\sigma_x$ and starts in $|0\rangle$. The time evolution operator is $U(t) = e^{-i\Omega t\,\sigma_x/2} = R_x(\Omega t)$.

**(a)** Find the state $|\psi(t)\rangle$ and draw the rotation axis on the Bloch sphere.

**(b)** Draw the state on the Bloch sphere at $\Omega t = \pi/2$, $\pi$, and $2\pi$. Label each state.

**(c)** What is the name for the pulse at $\Omega t = \pi$? What does it do physically?

**(d)** What is the name for the pulse at $\Omega t = \pi/2$? What does it do physically?

---

**29.** A qubit has Hamiltonian $H = \frac{\hbar\Omega}{2}\sigma_x$.

**(a)** Find the energy eigenstates of this Hamiltonian and their eigenvalues. Draw them on the Bloch sphere.

**(b)** If the qubit starts in one of these eigenstates, what happens under time evolution? Does the state move on the Bloch sphere?

**(c)** Explain in one sentence the general rule: why are eigenstates of the Hamiltonian stationary?


## Lecture 2.7 — Atomic Clocks and the Rotating Frame

**30.** The full Hamiltonian for a two-level atom driven by a laser at frequency $\omega_L$ is

$$H = \frac{\hbar\omega_0}{2}\sigma_z + \hbar\Omega\cos(\omega_L t)\sigma_x$$

where $\omega_0$ is the atomic transition frequency and $\Omega$ is the Rabi frequency.

**(a)** This Hamiltonian is time-dependent. In the rotating frame (rotating at $\omega_L$), the Hamiltonian becomes time-independent. Write the rotating frame Hamiltonian $H_R$ in terms of $\Delta = \omega_0 - \omega_L$ and $\Omega$.

**(b)** What does each term in $H_R$ correspond to physically? What does $\Delta$ represent?

**(c)** What does $H_R$ simplify to when the laser is exactly on resonance ($\Delta = 0$)?

---

**31.** A Ramsey sequence consists of: a $\pi/2$-pulse, free evolution for time $T$, then a second $\pi/2$-pulse. The qubit starts in $|0\rangle$. The $\pi/2$-pulse is a rotation $R_x(\pi/2)$ around the $x$-axis on the Bloch sphere.

**(a)** On resonance ($\Delta = 0$): Apply the first $\pi/2$-pulse to $|0\rangle$. What state is the qubit in? Draw it on the Bloch sphere.

**(b)** Still on resonance: there is no precession during the wait time ($\Delta = 0$). Apply the second $\pi/2$-pulse. Where does the state end up? Draw the full sequence on the Bloch sphere.

**(c)** Now suppose the laser is slightly detuned ($\Delta \neq 0$). After the first $\pi/2$-pulse, the laser is turned off and the qubit precesses around the $z$-axis at rate $\Delta$ during the free evolution time $T$. The accumulated phase is $\phi = \Delta T$. Draw the state on the Bloch sphere after the first $\pi/2$-pulse, after precessing by phase $\phi$, and after the second $\pi/2$-pulse.

**(d)** For the on-resonance case (part b), the second $\pi/2$-pulse sent the qubit to $|1\rangle$. How long must you wait (what value of $T$ in terms of $\Delta$) so that the second $\pi/2$-pulse instead sends the qubit back to $|0\rangle$? Draw this case on the Bloch sphere and explain why the precession reverses the outcome.

---

**32.** In a real experiment, the qubit is not perfectly isolated. During the free evolution time $T$ in a Ramsey sequence, $T_2$ dephasing causes random phase kicks that scramble the accumulated phase.

**(a)** On the Bloch sphere, the qubit starts at a definite point on the equator after the first $\pi/2$-pulse. If you repeat the experiment many times, each run accumulates a slightly different random phase. Draw what the collection of Bloch vectors from many runs looks like after the free evolution — at short times ($T \ll T_2$) and at long times ($T \gg T_2$).

**(b)** When you average over all the runs, the Bloch vector shrinks. What direction does it shrink toward? What does this mean for the contrast of the Ramsey fringes?

**(c)** Explain why $T_2$ dephasing limits the precision of an atomic clock. What sets the maximum useful free evolution time $T$?


## Lecture 3.1 — Two Qubits and the Tensor Product

**33.** Compute the following tensor products and write the result as a 4-component column vector.

**(a)** $|0\rangle \otimes |1\rangle$

**(b)** $|1\rangle \otimes |+\rangle$ where $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$

**(c)** $|+\rangle \otimes |-\rangle$ where $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$

**(d)** $|+\rangle \otimes |+\rangle$

---

**34.** Three qubits.

**(a)** How many computational basis states are there for three qubits? List them all.

**(b)** Write $|+\rangle \otimes |0\rangle \otimes |1\rangle$ as a sum of computational basis states.

**(c)** The GHZ state is $|GHZ\rangle = \frac{1}{\sqrt{2}}(|000\rangle + |111\rangle)$. Is it a product state? Try to factor it.

**(d)** If you measure all three qubits of $|GHZ\rangle$ in the computational basis, what outcomes are possible?


## Lecture 3.2 — Entanglement and the Bell States

**35.** Bell state verification.

**(a)** Write the amplitude table for each of the four Bell states and verify that $|\det(C)| = 1/2$ for all of them.

**(b)** Verify orthogonality: compute $\langle\Phi^+|\Phi^-\rangle$, $\langle\Phi^+|\Psi^+\rangle$, and $\langle\Psi^+|\Psi^-\rangle$.

**(c)** Show that $|\Phi^+\rangle$ can be written as: $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|{+}{+}\rangle + |{-}{-}\rangle)$, where $|{+}{+}\rangle = |+\rangle \otimes |+\rangle$ and $|{-}{-}\rangle = |-\rangle \otimes |-\rangle$. What does this say about correlations in the X-basis?

---

**36.** Projective measurement.

A qubit is in state $|\Psi\rangle = \frac{1}{\sqrt{3}}|0\rangle + \sqrt{\frac{2}{3}}|1\rangle$.

**(a)** Measure in the Z-basis. What are the probabilities for each outcome? What is the state after each outcome?

**(b)** Measure in the X-basis. Construct the projectors $|+\rangle\langle+|$ and $|-\rangle\langle-|$. What are the probabilities? What is the state after each outcome?

**(c)** Verify that the probabilities sum to 1 and the projectors satisfy $|+\rangle\langle+| + |-\rangle\langle-| = I$.

---

**37.** The uncertainty principle for spin.

**(a)** Verify the commutation relation $[\sigma_x, \sigma_y] = 2i\sigma_z$ by explicit matrix multiplication, where:

$$\sigma_x = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, \quad \sigma_y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}, \quad \sigma_z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}$$

**(b)** Show that $\sigma_i^2 = I$ for each Pauli matrix. Conclude that $(\Delta\sigma_i)^2 = 1 - \langle\sigma_i\rangle^2$ in any state.

**(c)** For the state $|0\rangle$: compute $\langle\sigma_x\rangle$ and $\langle\sigma_y\rangle$. Use part (b) to find $\Delta\sigma_x$ and $\Delta\sigma_y$. Are they maximal?

**(d)** Verify the same result by expanding $|0\rangle$ in the X-eigenbasis. What are the probabilities for getting $+1$ and $-1$?

**(e)** For the state $|+\rangle$: which component is definite? Compute $\Delta\sigma_y$ and $\Delta\sigma_z$. Verify $\Delta\sigma_y \cdot \Delta\sigma_z \geq |\langle\sigma_x\rangle|$.

**(f)** Can a state have $\Delta\sigma_x = \Delta\sigma_y = \Delta\sigma_z = 0$ (definite values for all three components simultaneously)? Explain using the commutation relations.


## Lecture 3.3 — Two-Qubit Measurement, Spooky Action, and EPR

**38.** Partial measurement of a product state.

Alice and Bob share the product state $|+\rangle \otimes |0\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |10\rangle)$. Alice measures her qubit in the Z-basis.

**(a)** Compute the probability that Alice gets $|0\rangle$.

**(b)** Compute the post-measurement state if Alice gets $|0\rangle$.

**(c)** Compute the probability and post-measurement state if Alice gets $|1\rangle$.

**(d)** What is Bob's state in each case? Explain why this is not "spooky action at a distance."

---

**39.** Partial measurement of an entangled state.

Alice and Bob share $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$. Alice measures her qubit in the Z-basis.

**(a)** Use the projector $|0\rangle\langle 0| \otimes I$ to compute the probability and post-measurement state for Alice getting $|0\rangle$.

**(b)** Repeat for Alice getting $|1\rangle$.

**(c)** What is Bob's state after each outcome?

**(d)** In what sense has the entanglement been broken by the measurement?

---

**40.** No-signaling.

Alice and Bob share $|\Phi^+\rangle$. Bob measures Z on his qubit.

**(a)** Suppose Alice measures Z first. What are Bob's Z-measurement probabilities?

**(b)** Suppose Alice measures X first. What are Bob's Z-measurement probabilities now?

**(c)** Suppose Alice does nothing. Compute Bob's Z-measurement probabilities directly from the state.

**(d)** Explain how these results support the no-signaling theorem.

---

**41.** Alice chooses Z or X.

Alice and Bob share $|\Psi^-\rangle$.

**(a)** If Alice measures Z and gets $+1$, what state is Bob's qubit in?

**(b)** If Alice measures X and gets $+1$, what state is Bob's qubit in?

**(c)** In which sense are these two Bob states physically different?

**(d)** Why can't Bob tell, from local measurements alone, which basis Alice chose?

---

**42.** The EPR argument.

Alice and Bob share the singlet state and are far apart.

**(a)** If Alice measures Z and gets $+1$, what can she predict about Bob's Z outcome?

**(b)** If Alice instead measures X and gets $+1$, what can she predict about Bob's X outcome?

**(c)** State EPR's criterion for an "element of physical reality."

**(d)** Use locality plus this criterion to explain why EPR concludes Bob must have both a definite Z-value and a definite X-value.

**(e)** Why does this lead EPR to say that quantum mechanics is incomplete?


## Lecture 3.4 — Bell's Theorem

**43.** The two-setting hidden variable model.

Alice and Bob share the singlet state, and each can choose to measure either $Z$ or $X$. Assume a local hidden-variable model in which each pair carries predetermined values $a_Z, a_X \in \{+1,-1\}$, with $b_Z = -a_Z$ and $b_X = -a_X$.

**(a)** Write all four possible lookup tables $(a_Z,a_X,b_Z,b_X)$.

**(b)** Verify that equal weights $p_1=p_2=p_3=p_4=\tfrac14$ give $E(Z,Z)=-1$, $E(X,X)=-1$, and $E(Z,X)=0$.

**(c)** Is the equal-weight distribution the only one that works? Find the most general distribution $(p_1,p_2,p_3,p_4)$ consistent with these three correlations.

I TOOK OUT THE Q-AXIS PROBLEMS.
<!-- ---

**44.** The third axis and the quantum predictions.

Define the third measurement axis $\sigma_Q=\frac{1}{\sqrt{2}}(\sigma_z+\sigma_x)$.

**(a)** Verify that $\sigma_Q^2=I$.

**(b)** For the singlet state, quantum mechanics predicts $E(\hat a,\hat b)=-\cos\theta$, where $\theta$ is the angle between the measurement axes. Use this to compute $E(Z,Q)$, $E(X,Q)$, and $E(X,Z)$.

**(c)** For $\pm 1$ outcomes, show that $P(\text{disagree})=\frac{1-E}{2}$.

**(d)** Hence compute $P(\text{disagree on } ZQ)$, $P(\text{disagree on } XQ)$, and $P(\text{disagree on } XZ)$.

---

**45.** Verifying the correlation function.

For the singlet $|\Psi^-\rangle=\frac{1}{\sqrt{2}}(|01\rangle-|10\rangle)$, verify the correlation formula directly in the following cases:

**(a)** $\hat a=\hat b=\hat z$ by computing $\langle \Psi^-|(\sigma_z\otimes\sigma_z)|\Psi^-\rangle$.

**(b)** $\hat a=\hat z$, $\hat b=\hat x$ by computing $\langle \Psi^-|(\sigma_z\otimes\sigma_x)|\Psi^-\rangle$.

**(c)** $\hat a=\hat z$, $\hat b=\hat q=\frac{1}{\sqrt{2}}(\hat z+\hat x)$ by computing $\langle \Psi^-|(\sigma_z\otimes\sigma_Q)|\Psi^-\rangle$.

---

**46.** Why three settings matter.

**(a)** Explain why no contradiction appears when only two settings ($X$ and $Z$) are used.

**(b)** Explain in your own words why adding a third setting $Q$ creates a contradiction. -->
