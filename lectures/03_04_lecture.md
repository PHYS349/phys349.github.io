# Lecture 3.4 — Bell’s Theorem

## Where We Left Off

Last lecture we showed that three assumptions — **locality**, **realism**, and **completeness of QM** — are mutually inconsistent. The EPR argument (1935) assumed locality and realism and concluded that quantum mechanics is incomplete: there must be hidden variables carrying predetermined measurement outcomes.

We made this precise with the lookup table picture. If realism is true, each particle pair carries a table of predetermined $\pm 1$ answers. If locality is true, Alice’s choice of measurement setting can’t change Bob’s table entries.

The question we left open:

**Can any distribution over lookup tables reproduce the quantum predictions?**

Today we answer that question. The answer is no.

------------------------------------------------------------------------

## Part 1: Two Settings — The Hidden Variable Model Works

### The Setup

Alice and Bob share the singlet state

$$
|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle).
$$

Each can choose to measure along either $Z$ or $X$. The quantum predictions are:

**Same axis: perfect anti-correlation**

$$
E(Z,Z) = -1, \qquad E(X,X) = -1.
$$

Whenever they measure the same observable, they always get opposite outcomes.

**Different axes: no correlation**

$$
E(Z,X) = E(X,Z) = 0.
$$

When they measure different observables, all four outcome pairs occur with probability $1/4$.

### Building the Lookup Table

Let each pair carry a hidden variable $\lambda$ encoding predetermined outcomes:

$$
a_Z,\ a_X \in \{+1,-1\} \qquad \text{(Alice’s predetermined outcomes).}
$$

For the singlet, outcomes are perfectly anti-correlated along the same axis, so Bob’s values must satisfy

$$
b_Z = -a_Z, \qquad b_X = -a_X.
$$

That leaves two free bits: $a_Z$ and $a_X$. There are $2^2=4$ possible lookup tables:

| Table $\lambda$ | $a_Z$ | $a_X$ | $b_Z$ | $b_X$ |
|----------------:|:-----:|:-----:|:-----:|:-----:|
|               1 | $+1$  | $+1$  | $-1$  | $-1$  |
|               2 | $+1$  | $-1$  | $-1$  | $+1$  |
|               3 | $-1$  | $+1$  | $+1$  | $-1$  |
|               4 | $-1$  | $-1$  | $+1$  | $+1$  |

Assign each table equal probability: $p_1=p_2=p_3=p_4=1/4$.

### Checking the Predictions

**ZZ correlation.** Every table has $b_Z=-a_Z$, so $a_Z b_Z=-1$ always. Therefore $E(Z,Z)=-1$. ✓

**XX correlation.** Every table has $b_X=-a_X$, so $a_X b_X=-1$ always. Therefore $E(X,X)=-1$. ✓

**ZX correlation (Alice measures** $Z$, Bob measures $X$). Compute $a_Z b_X$:

| Table | $a_Z$ | $b_X$ | $a_Z b_X$ |
|------:|:-----:|:-----:|:---------:|
|     1 | $+1$  | $-1$  |   $-1$    |
|     2 | $+1$  | $+1$  |   $+1$    |
|     3 | $-1$  | $-1$  |   $+1$    |
|     4 | $-1$  | $+1$  |   $-1$    |

Average:

$$
E(Z,X) = \frac{1}{4}(-1+1+1-1)=0.
$$

✓

So with two settings, a local hidden-variable lookup-table model reproduces the singlet predictions exactly.

> **Einstein Wins (So Far).**\
> With two measurement settings, a simple lookup table model reproduces all quantum predictions for the singlet state. The correlations are no different from classical correlations predetermined at the source.

------------------------------------------------------------------------

## Part 2: The Third Axis

### A New Measurement Direction

The hidden-variable model worked because with two orthogonal axes ($Z$ and $X$ at $90^\circ$), there is enough freedom to assign predetermined values consistently. Now add a third axis.

Define $Q$ as the axis at $45^\circ$ between $Z$ and $X$ on the Bloch sphere. The corresponding operator is

$$
\sigma_Q = \frac{1}{\sqrt{2}}(\sigma_z+\sigma_x)
        = \frac{1}{\sqrt{2}}
          \begin{pmatrix}
          1 & 1 \\
          1 & -1
          \end{pmatrix}.
