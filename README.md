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
for the exact solution. The hardware work comprises 27 confirmation jobs on `ibm_rensselaer`, `ibm_kingston`, and
`ibm_boston`, with calibration-based predictions archived before the held-out runs, and a registered replication of
10 jobs on `ibm_boston` (blocks R1–R5, November 17–20, 2026).

## Contents

| Path | Contents |
| --- | --- |
| `Quantum_Diffusion_Master.ipynb` | The only source file: problem construction, the HHL, QSVT, and VQLS implementations, the operator-level acceptance test, compilation, tomography, the calibration-based prediction models, the analysis, and every figure and table. |
| `requirements.txt` | Pinned package versions (Python 3.13). |
| `data/tables/` | The records behind every table, figure, and number of the article (CSV and JSON), as written by the November 21, 2026 run that produced release v1.2. Files starting with `revision_` hold the revision analysis, files starting with `replication_` the registered replication. |
| `data/hardware_jobs.csv` | Registry of all 40 physical jobs (27 confirmation, 10 replication, 3 development): job id, device, block, arm, timestamps, charged and estimated QPU seconds, and the calibration snapshots each job was compiled, submitted, and started under. |
| `data/verification.json` | Record of the restore-and-rerun reproducibility check of the release v1.0 archive (October 18–19, 2026). |
| `data/raw/hardware_jobs/` | Raw record of every physical job, including the measured counts and the submitted circuits (QPY). |
| `data/raw/calibrations/`, `data/raw/calibration_observations/` | Every calibration snapshot of the three processors used by the study (compile-time, pre-submission, and job-start). |
| `data/raw/hardware_campaigns/`, `data/raw/invocations/` | Campaign definitions and the environment record of every notebook invocation. |
| `data/raw/configuration.json`, `data/raw/environment.json` | Run configuration and software environment of the archived run. |
| `data/raw/portable_manifest.json` | SHA-256 of every file of the release v1.2 portable archive (see below); the files under `data/raw/` match it byte for byte, apart from the files listed only in `portable_manifest_v1.0.json`. |
| `data/raw/portable_manifest_v1.0.json` | Manifest of the release v1.0 archive. Its 770 files that are not in the v1.2 archive, the records of the four reproduction runs on Linux of October 18–19, 2026, including the calibration observations those runs made, are kept unchanged. |
| `figures/` | Figures 1–5 as written by the notebook (PDF and PNG). In the article the title lines are omitted, and Figures 2, 4, and 5 are redrawn at journal width from the same records (the panels of Figures 4 and 5 in two rows). |

## Reproducing the results

1. Install Python 3.13 and the pinned packages: `pip install -r requirements.txt`.
2. Download `portable_code_and_outputs.zip` from release v1.2 of this repository. Besides the raw records in `data/raw/`,
   it contains the per-stage records of the archived run (about 1.3 GB unpacked), which are not kept in git.
3. Open the notebook, run the definition cells, then call
   `restored_store = restore_run_export(archive_path, empty_output_directory)`.
   Restoration verifies every checksum and rebuilds the derived files in dependency order without running experiments
   (`data/raw/RESTORE_INSTRUCTIONS.md`).
4. Set the notebook output directory to that directory and run all cells. Without IBM Quantum access nothing is
   submitted, and every table, figure, and number of the article is regenerated from the saved records.

Re-running the hardware experiments requires IBM Quantum access. Credentials are read from Qiskit's saved-account
store and are never written to the notebook or to any record.

## Revision and registered replication

The cells after `journal_paper` were added during the revision in November 2026; no earlier cell was changed, and
`journal_export` was moved, unchanged, to the end so that its package holds every stage of the run.
`journal_revision_analysis` recomputes every hardware cell at 10^4 bootstrap replicates, and the decision cells with
their controls, from the saved counts without device access. `journal_replication_design` is the frozen design of a
replication on `ibm_boston`: five blocks under new calibrations, the decision circuit on all four basis inputs, a
four-phase-qubit HHL that places the largest eigenvalue on the top phase bin, and H4 with nodes 0 and 3 exchanged so
that its source is |11⟩. Design identifier: `journal_replication_v1-48c0c4b187ab`; content hash:
`48c0c4b187ab6bdb768c8e1a6ff14af23d76045144012effb26d027cd291d86e`. Commit `9770c9a`, which added this cell before the
first replication job, is the registration of the design (release v1.1, https://doi.org/10.5281/zenodo.24323769).
`journal_replication_run` submits one block per new calibration, `journal_replication_report` applies the frozen
decision rule, `journal_replication_table` writes the replication table and the registered checks, and
`journal_job_registry` writes `data/hardware_jobs.csv`.

Result of blocks R1–R5: the decision cell of the confirmation campaign did not replicate (endpoint P1: mean *G* = −0.02,
95% *t*-interval [−0.44, 0.40]). The four-phase-qubit top-bin HHL on the relabeled H4, with 156 CZ gates, gave a mean
*G* = 0.53, 95% *t*-interval [0.33, 0.73] (endpoint P2, one-sided *p* < 0.001), which establishes inversion on
`ibm_boston` under the registered Holm rule. Details: `data/tables/replication_decision.json` and
`replication_checks.json`.

The archive of release v1.2 was written by running the cells of the notebook in order, with the wait for queued jobs
in `journal_replication_run` set to 0 minutes because all jobs had finished; the copy of the notebook inside the
archive records that setting.

## License

Code: MIT (see `LICENSE`). Data and figures: CC BY 4.0.

## Citation

All releases are archived at Zenodo: https://doi.org/10.5281/zenodo.24047566. Release v1.0:
https://doi.org/10.5281/zenodo.24047567; release v1.1 (registration of the replication design):
https://doi.org/10.5281/zenodo.24323769. See `CITATION.cff`.
