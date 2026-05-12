# Decision-Focused-Learning-for-Truck-Drone-Routing

## Overview
We employ a unified three-phase black-box solver capable of handling all truck-drone routing variants, built on top of LKH-3. To handle uncertain truck and drone travel times, this project applies Decision-Focused Learning (DFL) to the simplest truck-drone routing variant (the Flying Sidekick TSP (FSTSP) involving one truck and one drone), making routing decisions before true travel times are realized.

## Methods
- **Oracle**: current black-box solver with true travel times
- **PtO (baseline)**: Decision-blind Predict-then-Optimize predicts travel times via MSE loss, then calls the solver
- **Six DFL methods**:
  1. **DBB** — Vlastelica et al. (2019). Differentiation of blackbox combinatorial solvers.
  2. **RS** — Dalle et al. (2022). Learning with combinatorial optimization layers: a probabilistic approach.
  3. **PGB** — Gupta & Huang (2024). Decision-focused learning with directional gradients.
  4. **PGC** — Gupta & Huang (2024). Decision-focused learning with directional gradients.
  5. **SPO+** — Elmachtoub & Grigas (2022). Smart "predict, then optimize".
  6. **PFYL** — Berthet et al. (2020). Learning with differentiable perturbed optimizers.

## Repository Contents
- `Amazon_Instance_Preparation.ipynb` — generates FSTSP instances from the Amazon Last-Mile Routing dataset
- `train.py` — main training script for all seven methods (PtO + 6 DFL)

## Methods Implemented
| Method | Reference |
|--------|-----------|
| PtO (baseline) | — |
| DBB | Vlastelica et al. (2019) |
| RS  | Dalle et al. (2022) |
| PGB | Gupta & Huang (2024) |
| PGC | Gupta & Huang (2024) |
| SPO+ | Elmachtoub & Grigas (2022) |
| PFYL | Berthet et al. (2020) |

## Requirements
```bash
conda create -n fstsp python=3.10
conda activate fstsp
pip install torch numpy pandas scikit-learn
```
LKH-3 binary is required for the FSTSP solver.

## Usage

### Step 1: Generate instances
Run `Amazon_Instance_Preparation.ipynb` using the Amazon Last-Mile Routing dataset \citep{merchan2022amazon}.

### Step 2: Train
```bash
# PtO baseline
python train.py --method PtO --epochs 100 --lr 5e-4 --subset 1000

# DFL methods (example: RS with best hyperparameter)
python train.py --method RS --sigma 0.1 --epochs 20 --lr 5e-4 --subset 1000 --output_dir saved_models
```

## Results
All six DFL methods outperform PtO. RS achieves 4.40% normalized regret vs 17.37% for PtO on 1,000 training instances.

| Method | Hyperparam | Test Regret |
|--------|------------|-------------|
| PtO    | —          | 17.37%      |
| DBB    | λ=0.5      | 8.33%       |
| RS     | σ=0.1      | **4.40%**   |
| PGB    | h=0.01     | 6.33%       |
| PGC    | h=0.01     | 5.68%       |
| SPO+   | —          | 5.90%       |
| PFYL   | σ=0.1      | 9.77%       |
