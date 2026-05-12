# Decision-Focused-Learning-for-Truck-Drone-Routing
We've developed a black-box solver to potentially solve all truck-drone routing problem variants with various objectives, which mainly utilizes LKH-3. Then, to consider the uncertain truck/drone travel times, we want to make best decisions before true uncertain parameters are realized. Hence, this project applies Decision-Focused Learning (DFL) methods to solve the simplest truck-drone routing (Flying Sidekick TSP, FSTSP with one truck and one drone). 

Oracle method: current solver

Baseline method (PtO): Decision-blind Predict-then-Optimize makes the best predicion on travel times, then call the solver

Six DFL methods: 
(1) BlackBox Backprop (DBB). Vlastelica, M., Paulus, A., Musil, V., Martius, G., & Rolínek, M. (2019). Differentiation of blackbox combinatorial solvers.
(2) Randomized Smoothing (RS). Dalle, G., Baty, L., Bouvier, L., & Parmentier, A. (2022). Learning with combinatorial optimization layers: a probabilistic approach.
(3) Perturbation Gradient Backward loss (PGB). Gupta, V., & Huang, M. (2024). Decision-focused learning with directional gradients.
(4) Perturbation Gradient Center loss (PGC). Gupta, V., & Huang, M. (2024). Decision-focused learning with directional gradients.
(5) Smart Predict-then-Optimize (SPO+). Elmachtoub, A. N., & Grigas, P. (2022). Smart “predict, then optimize”.
(6) Perturbed Fenchel-Young Loss (PFYL). Berthet, Q., Blondel, M., Teboul, O., Cuturi, M., Vert, J. P., & Bach, F. (2020). Learning with differentiable pertubed optimizers.



# Decision-Focused Learning for Flying Sidekick TSP (FSTSP)

## Overview
This project applies Decision-Focused Learning (DFL) to the Flying Sidekick Traveling Salesman Problem (FSTSP), where a truck and drone coordinate to serve customers under uncertain travel times. Rather than minimizing prediction error (Predict-then-Optimize), DFL methods directly minimize downstream routing cost.

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
