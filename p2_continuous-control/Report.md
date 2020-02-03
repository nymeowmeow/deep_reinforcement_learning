## Project 2: Continuous Control

For this project, we are to train a double-jointed arm to maintain its position at the target location for as many time steps as possible. A reward of +0.1 is provided for each step that the agent's hand is in the goal location. 

The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between -1 and 1.

This report focus on single agent, the task is episodic, and the agent has to get an average score of +30 over 100 consecutive episodes

## Learning algorithm

In this project, DDPG which is kind of actor critic algorithm was used. The set of parameters used is listed below, and how the parameter is used is explained below

| parameter | value |
| --------- | ----- |
| state size | 33|
| action size | 4|
| buffer size | 1000000|
| gamma (discount factor)  | 0.99|
| tau | 0.001|
| actor learning rate | 0.0002|
| critic learning rate | 0.0002|


### 1. Fixed Target
There are two target networks in this implementation, one for actor and one for critic. The purpose of target network is to have a stable target during training to enhance the stability. 

During training, the target values are non stationary. This is particular problematic for deep Q network. Within each training iteration, we update model parameters to move closer to the target. But this update will impact other estimation in a deep network, the surrounding states will be impacted.

using target network, enchance the stability during training.

### 2. Soft updates

The weights for both actor/critic target networks are updated by having them slowly tracking the learned network as below, such that the target values are constrained to change slowly which greatly improve the stability.

![img](image/slowupdate.PNG)

### 3. Experience replay
In order to break correlation between experience when we sample the data for training. Replay buffer is used, which is a finite sized cache, used to store samples. When actor or critic needs data for training, a minbatch was sampled randomly from this buffer to break the correlations between samples.

### 4. OU Noise
An Ornstein-Uhlenbeck process was used to construct an exploration policy as follows, where We constructed an exploration policy μ′ by adding noise sampled froma noise process N to our actor policy

![img](image/ou.PNG)

### 5. DDPG algorithm
![img](image/ddpg.PNG)

## Plot of Rewards
![img](image/rewards.PNG)

## Future Work
1. Current implementation only work with single agent, will extend the agent to work with multiple (20) agents in the provided environment
2. It seems it is kind of hard to train DDPG, seems to be sensitive to actor/critic learning rate and took long time to reach the score of 30, will like to explore other algorithm like PPO, A3C and compare their performance.

### reference
Continuous Control with Deep reinforcement Learning, Timothy P.Lillicrap,Jonathan J.Hunt, https://arxiv.org/pdf/1509.02971.pdf
