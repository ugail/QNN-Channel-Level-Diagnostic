# Channel-Level Diagnostic for Symmetry Breaking in Noisy Equivariant Quantum Neural Networks

Reference implementation, data, and reproducibility package for

> **A Channel-Level Diagnostic for Symmetry Breaking in Noisy Equivariant Quantum Neural Networks**
> by H. Ugail and N. Howard

This repository provides a training-free, channel-level audit
that tests whether a noisy quantum neural network (QNN) still respects an
intended symmetry once the ideal parameterised circuit is composed with the
device's noise channel. The audit reports a normalised commutator-defect
ratio $\Delta_G \in [0, 1]$ and a bounded compliance score
$C_G = \exp(-\gamma \Delta_G) \in [e^{-\gamma}, 1]$, where $C_G = 1$ exactly
when the realised channel commutes with the target group action on the
tested representation.

<img width="1104" height="830" alt="Figure5" src="https://github.com/user-attachments/assets/6e0de82d-a3cf-4780-b041-7523e8be1edd" />

---

## What's in this repository

```
.
├── Channel-Level-Diagnostic_pilot_notebook_.ipynb                   # main experiments
├── Channel-Level-Diagnostic_figures_.ipynb                          # figure generation
├── Channel-Level-Diagnostic_ibm_hardware_bloch_ordering_probe.ipynb # appendix hardware probe
├── results/                                                         # all CSV + JSON outputs
└── README.md
```

All committed numerical outputs — the simulation CSVs, the verdict tables,
and the IBM hardware probe artefacts — live in a single `results/`
directory. The three notebooks are intentionally separated so that the
simulation step, the figure step, and the optional hardware step can each be
run, re-run, or audited independently of the others.

The notebooks are written for Google Colab and mount Google Drive. Near the
top of each notebook a small configuration cell sets the input/output paths
(for example `CSV_DIR` and `FIG_DIR`). Point these at wherever you place the
`results/` directory and at a figure output directory of your choice; the
notebooks do not assume a fixed absolute layout. They run equally well
outside Colab once those path variables are edited and the Drive-mount cell
is removed.

### `Channel-Level-Diagnostic_pilot_notebook_.ipynb` — main experiments

Computes $\Delta_G$ and $C_G$ for the two symmetry settings of the paper:
$U(1)$ phase symmetry on two qubits, and collective $SU(2)$ rotation symmetry
on two qubits. Sweeps three noise families (depolarising, amplitude damping,
coherent $X$ over-rotation) plus a mixed-noise scenario, and runs the full
pre-specified validation battery: ten correctness checks (V1–V10) and nine
robustness checks (R1–R9). Writes the CSV files listed below to `results/`
and prints a pass/fail verdict for every check. It also computes the
representation-blind channel distance $D_{\mathrm{Id}}$ used in the
manuscript comparison (Table IV). Runs end-to-end on a single
CPU core in under a minute.

### `Channel-Level-Diagnostic_figures_.ipynb` — figure generation

Builds the six figures in the paper. It reads the CSVs produced by the pilot
notebook from `CSV_DIR` and writes the six PNGs to `FIG_DIR`, both set in the
notebook's configuration cell. Figure 1 is a pure illustration (the method
workflow schematic, no data input). Figures 2 and 3 are the two-panel $U(1)$
and $SU(2)$ noise sweeps drawn from `sweep_u1.csv` and `sweep_su2.csv`.
Figure 4 is the mixed-noise sweep from `mixed_sweep.csv`. Figure 5 recomputes
$C_G$ at three values of the compliance temperature $\gamma \in \{2, 5, 10\}$
from the existing $SU(2)$ sweep. Figure 6 is the $SU(2)$ Haar sampling
stability plot from `robustness_R6_sampling_stability.csv`. All figures use
IEEE Access two-column journal styling (single-column ≈ 3.5 in, double-column
span ≈ 7.16 in) at 300 dpi.

### `Channel-Level-Diagnostic_ibm_hardware_bloch_ordering_probe.ipynb` — Appendix A probe

Runs the optional one-qubit ordering probe on real IBM Quantum hardware via
Qiskit Runtime. Compares two orderings of non-commuting single-qubit
rotations $G = R_z(\pi/2)$ and $E = R_x(\pi/2)$, measures empirical Bloch
vectors in the $X$, $Y$, and $Z$ bases, and reports the Bloch-vector
distance with a 5000-resample bootstrap confidence interval. Outputs are
saved to `results/` named by `<backend>_<job_id>`. The artefacts committed
here come from job `d7qoqect738s73ceo4k0` on `ibm_kingston` (2026-05-02).
This probe is a hardware-execution sanity check, **not** a hardware estimate
of $C_G$ or $\Delta_G$ — see Appendix A of the manuscript for the exact
scope.

---

## Reproducing the paper's numerical results

Run the pilot notebook first; it regenerates every CSV in `results/`. The
mapping from manuscript content to file is:

| Manuscript content | File in `results/` |
|---|---|
| $U(1)$ sanity values | `sanity_u1.csv` |
| $SU(2)$ sanity values | `sanity_su2.csv` |
| $U(1)$ noise sweep (Fig. 2) | `sweep_u1.csv` |
| $SU(2)$ noise sweep (Fig. 3) | `sweep_su2.csv` |
| Mixed-noise sweep (Fig. 4) | `mixed_sweep.csv` |
| R6 Haar sampling stability (Fig. 6) | `robustness_R6_sampling_stability.csv` |
| R7 three-qubit check | `robustness_R7_n3.csv` |
| Representation-blind channel distance $D_{\mathrm{Id}}$ (Table IV) | `comparison_channel_distance.csv` |
| V1–V10 correctness verdicts | `verdict_v.csv` |
| R1–R9 robustness verdicts | `verdict_r.csv` |

Then run the figures notebook to regenerate the six PNGs from those CSVs.

### Hardware probe artefacts

The IBM Quantum probe outputs are committed for reference and are not
required to reproduce any simulation result:

| Content | File in `results/` |
|---|---|
| Raw counts and per-basis expectations | `ibm_kingston_d7qoqect738s73ceo4k0_hardware_ordering_probe_summary.csv` |
| Bloch-vector distance and bootstrap CI | `ibm_kingston_d7qoqect738s73ceo4k0_hardware_ordering_probe_distance.csv` |
| Full run metadata | `ibm_kingston_d7qoqect738s73ceo4k0_hardware_ordering_probe.json` |

---

## Citation

If you use the cross-replay paradigm, the code in this repository, or the precomputed result tables, please cite:

> H. Ugail and N. Howard,*A Channel-Level Diagnostic for Symmetry Breaking in Noisy Equivariant Quantum Neural Networks*, 2026. Under review.


---

## License

This repository is released under the MIT License.
