

### Problem 1: Classical vs Quantum Correlations

**(a)** Bertlmann randomly chooses to put the pink sock on his left foot or right foot with equal probability. Write down the joint probability distribution $P(\text{left}, \text{right})$ for sock colors.

**(b)** What is the correlation $E = \langle L \cdot R \rangle$ if we assign pink $= +1$ and blue $= -1$?

**(c)** Now Bertlmann randomly rotates both socks before putting them on, so each foot independently gets pink or blue with 50% probability. What is the correlation now?

**(d)** Explain why case (a) is analogous to entanglement measured in one basis, but case (c) is what the hidden variable model predicts for other bases.

---

### Problem 2: Verifying the X-Basis Expansion

Prove that $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle) = \frac{1}{\sqrt{2}}(|++\rangle + |--\rangle)$.

**(a)** Express $|0\rangle$ and $|1\rangle$ in terms of $|+\rangle$ and $|-\rangle$.

**(b)** Substitute into $|00\rangle$ and expand.

**(c)** Substitute into $|11\rangle$ and expand.

**(d)** Add and simplify to get $|00\rangle + |11\rangle = |++\rangle + |--\rangle$.

---

### Problem 3: The Singlet State

Consider the singlet state $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$.

**(a)** What are the probabilities of the four outcomes when both qubits are measured in the Z basis?

**(b)** Show that $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|-+\rangle - |+-\rangle)$ in the X basis.

**(c)** What are the probabilities of the four outcomes when both qubits are measured in the X basis?

**(d)** For the Y basis, $|+i\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$ and $|-i\rangle = \frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle)$. Show that the singlet has perfect anti-correlation in the Y basis too.

---

### Problem 4: The Correlation Function

For the singlet state, the correlation function is $E(\hat{a}, \hat{b}) = -\hat{a} \cdot \hat{b} = -\cos\theta$.

**(a)** What is $E$ when Alice and Bob measure along the same axis ($\theta = 0$)?

**(b)** What is $E$ when they measure along perpendicular axes ($\theta = 90°$)?

**(c)** What is $E$ when they measure along opposite axes ($\theta = 180°$)?

**(d)** At what angle $\theta$ is $E = -1/2$?

---

### Problem 5: Designing Hidden Variable Models

The simple hidden variable model (mixture of $|00\rangle$ and $|11\rangle$) failed because it predicted no X-basis correlations. Let's try to do better.

**(a)** Design a hidden variable model where each pair is assigned one of four hidden states:
- State A: "return +1 for both Z and X measurements" for both qubits
- State B: "return -1 for both Z and X measurements" for both qubits  
- State C: "return +1 for Z, -1 for X" for both qubits
- State D: "return -1 for Z, +1 for X" for both qubits

With equal probability (25% each), what does this model predict for:
- Z-basis correlation?
- X-basis correlation?

**(b)** Can you choose different probabilities for states A, B, C, D to match the quantum predictions (perfect correlation in both bases)? Try to find probabilities that work, or explain why it's impossible.

**(c)** Now consider a third basis, Y. In quantum mechanics, $|\Phi^+\rangle$ also has perfect correlation in Y. Can your model from part (b) also give perfect Y-correlation? Why or why not?

**(d)** This illustrates why hidden variable models fail: they can match quantum predictions in one or two bases, but not all simultaneously. Explain in your own words why adding more bases makes the hidden variable task harder.

---

### Problem 6: No-Signaling

Alice and Bob share $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$.

**(a)** If Alice measures in the Z basis, what are Bob's probabilities of getting 0 or 1 when he measures in Z?

**(b)** If Alice measures in the X basis instead, what are Bob's probabilities of getting 0 or 1 when he measures in Z? 

*Hint: Consider all possible Alice outcomes and weight by probability.*

**(c)** Compare your answers to (a) and (b). Can Bob tell which measurement Alice performed by looking at his own results?

**(d)** Explain why this result guarantees that entanglement cannot be used for faster-than-light communication.

---

### Problem 7: EPR Argument Analysis

Reconstruct the EPR argument in your own words.

**(a)** What is the "locality" assumption?

**(b)** What is the "realism" assumption (that measurement reveals pre-existing values)?

**(c)** How do these assumptions, combined with quantum predictions, lead to the conclusion that quantum mechanics is "incomplete"?

**(d)** Bell's theorem shows that quantum mechanics violates certain inequalities derived from local realism. What does this imply about the EPR assumptions?

---

### Problem 8: Qiskit Exploration

**(a)** Create the singlet state $|\Psi^-\rangle$ in Qiskit and verify its amplitudes.

**(b)** Measure it 10,000 times in the Z basis. Verify perfect anti-correlation.

**(c)** Measure it 10,000 times in the X basis (apply H to both qubits before measuring). Verify perfect anti-correlation.

**(d)** Measure with Alice in Z and Bob at angle $\theta = 45°$ (apply $R_y(\pi/4)$ to Bob's qubit before measuring). Calculate the correlation $E$ from your results and compare to the theoretical prediction $E = -\cos(45°) = -1/\sqrt{2}$.

---

### Problem 9: Conceptual Questions

**(a)** Alice and Bob share an entangled state. Alice measures and gets a result. Has anything "physical" happened to Bob's qubit? Discuss different interpretations.

**(b)** Why can't we use the perfect correlations in entanglement to send a message faster than light?

**(c)** Einstein called entanglement "spooky action at a distance." Is there actually any "action" (physical influence) at a distance? What would Einstein say? What would a modern physicist say?

**(d)** If quantum correlations can't be explained by hidden variables (as Bell's theorem suggests), what are the alternatives?

---

## Looking Ahead

We've seen that quantum correlations are stronger than classical correlations — they persist across multiple measurement bases. And we've previewed Bell's key insight: this strength can be quantified, leading to testable inequalities.

Next lecture, we'll derive the CHSH inequality and prove that quantum mechanics violates it. This is the mathematical heart of the "quantum vs classical" distinction — and the foundation for understanding why quantum computers might outperform classical ones.