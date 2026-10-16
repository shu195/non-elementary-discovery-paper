# Data-Driven Identification of Non-Elementary Dynamical Systems from Blind Sensor Telemetry

**Shubham Barmase**  
*Independent Researcher*  
`barmase32@gmail.com`

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Format: Open Research Paper & Data](https://img.shields.io/badge/Release-Paper%20%26%20Data-brightgreen.svg)]()
[![Domain: Computational Physics](https://img.shields.io/badge/Field-Nonlinear%20Dynamics-orange.svg)]()

---

## Overview

This repository hosts the academic research paper and empirical benchmark datasets for **"Data-Driven Identification of Non-Elementary Dynamical Systems from Blind Sensor Telemetry"**.

* **Full Academic Paper**: [`paper/paper.md`](paper/paper.md)
* **Empirical Datasets & Sweeps**: [`data/`](data/)

---

## Abstract

Inferring closed-form governing equations from discrete observational time series remains a central inverse problem in computational physics. Conventional symbolic regression frameworks predominantly operate in the explicit time domain, attempting to represent trajectories as compositions of elementary functions $x(t) \in \mathcal{E}$ over $\mathbb{C}(t)$. However, a fundamental algebraic barrier arises across nonlinear mechanics: even when a second-order dynamical system $\ddot{x} = g(x, \dot{x})$ is defined by a sparse elementary vector field, its integral curves $x(t)$ generically lie outside the class of elementary Liouvillian functions—requiring inversion of elliptic integrals of genus $g \ge 1$ (e.g., finite-amplitude pendulum, Duffing bistable beam, anharmonic crystal lattice) or converging to trans-Liouvillian limit cycles (e.g., van der Pol oscillator).

In this work, we investigate the resolution of this non-elementary trajectory barrier using a dual-track topological Code-Neuron discovery framework operating on blind scalar sensor telemetry $(t_i, x_i)$ with no equation templates and zero velocity measurements. The framework simultaneously evaluates explicit time-domain symbolic hypotheses $x = f(t)$ against phase-space differential hypotheses $\ddot{x} = g(x, \dot{x})$ reconstructed via symmetric $\mathcal{O}(\Delta t^4)$ finite-difference stencils and local quartic Savitzky-Golay polynomial projections.

Across five canonical non-elementary physical systems and one elementary Liouvillian control, explicit time-domain symbolic regression fails to extrapolate on all five non-elementary flows ($\mathrm{NRMSE} \in [0.30, 1.18]$), whereas blind phase-space code-neuron discovery recovers the exact symbolic structure and parameters of every governing differential law, reducing out-of-distribution future extrapolation error by $10^7\times$ to $2.6 \times 10^8\times$ ($\mathrm{NRMSE} \in [3.54 \times 10^{-9}, 2.53 \times 10^{-8}]$) within $16\text{--}82\text{ ms}$ on a single CPU core. Furthermore, we derive the exact finite-difference noise amplification factor $\sqrt{1414}/(12\Delta t^2) \approx 3.133 \Delta t^{-2}$ that causes naive stencils to break down below $60\text{--}70\text{ dB}$ SNR, and demonstrate that local quartic Savitzky-Golay differentiation shifts the exact symbolic support recovery boundary down to $40\text{ dB}$ on weak-damping bistable flows and $20\text{ dB}$ on self-sustained limit cycles ($10,000\times$ higher noise power tolerance).

---

## Key Experimental Results

### 1. Dual-Track Benchmark on Non-Elementary Systems

| System | Analytic Obstacle in Time $t$ | Ground-Truth Differential Law | Explicit $x(t)$ Extrap NRMSE | Phase-Space Code-Neuron Discovered Law | Phase-Space Extrap NRMSE | Error Reduction Factor |
| :--- | :--- | :--- | :---: | :--- | :---: | :---: |
| **Damped Nonlinear Pendulum** | Incomplete Elliptic Integral $F(\phi, k)$ | $\ddot{x} = -0.2\dot{x} - 9.81\sin(x)$ | $3.00 \times 10^{-1}$ | $\ddot{x} = -0.2000\dot{x} - 9.8100\sin(x)$ | **$2.53 \times 10^{-8}$** | **$1.2 \times 10^7\times$** |
| **Duffing Bistable Buckling** | Jacobi Elliptic $\mathrm{cn}(t; k), \mathrm{dn}(t; k)$ | $\ddot{x} = x - 0.15\dot{x} - x^3$ | $8.19 \times 10^{-1}$ | $\ddot{x} = 1.0000x - 0.1500\dot{x} - 1.0000x^3$ | **$3.54 \times 10^{-9}$** | **$2.3 \times 10^8\times$** |
| **van der Pol Oscillator** | Trans-Liouvillian Limit Cycle | $\ddot{x} = -x + \dot{x} - x^2\dot{x}$ | $3.45 \times 10^{-1}$ | $\ddot{x} = -1.0000x + 1.0000\dot{x} - 1.0000x^2\dot{x}$ | **$2.03 \times 10^{-8}$** | **$1.7 \times 10^7\times$** |
| **Apex-Crossing Ballistics** | Non-Smooth Sign-Reversing Drag | $\ddot{x} = -9.81 - 0.25\lvert\dot{x}\rvert\dot{x}$ | $1.18 \times 10^{0}$ | $\ddot{x} = -9.8100 - 0.2500\lvert\dot{x}\rvert\dot{x}$ | **$4.62 \times 10^{-9}$** | **$2.6 \times 10^8\times$** |
| **Anharmonic Lattice** | Weierstrass Elliptic $\wp(t; g_2, g_3)$ | $\ddot{x} = -4x - 0.1\dot{x} + 1.5x^2$ | $3.38 \times 10^{-1}$ | $\ddot{x} = -4.0000x - 0.1000\dot{x} + 1.5000x^2$ | **$5.38 \times 10^{-9}$** | **$6.3 \times 10^7\times$** |
| **Elementary Control Signal** | None (Liouvillian Extension Field) | $x(t) = \sin(t) + 2.5e^{-t}$ | **$1.13 \times 10^{-12}$** | $\ddot{x} = 0.1522 - 1.1095x + 0.3501x^3$ (Miss) | $3.14 \times 10^{-1}$ | **Explicit Wins ($2.8 \times 10^{11}\times$)** |

---

### 2. Temporal Resolution Scaling: Quartic Error Convergence

As established by Taylor expansion of the symmetric five-point stencils ($\hat{\dot{x}} = \dot{x} - \frac{1}{30}\Delta t^4 x^{(5)}$, $\hat{\ddot{x}} = \ddot{x} - \frac{1}{90}\Delta t^4 x^{(6)}$), future extrapolation error scales with the fourth power of the sampling step:

| Sampling Interval $\Delta t$ | Frequency | Duffing Extrap NRMSE | van der Pol Extrap NRMSE | Theoretical Ratio $(\Delta t / 0.002)^4$ | Measured Duffing Ratio |
| :---: | :---: | :---: | :---: | :---: | :---: |
| $0.002\text{ s}$ | $500\text{ Hz}$ | $5.10 \times 10^{-12}$ | $3.04 \times 10^{-11}$ | $1.0\times$ | $1.0\times$ |
| $0.005\text{ s}$ | $200\text{ Hz}$ | $2.18 \times 10^{-10}$ | $1.22 \times 10^{-09}$ | $39.1\times$ | $42.7\times$ |
| $0.010\text{ s}$ | $100\text{ Hz}$ | $3.54 \times 10^{-09}$ | $2.03 \times 10^{-08}$ | $625.0\times$ | $694.1\times$ |
| $0.020\text{ s}$ | $50\text{ Hz}$ | $5.85 \times 10^{-08}$ | $3.46 \times 10^{-07}$ | $10,000.0\times$ | $11,470.6\times$ |
| $0.050\text{ s}$ | $20\text{ Hz}$ | $2.53 \times 10^{-06}$ | $1.58 \times 10^{-05}$ | $390,625.0\times$ | $496,078.4\times$ |
| $0.100\text{ s}$ | $10\text{ Hz}$ | $4.80 \times 10^{-05}$ | $2.79 \times 10^{-04}$ | $6,250,000.0\times$ | $9,411,764.7\times$ |

---

### 3. Noise Phase Transition: Naive Stencils vs. Savitzky-Golay Projections

Finite differences exhibit second-order noise variance amplification:

$$
\mathrm{Var}(\hat{\ddot{\epsilon}}) = \frac{1414}{144 \Delta t^4} \sigma_{\epsilon}^2 \implies \frac{\sigma_{\hat{\ddot{\epsilon}}}}{\sigma_{\epsilon}} \approx \frac{3.1336}{\Delta t^2}
$$

At $\Delta t = 0.01\text{ s}$, noise is amplified by $31,336\times$, causing naive stencils to collapse below $60\text{--}70\text{ dB}$ SNR. 

Replacing naive stencils with local quartic Savitzky-Golay projections ($W = 18, p = 4$) reduces noise variance by four orders of magnitude and extends exact symbolic support recovery down to **$20\text{ dB}$** ($10,000\times$ higher noise power tolerance):

* **van der Pol at $30\text{ dB}$**: Recovers exact support $\ddot{x} = -0.9961x + 1.0514\dot{x} - 1.0528x^2\dot{x}$ ($\mathrm{NRMSE} = 4.30 \times 10^{-2}$).
* **van der Pol at $20\text{ dB}$**: Recovers exact support $\ddot{x} = -1.3102x + 1.2697\dot{x} - 1.1637x^2\dot{x}$ ($\mathrm{NRMSE} = 1.51 \times 10^{-1}$).
* **Duffing at $40\text{ dB}$**: Recovers exact support $\ddot{x} = 0.9793x - 0.1660\dot{x} - 0.9896x^3$ ($\mathrm{NRMSE} = 1.08 \times 10^{-2}$).

---

## Dataset Directory Structure

All empirical telemetry datasets and parameter sweep logs are hosted under [`data/`](data/):

```
data/
├── telemetry/
│   ├── systems_benchmark_results.csv     # Full metrics for Table 1
│   ├── sys01_pendulum_telemetry.csv      # Raw (t, x) sensor stream (1200 pts)
│   ├── sys02_duffing_telemetry.csv       # Raw (t, x) sensor stream (1200 pts)
│   ├── sys03_vanderpol_telemetry.csv     # Raw (t, x) sensor stream (1200 pts)
│   ├── sys04_drag_telemetry.csv          # Raw (t, x) sensor stream (1200 pts)
│   └── sys05_anharmonic_telemetry.csv    # Raw (t, x) sensor stream (1200 pts)
├── sampling_ablation/
│   └── sampling_sweep_results.csv        # Metrics across dt in [0.002s, 0.100s]
└── noise_ablation/
    └── noise_sweep_results.csv           # Naive vs Savitzky-Golay from inf to 20 dB
```

---

## Citation

```bibtex
@article{barmase2026nonelementary,
  author    = {Shubham Barmase},
  title     = {Data-Driven Identification of Non-Elementary Dynamical Systems from Blind Sensor Telemetry},
  journal   = {GitHub Archive},
  year      = {2026},
  url       = {https://github.com/shu195/non-elementary-discovery-paper}
}
```

---

## License

This research paper and associated empirical datasets are published under the [MIT License](LICENSE).
