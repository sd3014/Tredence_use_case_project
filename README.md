# 🧠 Self-Pruning Neural Network (CIFAR-10)
📌 Overview

This project implements a self-pruning neural network that dynamically learns which weights are important during training and removes unnecessary connections using learnable gates.

Unlike traditional pruning (post-training), this model integrates pruning directly into the learning process using L1 sparsity regularization on gates.

## Architecture
Custom PrunableLinear layer
Multi-layer feedforward network:
Input: 32×32×3 (CIFAR-10)
Hidden Layers: 256 → 128
Output: 10 classes

## Methodology
1. Gated Weights

Each layer learns:

weight
gate_scores

Gates computed as:

gates = sigmoid(20 * (gate_scores - 0.5))
2. Loss Function
Total Loss = CrossEntropyLoss + λ × SparsityLoss

Where:

SparsityLoss = mean of all gate values
λ controls pruning strength
3. Training
Dataset: CIFAR-10
Optimizer: Adam
Epochs: 10
Different λ values tested

## Results
<img width="971" height="291" alt="Screenshot 2026-04-21 095448" src="https://github.com/user-attachments/assets/cc52da2f-0d8f-4c2a-b661-43bdbe56f2a9" />

## Observations
Increasing λ increases sparsity significantly
Higher sparsity reduces average gate values
The model maintains stable accuracy even at high sparsity
Clear trade-off between compression and performance

## Key Insights
The network successfully learns to prune itself
Gates act as importance indicators for weights
L1 regularization encourages sparsity effectively
Higher λ leads to more aggressive pruning

## Visuallisation
<img width="757" height="562" alt="Screenshot 2026-04-21 095739" src="https://github.com/user-attachments/assets/764c5227-1fcb-49de-bc5b-82deb0f0870f" />
The training loss decreases steadily across epochs, indicating stable learning and effective optimization. The smooth downward trend suggests the model is successfully minimizing both classification and sparsity objectives without instability, demonstrating proper convergence behavior and balanced training between accuracy and pruning regularization.
<img width="724" height="523" alt="Screenshot 2026-04-21 095755" src="https://github.com/user-attachments/assets/a3bfba0f-6181-4dc4-a4bc-f76345062fc7" />
Accuracy slightly improves as λ increases, indicating that moderate sparsity regularization does not harm performance. Instead, pruning helps eliminate redundant weights, leading to better generalization. This highlights that controlled pruning can enhance efficiency while maintaining or even improving model accuracy.
<img width="809" height="524" alt="Screenshot 2026-04-21 095807" src="https://github.com/user-attachments/assets/9dd0a681-fe0d-4bb9-ab6c-48bc18380ceb" />
The gate distribution shows a strong concentration near zero, indicating many weights are effectively pruned. A secondary cluster at higher values represents important connections retained by the model. This bimodal pattern confirms successful self-pruning and clear separation between useful and redundant weights.

