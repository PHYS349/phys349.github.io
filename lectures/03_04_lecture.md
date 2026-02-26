
# Lecture 3.4 — Bell's Theorem

## Where We Left Off

Last lecture we showed that three assumptions — **locality**, **realism**, and **completeness of QM** — are mutually inconsistent. The EPR argument (1935) assumed locality and realism and concluded that quantum mechanics is incomplete: there must be hidden variables carrying predetermined measurement outcomes.

We made this precise with the **lookup table** picture. If realism is true, each particle pair carries a table of predetermined $\pm 1$ answers. If locality is true, Alice's choice of measurement setting can't change Bob's table entries. The question we left open:

> **Can any distribution over lookup tables reproduce the quantum predictions?**

Today we answer that question. The answer is **no**.

---

## Part 1: Two Settings — The Hidden Variable Model Works

### The Setup

Alice and Bob share the singlet state

$$|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle).$$

Each can choose to measure along either $Z$ or $X$. The quantum predictions are:

- **Same axis:** perfect anti-correlation.
  $$E(Z,Z) = -1, \qquad E(X,X) = -1.$$
  Whenever they measure the same thing, they always get opposite outcomes.

- **Different axes:** no correlation.
  $$E(Z,X) = E(X,Z) = 0.$$
  When they measure different things, the outcomes are completely random — each of the four possibilities $(+1,+1)$, $(+1,-1)$, $(-1,+1)$, $(-1,-1)$ occurs with probability $1/4$.

### Building the Lookup Table

Let's try Einstein's program. Each pair carries a lookup table $\lambda$ with predetermined values:

