### Introduction

This project is to train an agent to navigate and collect as many of yellow bananas as possible, in a large, square world.  

A reward of +1 is provided for collecting a yellow banana, and a reward of -1 is provided for collecting a blue banana.  Thus, the goal of your agent is to collect as many yellow bananas as possible while avoiding blue bananas.  

 ### Implementation

The implementation is pretty much reusing the code from deep Q-learning network project. The network consists of input, hidden and output layers. The hidden layer is of size 64. And the learning rate is 0.0005. 

Deep Q network has been found to be difficult to train, so in the current implementation, experience replay and fixed Q targets were also used to better train the network.

***Experience Replay*** 

The training data may be correlated, and not enough attention is paid to the rare events. 

In order to alleviate these issues. The system keeps the experience in replay buffer.,and then randomly sample from this replay buffer so as to break the temporal dependency among the observations used in the training of the network, so the data is more independent of each other, and closer to iid.

This allow more efficient use of previous experience by learning with it multiple times, and be able to recall rare events (we can actually assign prority to rare events for example)


***Fixed Q Targets*** 
In Q learning, the target values are non stationary. This is particular problematic for deep Q network. Within each training iteration, we update model parameters to move closer to the target. But this update will impact other estimation in a deep network, the surrounding states will be impacted. 

So in fixed Q targets, we create two deep network. Using the first one to retrieve the target values while the second one is used for training. And then after certain number of updates, we will synchronize these two networks. This is done in order to temporarily fix the Q value target, so we have a stable target during training.

***epislon greedy***
In order to encourage exploration, simple epison greedy algorithm was used, with initial epilson set to 1.0 and then exponentially decay until it reaches 0.01.

And then the agent is trained for 1500 episodes, and reach an average score of 15 per 100 episodes.

### Result

The agent is able to collect 13 bananas in 383 episodes. The average number of bananas collected keep going up and down, showing the effect of epison greedy algorithm.

 ### Future Work
 **Double DQN**, One of the alternative is to implement using Double DQN. DQN are known to overestimate the value function, using two separate Q-value estimators, each is used to update the other to avoid maximization bias by disentangling the updates from biased estimator.
