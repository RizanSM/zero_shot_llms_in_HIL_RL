# Zero-Shot LLMs in Human-in-the-Loop RL: Replacing Human Feedback for Reward Shaping

This repository accompanies our research on enhancing Human-in-the-Loop Reinforcement Learning (HITL-RL) by leveraging zero-shot Large Language Models (LLMs) for reward shaping in continuous control tasks. Our key contributions include: <br>
a. Demonstrating that off-the-shelf LLMs can replace biased or inconsistent human feedback in RL without task-specific fine-tuning. <br>
b. Proposing LLM-HFBF, a hybrid framework that flags and corrects biases in human feedback using LLMs.<br>
c. Achieving parity between zero-shot LLM feedback and unbiased human feedback in performance metrics, even under edge-case scenarios.<br>
d. Validating our approach in the MuJoCo-based Highway environment, showing improved scalability, robustness, and bias mitigation in RL training.<br>

The project introduces a novel way to enhance learning efficiency and reliability in RL agents by combining the strengths of LLMs and human oversight, while minimizing the pitfalls of human bias.
