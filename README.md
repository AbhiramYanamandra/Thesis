# Photonic Error Correction for High-Precision Computing

This repository contains the simulation and network-evaluation notebooks used in my honours thesis on correction methods for decomposition-based photonic multiplication.

The component simulator evaluates six implementations. At network level, these reduce to four distinct numerical behaviours under the ideal-table model because the direct 4-bit corner LUT and parallel corner correction are numerically equivalent, while the two low-bit implementations use the same congruent decoder output.

## Repository contents

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   ├── 01_component_simulator.ipynb
│   ├── 02_imagenet_inference.ipynb
│   └── 03_noise_aware_training.ipynb
└── results/
    ├── imagenet_full50k_results.json
    └── README.md
```

### `notebooks/01_component_simulator.ipynb`

Component-level evaluation of the six correction schemes. It includes the canonical FP16 and FP32 experiments, lookup-table checks, the low-bit congruent decoder, the decomposed residue calculation and the analytical photonic sub-product cost model.

### `notebooks/02_imagenet_inference.ipynb`

Full ImageNet-1k inference sweep using pretrained AlexNet, ResNet18 and VGG19. The notebook characterises each multiplier error model with 40,000 normalised mantissa pairs and uses the resulting first two error moments in the network-level layer model.

The final sweep uses the full 50,000-image ImageNet-1k validation set, five noise levels and three independent network-noise draws per point.

The notebook also contains two historical resume helpers. During the original experiment, execution was stopped after AlexNet and ResNet18 had completed and the saved checkpoint was later used to finish VGG19. A second results-only reload cell was added after the runtime was restarted again before the final plots were produced. These cells are retained and documented in the notebook, but are disabled or harmless during a normal fresh run.

### `notebooks/03_noise_aware_training.ipynb`

Noise-aware training experiment on CIFAR-100 using ResNet18. The reported experiment trains for 12 epochs with three seeds and evaluates four noise levels.

The notebook compares clean training and clean evaluation, clean training with noisy inference, and noise-aware training with noisy inference. The failed low-bit splice rule is retained as a control. The low-bit decomposed implementation is not trained separately because it is numerically identical to the low-bit congruent implementation under the ideal-table model.

## Results

`results/imagenet_full50k_results.json` contains the final raw per-run Top-1 accuracies from the ImageNet-1k sweep. The file contains the three clean references and 180 noisy evaluations used to produce the reported means and noise-tolerance thresholds.

A separate raw checkpoint for the CIFAR-100 noise-aware training experiment is not included. The completed training results are retained in the output cells of `03_noise_aware_training.ipynb`.

See `results/README.md` for the result-file format.

## Running the notebooks

Install the Python dependencies with:

```bash
pip install -r requirements.txt
```

The notebooks were developed primarily in Google Colab.

The component simulator does not require an external dataset.

The ImageNet notebook requires access to the gated `ILSVRC/imagenet-1k` dataset on Hugging Face. The notebook expects a read token through Colab Secrets as `HF_TOKEN`, or prompts for one securely if it is not available. Tokens and downloaded ImageNet data should not be committed to the repository.

The noise-aware training notebook uses CIFAR-100 through torchvision and can download the dataset when it is not already cached.

The notebooks do not need to be run in sequence. The ImageNet and noise-aware-training experiments are computationally expensive and checkpoint completed work so interrupted runs can be resumed.

## Reproducibility notes

All proposed correction results in this repository are simulation results. The photonic multiplier noise parameters are based on the LightMat-HP characterisation used in the thesis.

Final correction accuracy uses ideal exact lookup-table contents. The component simulator includes a separate calibration-realism check, but physical calibration drift and device variation were not measured in hardware.

The historical Stage A characterisation in the network notebooks used a scheme-specific seed offset derived from Python's built-in `hash()` function. Python string hashes may differ between interpreter processes. A fresh process is therefore not guaranteed to draw the exact same 40,000 characterisation samples from the base seed alone. The completed ImageNet JSON and the retained noise-aware-training notebook outputs are the reference results used in the thesis. A future rerun should replace the built-in hash with a fixed per-scheme seed map.

## Thesis

This repository accompanies the honours thesis **High-Precision Photonic Computing for Accelerating Scientific Computing Workloads**. The report contains the full methodology, assumptions, evaluation and discussion of the correction schemes.
