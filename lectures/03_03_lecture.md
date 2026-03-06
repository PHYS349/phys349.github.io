# Lecture 3.3 — Two-Qubit Measurement, Spooky Action, and EPR

## Where We Left Off

Last lecture we learned to identify entanglement (the determinant test), met the four Bell states, and reviewed the measurement formalism for a single qubit — projectors, probabilities, post-measurement states.

Today we extend measurement to **two qubits**: what happens when you measure just one qubit of an entangled pair? The answer leads to Einstein's deepest objection to quantum mechanics, the EPR argument, and the question that Bell eventually settled.

------------------------------------------------------------------------

## Part 1: Operators on Two Qubits

### The Tensor Product of Operators

We've built two-qubit states using the tensor product of vectors. We can do the same thing with operators.

If $A$ is an operator on qubit A and $B$ is an operator on qubit B, then $A \otimes B$ acts on the joint state in the natural way:

$$(A \otimes B)\big(|\psi\rangle \otimes |\phi\rangle\big) = A|\psi\rangle \otimes B|\phi\rangle$$

Each operator acts on its own qubit. For example:

$$(\sigma_z \otimes \sigma_x)\big(|0\rangle \otimes |+\rangle\big) = \sigma_z|0\rangle \otimes \sigma_x|+\rangle = (+1)|0\rangle \otimes (+1)|+\rangle = |0\rangle \otimes |+\rangle$$

The key special case is when we want to act on **one qubit and leave the other alone**. We use the identity operator $I$ on the qubit we're not touching:

$$A \otimes I \quad \text{acts on qubit A, leaves B alone}$$
$$I \otimes B \quad \text{acts on qubit B, leaves A alone}$$

------------------------------------------------------------------------

### Partial Measurement

To measure qubit A in the Z-basis and leave qubit B alone, we use the projector:

$$|0\rangle\langle 0| \otimes I$$

This projects qubit A onto $|0\rangle$ while doing nothing to qubit B.

The full measurement recipe is the same as before, just with tensor product projectors:

1.  **Projector for outcome:** $|m\rangle\langle m| \otimes I$ (measure A, leave B)
2.  **Probability:** $p_m = \langle\Psi|\big(|m\rangle\langle m| \otimes I\big)|\Psi\rangle$
3.  **Post-measurement state:** $\displaystyle|\Psi_{\text{after}}\rangle = \frac{\big(|m\rangle\langle m| \otimes I\big)|\Psi\rangle}{\sqrt{p_m}}$

------------------------------------------------------------------------

### Example: Measuring a Product State

Let's start with the boring case to establish a baseline. Alice and Bob share the product state $|+\rangle \otimes |0\rangle$. Alice measures Z.

$$|+\rangle \otimes |0\rangle = \frac{1}{\sqrt{2}}\big(|0\rangle + |1\rangle\big) \otimes |0\rangle = \frac{1}{\sqrt{2}}\big(|00\rangle + |10\rangle\big)$$

**Alice gets** $|0\rangle$ (outcome $+1$):

$$\big(|0\rangle\langle 0| \otimes I\big)\frac{1}{\sqrt{2}}\big(|00\rangle + |10\rangle\big) = \frac{1}{\sqrt{2}}|00\rangle$$

After normalizing: $|0\rangle \otimes |0\rangle$. Bob's qubit is $|0\rangle$.

**Alice gets** $|1\rangle$ (outcome $-1$):

$$\big(|1\rangle\langle 1| \otimes I\big)\frac{1}{\sqrt{2}}\big(|00\rangle + |10\rangle\big) = \frac{1}{\sqrt{2}}|10\rangle$$

After normalizing: $|1\rangle \otimes |0\rangle$. Bob's qubit is $|0\rangle$.

**Bob's state is** $|0\rangle$ regardless of Alice's outcome. Alice's measurement tells her something about her own qubit, but it has no effect on Bob. This is what "independent" means — no correlations, no surprises.

------------------------------------------------------------------------

## Part 2: Partial Measurement on Entangled States