$$

Its eigenstates are

$$
|Q_+\rangle = \cos\frac{\pi}{8}\,|0\rangle + \sin\frac{\pi}{8}\,|1\rangle,
\qquad
|Q_-\rangle = -\sin\frac{\pi}{8}\,|0\rangle + \cos\frac{\pi}{8}\,|1\rangle,
$$

where $\pi/8 = 22.5^\circ$. We won’t need the explicit eigenstates — what matters are the angles between measurement axes.

### The Correlation Function for the Singlet

For the singlet state, quantum mechanics predicts the correlator

$$
E(\hat a,\hat b) = -\cos\theta,
$$

where $\theta$ is the angle between the measurement axes on the Bloch sphere.

This is plausible because the singlet is rotationally invariant: the correlation can depend only on the relative angle. When $\theta=0^\circ$ (same axis), perfect anti-correlation gives $E=-1$. When $\theta=180^\circ$ (opposite axes), $E=+1$. The cosine interpolates smoothly.

(One derivation uses $\langle\Psi^-|(\hat a\cdot\vec\sigma)\otimes(\hat b\cdot\vec\sigma)|\Psi^-\rangle = -\hat a\cdot\hat b$; see homework.)

### The Three Pairs of Measurements

Our three axes and their pairwise angles:

-   $Z$ and $Q$: separated by $45^\circ$
-   $X$ and $Q$: separated by $45^\circ$
-   $X$ and $Z$: separated by $90^\circ$

For $\pm 1$ outcomes, the probability of getting opposite outcomes (“disagree”) is

$$
P(\text{disagree}) = \frac{1 - E}{2}.
$$

Compute:

$Z$ and $Q$ ($\theta=45^\circ$):

$$
E(Z,Q) = -\cos 45^\circ = -\frac{1}{\sqrt{2}} \approx -0.707,
$$

$$
P(\text{disagree}) = \frac{1 + 1/\sqrt{2}}{2} \approx 0.8536.
$$

$X$ and $Q$ ($\theta=45^\circ$):

$$
E(X,Q) = -\frac{1}{\sqrt{2}} \approx -0.707,
\qquad
P(\text{disagree}) \approx 0.8536.
$$

$X$ and $Z$ ($\theta=90^\circ$):

$$
E(X,Z) = -\cos 90^\circ = 0,
\qquad
P(\text{disagree}) = \frac{1}{2}.
$$

Summary:

| Measurement pair |      Angle |           $E$ | $P(\text{disagree})$ |
|------------------|-----------:|--------------:|---------------------:|
| $Z$ and $Q$      | $45^\circ$ | $-1/\sqrt{2}$ |     $\approx 0.8536$ |
| $X$ and $Q$      | $45^\circ$ | $-1/\sqrt{2}$ |     $\approx 0.8536$ |
| $X$ and $Z$      | $90^\circ$ |           $0$ |                $0.5$ |

These are the predictions we will test against local hidden-variable lookup tables.

------------------------------------------------------------------------

## Part 3: The Venn Diagram Argument

### The Lookup Table with Three Settings

Now each pair must carry predetermined values for three axes:

$$
A_X(\lambda),\ A_Z(\lambda),\ A_Q(\lambda) \in \{+1,-1\}.
$$

Perfect anti-correlation along the same axis forces

$$
B_s(\lambda) = -A_s(\lambda),
\qquad s\in\{X,Z,Q\}.
$$

There are $2^3=8$ possible lookup tables, determined by Alice’s triple $(A_X,A_Z,A_Q)$.

### Key Translation: “Disagree” $\Longleftrightarrow$ Equality of Alice’s Bits

Suppose Alice measures setting $s$ and Bob measures setting $t$. In the lookup-table model:

-   Alice outputs $A_s$.
-   Bob outputs $B_t=-A_t$.

They **disagree** (opposite outcomes) when

$$
A_s \neq B_t
\iff
A_s \neq (-A_t)
\iff
A_s = A_t.
$$

So, in the singlet lookup-table model,

$$
P(\text{disagree when measuring } s \text{ and } t) = P(A_s = A_t).
$$

### What the Quantum Predictions Require

Translate the three disagreement rates into constraints on Alice’s predetermined triple:

From the $(X,Q)$ measurement pair:

$$
P(A_X = A_Q) \approx 0.8536.
$$

From the $(Z,Q)$ measurement pair:

$$
P(A_Z = A_Q) \approx 0.8536.
$$

From the $(X,Z)$ measurement pair:

$$
P(A_X = A_Z) = 0.5.
$$

Now ask:

**Is there any probability distribution over** $(A_X,A_Z,A_Q)\in\{+1,-1\}^3$ that satisfies all three constraints?

### The Set-Theory (Venn Diagram) Argument

Define two events over hidden variables:

-   $\mathcal A = \{A_X = A_Q\}$
-   $\mathcal B = \{A_Z = A_Q\}$

Quantum mechanics requires:

$$
P(\mathcal A) \approx 0.8536,
\qquad
P(\mathcal B) \approx 0.8536.
$$

By inclusion–exclusion,

$$
P(\mathcal A \cap \mathcal B)
\ge P(\mathcal A) + P(\mathcal B) - 1
\approx 0.8536 + 0.8536 - 1
\approx 0.7072.
$$

So at least about $70\%$ of the time, both equalities hold: $A_X=A_Q$ and $A_Z=A_Q$. But then, for any such $\lambda$,

$$
A_X = A_Q \ \text{and}\ A_Z = A_Q
\ \Longrightarrow\
A_X = A_Z.
$$

Therefore,

$$
P(A_X = A_Z) \ge P(\mathcal A\cap\mathcal B) \approx 0.7072.
$$

But quantum mechanics predicts

$$
P(A_X = A_Z) = 0.5.
$$

So we reach a contradiction:

$$
\boxed{0.5 \ \ge\ 0.7072 \quad \text{— contradiction.}}
$$

### What Just Happened?

This was nothing more than basic logic about three $\pm 1$ values:

-   $X$ and $Q$ are opposite (between Alice and Bob) about $85\%$ of the time.
-   $Z$ and $Q$ are opposite about $85\%$ of the time.
-   In a singlet lookup-table model, “Alice and Bob are opposite on $(s,t)$” means $A_s=A_t$.
-   If $A_X=A_Q$ and $A_Z=A_Q$, then $A_X=A_Z$.

So $A_X$ and $A_Z$ would have to match at least about $70\%$ of the time — but QM says only $50\%$.

No probability distribution over predetermined triples can satisfy all three constraints. The lookup-table model fails.

> **Why Two Settings Weren’t Enough.**\
> With only two axes ($X$ and $Z$), we built a working hidden-variable model — four lookup tables, equal weights, done.\
> The contradiction requires three axes because the hidden variable must satisfy multiple pairwise constraints simultaneously, and the quantum correlations for those pairs are mutually incompatible.

------------------------------------------------------------------------

## Part 4: The Core of Bell’s Theorem

### The Problem Is Consistency Across Multiple Angles

The root cause is the shape of the quantum correlation function

$$
E = -\cos\theta.
$$

This says:

-   Axes $45^\circ$ apart are still highly anti-correlated: $E\approx -0.707$ (disagree $\approx 0.854$).
-   Axes $90^\circ$ apart are uncorrelated: $E=0$ (disagree $0.5$).

A local hidden-variable model tries to explain everything using pre-existing $\pm 1$ answers for $X$, $Z$, and $Q$. But once you demand all three pairwise relations at once, logic forces a constraint.

A compact way to state the Venn argument is the inequality:

$$
P(A_X = A_Z) \ge P(A_X = A_Q) + P(A_Z = A_Q) - 1.
$$

Quantum requires the right-hand side to be about $0.7072$, but also requires $P(A_X = A_Z)=0.5$. Those cannot both be true.

> **Bell’s Theorem (Core Message).**\
> No theory that satisfies **locality** and **realism** (predetermined outcomes) can reproduce all the predictions of quantum mechanics.\
> This is quantitative: local realism forces $P(A_X=A_Z)\gtrsim 0.70$, while quantum mechanics predicts $0.50$ for these settings.

------------------------------------------------------------------------

## Part 5: What Do We Give Up?

We have three assumptions that cannot all be true:

