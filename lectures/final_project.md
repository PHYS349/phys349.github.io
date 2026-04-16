---
kernelspec:
  name: python3
  display_name: Python 3
---

# Final Project — Quantum Computing Meets Real Hardware

## Overview

The final project has two phases. In **Phase 1**, everyone completes the same individual warm-up: prepare and measure Bell states on a real IBM quantum processor. In **Phase 2**, you'll join a team of 2–3, pick one of five projects from the menu below, and present your results to the class.

The central theme is **theory vs. reality**. Every algorithm and protocol we've studied this semester assumed perfect qubits and noiseless gates. Real hardware has gate errors, decoherence, crosstalk, and measurement noise. Your job is to run something we've learned on a real quantum computer, measure how reality deviates from theory, and explain *why*.

**Total: 200 points** (equivalent to two homework assignments)

| Component | Points | Due |
|:----------|:------:|:----|
| Phase 1: Individual IBM warm-up | 50 | Wednesday, April 22 |
| Phase 2: Team project notebook | 100 | **Friday, April 24** |
| Phase 2: Team presentation | 50 | Tue Apr 28 / Thu Apr 30 (in class) |

------------------------------------------------------------------------

## IBM Quantum Account Setup

Before starting Phase 1, create a free IBM Quantum account:

1. Go to [quantum.ibm.com](https://quantum.ibm.com) and click **Sign Up**.
2. Create an account with your email (no credit card needed).
3. The **Open Plan** gives you **10 minutes of free QPU time per month** — more than enough for this project.
4. Save your API token (found under Account Settings). You'll need it to submit jobs from Qiskit:

```python
from qiskit_ibm_runtime import QiskitRuntimeService
QiskitRuntimeService.save_account(channel="ibm_quantum", token="YOUR_TOKEN_HERE")
```

```{admonition} Important Practical Notes
:class: warning
- **Develop on the simulator first.** Use `AerSimulator` for debugging and development. Only send your final experiments to real hardware.
- **Queue times vary.** Jobs may take minutes to hours depending on demand. Do not leave everything to the last night.
- **Each circuit execution uses a few seconds of QPU time.** With 10 minutes/month, you can run hundreds of small experiments.
```

------------------------------------------------------------------------

## Phase 1: Bell States on Real Hardware (Individual, 50 pts)

[📥 Download Phase 1 Starter Notebook (.ipynb)](../homework/download_final_phase1.html)

### Goal

Prepare all four Bell states on a real IBM quantum processor, measure their correlations, and quantify how much the real results deviate from the ideal predictions.

### 1. Prepare and measure all four Bell states (20 pts)

Build circuits for each Bell state:

$$|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle), \quad |\Phi^-\rangle = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle)$$

$$|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle), \quad |\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$$

For each Bell state, run 4096 shots on the `AerSimulator` (ideal) and on a real IBM backend. Plot the measurement histograms side by side (ideal vs. real) for all four states.

### 2. Measure correlations in two bases (15 pts)

For each Bell state, measure the two-qubit correlations $\langle ZZ \rangle$ and $\langle XX \rangle$:

- **Z-basis:** measure directly. Compute $\langle ZZ \rangle = P(00) + P(11) - P(01) - P(10)$.
- **X-basis:** apply H to both qubits before measuring. Compute $\langle XX \rangle$ the same way.

Make a table comparing ideal vs. measured values for all four Bell states in both bases.

### 3. Analysis (15 pts)