Now the interesting case. Alice and Bob share $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$. They take their qubits and separate — maybe to opposite sides of the room, maybe to opposite sides of the planet.

Alice measures her qubit in the Z-basis.

**Alice gets** $|0\rangle$ (outcome $+1$):

$$\big(|0\rangle\langle 0| \otimes I\big)\frac{1}{\sqrt{2}}\big(|00\rangle + |11\rangle\big) = \frac{1}{\sqrt{2}}|00\rangle$$

After normalizing: $|0\rangle \otimes |0\rangle$. Bob's qubit is **definitely** $|0\rangle$.

**Alice gets** $|1\rangle$ (outcome $-1$):

$$\big(|1\rangle\langle 1| \otimes I\big)\frac{1}{\sqrt{2}}\big(|00\rangle + |11\rangle\big) = \frac{1}{\sqrt{2}}|11\rangle$$

After normalizing: $|1\rangle \otimes |1\rangle$. Bob's qubit is **definitely** $|1\rangle$.

Before Alice measured, Bob's qubit had no definite Z-value — $|\Phi^+\rangle$ is entangled, not a mixture of $|00\rangle$ and $|11\rangle$. After Alice's measurement, Bob's qubit is in a definite state. The entanglement has been broken, and the two qubits are now in a product state.

This is what Einstein called **"spooky action at a distance."**

------------------------------------------------------------------------

### No-Signaling

This sounds like Alice is sending information to Bob's qubit instantaneously. But she isn't.

Think about what Bob actually sees. Before comparing notes with Alice, he just has his own qubit. Half the time Alice got $|0\rangle$ (and Bob's qubit is $|0\rangle$) and half the time she got $|1\rangle$ (and Bob's qubit is $|1\rangle$). So Bob measures Z and gets:

$$p(+1) = \frac{1}{2}, \qquad p(-1) = \frac{1}{2}$$

Just 50/50. Now suppose Alice had measured X instead of Z. A similar calculation (see the homework) shows Bob's statistics are *still* 50/50 in any basis he chooses. And if Alice does nothing at all — same thing: 50/50.

```{admonition} No-Signaling Theorem :class: important

Bob's local measurement statistics are identical regardless of: - whether Alice measures or not - which basis Alice measures in - what outcome Alice gets

The correlations between Alice and Bob are real, but they only become visible when Alice and Bob **compare their results** through a classical communication channel. Entanglement cannot be used for faster-than-light communication.

```         

---

## Part 3: Alice Chooses Her Basis

Here is where it gets truly strange. Let's switch to the singlet state $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$, which is anti-correlated in every basis.

### Alice Measures Z

**Alice gets $|0\rangle$ (outcome $+1$):**

$$\big(|0\rangle\langle 0| \otimes I\big)\frac{1}{\sqrt{2}}\big(|01\rangle - |10\rangle\big) = \frac{1}{\sqrt{2}}|01\rangle$$

After normalizing: $|0\rangle \otimes |1\rangle$. Bob's qubit is $|1\rangle$ — a Z-eigenstate.

### Alice Measures X Instead

Write $|\Psi^-\rangle$ in the X-basis. Using $|0\rangle = \frac{1}{\sqrt{2}}(|+\rangle + |-\rangle)$ and $|1\rangle = \frac{1}{\sqrt{2}}(|+\rangle - |-\rangle)$:

$$|\Psi^-\rangle = \frac{1}{\sqrt{2}}\big(|01\rangle - |10\rangle\big) = -\frac{1}{\sqrt{2}}\big(|{+}{-}\rangle - |{-}{+}\rangle\big)$$

