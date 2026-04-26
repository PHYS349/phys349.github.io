---
kernelspec:
  name: python3
  display_name: Python 3
---

# Final Project — Quantum Computing Meets Real Hardware

## Overview

The final project has two phases, both individual. In **Phase 1**, everyone completes the same warm-up: prepare and measure Bell states on a real IBM quantum processor. In **Phase 2**, everyone runs Grover's algorithm at increasing qubit counts on real hardware and quantifies how noise overwhelms the algorithm as circuit depth grows.

The central theme is **theory vs. reality**. Every algorithm and protocol we've studied this semester assumed perfect qubits and noiseless gates. Real hardware has gate errors, decoherence, crosstalk, and measurement noise. Your job is to run something we've learned on a real quantum computer, measure how reality deviates from theory, and explain *why*.

**Total: 150 points**

| Component | Points | Due |
|:----------|:------:|:----|
| Phase 1: Bell-state warm-up notebook | 50 | Wednesday, April 22 |
| Phase 2: Grover's-on-hardware notebook | 100 | **Friday, May 8** |

------------------------------------------------------------------------

## IBM Quantum Account Setup

Before starting Phase 1, create a free IBM Quantum account on IBM Cloud:

1. Go to [quantum.cloud.ibm.com](https://quantum.cloud.ibm.com) and log in (Google OAuth with your university or personal Gmail is fastest).
2. On first login, a "Welcome" modal appears. Leave the defaults (`open-instance`, `us-east`, Open Plan) and click **Create instance**. The **Open Plan** gives you **10 minutes of free QPU time per month** (no credit card needed).
3. Create an **API key**: click **Create +** in the API key section on the home page. **Download the key immediately** — it is shown only once and disappears after ~5 minutes.
4. Copy your **instance CRN**: go to Instances → click `open-instance` → copy the CRN (a long string starting with `crn:v1:bluemix:...` and ending with `::`).
5. Save both credentials by running this once in Python:

```python
from qiskit_ibm_runtime import QiskitRuntimeService
QiskitRuntimeService.save_account(
    channel="ibm_cloud",
    token="YOUR_API_KEY_HERE",
    instance="YOUR_INSTANCE_CRN_HERE",
    overwrite=True,
    set_as_default=True,
)
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

## Phase 2: Grover's Algorithm on Real Hardware (Individual)

### What you'll do

Port your Grover's-algorithm code from HW 4.6 to a real IBM quantum processor and measure the
gap between theory and hardware reality. You'll run Grover at $n=2$, $n=3$, and $n=4$ qubits,
sweep the iteration count at $n=3$, and quantify why success degrades with depth.

The science question: **at what point does hardware noise overwhelm the quantum advantage?**

### Starter notebook

Open `lectures/final_project_testing.ipynb` (your project notebook). It walks you through:

- **Part A** — bring forward `build_oracle` and `build_diffusion` from HW 4.6, verify on simulator
- **Part B** — one-time IBM Cloud account + credential setup
- **Part C** — transpile and submit Grover at $n = 2, 3, 4$; compare hardware vs. simulator
- **Part D** — iteration sweep at $n=3$; does the $\sin^2$ overshoot survive on hardware?
- **Part E** — pull the device's $T_1$, $T_2$, gate errors for the *specific qubits* your
  circuits used, then explain your results in 4–6 sentences

### Grading (100 pts)

| Part | Points | What earns the points |
|:-----|:------:|:---------------------|
| A — code working on simulator | 15 | Sanity checks pass for n=2, 3, 4 |
| B — IBM creds saved, can connect | 5 | One-time setup |
| C — circuits ran on hardware, plot 1 included | 25 | Hardware execution + sim/hw comparison |
| D — iteration sweep on hardware, plot 2 included | 25 | Iteration-sweep curves on both sim and hw |
| E — analysis quoting specific numbers | 30 | The actual learning outcome |

**Total: 100 points.** This replaces the final exam.

```{admonition} Important Practical Notes
:class: warning
- **Develop on the simulator first.** Use `AerSimulator` for debugging. Only send your final
  experiments to real hardware.
- **Queue times vary** from minutes to hours depending on demand. Submit at least a day before
  the deadline.
- **Save your job IDs.** If your laptop closes or your kernel restarts, you can recover results
  with `service.job(job_id).result()` — but only if you have the ID.
- **You have 10 minutes/month of QPU time.** A full run-through of the notebook uses ~30
  seconds, so you have headroom — but don't loop in a debugging session.
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
| **Fri, May 8** | **Phase 2 notebook due** (no presentation; written analysis only) |

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
The IBM Quantum dashboard at [quantum.cloud.ibm.com](https://quantum.cloud.ibm.com) shows each qubit's $T_1$, $T_2$, single-qubit gate error rates, two-qubit gate error rates, and readout error rates. Use these to explain your results. You can also access these programmatically via `backend.target` — the Phase 1 notebook has a cell that pulls all the relevant numbers.
```
