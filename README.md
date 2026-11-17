# Inversion Gain: HHL, QSVT, and VQLS on IBM hardware

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24047566.svg)](https://doi.org/10.5281/zenodo.24047566)

Code, data, and figures for the article

> B. Yalcin, M. Agirsoy, M. A. Oehlschlaeger, S. Patterson, and L. T. Zhang,
> *Inversion Gain: A Verification-First Hardware Benchmark of HHL, QSVT, and VQLS for Regularized Graph Diffusion*
> (submitted to *Quantum Information Processing*, 2026).

The study benchmarks the Harrow–Hassidim–Lloyd (HHL) algorithm, quantum singular value transformation (QSVT), and the
variational quantum linear solver (VQLS) on regularized graph-Laplacian systems. Coherent circuits are accepted only if
their full postselected operator matches the scaled inverse, outputs are measured by conditional state tomography on a
common preparation, and results are reported as an inversion gain *G* that is zero for input-state preparation and one
for the exact solution. The hardware campaign comprises 27 confirmation jobs on `ibm_rensselaer`, `ibm_kingston`, and
`ibm_boston`, with calibration-based predictions archived before the held-out runs.

## Contents

| Path | Contents |
| --- | --- |
| `Quantum_Diffusion_Master.ipynb` | The only source file: problem construction, the HHL, QSVT, and VQLS implementations, the operator-level acceptance test, compilation, tomography, the calibration-based prediction models, the analysis, and every figure and table. |
| `requirements.txt` | Pinned package versions (Python 3.13). |
| `data/tables/` | The records behind every table, figure, and number of the article (CSV and JSON). |
| `data/hardware_jobs.csv` | Registry of all 30 physical jobs (27 confirmation, 3 development): job id, device, block, arm, timestamps, charged and estimated QPU seconds, and the calibration snapshots each job was compiled, submitted, and started under. |
| `data/verification.json` | Record of the restore-and-rerun reproducibility check. |
| `data/raw/hardware_jobs/` | Raw record of every physical job, including the measured counts and the submitted circuits (QPY). |
| `data/raw/calibrations/`, `data/raw/calibration_observations/` | Every calibration snapshot of the three processors used by the study (compile-time, pre-submission, and job-start). |
| `data/raw/hardware_campaigns/`, `data/raw/invocations/` | Campaign definitions and the environment record of every notebook invocation. |
| `data/raw/configuration.json`, `data/raw/environment.json` | Run configuration and software environment of the archived run. |
| `data/raw/portable_manifest.json` | SHA-256 of every file of the portable archive (see below); the files under `data/raw/` match it byte for byte. |
| `figures/` | Figures 1–5 as written by the notebook (PDF and PNG). In the article the title lines are omitted, and Figures 2, 4, and 5 are redrawn at journal width from the same records (the panels of Figures 4 and 5 in two rows). |

## Reproducing the results

1. Install Python 3.13 and the pinned packages: `pip install -r requirements.txt`.
2. Download `portable_code_and_outputs.zip` from release v1.0 of this repository. Besides everything in `data/raw/`, it
   contains the per-stage records of the archived run (about 1 GB unpacked), which are not kept in git.
3. Open the notebook, run the definition cells, then call
   `restored_store = restore_run_export(archive_path, empty_output_directory)`.
   Restoration verifies every checksum and rebuilds the derived files in dependency order without running experiments
   (`data/raw/RESTORE_INSTRUCTIONS.md`).
4. Set the notebook output directory to that directory and run all cells. Without IBM Quantum access nothing is
   submitted, and every table, figure, and number of the article is regenerated from the saved records.

Re-running the hardware experiments requires IBM Quantum access. Credentials are read from Qiskit's saved-account
store and are never written to the notebook or to any record.

## Revision and registered replication

The cells after `journal_paper` were added during the revision in November 2026; no earlier cell was changed.
`journal_revision_analysis` recomputes every hardware cell at 10^4 bootstrap replicates, and the decision cells with
their controls, from the saved counts without device access. `journal_replication_design` is the frozen design of a
replication on `ibm_boston`: five blocks under new calibrations, the decision circuit on all four basis inputs, a
four-phase-qubit HHL that places the largest eigenvalue on the top phase bin, and H4 with nodes 0 and 3 exchanged so
that its source is |11⟩. Design identifier: `journal_replication_v1-48c0c4b187ab`; content hash:
`48c0c4b187ab6bdb768c8e1a6ff14af23d76045144012effb26d027cd291d86e`. The commit that adds this cell is the registration
of the design: `journal_replication_run` submits nothing until `REPLICATION_RUN['public_registration']` names that
commit, and `journal_replication_report` applies the frozen decision rule.

## License

Code: MIT (see `LICENSE`). Data and figures: CC BY 4.0.

## Citation

Release v1.0 is archived at Zenodo: https://doi.org/10.5281/zenodo.24047567 (all versions:
https://doi.org/10.5281/zenodo.24047566). See `CITATION.cff`.
