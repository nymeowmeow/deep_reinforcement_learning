## Project3 - Collaboration and Competition

In this exercise, two agents control rackets to bounce a ball over a net. If an agent hits the ball over the net, it receives a reward of +0.1. If an agent lets a ball hit the ground or hits the ball out of bounds, it receives a reward of -0.01. Thus, the goal of each agent is to keep the ball in play.

The observation space consists of 8 variables corresponding to the position and velocity of the ball and racket. Each agent receives its own, local observation. Two continuous actions are available, corresponding to movement toward (or away from) the net, and jumping.

The task is episodic, and in order to solve the environment, your agents must get an average score of +0.5 (over 100 consecutive episodes, after taking the maximum over both agents). Specifically,

After each episode, we add up the rewards that each agent received (without discounting), to get a score for each agent. This yields 2 (potentially different) scores. We then take the maximum of these 2 scores. This yields a single score for each episode.

The environment is considered solved, when the average (over 100 episodes) of those scores is at least +0.5.

## DDPG
The initial implementation reuse the ddpg algorithm implemented in project 2, which uses 2 independent DDPG agent, trying to solve the problem. But was never able to get good result, the maximum moving average (100 episodes) was only 0.2. Probably as explained in the reference paper (multi-agent actor-critic), each agent's policy is changing as training progresses, and the environment becomes non-stationary from the perspective of any individual agent.

The course forum has suggestion to use shared replay buffer between the two agent, use smaller replay buffer size, use high TAU value of 1e-2, and low batch size. But was still unable to get the ddpg implementation to solve this problem

## MADDPG

implementing the idea in the paper (reference 1 below), Instead of independently training each agent separately, a simple extension is to have the critic augmented with extra information about the policies of other agents, while the actor only has access to local information.

The paper describes the algorithm using a centralized critic and decentralized actor, in my case, each agent has both critic/actor, and individual critic has access to the full state and action for all the agent, while the actor has only access to its state and action.

The usual replay buffer was used in the implementation to improve stability, but in this case, the replay buffer is shared among the critic in each agent.

Also to improve stability, the noise level is decaying by multiplying the current value by a decay factor, until it reach the final level. In this way, the earlier episode is encouraged to explore, while at the latter stage when the agent has more experience, the exploration is maintained at low level.

## Hyperparameters

|            Parameter |  Value |
| -------------------- | ------ |
|           State Size |     24 |
|           Action Sie |      2 |
|          Buffer Size | 100000 |
|          Batch Size  |    256 |
|               Gamma  |   0.99 |
|                 Tau  |  0.001 |
|  Actor Learning Rate | 0.0001 |
| Critic Learning Rate | 0.0003 | 
|          Noise Decay |  0.999 |
|    Noise Final Level |    0.1 |


* the actor network has following configuration, 
         <ul>
         <li>input layer: state size x 256 
         <li>relu
         <li>hidden layer: 256 x 128
         <li>relu
         <li>output layer: 128 x action size
         <li>tanh
        </ul>
* The critic network has the following configuration
        <ul>
        <li>input layer: (state size * number of agent) x 256
        <li>relu
        <li>hidden layer: 256 + (number of agent * action_size) x 128
        <li>relu
        <li>output layer: 128 x 1
        </ul>

## Plot of Rewards
![img](images/reward.PNG)

## Future Work
1. try alternative algorithm like PPO, A3C and compare their performance.
2. reimplement the algorithm using DDPG, as reported by peer student, DDPG should work with shared replay buffer, with high TAU vaue, low buffer size and low bath size, and suitably decaying the noise level as training progress.

## Reference
1. Multi-Agent Actor-Critic for Mixed Cooperative-Competitive Environments, Ryan Lowe, Yi Wu, https://arxiv.org/pdf/1706.02275.pdf
