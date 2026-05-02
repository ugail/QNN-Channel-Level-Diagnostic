# Channel-Level Diagnostic for Symmetry Breaking in Noisy Equivariant Quantum Neural Networks

Reference implementation, data, and reproducibility package for

> **A Channel-Level Diagnostic for Symmetry Breaking in Noisy Equivariant Quantum Neural Networks**
> Hassan Ugail and Newton Howard

This repository provides a training-free, channel-level audit
that tests whether a noisy quantum neural network (QNN) still respects an
intended symmetry once the ideal parameterised circuit is composed with the
device's noise channel. The audit reports a normalised commutator-defect
ratio $\Delta_G \in [0, 1]$ and a bounded compliance score
$C_G = \exp(-\gamma \Delta_G) \in [e^{-\gamma}, 1]$, where $C_G = 1$ exactly
when the realised channel commutes with the target group action on the
tested representation.

The repository contains everything needed to reproduce every figure, every
verdict table, and every numerical value reported in the paper from a CPU
runtime, with no proprietary hardware or non-public software dependencies.

---
<img width="1104" height="830" alt="Figure5" src="https://github.com/user-attachments/assets/c3142ada-e7a4-4bd5-8f7a-5be750838326" />

## What's in this repository

```
.
├── notebooks/
│   ├── Channel-Level-Diagnostic_pilot_notebook_.ipynb            # main experiments
│   ├── Channel-Level-Diagnostic_figures_.ipynb                   # figure generation
│   └── Channel-Level-Diagnostic_ibm_hardware_bloch_ordering_probe.ipynb
│                                                                 # appendix hardware probe
├── results/                  # CSV outputs from the pilot notebook
└── README.md
```

The three notebooks are the heart of the repository. They are intentionally
separated so that the simulation step, the figure step, and the optional
hardware step can each be run, re-run, or audited independently of the
others.

### `Channel-Level-Diagnostic_pilot_notebook_.ipynb` — main experiments

Computes $\Delta_G$ and $C_G$ for the two symmetry settings of the paper:
$U(1)$ phase symmetry on two qubits, and collective $SU(2)$ rotation symmetry
on two qubits. Sweeps three noise families (depolarising, amplitude damping,
coherent $X$ over-rotation) plus a mixed-noise scenario, and runs the full
pre-specified validation battery: ten correctness checks (V1–V10) and nine
robustness checks (R1–R9). Writes nine CSV files to `results/` and prints a
pass/fail verdict for every check. Runs end-to-end on a single CPU core.

### `Channel-Level-Diagnostic_figures_.ipynb` — figure generation


### `Channel-Level-Diagnostic_ibm_hardware_bloch_ordering_probe.ipynb` — 

Runs the optional one-qubit ordering probe on real IBM Quantum hardware via
Qiskit Runtime. Compares two orderings of non-commuting single-qubit
rotations $G = R_z(\pi/2)$ and $E = R_x(\pi/2)$, measures empirical Bloch
vectors in the $X$, $Y$, and $Z$ bases, and reports the Bloch-vector
distance with a 5000-resample bootstrap confidence interval. Outputs are
saved to `hardware_results/` named by `<backend>_<job_id>`. This probe is a
hardware-execution sanity check, **not** a hardware estimate of $C_G$ or
$\Delta_G$.

---


## License

This repository is released under the MIT License. 