-   **Locality** — no instantaneous influence at a distance\
-   **Realism** — outcomes are predetermined (properties exist before measurement)\
-   **Completeness of QM** — the quantum state is the full description

EPR highlighted the logical tension. Bell made it experimentally testable: **locality + realism** imply quantitative constraints (Bell-type inequalities) that disagree with quantum predictions.

So which assumption fails?

-   If we keep **locality** and **realism**, then Bell-type inequalities must hold — and experiments decide.
-   If we give up **realism** (Copenhagen-ish), there are no predetermined values. The lookup table does not exist as a physical object; counterfactual questions like “what would have happened if…” are not describing real properties.
-   If we give up **locality** (e.g., Bohmian mechanics), predetermined values exist but Alice’s measurement can influence Bob’s system nonlocally. The **no-signaling** theorem still prevents faster-than-light communication.

Bell turned this from a philosophical menu into an experimental question: **does the inequality hold, or is it violated?**

------------------------------------------------------------------------

## Part 6: A Real Bell Test

So far Bell's theorem has been a math argument. The next question is experimental:

**Can we actually measure these correlations in the lab, and do they obey Bell inequalities or violate them?**

Bell tests are often done with polarization-entangled photons rather than spins, but the mathematics is the same: each system has two outcomes, and each observer chooses among different measurement settings.

### The Main Experimental Challenge

To test Bell fairly, the experiment must rule out ordinary classical explanations. Two loopholes mattered most:

-   **Locality loophole:** Alice's and Bob's settings must be chosen and implemented quickly enough that no ordinary light-speed signal can coordinate the outcomes.
-   **Detection loophole:** the detected pairs must be representative of all emitted pairs. Otherwise a hidden-variable model could hide its failures among the undetected events.

If either loophole remains open, a clever local hidden-variable model might still mimic a Bell violation.

### Loophole-Free Bell Tests (2015)

In 2015, several groups performed Bell tests that closed both loopholes at once. One important example was the Delft experiment, which used entangled electron spins in nitrogen-vacancy centers.

The logic was:

1. Create entangled systems shared between two distant labs.
2. Choose measurement settings randomly and quickly.
3. Ensure the relevant events are space-like separated.
4. Detect enough events reliably to avoid unfair sampling.
5. Compare the measured correlations with the Bell inequality bound.

The result was clear: the Bell inequalities were violated, in agreement with quantum mechanics and in disagreement with local hidden-variable theories.

This is why Bell's theorem is not just philosophy. The inequality is a quantitative prediction of local realism, and nature violates it.

------------------------------------------------------------------------

## Part 7: What Bell Does and Does Not Say

Bell's theorem and the experiments together say something very precise:

> **Nature violates local realism.**\
> No theory with predetermined local outcomes can reproduce the observed quantum correlations.

That does **not** mean faster-than-light signaling is possible. Bob's local outcomes are still individually random; the nonclassical structure appears only in the correlations when Alice and Bob later compare data through ordinary classical communication.

It also does **not** tell us which assumption to give up. Different interpretations make different choices:

-   Give up **realism**: there are no predetermined values before measurement.
-   Give up **locality**: influences are nonlocal, though still not usable for signaling.
-   Reject a **local hidden-variable completion** of QM.

Bell's achievement was narrower and sharper than "quantum mechanics is weird." He turned EPR's philosophical tension into an experimentally testable inequality, and experiment sided with quantum mechanics.

------------------------------------------------------------------------
## Summary

-   With **two settings** ($X$ and $Z$), a simple lookup table with four entries can reproduce all singlet predictions.
-   With **three settings** ($X$, $Z$, $Q$ at $45^\circ$), the quantum predictions become mutually incompatible with any local hidden-variable lookup-table model.
-   The Venn diagram argument is set theory: two \~85% constraints force a $\ge 70\%$ constraint, but QM predicts $50\%$.
-   **Bell’s theorem (1964):** no local hidden-variable model reproduces all quantum predictions; this is quantitative and testable.
-   Experiments violate Bell inequalities, including loophole-free tests in 2015.
-   Conclusion: **locality + realism cannot both hold**.




## Homework

### Problem 1: The Two-Setting Hidden Variable Model

Alice and Bob share the singlet state, and each can choose to measure either $Z$ or $X$.

