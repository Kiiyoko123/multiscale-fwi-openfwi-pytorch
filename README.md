# Multiscale Full Waveform Inversion in PyTorch (OpenFWI Benchmark)

Undergraduate thesis code, Department of Geophysical Engineering, Institut Teknologi Sepuluh Nopember (ITS): *Seismic Inversion Based on Multiscale Full Waveform Inversion: Quantitative Analysis of P-Wave Velocity Estimation Using OpenFWI Benchmark Dataset.*

## What this does

A differentiable 2D acoustic FWI workflow in which gradients come from PyTorch automatic differentiation instead of a hand-derived adjoint-state gradient.

- **Forward modeling:** finite-difference time-domain (FDTD) solution of the acoustic wave equation
- **Boundaries:** perfectly matched layer (PML) absorbing boundaries
- **Multiscale strategy:** inversion proceeds through successive frequency bands, with a Butterworth filter applied per frequency band
- **Baseline:** conventional single-band FWI run for 300 iterations, used as the reference for comparison

## Datasets

Five OpenFWI velocity models of increasing structural complexity: FlatVel-A, CurveVel-A, FlatFault-A, CurveFault-A, and Style-A. Each is tested with two starting conditions, a **near** initial model (close to the true model) and a **far** initial model (poor starting guess), to examine how initial-model quality affects the benefit of the multiscale strategy.

Reference for the benchmark: Deng et al. (2022), *OpenFWI: Large-scale Multi-structural Benchmark Datasets for Full Waveform Inversion*. The data are not included in this repository; download them from the OpenFWI project page.

## Key finding

Multiscale FWI gave only limited improvement over conventional FWI when the initial model was already close to the true model. This agrees with the cycle-skipping argument behind multiscale strategies (Bunks et al., 1995): low-to-high frequency progression matters most when the starting model is poor, and adds little when it is already good. The quantitative comparison (P-wave velocity estimation error per dataset, near vs. far initial model, multiscale vs. conventional) is reported in the thesis.

## Reproducing

1. **Environment:** Python 3 with PyTorch, NumPy, SciPy, and Matplotlib. A CUDA-capable GPU is recommended, since forward modeling and backpropagation through the FDTD time loop are computationally heavy.
2. **Data:** download the OpenFWI datasets listed above and place the files where the notebook's data-loading cell expects them (adjust the path variable at the top of the notebook if needed).
3. **Run:** open `multiscale_FWI.ipynb` and run all cells. Select the dataset and the initial model type (near or far) in the configuration cell. The notebook runs the multiscale inversion and the 300-iteration conventional baseline for comparison.

## Development notes

Problems encountered while building the workflow:

- Adaptive stopping thresholds caused over-smoothing of the inverted model
- Padding artifacts appeared in the smoothing step
- Gradient failures occurred in runs with the far initial model

## Limitations

- 2D acoustic modeling with constant density
- Synthetic data only; not validated on field data
- Benchmarked on OpenFWI velocity models, which are simpler than real subsurface structure
- Only P-wave velocity is estimated

## Reference

- Bunks, C., Saleck, F. M., Zaleski, S., and Chavent, G. (1995). Multiscale seismic waveform inversion. *Geophysics*, 60(5), 1457-1473.
- Deng, C., Feng, S., Wang, H., Zhang, X., Jin, P., Feng, Y., Zeng, Q., Chen, Y., and Lin, Y. (2022). OpenFWI: Large-scale multi-structural benchmark datasets for full waveform inversion. *Advances in Neural Information Processing Systems (NeurIPS), Datasets and Benchmarks Track*.

## Author

Mirza Amanda Azzahra. Contact: mirzaamanda1102@gmail.com