In a markdown cell (4–6 sentences), answer:
- How close are your measured correlations to the ideal values?
- What is the dominant source of error? (Hint: look up the backend's gate error rates and $T_1$/$T_2$ times using the IBM dashboard.)
- You prepared 4 circuits that differ by at most one or two single-qubit gates. Did all four Bell states have the same fidelity, or did some perform better? Why might that be?

```{admonition} Submission
:class: note
Submit your Jupyter notebook (`.ipynb`) with all code, plots, and written analysis. Include screenshots or printouts of your IBM job IDs so we can verify you ran on real hardware.
```

------------------------------------------------------------------------

## Phase 2: Team Projects (Teams of 2–3, 150 pts)

### How It Works

1. Form a team of 2–3 students.
2. Pick one project from the menu below (first come, first served — no two teams may pick the same project).
3. Work on the project for ~1.5 weeks.
4. Submit a Jupyter notebook and give a 12–15 minute presentation to the class.

### Presentation Requirements (50 pts)

Your presentation should include:

- **Background** (2–3 min): What algorithm/protocol are you testing? Brief theory recap for classmates who may not remember the details.
- **Method** (3–4 min): What circuits did you build? How did you run them? Show at least one circuit diagram.
- **Results** (4–5 min): Ideal vs. real hardware comparison. Plots are required. What went wrong and why?
- **Conclusions** (2–3 min): What did you learn about the gap between theory and practice? What would you need to close the gap?
- **Q&A** (3–5 min): The class and instructor will ask questions. **Every team member must speak** during the presentation and be able to answer questions.

### Notebook Requirements (100 pts)

| Category | Points | Description |
|:---------|:------:|:------------|
| Correct implementation | 30 | Circuits work correctly on the simulator |
| Real hardware results | 25 | Ran on IBM hardware with sufficient shots; job IDs included |
| Analysis and plots | 25 | Clear comparison of ideal vs. real; quantitative error analysis |
| Written discussion | 20 | Thoughtful explanation of noise sources and their effects |

------------------------------------------------------------------------

## Project Menu

### Project 1: Grover's Algorithm — Simulator vs. Reality

**Question:** How does Grover's search success probability degrade as circuit depth increases on real hardware?

**What to do:**

- Implement Grover's algorithm for $n = 2, 3, 4,$ and $5$ qubits (single marked state) using your homework code or Qiskit's `GroverOperator`.
- For each $n$, compute the optimal number of iterations and build the full circuit.
- Run each circuit on the simulator and on real IBM hardware (4096 shots each).
- Record the **success probability** (probability of measuring the marked state) for ideal vs. real.
- Plot: (a) success probability vs. $n$ for simulator and hardware; (b) transpiled circuit depth vs. $n$.
- For one value of $n$ (e.g., $n = 4$), sweep the number of Grover iterations from 0 to $2t_{\text{opt}}$ on both simulator and hardware. Plot the overshoot curve for both.

```{admonition} Analysis Questions
:class: question
- At what $n$ does the real hardware success probability drop below 50%? Why?
- How does transpiled circuit depth scale with $n$? How does this relate to the hardware's $T_2$ time?
- Does the overshoot curve on real hardware match the theoretical $\sin^2$ prediction?
```

------------------------------------------------------------------------

### Project 2: Quantum Error Correction in Practice

**Question:** Does the 3-qubit bit-flip code actually help on real hardware, or does the overhead of extra gates make things worse?

**What to do:**

- Implement the 3-qubit bit-flip code (from the [error correction lecture](05_02_lecture.md)) with mid-circuit measurement and conditional correction.
- **Experiment A — Correctable error:** Intentionally apply an $X$ error on one data qubit. Run with and without the correction step. Compare the logical error rate (fraction of wrong final measurements) for both cases, on simulator and hardware.
- **Experiment B — No intentional error, just hardware noise:** Skip the intentional $X$ error. Run the encode → syndrome → correct → decode pipeline on real hardware. Compare the logical error rate of the *encoded* qubit to the error rate of a *single unencoded* qubit doing the same computation (just prepare and measure, no encoding).
- **Experiment C — Vary the encoding:** Try encoding $|0\rangle$, $|1\rangle$, and $|+\rangle$. Does the code protect all states equally well on real hardware?

```{admonition} Analysis Questions
:class: question
- In Experiment B, does error correction help or hurt compared to the unencoded qubit? Why?
- What are the dominant error sources? (Gate errors during encoding/syndrome? Measurement errors on the ancillas? Crosstalk?)
- Based on the backend's reported error rates, estimate the threshold below which QEC would start helping. How far is current hardware from that threshold?
```

------------------------------------------------------------------------

### Project 3: Mapping Entanglement Quality Across the Chip

**Question:** Not all qubits on a quantum processor are created equal. How does Bell state fidelity vary across different qubit pairs?

**What to do:**

- Choose a real IBM backend and look up its **coupling map** (which qubits are physically connected).
- Select 8–10 different qubit pairs spread across the chip (near the center, at the edges, adjacent vs. separated by SWAP gates).
- For each pair, prepare $|\Phi^+\rangle$ and measure $\langle ZZ \rangle$, $\langle XX \rangle$, and $\langle YY \rangle$ with 4096 shots.
- Compute the **Bell state fidelity** for each pair: $F = \frac{1}{4}(1 + \langle XX \rangle + \langle YY \rangle + \langle ZZ \rangle)$.
- Create a **heatmap** or **chip layout diagram** showing fidelity across the processor. (The IBM dashboard shows qubit layout; you can annotate it or create your own visualization.)
- Look up the reported CNOT error rates and $T_1$/$T_2$ times for each qubit pair from the IBM dashboard. Correlate these with your measured fidelities.

```{admonition} Analysis Questions
:class: question
- Which qubit pairs had the best and worst fidelity? By how much do they differ?
- How well do IBM's reported error rates predict your measured fidelities?
- If you were choosing qubits for a real computation, what criteria would you use? Does qubit placement matter?
```

------------------------------------------------------------------------

### Project 4: Quantum Teleportation Fidelity

**Question:** Quantum teleportation works perfectly in theory. How faithfully does it work on real hardware?

**What to do:**

- Implement the 3-qubit teleportation protocol using dynamic circuits (`if_test` for conditional corrections).
- Choose 6 input states to teleport, spanning the Bloch sphere: $|0\rangle$, $|1\rangle$, $|+\rangle$, $|-\rangle$, $|{+i}\rangle = \frac{1}{\sqrt{2}}(|0\rangle + i|1\rangle)$, and a general state $R_y(\pi/3)|0\rangle$.
- For each input state, run the teleportation circuit on the simulator and on real hardware (4096 shots).
- Measure Bob's output state using **state tomography**: measure in the $X$, $Y$, and $Z$ bases to reconstruct $\langle X \rangle$, $\langle Y \rangle$, $\langle Z \rangle$, and compute the **state fidelity** $F = \langle \psi_{\text{ideal}} | \rho_{\text{measured}} | \psi_{\text{ideal}} \rangle$.
- Compare the fidelity of teleportation to simply preparing the state directly (no teleportation) on the same hardware. The difference tells you the cost of the teleportation protocol.

```{admonition} Analysis Questions
:class: question
- Which input states teleport with the highest fidelity? The lowest? Why?
- How does teleportation fidelity compare to direct state preparation? What is the "overhead" of teleportation in terms of fidelity loss?
- The teleportation circuit uses mid-circuit measurement. Look up whether your backend supports this natively or needs workarounds. How does this affect your results?
```

------------------------------------------------------------------------

### Project 5: Deutsch-Jozsa and Bernstein-Vazirani at Scale

**Question:** Both algorithms solve their problems in a single query — but the circuits get deeper as $n$ grows. At what point does hardware noise overwhelm the quantum advantage?

**What to do:**

- Implement the Deutsch-Jozsa (DJ) algorithm for $n = 2, 3, 4, 5, 6$ input qubits with one constant and one balanced oracle at each $n$.
- Implement the Bernstein-Vazirani (BV) algorithm for the same range of $n$ with a secret string of your choice at each $n$.
- Run all circuits on the simulator and on real hardware (4096 shots each).
- For DJ: record whether the algorithm gives the correct answer (all-zeros vs. not-all-zeros). Compute the **confidence** = probability of the correct outcome.
- For BV: record the probability of measuring the correct secret string.
- Plot confidence vs. $n$ for both algorithms on simulator and hardware.
- Compare the transpiled circuit depths for DJ vs. BV at each $n$. BV should be shallower (simpler oracles) — does this show up in the hardware results?

```{admonition} Analysis Questions
:class: question
- At what $n$ does each algorithm drop below 90% confidence on real hardware? Below 50%?
- BV uses a simpler oracle than DJ (just CNOTs). Does it perform measurably better on hardware? By how much?
- For your largest $n$, the transpiled circuit may use SWAP gates to handle qubit connectivity. How many SWAPs were inserted? How does this affect the result?
```

------------------------------------------------------------------------

## Timeline

| Date | Milestone |
|:-----|:----------|
| Tue, Apr 14 | Hardware lecture (5.1). Create IBM accounts in class. |
| Thu, Apr 16 | **Project posted** (Phase 1 + Phase 2). Phase 1 tips and Q&A. HW 4.6 due. |
| **Wed, April 22** | **Phase 1 due** (individual Bell state notebook) |
| Tue, Apr 21 | Team project work day (in class). |
| Thu, Apr 23 | Error correction lecture (5.2) + quTools hardware demo (if ready). |
| **Fri, April 24** | **Phase 2 notebook due** (before quiet week) |
| Tue, Apr 28 | **Presentations in class** (Groups 1–3) — quiet week, no new assignments |
| Thu, Apr 30 | **Presentations in class** (Groups 4–5) — last day of class |

------------------------------------------------------------------------

## Tips for Success

```{admonition} Development Workflow
:class: tip
1. **Build and test on the simulator first.** Get your circuits working perfectly with `AerSimulator` before touching real hardware.
2. **Transpile before submitting.** Use `generate_preset_pass_manager` with `optimization_level=3` to minimize circuit depth. Compare the transpiled circuit depth to the original — this is itself interesting data.
3. **Save your job IDs.** Every IBM job has a unique ID. Record these in your notebook as proof of hardware execution.
4. **Don't waste QPU time on debugging.** With 10 minutes/month, you can run hundreds of small experiments — but not if you're debugging on the real machine.
```

```{admonition} For Presentations
:class: tip
The best presentations will connect hardware imperfections back to concepts from this course: decoherence ($T_1$, $T_2$) from Lecture 2.7, gate fidelity from Lecture 5.1, and error correction from Lecture 5.2. Show your plots, explain your physics, and be ready for questions.
```

```{admonition} Reading Hardware Specs
:class: note
The IBM Quantum dashboard shows each qubit's $T_1$, $T_2$, single-qubit gate error rates, two-qubit gate error rates, and readout error rates. Use these to explain your results. You can also access these programmatically — see the [IBM Quantum documentation](https://docs.quantum.ibm.com) for details.
```