Assume a local hidden-variable model in which each pair carries predetermined values
$$
a_Z,\ a_X \in \{+1,-1\},
\qquad
b_Z = -a_Z,\qquad b_X = -a_X.
$$

**(a)** Write all four possible lookup tables $(a_Z,a_X,b_Z,b_X)$.

**(b)** Verify that equal weights $p_1=p_2=p_3=p_4=\tfrac14$ give
$$
E(Z,Z)=-1,\qquad E(X,X)=-1,\qquad E(Z,X)=0.
$$

**(c)** Is the equal-weight distribution the only one that works? Find the most general distribution $(p_1,p_2,p_3,p_4)$ consistent with these three correlations.

------------------------------------------------------------------------

### Problem 2: The Third Axis and the Quantum Predictions

Define the third measurement axis
$$
\sigma_Q=\frac{1}{\sqrt{2}}(\sigma_z+\sigma_x).
$$

**(a)** Verify that $\sigma_Q^2=I$.

**(b)** For the singlet state, quantum mechanics predicts
$$
E(\hat a,\hat b)=-\cos\theta,
$$
where $\theta$ is the angle between the measurement axes. Use this to compute
$$
E(Z,Q),\qquad E(X,Q),\qquad E(X,Z).
$$

**(c)** For $\pm 1$ outcomes, show that
$$
P(\text{disagree})=\frac{1-E}{2}.
$$

**(d)** Hence compute
$$
P(\text{disagree on } ZQ),\qquad
P(\text{disagree on } XQ),\qquad
P(\text{disagree on } XZ).
$$

------------------------------------------------------------------------

### Problem 3: Verifying the Correlation Function in Specific Cases

For the singlet
$$
|\Psi^-\rangle=\frac{1}{\sqrt{2}}(|01\rangle-|10\rangle),
$$
verify the correlation formula directly in the following cases:

**(a)** $\hat a=\hat b=\hat z$ by computing
$$
\langle \Psi^-|(\sigma_z\otimes\sigma_z)|\Psi^-\rangle.
$$

**(b)** $\hat a=\hat z$, $\hat b=\hat x$ by computing
$$
\langle \Psi^-|(\sigma_z\otimes\sigma_x)|\Psi^-\rangle.
$$

**(c)** $\hat a=\hat z$, $\hat b=\hat q=\frac{1}{\sqrt{2}}(\hat z+\hat x)$ by computing
$$
\langle \Psi^-|(\sigma_z\otimes\sigma_Q)|\Psi^-\rangle.
$$

------------------------------------------------------------------------
<!-- 
### Problem 4: The Key Inequality

For any three $\pm 1$ random variables $A_X,A_Z,A_Q$, prove the inequality
$$
P(A_X=A_Z)\ge P(A_X=A_Q)+P(A_Z=A_Q)-1.
$$

**(a)** Give a one-line set-theory proof using the events
$$
\mathcal A=\{A_X=A_Q\},\qquad \mathcal B=\{A_Z=A_Q\}.
$$

**(b)** In the singlet lookup-table model, explain why
$$
P(\text{disagree on } s,t)=P(A_s=A_t).
$$

**(c)** Substitute the quantum values from Problem 2 and show that the inequality is violated.

**(d)** State clearly what this violation means for local hidden-variable theories. -->

------------------------------------------------------------------------

### Problem 4: Why Three Settings Matter

**(a)** Explain why no contradiction appears when only two settings ($X$ and $Z$) are used.

**(b)** Explain in your own words why adding a third setting $Q$ creates a contradiction.

<!-- **(c)** Suppose $Q$ were instead at $90^\circ$ from both $X$ and $Z$ (that is, $Q=Y$). Compute the predicted correlations. Does the same Venn-diagram argument still produce a contradiction?

**(d)** What is special about choosing $Q$ at $45^\circ$? -->

<!-- ------------------------------------------------------------------------

### Problem 6: Bell’s Theorem and Experiment

**(a)** State the core message of Bell’s theorem in your own words.

**(b)** Bell tests must close both the locality loophole and the detection loophole. Explain what each loophole is. 

**(c)** Why is it important to close both loopholes simultaneously?

**(d)** Bell inequality violations do not allow faster-than-light signaling. Explain why not.

------------------------------------------------------------------------

 -->
