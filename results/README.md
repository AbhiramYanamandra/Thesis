# Results

This directory contains the saved numerical outputs that are distributed with the repository.

## `imagenet_full50k_results.json`

This file is the final raw checkpoint from the full ImageNet-1k inference sweep.

It contains:

- 3 clean digital reference evaluations
- 3 models: AlexNet, ResNet18 and VGG19
- 4 network-level numerical behaviours
- 5 photonic noise levels
- 3 independent network-noise draws per noisy point
- 183 saved evaluations in total

The four noisy behaviours are:

- `base` — uncorrected 4-bit decomposition
- `nonuni` — non-uniform slicing with exact 2-bit corner lookup
- `parallel` — parallel MSB-corner correction
- `lowbit` — low-bit decomposed correction

Under the ideal-table model, the direct 4-bit corner LUT has the same numerical output as `parallel`, and the direct low-bit congruent implementation has the same numerical output as `lowbit`. They are therefore not repeated as separate network-level sweeps.

Noisy result keys use the form:

```text
model|scheme|precision|sigma|seed
```

The values are raw Top-1 accuracies in percent. Means, accuracy gaps and threshold extensions reported in the thesis are derived from these per-run values.

The repository name `imagenet_full50k_results.json` is a shorter name for the final checkpoint produced by the ImageNet notebook.

## Noise-aware training results

A separate raw `noise_aware_training_results.json` checkpoint is not included in this repository.

The completed CIFAR-100 noise-aware training results are retained in the output cells of:

```text
notebooks/03_noise_aware_training.ipynb
```

Those retained outputs are the results reported in the thesis. No replacement JSON has been reconstructed from the notebook output, so there is no ambiguity between an original checkpoint and a derived file.
