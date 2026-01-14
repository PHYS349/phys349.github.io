



## Where Are We with Quantum Computers?

The fundamental challenge is that quantum states are fragile. A qubit in superposition is a delicate thing. Any interaction with the environment—a stray photon, a vibrating atom in the substrate, a fluctuating magnetic field—can "measure" the qubit and collapse the superposition. This is **decoherence**, and it destroys quantum information.

For decades, this seemed like an insurmountable obstacle. Qubits decohered in microseconds; useful computations would take milliseconds or longer. The math did not work.

### Quantum Error Correction: The Breakthrough

The conceptual breakthrough came in the 1990s with quantum error correction. The idea is counterintuitive: you cannot copy a quantum state (the no-cloning theorem forbids it), so how can you back it up? The answer is to encode a single _logical_ qubit across many _physical_ qubits in such a way that errors can be detected and corrected without ever measuring the encoded information directly.

This is not just theoretical. It has a threshold: if the error rate of your physical qubits is below a certain value (roughly 1%), you can stitch them together into logical qubits that have _arbitrarily low_ error rates. You pay for this with overhead—you need many physical qubits per logical qubit—but in principle, you can compute indefinitely.

As of late 2024, we have crossed this threshold. Google's Willow chip (105 physical qubits) demonstrated that their logical qubits are more reliable than their physical qubits—the error correction is _working_, not just adding noise. This is a phase transition. It means the path to fault-tolerant quantum computing is no longer speculative; it is an engineering problem.

### Physical Qubits vs. Logical Qubits

The distinction matters:

- **Physical qubit:** A single hardware device (one superconducting circuit, one trapped ion, one atom, etc.)
- **Logical qubit:** An error-corrected qubit encoded across many physical qubits

Raw physical qubits are noisy. Error correction can, in principle, make logical qubits arbitrarily reliable—but the cost is overhead: many physical qubits per logical qubit.

So when someone announces "we have $N$ qubits," the right questions are: Physical or logical? What error rates? What circuit depth is realistic?

### What These Machines Look Like

If you ever visit a quantum computing lab, here is what you will see:

**Superconducting systems (IBM, Google):** A large gold-colored cylinder, about the size of a chandelier, hanging from the ceiling. This is a dilution refrigerator. Inside, the temperature is about 15 mK—colder than outer space, colder than anywhere in the natural universe. At the bottom sits a chip, maybe 2 cm on a side, containing the qubits: tiny aluminum circuits that behave like artificial atoms. Microwave pulses, delivered through coaxial cables, manipulate the qubits. Gate times are around 20 ns, with coherence times of tens to hundreds of microseconds. The whole system is exquisitely engineered to keep out noise: special foundations to dampen vibrations, carefully filtered wiring, extensive shielding.

**Trapped ion systems (Quantinuum, IonQ):** A steel vacuum chamber, perhaps the size of a shoebox, sitting on an optical table covered in mirrors and lenses. Inside, a few dozen ions—often ytterbium or barium—float in a line, suspended by oscillating electric fields. Lasers do everything: cooling the ions, manipulating their internal states (the qubits), and reading out the results through fluorescence. Gate times are slower (microseconds), but coherence times are much longer (seconds to minutes), and every qubit is identical because they are actual atoms, not fabricated circuits.

**Neutral atom systems (QuEra):** Similar to trapped ions, but using neutral atoms held by optical tweezers—tightly focused laser beams that act like tiny force fields. Arrays of atoms can be rearranged dynamically, allowing flexible connectivity. Entanglement is generated through Rydberg interactions: exciting atoms to high-$n$ states where they have enormous dipole moments and interact strongly over micron-scale distances. This platform has advanced rapidly in the last few years.

No one knows which platform will "win." They may coexist for different applications. The competition is fierce and the progress is fast.

### The Current Moment

We are in the **NISQ era**—Noisy Intermediate-Scale Quantum—but we are leaving it. The machines of 2020 could do impressive physics experiments but nothing practically useful. The machines of 2025 are beginning to cross into "quantum utility": computations that are genuinely easier to do on quantum hardware than on classical supercomputers, for problems people actually care about (mostly in chemistry and materials science).

The next milestone is fault-tolerant quantum computing at scale: thousands of logical qubits with low enough error rates to run deep circuits. That is probably 5–10 years away. When it arrives, algorithms like Shor's (which breaks RSA encryption) and quantum simulation of complex molecules will become practical.

This is the moment we are in: the transition from "it works in principle" to "it works in practice."

### Summary: Two Worlds

|Feature|Classical Computing|Quantum Computing|
|---|---|---|
|**Basic unit**|Bit (0 or 1)|Qubit (coherent superposition)|
|**Key resource**|Transistor count|Superposition and entanglement|
|**Logic**|Boolean (deterministic)|Unitary (interference-based)|
|**State space scaling**|Linear ($N$ bits → $N$ values)|Exponential ($N$ qubits → $2^N$ amplitudes)|
|**Outcome**|Deterministic|Probabilistic (engineered)|

### What to Take Away Before Next Lecture

1. Some problems are hard because the number of possibilities explodes.
2. Many "hard" problems can be rewritten as "find the minimum of an energy function."
3. That connects directly to physics language: Hamiltonians and ground states.
4. Quantum computers exist, but "useful, error-corrected, scalable" is still a work in progress.
5. Next: we define a qubit cleanly and learn how quantum gates move states around.


## Homework

- Keep working on your wedding python code. 