(We'll verify this identity in the homework. The minus sign out front is a global phase.)

**Alice gets $|+\rangle$ (outcome $+1$):**

$$\big(|+\rangle\langle+| \otimes I\big)|\Psi^-\rangle = -\frac{1}{\sqrt{2}}|+\rangle \otimes |-\rangle$$

After normalizing: $|+\rangle \otimes |-\rangle$. Bob's qubit is $|-\rangle$ — an X-eigenstate.

### What Just Happened?

Alice's **choice of measurement axis** determines what **type of state** Bob ends up in:

| Alice measures | Alice gets $+1$ | Bob's state |
|---|---|---|
| Z | $\|0\rangle$ | $\|1\rangle$ (Z-eigenstate) |
| X | $\|+\rangle$ | $\|-\rangle$ (X-eigenstate) |

If Alice measures Z, Bob's qubit becomes a Z-eigenstate. If Alice measures X, it becomes an X-eigenstate. These are *different physical states* — a Z-eigenstate is maximally uncertain in X (we proved this last lecture), while an X-eigenstate is maximally uncertain in Z.

And yet Bob can't tell the difference. In either case, whatever Bob measures gives 50/50. The difference only shows up in the correlations: if they both measure the same basis, they always get opposite results.

---

## Part 4: The Correlation Table

Let's map out the correlations systematically. For each Bell state, both Alice and Bob measure in the same basis (ZZ, XX, or YY). The **correlation** is $+1$ if their outcomes always agree, $-1$ if they always disagree.

For $|\Phi^+\rangle$ in the ZZ basis, we already did this: outcomes are $00$ or $11$ (always same), so correlation $= +1$.

For $|\Phi^+\rangle$ in the XX basis, we need to expand in the X-eigenbasis:

$$|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}} = \frac{|{+}{+}\rangle + |{-}{-}\rangle}{\sqrt{2}}$$

Outcomes are $++$ or $--$ (always same), so correlation $= +1$.

Including YY measurements (derivation is similar; see the homework), the full picture is:

| Bell state | ZZ | XX | YY |
|---|---|---|---|
| $\|\Phi^+\rangle$ | $+1$ (same) | $+1$ (same) | $-1$ (opposite) |
| $\|\Phi^-\rangle$ | $+1$ (same) | $-1$ (opposite) | $+1$ (same) |
| $\|\Psi^+\rangle$ | $-1$ (opposite) | $+1$ (same) | $+1$ (same) |
| $\|\Psi^-\rangle$ | $-1$ (opposite) | $-1$ (opposite) | $-1$ (opposite) |

### Reading the Table

Every column has two $+1$'s and two $-1$'s — **no single basis can distinguish all four Bell states**. But any *two* bases together can:

- **ZZ** tells you $\Phi$ vs $\Psi$: same-outcome (ZZ $= +1$) means a $\Phi$ state, opposite-outcome (ZZ $= -1$) means a $\Psi$ state.
- **XX** then tells you $+$ vs $-$: same-outcome (XX $= +1$) means the $+$ variant, opposite-outcome means $-$.

Two correlations, four states, complete identification. The computational basis alone (which is ZZ) can't do it — you need the freedom to choose your measurement basis.

### The Singlet Is Special

$|\Psi^-\rangle$ is the odd one out: anti-correlated in **every** basis. This is a consequence of it being the spin-0 (total angular momentum zero) state of two spin-$\frac{1}{2}$ particles — there's no preferred direction, so the anti-correlation is rotationally invariant.

The other three Bell states each have one basis where the correlation flips sign. $|\Psi^-\rangle$ doesn't — it's anti-correlated uniformly. This makes it the natural state for Bell's theorem, where we'll need perfect anti-correlation along *any* shared measurement axis.

```{admonition} The Key Lesson
:class: important

**The choice of measurement basis determines what information you can access.** A single measurement basis reveals only partial information about an entangled state. The full structure of quantum correlations — and the difference between the four Bell states — only emerges when you can choose among multiple bases.

This freedom to choose measurement bases is the seed of EPR, Bell's theorem, and quantum key distribution.
```

------------------------------------------------------------------------

## Part 5: The EPR Argument

### A Classical Explanation?

Remember the correlated marbles from Lecture 3.1? Same-color pairs: 00 or 11, each 50%. We explained that by saying someone prepared the pair — a hidden variable determined the outcome in advance, and opening a box just revealed a pre-existing fact.

Einstein looked at $|\Phi^+\rangle$ and proposed the same explanation. This is exactly the correlated marbles — the hidden variable is the preparation procedure, and measurement just reveals what was already there. Einstein's claim is that quantum entanglement is no different from classical correlation:

