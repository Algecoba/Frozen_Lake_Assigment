# Q-Learning in FrozenLake 8×8: Experimental Analysis under Stochastic Dynamics

This repository presents an experimental study of **Q-learning** applied to the **FrozenLake 8×8** environment from **Gymnasium**, focusing on how hyperparameter tuning and reward shaping affect learning dynamics, convergence, stability, and policy quality in a stochastic setting.

The project follows a **factorial Design of Experiments (DOE)** approach and is intended both as a reproducible research artifact and as an educational resource for reinforcement learning in discrete Markov Decision Processes (MDPs).


## Problem Description

Many engineering and robotics problems require agents to learn optimal decision-making policies without access to an explicit model of the environment. This challenge becomes more complex in **stochastic and discrete environments**, where uncertainty in state transitions limits deterministic planning.

FrozenLake is a classical benchmark environment that captures these challenges. In its **8×8 configuration**, the state–action space grows significantly compared to the 4×4 map, making learning slower and more sensitive to hyperparameter selection. Additionally, the *slippery* dynamics introduce stochastic transitions that emulate uncertainty found in real physical systems.


## Objectives

The main objectives of this project are:

- To analyze the impact of the **learning rate (α)** and **discount factor (γ)** on:
  - Learning speed
  - Stability
  - Final policy performance
- To evaluate whether **reward penalization** (negative reward when falling into holes) improves convergence and robustness.
- To identify an **optimal configuration** for Q-learning in the FrozenLake 8×8 environment.
- To provide visual and quantitative evidence of learning through:
  - Learning curves
  - Stability metrics
  - Q-table heatmaps


## Experimental Design

A **full factorial grid search** was conducted using the following parameters:

### Hyperparameters
- **Learning rate (α):** {0.001, 0.01, 0.1, 0.5, 0.9}
- **Discount factor (γ):** {0.85, 0.9, 0.95, 0.98, 0.99}
- **Reward penalization:** {True, False}
- **Episodes:** 20,000 per configuration
- **Exploration strategy:** ε-greedy with linear decay

This results in **50 experimental configurations**, each evaluated using multiple performance metrics.


## Metrics Collected

For each configuration, the following metrics are computed:

- **Final success rate** (average over the last 100 episodes)
- **Average number of steps per episode**
- **Episode of first success / convergence**
- **Stability (standard deviation of success rate)**
- **Learning curves** (moving average over 500 episodes)
- **Q-table heatmaps** (maximum Q-value per state)

All results are stored in structured CSV format for reproducibility.


## Key Findings

- The **learning rate (α)** is the most influential hyperparameter.
  - Very small values (α = 0.001) lead to learning stagnation.
  - Large values (α ≥ 0.5) cause instability.
  - **α ≈ 0.1** provides the best balance between speed and stability.
- High discount factors (**γ ≥ 0.95**) are essential for long-horizon planning in sparse-reward environments.
- Reward penalization does not always maximize peak success but acts as a **regularizer**, improving robustness and reducing variance.
- Policies with higher success rates tend to use **longer, safer trajectories**, highlighting a trade-off between efficiency and risk avoidance.
- Due to stochastic transitions (*slippery*), achieving 100% success is theoretically unattainable, even with optimal policies.


## Recommended Configuration (FrozenLake 8×8)

| Parameter | Value |
|---------|------|
| Penalization | True |
| Learning rate (α) | 0.1 |
| Discount factor (γ) | 0.95 |
| Training episodes | ≥ 20,000 |
| Exploration | ε-greedy with slow decay |

This configuration achieves the best compromise between **success rate, stability, and policy coherence**.
