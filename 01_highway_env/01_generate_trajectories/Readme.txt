About 01.generate_trajectories_highway: 

This code is used to generate the initial trajectory data of the highway environment. 
A PPO model is trained on the default highway environment and is used to generate the trajectories. 

CHANGES TO BE MADE BEFORE EXECUTION OF THE CODE:
(Use Ctrl+F to search)
Update the google drive directory locations in the line marked with the following comments:
# Update directory location 1 : Path to save the training logs
# Update directory location 2 : Path to save the PPO model with initial training
# Update directory location 3 : Path to load the PPO model with initial training
# Update directory location 4 : Path to save the trajectory data in csv format
# Update directory location 5 : Path to save the trajectory data frame(trajectory_df.pkl) as pickle file
____________________________________________________________________________________________________________________________________________________________________

About 02.generate_trajectories_for_bias_flagging:

This code is used to generate biased trajectory data of the highway environment to be used as a source for the LLM_HF_BF(Bias Flagging).

The bias value can be changed in the line marked with the following comments:(Use Ctrl+F to search)
A. LANE CHANGE FEEDBACK SCORE:
# Bias value 1  : No lane change feedback score
# Bias value 2  : One lane change feedback score
# Bias value 3  : Two lane change feedback score
# Bias value 4  : Three lane change feedback score
B. COLLISION AVOIDANCE FEEDBACK SCORE:
# Bias value 5  : Potential Collision Risk-Avoided Collision by slowing down
# Bias value 6  : Potential Collision Risk-Avoided Collision by speeding up
# Bias value 7  : Potential Collision Risk-Avoided Collision by lane change
# Bias value 8  : Immediate Collision Risk-Emergency Avoidance
# Bias value 9  : Safe Path
C. SPEED OPTIMIZATION FEEDBACK SCORE: 
# Bias value 10 : Low traffic density-Low risk, High Speed optimization
# Bias value 11 : Low traffic density-Low risk, Moderate Speed optimization
# Bias value 12 : Low traffic density-Low risk, Low Speed optimization
# Bias value 13 : Moderate traffic density-Moderate risk, High Speed optimization
# Bias value 14 : Moderate traffic density-Moderate risk, Moderate Speed optimization
# Bias value 15 : Moderate traffic density-Moderate risk, Low Speed optimization
# Bias value 16 : High traffic density-High risk, High Speed optimization
# Bias value 17 : High traffic density-High risk, Moderate Speed optimization
# Bias value 18 : High traffic density-High risk, Low Speed optimization

CHANGES TO BE MADE BEFORE EXECUTION OF THE CODE: 
Update the google drive directory locations in the line marked with the following comments:(Use Ctrl+F to search)
# Update directory location 1 : Path to load the trajectory_df.pkl (pickle file saved in 01.generate_trajectories_highway)
# Update directory location 2 : Path to save the biased trajectory data frame (1_biased_hf_trajectories_for_bias_flagging_df) to be used for bias flagging.
___________________________________________________________________________________________________________________________________________________________________
