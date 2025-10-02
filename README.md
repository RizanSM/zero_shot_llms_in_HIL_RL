[![arXiv](https://img.shields.io/badge/arXiv-1234.56789-b31b1b.svg)](https://arxiv.org/abs/2503.22723)

# Zero-Shot LLMs in Human-in-the-Loop RL: Replacing Human Feedback for Reward Shaping

This repository accompanies our research on enhancing Human-in-the-Loop Reinforcement Learning (HITL-RL) by leveraging zero-shot Large Language Models (LLMs) for reward shaping in continuous control tasks. Our key contributions include: <br>
a. Demonstrating that off-the-shelf LLMs can replace biased or inconsistent human feedback in RL without task-specific fine-tuning. <br>
b. Proposing LLM-HFBF, a hybrid framework that flags and corrects biases in human feedback using LLMs.<br>
c. Achieving parity between zero-shot LLM feedback and unbiased human feedback in performance metrics, even under edge-case scenarios.<br>
d. Validating our approach in the MuJoCo-based Highway environment, showing improved scalability, robustness, and bias mitigation in RL training.<br>

The project introduces a novel way to enhance learning efficiency and reliability in RL agents by combining the strengths of LLMs and human oversight, while minimizing the pitfalls of human bias.

____________________________________________________________________________________________________________________________________________________________________________________________________________________

Environments: Highway & Reacher · Scenarios: Default + Edge Cases · Runner: Google Colab

This repo provides Colab-first notebooks to (1) generate trajectories, (2) train PPO policies under different feedback regimes (Human Direct, Learned Reward Shaping, LLM Direct, LLM Bias-Flagging), and (3) evaluate on default and edge-case scenarios.

____________________________________________________________________________________________________________________________________________________________________________________________________________________

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
10. Acknowledgements, Citation
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

____________________________________________________________________________________________________________________________________________________________________________________________________________________

## Repo layout (abridged)


├── 01_Highway_Env <br />
│   ├── 01_Generate_Trajectories  <br>
│   │     ├── 01_Generate_Trajectories_Highway.ipynb <br />
│   │     ├── 02_Generate_Trajectories_for_Bias_Flagging_Highway.ipynb <br />
│   │     └── Readme.md <br />
│   ├── 02_Default_Environment  <br />
│   │     ├── 01_HF_DIRECT/ ... (train → gen tests → evaluate)  <br />
│   │     ├── 02_HF_RSM/    ... (train → gen tests → evaluate)  <br />
│   │     ├── 03_BIASED_HF_DIRECT_AGG/      ...    <br />
│   │     ├── 04_BIASED_HF_RSM_AGG/         ...    <br />
│   │     ├── 05_BIASED_HF_DIRECT_RAD/      ...    <br />
│   │     ├── 06_BIASED_HF_RSM_RAD/         ...    <br />
│   │     ├── 07_LLM_DIRECT/                ...    <br />
│   │     └── 08_LLM_HF_BF/                 ...    <br />
│   ├── 03_Edge_Case_Scenario_1/ (HF_DIRECT, BIASED_AGG, BIASED_RAD, LLM_DIRECT)  <br />
│   └── 04_Edge_Case_Scenario_2/ (HF_DIRECT, BIASED_AGG, BIASED_RAD, LLM_DIRECT)  <br />
├── 02_Reacher_Env  <br />
│   ├── 01_Generate_Trajectories/01_Generating_Reacher_Trajectories.ipynb   <br />
│   ├── 02_Default_Environment  <br />
│   │      ├── 01_HF_DIRECT_REACHER/  ... (train → gen tests → evaluate)   <br />
│   │      ├── 02_HF_D_REACHER_AGG/   ...    <br />
│   │      ├── 03_HF_D_REACHER_RAD/   ...    <br />    
│   │      ├── 04_LLM_DIRECT_REACHER/ ...    <br />
│   │      ├── 05_LLM_HF_BF_REACHER/  ...    <br />
|   └── Readme.md   <br />
└── README.md   <br />

Each folder contains a concise Readme.txt listing what to change before running (search for Update directory location X in each notebook).

____________________________________________________________________________________________________________________________________________________________________________________________________________________

## Quick start (Colab)

1. Open 01_highway_env/01_generate_trajectories/01_generate_trajectories_highway.ipynb in Colab.
2. (Optional but recommended) Runtime → Change runtime type → T4 GPU.
3. Run the install & Google Drive mount cells.
4. Update the five paths marked # Update directory location 1–5 (logs, model save/load, CSV, PKL).
5. Run all cells → produces a baseline PPO and initial trajectory DataFrame(s).
6. Proceed to any training pipeline (e.g., HF_DIRECT) → generate test trajectories (5 seeds) → evaluate.

____________________________________________________________________________________________________________________________________________________________________________________________________________________

## Path setup tip

To avoid editing many lines, define a base block up top and derive paths:

BASE_DIR = "/content/drive/MyDrive/data_rp1"
LOGS    = f"{BASE_DIR}/0_log_dir"
MODELS  = f"{BASE_DIR}/1_trained_models"
TRAJ    = f"{BASE_DIR}/2_trajectories"

Then set each Update directory location X to a path built from these constants.
____________________________________________________________________________________________________________________________________________________________________________________________________________________

## Run paths — Highway

### A) Generate trajectories

#### 1) Initial trajectories
Notebook: 01_Highway_Env/01_Generate_Trajectories/01_Generate_Trajectories_Highway.ipynb
Update before running:
1. Save training logs
2. Save PPO model (initial) {0_PPO_HIGHWAY_INITIAL_TRAINING}
3. Load PPO model (initial) {0_PPO_HIGHWAY_INITIAL_TRAINING}
4. Save initial trajectory dataframe (CSV) {0_INITIAL_TRAJECTORY_HIGHWAY_DF.csv}
5. Save initial trajectory dataframe (PKL) {0_INITIAL_TRAJECTORY_HIGHWAY_DF.pkl}

#### 2) Biased trajectories for bias-flagging
Notebook: 01_Highway_env/01_Generate_Trajectories/02_Generate_Trajectories_for_Bias_Flagging_Highway.ipynb

Update:
1. Load trajectory_df.pkl from step A-1
2. Save biased trajectory dataframe (PKL) [1_BIASED_HF_TRAJECTORY_DF_BIAS_FLAGGING.pkl] for later LLM bias-flagging

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### B) Default environment
#### 1) HF_DIRECT — Ideal
Folder: 01_highway_env/02_default_env/01_HF_DIRECT/

Train — 01_HF_DIRECT_Policy_Training.ipynb
Update:
(1) Load initial {0_INITIAL_TRAJECTORY_HIGHWAY_DF.pkl} 
(2) Save Ideal feedback data frame {1_HF_DIRECT_HIGHWAY_DF.pkl}
(3) Save PPO training logs
(4) Save PPO model {1_PPO_HIGHWAY_HF_DIRECT.zip}

Generate tests — 02_Generate_Trajectories_HF_D_default_highway_env.ipynb
Update:
(1) Load model {1_PPO_HIGHWAY_HF_DIRECT}
(2–6) Save five test DFs: 1..5_HF_DIRECT_HIGHWAY_DF.pkl

Evaluate — 03_Model_Testing_HF_D_Default_highway_env.ipynb
Update: (1–5) Load the five test DFs

#### 2) HF_RSM — Learned Reward Shaping Model (Ideal)
Folder: 01_Highway_Env/02_Default_Environment/02_HF_RSM/

Train — 01_HF_RSM_Policy_training.ipynb
Update:
(1) Load {1_HF_DIRECT_HIGHWAY_DF.pkl}
(2) Save PPO logs
(3) Save PPO model {2_PPO_HIGHWAY_HF_RSM}

Generate tests — 02_Generate_trajectories_HF_RSM_default_highway_env.ipynb
Update:
(1) Load model {2_PPO_HIGHWAY_HF_RSM}
(2–6) Save five test DFs: 1..5_HF_RSM_HIGHWAY_DF.pkl

Evaluate — 03_Model_testing_HF_RSM_Default_highway_env.ipynb
Update:
(1–5) Load the five test DFs

#### 3) BIASED_HF_D_AGG — Aggressive (Direct feedback)
Folder: 01_Highway_Env/02_Default_Environment/03_BIASED_HF_DIRECT_AGG/

Train — 01_Biased_HF_D_AGG_Policy_training.ipynb
Update:
(1) Load {0_INITIAL_TRAJECTORY_HIGHWAY_DF.pkl} 
(2) Save Biased feeback data frame {2_BIASED_HF_DIRECT_AGG_HIGHWAY_DF.pkl}
(3) Save PPO logs
(4) Save PPO model {3_PPO_HIGHWAY_BIASED_HF_DIRECT_AGG}

Generate tests — 02_Generate_trajectories_biased_HF_D_AGG_default_highway_env.ipynb
Update:
(1) Load model {3_PPO_HIGHWAY_BIASED_HF_DIRECT_AGG}
(2–6) Save five test DFs: 1..5_BIASED_HF_DIRECT_HIGHWAY_AGG_DF.pkl

Evaluate — 03_Model_testing_Biased_HF_D_AGG_Default_highway_env.ipynb
Update:
(1–5) Load the five test DFs

#### 4) BIASED_HF_RSM_AGG — Aggressive (Learned)
Folder: 01_Highway_Env/02_Default_Environment/04_BIASED_HF_RSM_AGG/

Train — 01_Biased_HF_RSM_Policy_training_AGG.ipynb
Update:
(1) Load {2_BIASED_HF_DIRECT_AGG_HIGHWAY_DF.pkl}
(2) Save PPO logs
(3) Save PPO model {4_PPO_HIGHWAY_BIASED_HF_RSM_AGG}

Generate tests — 02_Generate_trajectories_Biased_HF_RSM_AGG_default_highway_
Update:

(1) Load model {4_PPO_HIGHWAY_BIASED_HF_RSM_AGG}
(2–6) Save five test DFs: 1..5_BIASED_HF_RSM_HIGHWAY_AGG_DF.pkl

Evaluate — 03_Model_testing_Biased_HF_RSM_AGG_Default_highway_env.ipynb
Update:
(1–5) Load the five test DFs

#### 5) BIASED_HF_D_RAD — Reckless Adaptive (Direct feedback)
Folder: 01_Highway_Env/02_Default_Environment/05_BIASED_HF_DIRECT_RAD/

Train — 01_Biased_HF_D_RAD_Policy_training.ipynb
Update:
(1) Load {0_INITIAL_TRAJECTORY_HIGHWAY_DF.pkl} 
(2) Save {3_BIASED_HF_DIRECT_RAD_HIGHWAY_DF.pkl}
(3) Save PPO logs
(4) Save PPO model {5_PPO_HIGHWAY_BIASED_HF_DIRECT_RAD}

Generate tests — 02_Generate_trajectories_Biased_HF_D_RAD_default_highway_env
Update:
(1) Load model {5_PPO_HIGHWAY_BIASED_HF_DIRECT_RAD}
(2–6) Save five test DFs: 1..5_BIASED_HF_DIRECT_HIGHWAY_RAD_DF.pkl

Evaluate — 03_Model_testing_Biased_HF_D_RAD_Default_highway_env.ipynb
Update:
(1–5) Load the five test DFs

#### 6) BIASED_HF_RSM_RAD — Reckless Adaptive (Learned)
Folder: 01_Highway_Env/02_Default_Environment/06_BIASED_HF_RSM_RAD/

Train — 01_Biased_HF_RSM_Policy_training_RAD.ipynb
Update:
(1) Load {3_BIASED_HF_DIRECT_RAD_HIGHWAY_DF.pkl}
(2) Save PPO logs
(3) Save PPO model {6_PPO_HIGHWAY_BIASED_HF_RSM_RAD}

Generate tests — 02_Generate_trajectories_Biased_HF_RSM_RAD_default_highway_env.ipynb
Update:
(1) Load model 6_PPO_HIGHWAY_BIASED_HF_RSM_RAD
(2–6) Save five test DFs: 1..5_BIASED_HF_RSM_HIGHWAY_RAD_DF.pkl

Evaluate — 03_Model_testing_Biased_HF_RSM_RAD_Default_highway_env.ipynb
Update:
(1–5) Load the five test DFs

#### 7) LLM_DIRECT
Folder: 01_Highway_Env/02_Default_Environment/07_LLM_DIRECT/
Colab: select T4 GPU.

Train — 01_LLM_D.ipynb
Update:
(1) Load 0_initial_trajectory_df.pkl
(2) Save LLM recalibrated rewards DF 1_llm_feedback_ideal_df_with_sil_score.pkl
(3) Save PPO logs
(4) Save PPO model 7_PPO_HIGHWAY_LLM_DIRECT

Generate tests — 02_Generate_trajectories_LLM_D_default_highway_env.ipynb
Update:
(1) Load model 7_PPO_HIGHWAY_LLM_DIRECT
(2–6) Save five test DFs: 1..5_LLM_DIRECT_HIGHWAY_DF.pkl

Evaluate — 03_Model_testing_LLM_D_Default_highway_env.ipynb
Update:
(1–5) Load the five test DFs

#### 8) LLM_HF_BF — Bias flagging
Folder: 01_Highway_Env/02_Default_Environment/08_LLM_HF_BF/
Colab: select T4 GPU.

Policy Training — 01_LLM_HF_BF.ipynb
Update:
(1) Load biased Human Feedback Data Frame 1_BIASED_HF_TRAJECTORY_DF_BIAS_FLAGGING.pkl
(2) Save LLM bias-flagging Justification Data Frame 0_LLM_HF_BF_JUSTIFICATION.pkl
(3) Save LLM bias-flagging Data Frame 1_LLM_HF_BF_HIGHWAY_DF.pkl
(4) Save PPO logs
(5) Save PPO model 8_PPO_HIGHWAY_LLM_HF_BF

Generate tests — 02_Generate_trajectories_LLM_HF_BF_default_highway_env.ipynb
Update:
(1) Load model 8_PPO_HIGHWAY_LLM_HF_BF
(2–6) Save five test DFs: 1..5_LLM_HF_BF_DF.pkl

Evaluate — 03_Model_testing_LLM_HF_BF.ipynb
Update:
(1–5) Load the five test DFs

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### C) Edge case scenario 1

Scenario: Ego starts rightmost lane @ 25 m/s; rightmost lane congested (slower vehicles); adjacent lanes faster (≈25–30 m/s).

Folder: 01_Highway_Env/03_Edge_Case_Scenario_1/
For each policy (01_HF_DIRECT, 02_BIASED_HF_DIRECT_AGG, 03_BIASED_HF_DIRECT_RAD, 04_LLM_DIRECT):

Generate tests — 01_Generate_Trajectories_for_Model_Testing_*_Edge_Case_1.ipynb
Update:
(1) Load the corresponding PPO model from Default training (e.g., 1_PPO_HIGHWAY_HF_DIRECT)
(2–6) Save five test DFs: *_edge_1_df.pkl

Evaluate — 02_Model_Testing_*_Edge_Case_1.ipynb
Update:
(1–5) Load the five test DFs

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### D) Edge case scenario 2

Scenario: Ego starts middle lane (2) @ 25 m/s; middle lane congested; adjacent lanes faster (≈25–30 m/s).

Folder: 01_Highway_Env/04_Edge_Case_Scenario_2/
For each policy (01_HF_DIRECT, 02_BIASED_HF_DIRECT_AGG, 03_BIASED_HF_DIRECT_RAD, 04_LLM_DIRECT):

Generate tests — 01_Generate_Trajectories_for_Model_Testing_*_Edge_Case_2.ipynb
Update:
(1) Load the corresponding PPO model from Default training
(2–6) Save five test DFs: *_edge_2_df.pkl

Evaluate — 02_Model_Testing_*_Edge_Case_2.ipynb
Update: (1–5) Load the five test DFs
____________________________________________________________________________________________________________________________________________________________________________________________________________________

## Run paths — Reacher

Folder: 02_Reacher_Env/
The Reacher workflows mirror Highway:
Generate trajectories — 01_Generate_Trajectories/01_Generating_Reacher_Trajectories.ipynb
Update:
(1) Save training logs
(2) Save PPO model (initial)
(3) Load PPO model (initial)
(4) Save initial trajectory DF (CSV)
(5) Save initial trajectory DF (PKL)

Train / Generate tests / Evaluate for:
Default Environment testing : 02_Default_Environment/
01_HF_DIRECT_REACHER
02_HF_D_REACHER_AGG
03_HF_D_REACHER_RAD
04_LLM_DIRECT_REACHER (T4 GPU)
05_LLM_HF_BF_REACHER (T4 GPU)

Each subfolder contains:
01_Policy_Training_*.ipynb (update load/save paths, run to train PPO)
02_Generate_Trajectories_for_Model_Testing_*.ipynb (save 5 test DFs)
03_Model_Testing_*.ipynb (load those 5 DFs and compute metrics)

____________________________________________________________________________________________________________________________________________________________________________________________________________________

## Bias value guide (Highway)

Use these when setting/adjusting human feedback in HF notebooks:
A. Lane change feedback score
1: No lane change
2: One lane change
3: Two lane changes
4: Three lane changes

B. Collision avoidance feedback score
5: Potential risk — avoided by slowing down
6: Potential risk — avoided by speeding up
7: Potential risk — avoided by lane change
8: Immediate risk — emergency avoidance
9: Safe path

C. Speed optimization feedback score
10–12: Low traffic density (High / Moderate / Low speed optimization)
13–15: Moderate density (High / Moderate / Low)
16–18: High density (High / Moderate / Low)

Each policy’s Readme.txt lists exactly which paths to update (e.g., “Update directory location 1–4”). Use those as your checklist inside the notebook.

____________________________________________________________________________________________________________________________________________________________________________________________________________________

## Outputs & metrics

A. Training logs → .../0_log_dir/.../monitor.csv (TensorBoard / reward curves)
B. Models → .../1_trained_models/<model_name>
C. Trajectories → .../2_trajectories/.../*.pkl and *.csv
D. Evaluation metrics (from 03_Model_Testing_*.ipynb):
1. Final Misalignment(FMA)
2. Average Terminate Time(ATT)
3. Average Episodic Reward(AER)

____________________________________________________________________________________________________________________________________________________________________________________________________________________

## Troubleshooting

1. google.colab import errors (local runs): Remove from google.colab import ... and use local paths instead of Drive.
2. Highway env version mismatch: Notebooks use 'highway-v0'. If unavailable, upgrade highway-env or change to a supported spec in your install.
3. monitor.csv missing: Ensure Monitor(env, log_dir) is set and the path exists before training.
4. State/shape mismatches (HF_RSM notebooks): Confirm state arrays are flattened and concatenated with action, collision_flag, and lane_index as shown.
5. GPU / CUDA: For LLM notebooks, ensure T4 GPU runtime is selected.
6. Drive space / permissions: Re-mount Drive and free space if saves fail.

____________________________________________________________________________________________________________________________________________________________________________________________________________________

## Acknowledgements, Citation

Built on: Stable-Baselines3, highway-v0, Reacher-v5

Citation:
@misc{rizan2025-zeroshot-hilrl,
  title     = {Zero-shot LLMs in Human-in-the-Loop RL},
  author    = {Mohammad Saif Nazir and Chayan Banerjee},
  year      = {2025},
  url       = {https://github.com/RizanSM/zero_shot_llms_in_HIL_RL/tree/main},
  version   = {v1.0.0},
  urldate   = {2025-10-02},
  note      = {GitHub repository}
}
____________________________________________________________________________________________________________________________________________________________________________________________________________________

## Run checklist
1. Colab runtime set (T4 GPU for LLM notebooks)
2. BASE_DIR constants defined; all Update directory location X paths filled
3. Bias values set (Highway A–C groups)
4. Drive has free space for logs/models/trajectories
5. API config for LLM prompts set as required in notebooks

Tip: Keep your directory names consistent with the examples (e.g., 1_ppo_highway_hf_direct_ideal, 2_ppo_highway_hf_lrs_ideal, etc.) so later steps find the correct models and dataframes automatically.

____________________________________________________________________________________________________________________________________________________________________________________________________________________