**Einstein's hypothesis:** The qubits aren't "really" in a superposition. They were "secretly" in $|00\rangle$ or $|11\rangle$ all along. Quantum mechanics just doesn't tell us which. The theory is *incomplete* — there are **hidden variables** that determine the outcomes before measurement.

### Three Assumptions

The EPR argument rests on three assumptions. Let's state them clearly, because the whole debate — from 1935 to today — is about which one to give up.

```{admonition} The Three Assumptions :class: important

1.  **Locality.** Alice's measurement cannot instantly affect Bob's qubit. If they are far apart, nothing Alice does can change what Bob has.

2.  **Realism.** Physical properties have definite values before measurement. Measurement reveals pre-existing facts; it doesn't create them.

3.  **Completeness of QM.** The quantum state $|\Psi\rangle$ provides a complete description of the physical system. There is nothing more to know.

```         

EPR showed that **these three cannot all be true at the same time.**

### The EPR Logic

Einstein, Podolsky, and Rosen made their argument in 1935, using the singlet state $|\Psi^-\rangle$ which is anti-correlated in every basis.

**Step 1 — Locality.** Alice and Bob are far apart. Alice's measurement cannot instantly affect Bob's qubit. Whatever Bob "has" must have been determined before Alice measured.

**Step 2 — Reality from prediction.** Alice measures Z and gets $+1$. She can predict with certainty that Bob will get $-1$ if he measures Z. Since Alice's measurement didn't disturb Bob (locality), Bob's Z-value must have been $-1$ all along. EPR called this an **"element of physical reality"**: if you can predict a measurement outcome with certainty without disturbing the system, there must be a real, pre-existing property corresponding to that outcome.

**Step 3 — Apply to another basis.** Alice could instead have measured X. She would then predict Bob's X-outcome with certainty. By the same argument, Bob's X-value was also predetermined.

**Step 4 — But quantum mechanics can't describe this.** We showed last lecture that $\sigma_z$ and $\sigma_x$ don't commute — no quantum state assigns definite values to both simultaneously. The uncertainty principle forbids it. So if Bob has pre-existing values for *both* Z and X, quantum mechanics is missing information that exists in the physical world.

**EPR's conclusion:** Assumptions (1) and (2) are obviously true. Therefore assumption (3) is false. Quantum mechanics is **incomplete.**

---

### Walking Around the Triangle

EPR assumed locality and realism, and concluded that QM is incomplete. But that's only one way to resolve the contradiction. Any two of the three assumptions imply the negation of the third:

