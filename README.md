[![arXiv](https://img.shields.io/badge/arXiv-1234.56789-b31b1b.svg)](https://arxiv.org/abs/2503.22723)

# Zero-Shot LLMs in Human-in-the-Loop RL: Replacing Human Feedback for Reward Shaping

This repository accompanies our research on enhancing Human-in-the-Loop Reinforcement Learning (HITL-RL) by leveraging zero-shot Large Language Models (LLMs) for reward shaping in continuous control tasks. Our key contributions include: <br>
a. Demonstrating that off-the-shelf LLMs can replace biased or inconsistent human feedback in RL without task-specific fine-tuning. <br>
b. Proposing LLM-HFBF, a hybrid framework that flags and corrects biases in human feedback using LLMs.<br>
c. Achieving parity between zero-shot LLM feedback and unbiased human feedback in performance metrics, even under edge-case scenarios.<br>
d. Validating our approach in the MuJoCo-based Highway environment, showing improved scalability, robustness, and bias mitigation in RL training.<br>

The project introduces a novel way to enhance learning efficiency and reliability in RL agents by combining the strengths of LLMs and human oversight, while minimizing the pitfalls of human bias.

_______________________________________________________________________________________________________________________________________________________________________________________________________________________

Environments: Highway & Reacher · Scenarios: Default + Edge Cases · Runner: Google Colab

This repo provides Colab-first notebooks to (1) generate trajectories, (2) train PPO policies under different feedback regimes (Human Direct, Learned Reward Shaping, LLM Direct, LLM Bias-Flagging), and (3) evaluate on default and edge-case scenarios.

_____________________________________________________________________________________________________________________________________________________________________________________________________________________

## Table of contents

1. Overview
2. Repo layout (abridged)
3. Quick start (Colab)
4. Path setup tip
5. Run paths — Highway
   A) Generate trajectories
   B) Default environment
   C) Edge case scenario 1
   D) Edge case scenario 2
6. Run paths — Reacher
7. Bias value guide (Highway)
8. Outputs & metrics
9. Troubleshooting
10. Acknowledgements, License, Citation
11. Run checklist

_____________________________________________________________________________________________________________________________________________________________________________________________________________________

## Overview

### Feedback regimes
1. HF_DIRECT — Direct human-feedback-style reward recalibration
2. HF_RSM — Reward Shaping via a learned model trained on HF_DIRECT outputs
3. *BIASED_HF_ ** — Aggressive (AGG) and Reckless Adaptive (RAD) human-feedback variants
4. LLM_DIRECT — Reward shaping directly from zero-shot LLM feedback
5. LLM_HF_BF — LLM-based bias flagging over biased feedback trajectories

All notebooks include an Open in Colab badge. For LLM notebooks (LLM_DIRECT, LLM_HF_BF), select T4 GPU in Colab.
