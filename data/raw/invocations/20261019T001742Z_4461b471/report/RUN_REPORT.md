# Notebook run 20261019T001742Z_4461b471

Loaded stages: 257; newly computed stages: 0.

| Algorithm | Exact-state local residual | Ideal TV | Fake-noise TV | Hardware TV |
|---|---:|---:|---:|---:|
| hhl | 0.0146677 | 0.0292804 | 0.586946 | 0.634262 |
| qsvt | 1.92179e-05 | 0.0110748 | 0.563475 | 0.681159 |
| vqls_statevector | 2.13944e-08 | 0.00457651 | 0.0111583 | — |
| vqls_measured | 0.13788 | 0.0214599 | 0.025122 | 0.032202 |

Local residuals require a complete complex state; TV is a different, count-based quantity.
Fake noise is a model derived from the named archived snapshot. Hardware values appear only after successful physical collection.
This development pilot does not establish algorithmic advantage, drift robustness, or on-device optimization.

Heterogeneous-layout aggregate diagnostic: 0.0419234; delta-method standard error 0.0061528.
This is not a validated single-state VQLS cost or a fidelity certificate. The five settings used different ordered physical layouts.
The ratio is finite-shot biased; its delta-method uncertainty excludes preparation mismatch, calibration drift and readout bias.
