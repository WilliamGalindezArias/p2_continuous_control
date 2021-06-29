## Continuous Control with Actor-Critic DDPG
### William Galindez Arias


#### Abstract

The environment to be solved in this project consist of 20 robotic arms which are considered identical agents. To solve this environment an implementation of a DDPG Agent was trained.


#### 1. The Environment

The environment is called Reacher and its detail can be found in this repository in the **README** description. It consist of 20 robotic arms where the goal
is to learn how to control and move a ball around. The longer the robotic arm is able to stick to the ball and controls it, the higher will be the reward.

#### 2. Actions & States

There are 20 agents. Each observe a state with length: 33. The state contains agent position, velocity and rotation, wrapped in a vector as it looks below :

``` state = [ 0.00000000e+00 -4.00000000e+00  0.00000000e+00  1.00000000e+00
 -0.00000000e+00 -0.00000000e+00 -4.37113883e-08  0.00000000e+00
  0.00000000e+00  0.00000000e+00  0.00000000e+00  0.00000000e+00
  0.00000000e+00  0.00000000e+00 -1.00000000e+01  0.00000000e+00
  1.00000000e+00 -0.00000000e+00 -0.00000000e+00 -4.37113883e-08
  0.00000000e+00  0.00000000e+00  0.00000000e+00  0.00000000e+00
  0.00000000e+00  0.00000000e+00  5.75471878e+00 -1.00000000e+00
  5.55726624e+00  0.00000000e+00  1.00000000e+00  0.00000000e+00
 -1.68164849e-01]
 ```
 
 ### 3. Learning Algorithm 
 
 The baseline code used for this project is the one corresponding to DDPG agent contained in the nanodegree p2_continuous_control folder. The model major structure can be found here
 [DDPG](https://arxiv.org/abs/1509.02971)
 
 The major highlights of implementing this model are as follows:
 - It builds on top of the successful Deep-Q-Learning algorithm adapted to the continuous space 
 - Essentially consist of an actor-critic, model-free algorithm with policy gradient 
 - Implements Replay-buffer, which can be large given that this algorithm DDPG is an off-policy. Learning in mini-batches rather than online
 - To address the unstable behaviour from Q-Learning, soft target updates are executed to the actor and critic networks. The target values are slowly changed
 - improving the stability of the learning and avoiding divergence.
 - The exploration and learning of continuos spaces is done independently from the learning


### 4. Network Architecture 

Consist of two network architectures *actor-critic* with the corresponding architecture:


 ```    self.fc1 = nn.Linear(state_size, fc1_units)
        self.fc2 = nn.Linear(fc1_units, fc2_units)
        self.fc3 = nn.Linear(fc2_units, action_size)

 ```
 where the input size = state_size = 33
 
 the layer structure is as follows:
  ``` 33 units (inputs) -o 400 [Fully-connected-layer] -o 300 [fully-connected-layer] - ReLU - 4 [action] ```
  
### 4.1 Network hyper-parameters

```
BUFFER_SIZE = int(1e5)  # replay buffer size
BATCH_SIZE = 128        # minibatch size
GAMMA = 0.99            # discount factor
TAU = 1e-3              # for soft update of target parameters
LR_ACTOR = 5e-4         # learning rate of the actor 
LR_CRITIC = 5e-4        # learning rate of the critic
WEIGHT_DECAY = 0.000    # L2 weight decay
UPDATE_EVERY = 1        # how often to update the networks

```

The learning rate was updated after several try/error and observing the score in the first 10 episodes.

### 5. Training 

The environment is solved after getting a score greater than 30 on average. The rewards over each episode can be seen below. Also more detail is presented in the 
jupyter notebook in this repo

![Screen Shot 2021-06-29 at 1 24 04 PM](https://user-images.githubusercontent.com/25883464/123789267-47dd7580-d8dd-11eb-89b3-0bfa5838125a.png)
The average score for all 20 agents during training

### 6. Ideas to improve

- Try other network architectures that can learn faster such A2C or A3C
- Fine tune units in the network, altering the number of nodes to improve convergence
- try different replay buffers sizes to dismiss less old values 