$$a_Z, \quad a_X \in \{+1, -1\} \qquad \text{(Alice's predetermined outcomes).}$$

The singlet has perfect anti-correlation along any shared axis, so Bob's values are forced:

$$b_Z = -a_Z, \qquad b_X = -a_X.$$

That leaves only two free bits: Alice's $Z$-value and Alice's $X$-value. There are $2^2 = 4$ possible lookup tables:

| Table $\lambda$ | $a_Z$ | $a_X$ | $b_Z$ | $b_X$ |
|---|---|---|---|---|
| 1 | $+1$ | $+1$ | $-1$ | $-1$ |
| 2 | $+1$ | $-1$ | $-1$ | $+1$ |
| 3 | $-1$ | $+1$ | $+1$ | $-1$ |
| 4 | $-1$ | $-1$ | $+1$ | $+1$ |

Assign each table equal probability: $p_1 = p_2 = p_3 = p_4 = 1/4$.

### Checking the Predictions

**ZZ correlation.** Every table has $b_Z = -a_Z$, so $a_Z \cdot b_Z = -1$ always. Therefore $E(Z,Z) = -1$. ✓

**XX correlation.** Every table has $b_X = -a_X$, so $a_X \cdot b_X = -1$ always. Therefore $E(X,X) = -1$. ✓

**ZX correlation (Alice measures Z, Bob measures X).** Compute $a_Z \cdot b_X$ for each table:

| Table | $a_Z$ | $b_X$ | $a_Z \cdot b_X$ |
|---|---|---|---|
| 1 | $+1$ | $-1$ | $-1$ |
| 2 | $+1$ | $+1$ | $+1$ |
| 3 | $-1$ | $-1$ | $+1$ |
| 4 | $-1$ | $+1$ | $-1$ |

Average:

$$E(Z,X) = \frac{1}{4}(-1 + 1 + 1 - 1) = 0.$$

✓

Every quantum prediction is reproduced exactly. The hidden variable model works perfectly.

```{admonition} Einstein Wins (So Far)
:class: note

With two measurement settings, a simple lookup table model reproduces all quantum predictions for the singlet state. The correlations that seemed "spooky" are no different from classical correlations that were predetermined at the source.

If this were the whole story, Einstein could say: quantum mechanics is incomplete but useful, like thermodynamics before atoms.
```

```{admonition} iClicker
:class: tip

We just showed that a hidden variable model works perfectly for two measurement settings. Do you think it will still work if we add a third measurement axis?

**(A)** Yes — just add more entries to the lookup table

**(B)** No — something will go wrong

**(C)** It depends on the angle of the third axis
```

---

## Part 2: The Third Axis

### A New Measurement Direction

The hidden variable model worked because with two orthogonal axes ($Z$ and $X$ at $90°$), there is enough freedom to assign predetermined values consistently. Now we add a **third** measurement axis.

Define $Q$ as the axis at **45°** between $Z$ and $X$ on the Bloch sphere. The corresponding operator is:

$$\sigma_Q = \frac{1}{\sqrt{2}}(\sigma_z + \sigma_x) = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}.$$

Its eigenstates are:

$$|Q_+\rangle = \cos\frac{\pi}{8}\,|0\rangle + \sin\frac{\pi}{8}\,|1\rangle, \qquad |Q_-\rangle = -\sin\frac{\pi}{8}\,|0\rangle + \cos\frac{\pi}{8}\,|1\rangle,$$

where $\pi/8 = 22.5°$. We won't need the explicit eigenstates — what matters is the **angles** between the three axes.

### The Correlation Function

For the singlet state, quantum mechanics predicts a simple correlation when Alice measures along axis $\hat{a}$ and Bob along axis $\hat{b}$:

$$E(\hat{a}, \hat{b}) = -\cos\theta,$$

where $\theta$ is the angle between the two measurement axes on the Bloch sphere.

Why is this plausible? The singlet is rotationally invariant (total spin zero), so the correlation can only depend on the relative angle. When $\theta = 0°$ (same axis), perfect anti-correlation gives $E = -1$. When $\theta = 180°$ (opposite axes), $E = +1$. The cosine interpolates smoothly between these extremes.

(The full derivation uses $\langle\Psi^-|(\hat{a}\cdot\vec{\sigma})\otimes(\hat{b}\cdot\vec{\sigma})|\Psi^-\rangle = -\hat{a}\cdot\hat{b}$; see homework.)

### The Three Pairs of Measurements

Our three axes and their pairwise angles:

- $Z$ and $Q$: separated by $45°$
- $X$ and $Q$: separated by $45°$
- $X$ and $Z$: separated by $90°$

Now compute the quantum predictions for each pair. For $\pm 1$ outcomes, the probability of getting opposite outcomes ("disagree") is

$$P(\text{disagree}) = \frac{1 - E}{2}.$$

**$Z$ and $Q$** ($\theta = 45°$):
$$E(Z, Q) = -\cos 45° = -\frac{1}{\sqrt{2}} \approx -0.707,$$
$$P(\text{disagree}) = \frac{1 + 1/\sqrt{2}}{2} \approx 85\%.$$

**$X$ and $Q$** ($\theta = 45°$):
$$E(X, Q) = -\frac{1}{\sqrt{2}} \approx -0.707,$$
$$P(\text{disagree}) \approx 85\%.$$

**$X$ and $Z$** ($\theta = 90°$):
$$E(X, Z) = -\cos 90° = 0,$$
$$P(\text{disagree}) = 50\%.$$

Collect these:

| Measurement pair | Angle | $E$ | $P(\text{disagree})$ |
|---|---|---|---|
| $Z$ and $Q$ | $45°$ | $-1/\sqrt{2}$ | $\approx 85\%$ |
| $X$ and $Q$ | $45°$ | $-1/\sqrt{2}$ | $\approx 85\%$ |
| $X$ and $Z$ | $90°$ | $0$ | $50\%$ |

These are the predictions we will test against the hidden variable model.

---

## Part 3: The Venn Diagram Argument

### The Lookup Table with Three Settings

Now each particle pair must carry predetermined values for **three** axes instead of two:

$$A_X(\lambda), \quad A_Z(\lambda), \quad A_Q(\lambda) \in \{+1, -1\}.$$

For the singlet's perfect anti-correlation along a shared axis, we must have

$$B_s(\lambda) = -A_s(\lambda), \qquad s \in \{X, Z, Q\}.$$

There are $2^3 = 8$ possible lookup tables, determined by Alice's triple $(A_X, A_Z, A_Q)$. Some probability distribution over these 8 tables must reproduce the quantum predictions.

### A crucial translation: "disagree" $\leftrightarrow$ equality of Alice's bits

Suppose Alice measures setting $s$ and Bob measures setting $t$. In the lookup-table model:

- Alice outputs $A_s$.
- Bob outputs $B_t = -A_t$.

They disagree (opposite outcomes) when

$$A_s \neq B_t = A_s \neq -A_t,$$

i.e.,

$$A_s = A_t.$$

**Wait — that's backwards from what you might expect.** Let's trace through a concrete example. Take lookup table 1, where $A_X = +1$ and $A_Q = +1$:

- Alice measures X, gets $A_X = +1$.
- Bob measures Q, gets $B_Q = -A_Q = -1$.
- Alice got $+1$, Bob got $-1$ — they **disagree**.
- And indeed $A_X = A_Q$ (both $+1$).

The reason: Bob's output is the *negative* of his hidden value, so Alice and Bob disagree precisely when Alice's hidden values for the two settings are *equal*.

So, for the singlet lookup-table model:

$$P(\text{disagree when measuring } s \text{ and } t) = P(A_s = A_t).$$

### What the Quantum Predictions Require

The three quantum disagreement rates become constraints on Alice's predetermined triple:

- From the $(X, Q)$ measurement pair:
  $$P(A_X = A_Q) \approx 85\%.$$

- From the $(Z, Q)$ measurement pair:
  $$P(A_Z = A_Q) \approx 85\%.$$

- From the $(X, Z)$ measurement pair:
  $$P(A_X = A_Z) = 50\%.$$

Now we ask:

**Is there any probability distribution over $(A_X, A_Z, A_Q) \in \{+1, -1\}^3$ that satisfies all three constraints?**

### The Set Theory Argument

Define two sets over the space of hidden variables:

- $\mathcal{A}$ = set of lookup tables where $A_X = A_Q$
- $\mathcal{B}$ = set of lookup tables where $A_Z = A_Q$

Quantum mechanics requires:

$$|\mathcal{A}| = P(A_X = A_Q) \approx 0.85, \qquad |\mathcal{B}| = P(A_Z = A_Q) \approx 0.85.$$

How large is the overlap $\mathcal{A} \cap \mathcal{B}$? By inclusion–exclusion:

$$|\mathcal{A} \cap \mathcal{B}| \geq |\mathcal{A}| + |\mathcal{B}| - 1 = 0.85 + 0.85 - 1 = 0.70.$$

So at least 70% of the lookup tables have **both** $A_X = A_Q$ **and** $A_Z = A_Q$.

But for any specific lookup table in $\mathcal{A} \cap \mathcal{B}$:

- $A_X = A_Q$
- $A_Z = A_Q$

Therefore $A_X = A_Z$. So:

$$P(A_X = A_Z) \geq 70\% \qquad \Longrightarrow \qquad P(A_X = A_Z) \geq 0.70.$$

But quantum mechanics predicts:

$$P(A_X = A_Z) = 0.50.$$

So we arrive at:

$$\boxed{0.50 \geq 0.70 \quad \text{— Contradiction.}}$$

### What Just Happened

This was nothing more than basic logic about three $\pm 1$ values:

- $X$ and $Q$ are opposite (between Alice and Bob) about 85% of the time.
- $Z$ and $Q$ are opposite about 85% of the time.
- In a singlet lookup-table model, "Alice and Bob are opposite on $(s, t)$" means Alice's hidden bits satisfy $A_s = A_t$.
- If $A_X = A_Q$ and $A_Z = A_Q$, then $A_X = A_Z$.
- Therefore $A_X$ and $A_Z$ must match at least 70% of the time.
- But quantum mechanics says they match only 50% of the time.

**No distribution over predetermined triples can satisfy all three constraints simultaneously.** The lookup table model fails.

```{admonition} Why Two Settings Weren't Enough
:class: note

With only two axes (X and Z), we built a working hidden variable model — four lookup tables, equal weights, done. There was no third axis to create an inconsistency.

The contradiction requires three axes because the hidden variable must simultaneously satisfy constraints between multiple *pairs* of settings, and the quantum correlations between those pairs are mutually incompatible.
```

---

## Part 4: The Core of Bell's Theorem

### The Problem Is Consistency Across Multiple Angles

The root cause is the shape of the quantum correlation function:

$$E = -\cos\theta.$$

This function says:

- Axes $45°$ apart are still highly anti-correlated: $E \approx -0.707$ (disagree $\approx 85\%$).
- Axes $90°$ apart are completely uncorrelated: $E = 0$ (disagree $50\%$).

A local hidden-variable model tries to explain everything using pre-existing $\pm 1$ answers for $X$, $Z$, and $Q$. But once you demand all three pairwise relations at once, simple logic forces a constraint.

One compact way to state the Venn argument is the inequality:

$$P(A_X = A_Z) \geq P(A_X = A_Q) + P(A_Z = A_Q) - 1.$$

Quantum requires the right-hand side to be $0.85 + 0.85 - 1 = 0.70$, but also requires $P(A_X = A_Z) = 0.50$. Those cannot both be true.

```{admonition} Bell's Theorem
:class: important

No theory that satisfies **locality** and **realism** (predetermined measurement outcomes) can reproduce all the predictions of quantum mechanics.

This is not a philosophical disagreement — it is **quantitative**. In a local hidden-variable model the $45°$–$45°$ predictions force $P(A_X = A_Z) \geq 70\%$, while quantum mechanics predicts $50\%$.
```

---

## Part 5: What Do We Give Up?

Last lecture we identified three assumptions that cannot all be true:

1. **Locality** — no instant action at a distance
2. **Realism** — properties exist before measurement (predetermined outcomes)
3. **Completeness of QM** — the quantum state is the full description

EPR showed the logical tension. Bell made it **experimental**: locality + realism implies quantitative constraints that differ from QM.

So which assumption fails?

**If we give up completeness** (EPR's choice): hidden variables exist, and Bell-type inequalities must hold. The experiment decides.

**If we give up realism** (Copenhagen-ish): there are no predetermined values. The lookup table doesn't exist — not because we can't find the right distribution, but because the table *itself* is not meaningful. Properties come into existence when measured, and the question "what would Bob have gotten if Alice had measured differently?" has no physical answer.

**If we give up locality** (e.g., Bohmian mechanics): predetermined values exist, but Alice's measurement influences Bob's system nonlocally. The no-signaling theorem ensures this cannot be used to communicate.

Bell turned this from a philosophical menu into an experimental question: **does the inequality hold, or is it violated?**

---

## Part 6: The Experiments

The experiments that tested Bell's theorem used **polarization-entangled photons** rather than electron spins — but the physics is identical. Any two-level quantum system (photon polarization, electron spin, superconducting qubit) obeys the same mathematics, and Bell's theorem applies to all of them.

### Freedman and Clauser (1972)

Stuart Freedman and John Clauser at UC Berkeley performed the first experimental test of a Bell inequality. They used pairs of entangled photons from a calcium atomic cascade — when a calcium atom de-excites through two successive transitions, it emits two photons whose polarizations are entangled.

Alice and Bob each measured the polarization of their photon along chosen axes. The results clearly violated the Bell inequality and agreed with the quantum prediction.

This was the first direct evidence that no local hidden variable model could explain the data. But the experiment had significant limitations — the measurement settings were fixed in advance, not changed during the flight of the photons.

### Aspect (1982)

Alain Aspect and collaborators in Paris performed a dramatically improved version. The crucial advance: they switched the measurement settings **while the photons were in flight**, using fast acousto-optic modulators that redirected each photon to one of two detectors with different polarization settings.

This addressed the **locality loophole** — the worry that the particles could somehow coordinate their answers based on advance knowledge of the settings. By choosing the settings after the photons were emitted and ensuring the choice events were space-like separated (no light signal could travel from one to the other in time), Aspect showed that the particles couldn't have known which measurement was coming.

The results again violated the Bell inequality, decisively.

### The Loophole-Free Experiments (2015)

Despite Aspect's work, two stubborn loopholes remained:

**The detection loophole:** Not all photons are detected (typical efficiency ~20%). If the missed photons would have obeyed the inequality, the detected subset could violate it by selection bias. Closing this requires detecting a sufficiently high fraction of all pairs.

**The locality loophole:** Aspect's switching was fast but not perfect — there were small time windows where communication was theoretically possible.

In 2015, three independent groups closed both loopholes simultaneously:

- **Delft** (Hensen et al.): used entangled electron spins in nitrogen-vacancy centers in diamond, 1.3 km apart. Detection efficiency was high enough to close the detection loophole, and the distance ensured space-like separation.

- **Vienna** (Giustina et al.): used entangled photons with superconducting detectors achieving >75% efficiency.

- **NIST Boulder** (Shalm et al.): similar approach to Vienna with independent methodology.

All three found clear Bell inequality violations with no loopholes. The result was definitive.

### The Nobel Prize (2022)

The 2022 Nobel Prize in Physics was awarded to **John Clauser**, **Alain Aspect**, and **Anton Zeilinger** "for experiments with entangled photons, establishing the violation of Bell inequalities and pioneering quantum information science."

The citation recognized not just the foundational tests, but the broader point: the experimental confirmation of Bell inequality violations established entanglement as a **physical resource**, not just a philosophical curiosity. This launched the field of quantum information — quantum computing, quantum cryptography, and quantum teleportation all depend on entanglement being real and exploitable.

---

## Part 7: What Bell Does and Does Not Say

Let's be precise about the conclusion.

**Bell's theorem says:** No theory that is both **local** and **realistic** (in the sense of predetermined measurement outcomes) can reproduce all the predictions of quantum mechanics. This is a mathematical theorem. The experiments confirm that nature agrees with quantum mechanics, not with local realism.

**Bell's theorem does NOT say:**

- *Faster-than-light signaling is possible.* It isn't. Bob's local statistics are always those of a maximally mixed state, $\rho_B = I/2$, regardless of what Alice does. The correlations appear only when results are compared using ordinary classical communication.

- *Which assumption to give up.* Bell rules out locality + realism together; interpretations differ on which to abandon.

- *That quantum mechanics is the final theory.* Any deeper theory must still reproduce Bell violations (i.e., it cannot restore local realism). But QM could still be incomplete in ways that don't restore local realism.

**The practical legacy:** Before Bell, entanglement was a philosophical puzzle. After Bell (and especially after the experiments), entanglement became a **resource** — something you can produce, distribute, and use. Quantum key distribution, quantum teleportation, and quantum computing all rely on the fact that entangled correlations are stronger than anything achievable with shared classical information. Bell's theorem is the reason we know this.

---

## Summary

1. **Two measurement settings** (X and Z): a simple lookup table with four entries and equal weights reproduces all quantum predictions perfectly. Einstein's hidden variable program works.

2. **Three measurement settings** (X, Z, Q at 45°): the quantum predictions become mutually incompatible with any local hidden-variable lookup-table model.

3. **The Venn diagram argument** is pure set theory: if $A_X = A_Q$ and $A_Z = A_Q$ are each true most of the time, then $A_X = A_Z$ must be true most of the time. Quantum correlations violate this.

4. **Bell's theorem (1964):** no local hidden-variable model can reproduce all quantum predictions. This is quantitative and testable.

5. **Experiments** confirm the violation: Freedman & Clauser (1972), Aspect (1982, fast switching), loophole-free tests (2015, Delft/Vienna/NIST). The 2022 Nobel Prize recognized this as foundational to quantum information science.

6. **What we give up:** locality + realism cannot both hold.

---

## Homework

### Problem 1: The Two-Setting Hidden Variable Model

Alice and Bob share the singlet state and each choose to measure $Z$ or $X$.

**(a)** Write out all four possible lookup tables $(a_Z, a_X, b_Z, b_X)$, using the constraint $b_s = -a_s$.

**(b)** Verify that equal weights $p = 1/4$ give $E(Z,Z) = -1$, $E(X,X) = -1$, and $E(Z,X) = 0$.

**(c)** Is the equal-weight distribution the *only* one that works? Find the most general distribution $(p_1, p_2, p_3, p_4)$ consistent with all three correlations.

---

### Problem 2: The Third Axis

The operator is $\sigma_Q = \frac{1}{\sqrt{2}}(\sigma_z + \sigma_x)$.

**(a)** Verify that $\sigma_Q^2 = I$.

**(b)** Find the eigenstates of $\sigma_Q$.

**(c)** Write $|0\rangle$ in the $\sigma_Q$ eigenbasis. Compute $P(\text{get } +1 \text{ measuring } Q)$.

---

### Problem 3: The Correlation Function

For the singlet state $|\Psi^-\rangle$ and measurement axes $\hat{a}$, $\hat{b}$, the correlation is

$$E(\hat{a}, \hat{b}) = -\hat{a}\cdot\hat{b} = -\cos\theta.$$

Verify this for:

**(a)** $\hat{a} = \hat{b} = \hat{z}$, by computing $\langle\Psi^-|(\sigma_z \otimes \sigma_z)|\Psi^-\rangle$.

**(b)** $\hat{a} = \hat{z}$, $\hat{b} = \hat{x}$, by computing $\langle\Psi^-|(\sigma_z \otimes \sigma_x)|\Psi^-\rangle$.

**(c)** $\hat{a} = \hat{z}$, $\hat{b} = \hat{q} = \frac{1}{\sqrt{2}}(\hat{z} + \hat{x})$, by computing $\langle\Psi^-|(\sigma_z \otimes \sigma_Q)|\Psi^-\rangle$.

---

### Problem 4: The Venn Diagram in Detail

There are 8 possible lookup tables for three axes, corresponding to Alice's triple $(A_X, A_Z, A_Q)$.

**(a)** List all 8 triples. For each one, determine whether $A_X = A_Z$, whether $A_X = A_Q$, and whether $A_Z = A_Q$.

**(b)** Try to find probabilities on the 8 triples satisfying:

- $P(A_X = A_Q) = 85\%$
- $P(A_Z = A_Q) = 85\%$
- $P(A_X = A_Z) = 50\%$

Show this is impossible.

---

### Problem 5: Why Three Settings?

**(a)** Show that with only two measurement settings (X and Z), you can always construct a hidden variable model for the singlet.

**(b)** Explain in your own words why adding a third setting (Q) creates a contradiction. What role does Q play?

**(c)** Suppose $Q$ were at $90°$ from both $X$ and $Z$ (i.e., $Q = Y$). Compute the predicted correlations. Does the Venn diagram argument still produce a contradiction? What is special about the $45°$ choice?

---

### Problem 6: A Useful Inequality (Triangle/Overlap Form)

For any three $\pm 1$ random variables $A_X, A_Z, A_Q$, prove:

$$P(A_X = A_Z) \geq P(A_X = A_Q) + P(A_Z = A_Q) - 1.$$

**(a)** Give a one-line set-theory proof using $\mathcal{A} = \{A_X = A_Q\}$ and $\mathcal{B} = \{A_Z = A_Q\}$.

**(b)** Substitute the quantum values $0.85$, $0.85$, $0.50$ and show the inequality is violated.

**(c)** Rewrite the inequality in terms of correlators $E(X,Z)$, $E(X,Q)$, $E(Z,Q)$.

---

### Problem 7: Closing the Loopholes

**(a)** **Locality loophole.** Explain why it's important that the measurement settings are chosen after emission and are space-like separated.

**(b)** **Detection loophole.** Sketch how a hidden variable could correlate outcomes with whether a detector clicks, producing an apparent violation on a biased subset.

**(c)** Why was it important to close both loopholes simultaneously?

---

## Looking Ahead

Bell's theorem, as we've presented it, used three settings and an ideal singlet. Real experiments aren't perfect: noise reduces correlations.

The standard tool is the **CHSH inequality** (Clauser, Horne, Shimony, Holt, 1969): a single number $S$ computed from four correlations, with $|S| \leq 2$ for any local hidden variable model and $|S| = 2\sqrt{2} \approx 2.83$ for the optimal quantum state. Next lecture, we'll derive CHSH and see why $2\sqrt{2}$ is the quantum limit.