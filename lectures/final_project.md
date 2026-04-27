---
kernelspec:
  name: python3
  display_name: Python 3
---

# Final Project — Grover's Algorithm on Real Quantum Hardware

This project replaces the final exam. It is **individual** (each student submits their own notebook), but you are welcome to work alongside a partner or small group while debugging.

## What you'll do

Port your Grover's algorithm code from HW 4.6 to a real IBM quantum processor, study how the success probability degrades as circuit depth grows, and quantify *why* using the device's gate error rates and decoherence times. The science question:

> **At what point does hardware noise overwhelm the quantum advantage?**

## Schedule

| Date | What |
|:-----|:-----|
| Tue, Apr 28 | **In class:** intro to the project, then IBM Cloud account setup together. **Bring your laptop.** |
| Thu, Apr 30 | **In class:** walk through the notebook end-to-end. Ishita and I will help debug. |
| **Fri, May 8** | **Notebook due** at midnight (Brightspace) |

## Notebook

[📥 Download Final Project Notebook (.ipynb)](../homework/download_final_project.html)

The notebook is structured in seven parts:

- **Part A** — Bring forward your `build_oracle` and `build_diffusion` from HW 4.6 and verify them on the simulator. Runs entirely on your laptop, no IBM account needed.
- **Part B** — IBM Cloud account, instance, API key, CRN, save_account. Detailed walkthrough included.
- **Part C** — Submit Grover's at $n = 2, 3, 4$ to a real backend and compare to the simulator.
- **Part D** — Iteration sweep at $n = 3$: does the $\sin^2$ overshoot survive on hardware?
- **Part E** — Pull $T_1$, $T_2$, gate errors for the *specific qubits* your circuits used.
- **Part F** — A 200–250 word memo to a hypothetical lab director: should they run $n=6$ next month, yes or no?
- **Part G** — Design a follow-up experiment to pin down where this backend crosses the "quantum uselessness threshold."

## Estimated time

About **4–6 hours of focused work**, plus 1–4 hours of queue waits depending on demand. Do not start the night before.

```{admonition} Important Practical Notes
:class: warning
- **Develop on the simulator first.** Get every circuit working on `AerSimulator` before sending it to real hardware.
- **Queue times vary** from minutes to hours depending on demand. Submit at least a day before the deadline.
- **Save your job IDs.** If your laptop closes or your kernel restarts, you can recover results with `service.job(job_id).result()` — but only if you have the ID. The notebook has a recovery cell that walks you through this.
- **You have 10 minutes/month of QPU time.** A full run-through of the notebook uses ~30 seconds, so you have plenty of headroom — but don't loop on real hardware while debugging.
```

```{admonition} If you get stuck
:class: tip
Office hours, the course Slack, and email all work. The IBM Cloud setup is the highest-failure-rate step in the whole project, which is why we're doing it together in class on Tuesday — please come.
```