**Locality + Realism → QM is incomplete** (EPR's conclusion)
Hidden variables carry the missing information. QM is a useful but incomplete statistical theory, like thermodynamics before we knew about atoms.

**Locality + Completeness → Realism is wrong** (Copenhagen interpretation)
There are no pre-existing values. Before Alice measures, Bob's qubit has no definite Z-value *and* no definite X-value. Measurement doesn't reveal a fact — it **creates** one. Properties come into existence only when observed.

**Realism + Completeness → Locality is wrong** (Bohmian mechanics)
Pre-existing values exist, and QM describes them fully — but Alice's measurement *does* instantly affect Bob's qubit, through a nonlocal "pilot wave." The universe has faster-than-light influences, but they're hidden: they can never be used to send a signal (the no-signaling theorem ensures this).

```{admonition} iClicker
:class: tip

Which assumption would you be most willing to give up?

**(A)** Locality — allow instant influences at a distance

**(B)** Realism — properties don't exist until measured

**(C)** Completeness — QM is missing something (hidden variables)

**(D)** I'm not sure yet
```

There is no "right answer" here — all three options have serious defenders. But in 1935, this was a purely philosophical debate. The three positions make the same predictions for every conceivable experiment. There seemed to be no way to tell them apart.

------------------------------------------------------------------------

## Part 6: From Philosophy to Physics

### Twenty-Nine Years of Philosophy

After 1935, the EPR debate became a conversation about *interpretation*, not experiment. Bohr and Einstein argued. Physicists picked sides. But no measurement could settle the question, because all three positions agreed on the predictions of quantum mechanics.

Einstein's position — that hidden variables exist — seemed perfectly reasonable. After all, we already saw that the correlated marbles from Lecture 3.1 are *exactly* this: a hidden variable (the preparation) explains the correlations, and measurement just reveals pre-existing facts. Why shouldn't quantum correlations work the same way?

### Bell's Question (1964)

In 1964, the Irish physicist John Bell asked the decisive question: **is this testable?**

Bell's insight was that the hidden variable hypothesis isn't just a philosophical position — it's a *theory* that makes quantitative predictions. And those predictions might differ from quantum mechanics.

Here's the key idea. If Einstein is right — if locality and realism both hold — then each pair of qubits must carry **predetermined answers** to every possible measurement. Before anyone measures anything, the outcomes are already decided.

### The Lookup Table

Let's make this concrete. Suppose Alice and Bob share entangled pairs, and each can choose to measure Z or X. If realism is true, then *before* any measurement occurs, each qubit already "knows" what it would give for each setting.

We can imagine each particle pair carrying a **lookup table**:

|                     | Alice measures Z | Alice measures X |
|---------------------|------------------|------------------|
| **Alice's outcome** | $a_Z$            | $a_X$            |
| **Bob's outcome**   | $b_Z$            | $b_X$            |

Here $a_Z, a_X, b_Z, b_X \in \{+1, -1\}$ are the predetermined values. Different pairs might carry different tables — there's some probability distribution over all possible tables — but the values exist in advance of measurement. Neither Alice nor Bob know the table; they each see only the entry corresponding to their chosen setting.

**Locality** means the table entries are *fixed* before the measurement begins. Alice's choice of setting (Z or X) determines which column *she* reads — but it cannot change Bob's column. The outcomes are determined by the shared table and the individual setting choices, nothing else.

This is Einstein's picture made precise: a hidden variable $\lambda$ (the lookup table) is distributed to both particles at the source. Each particle carries its half. When measured, it reports the pre-assigned answer.

\`\`\`{admonition} The Hidden Variable Model :class: note

In a local hidden variable theory:

1.  At the source, each pair receives a **lookup table** $\lambda$ assigning outcomes $\pm 1$ to every possible measurement setting.
2.  Alice's outcome depends only on her setting and $\lambda$. Bob's depends only on his setting and $\lambda$.
3.  Alice's setting choice cannot influence Bob's outcome, and vice versa.

The only question is: **can any distribution over lookup tables reproduce the correlations predicted by quantum mechanics?** \`\`\`

### The Question for Next Lecture

The singlet state $|\Psi^-\rangle$ has perfect anti-correlation: whenever Alice and Bob measure the same axis, they get opposite results. A lookup table can handle this — just make sure $b_Z = -a_Z$ and $b_X = -a_X$ for every table.

But quantum mechanics also predicts specific correlations when Alice and Bob measure *different* axes — say Alice measures Z while Bob measures X, or Alice measures along some axis at 45° to Z. The correlation depends smoothly on the angle between their axes: $E = -\cos\theta$.

The question is: can the lookup tables — constrained by locality and realism — reproduce this smooth angular dependence?

Bell proved the answer is **no.** The lookup table model makes predictions that are quantitatively different from quantum mechanics, and the difference is measurable. Next lecture, we'll see exactly why, and what the experiments found.

------------------------------------------------------------------------

## Summary

1.  **Tensor product operators** $(A \otimes B)$ act on each qubit independently: $(A \otimes B)(|\psi\rangle \otimes |\phi\rangle) = A|\psi\rangle \otimes B|\phi\rangle$. To measure one qubit and leave the other alone, use $|m\rangle\langle m| \otimes I$.

2.  **Partial measurement on entangled states** collapses both qubits. Measuring qubit A of $|\Phi^+\rangle$ and getting $|0\rangle$ puts Bob's qubit in $|0\rangle$ — instantly, regardless of distance.

3.  **No-signaling:** Bob can't tell whether Alice measured, which basis she chose, or what outcome she got. The correlations only appear when they compare results classically. Entanglement cannot transmit information.

4.  **Alice's basis choice matters.** On the singlet: measuring Z collapses Bob to a Z-eigenstate; measuring X collapses Bob to an X-eigenstate. Different physical states — but locally indistinguishable for Bob.

5.  **The correlation table** for Bell states in ZZ/XX/YY reveals that no single basis distinguishes all four. The singlet $|\Psi^-\rangle$ is special: anti-correlated in every basis (rotational invariance of spin-0).

6.  **The EPR argument** shows three assumptions are mutually inconsistent: *locality*, *realism*, and *completeness of QM*. Any two imply the negation of the third. This gives rise to different interpretations (EPR → hidden variables, Copenhagen → no pre-existing values, Bohmian → nonlocality).

7.  **Bell's question (1964):** Is the EPR debate testable? If realism and locality hold, each particle pair carries a **lookup table** of predetermined outcomes. Next lecture: can these lookup tables reproduce quantum correlations? Bell proved they cannot.

## Homework

------------------------------------------------------------------------

### Problem 1: Partial Measurement of a Product State

Alice and Bob share the product state $$
|+\rangle \otimes |0\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |10\rangle).
$$

Alice measures her qubit in the Z-basis.

**(a)** Compute the probability that Alice gets $|0\rangle$.

**(b)** Compute the post-measurement state if Alice gets $|0\rangle$.

**(c)** Compute the probability and post-measurement state if Alice gets $|1\rangle$.

**(d)** What is Bob's state in each case? Explain why this is not "spooky action at a distance."

------------------------------------------------------------------------

### Problem 2: Partial Measurement of an Entangled State

Alice and Bob share $$
|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle).
$$

Alice measures her qubit in the Z-basis.

**(a)** Use the projector $|0\rangle\langle 0| \otimes I$ to compute the probability and post-measurement state for Alice getting $|0\rangle$.

**(b)** Repeat for Alice getting $|1\rangle$.

**(c)** What is Bob's state after each outcome?

**(d)** In what sense has the entanglement been broken by the measurement?

------------------------------------------------------------------------

### Problem 3: No-Signaling

Alice and Bob share $|\Phi^+\rangle$. Bob measures Z on his qubit.

**(a)** Suppose Alice measures Z first. What are Bob's Z-measurement probabilities?

**(b)** Suppose Alice measures X first. What are Bob's Z-measurement probabilities now?

**(c)** Suppose Alice does nothing. Compute Bob's Z-measurement probabilities directly from the state.

**(d)** Explain how these results support the no-signaling theorem.

------------------------------------------------------------------------

### Problem 4: Alice Chooses Z or X

Alice and Bob share $|\Psi^-\rangle$.

**(a)** If Alice measures Z and gets $+1$, what state is Bob's qubit in?

**(b)** If Alice measures X and gets $+1$, what state is Bob's qubit in?

**(c)** In which sense are these two Bob states physically different?

**(d)** Why can't Bob tell, from local measurements alone, which basis Alice chose?

------------------------------------------------------------------------

### Problem 5: Bell-State Correlation Table

For each Bell state, both Alice and Bob measure in the same basis.

**(a)** Verify the ZZ correlation for $|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle)$.

**(b)** Verify the XX correlation for $|\Phi^+\rangle$.

**(c)** Use the lecture's correlation table to explain why one basis alone cannot distinguish all four Bell states.

**(d)** Show how ZZ together with XX can distinguish all four Bell states.

------------------------------------------------------------------------

### Problem 6: The EPR Argument

Alice and Bob share the singlet state and are far apart.

**(a)** If Alice measures Z and gets $+1$, what can she predict about Bob's Z outcome?

**(b)** If Alice instead measures X and gets $+1$, what can she predict about Bob's X outcome?

**(c)** State EPR's criterion for an "element of physical reality."

**(d)** Use locality plus this criterion to explain why EPR concludes Bob must have both a definite Z-value and a definite X-value.

**(e)** Why does this lead EPR to say that quantum mechanics is incomplete?