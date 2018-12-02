# Project 1 - Navigation Report

## Introduction
This project was more of an introduction some of the tools we haven't been exposed to, such as Unity and github. For readers not familiar with the environment, the task is to create an agent that can learn to seek out yellow banannas while avoiding blue bananas in a geometrically square space. 

![bananas](https://user-images.githubusercontent.com/10624937/42135619-d90f2f28-7d12-11e8-8823-82b970a54d7e.gif)

The code for the agent and model is very similar to previous lessons.  Essentially we have a state space size of 37 and a action size of 4. The lesson instructions suggested the learning could be done in 1800 episodes, so I was surpized to find it could meet the lesson criteria of an average reward of 13+ over 100 episodes in ~300 episodes. Below is a plot of rewards during training using the best hyper parameters found during the exercise.

### Plot of rewards
![Epsilon pt1](https://github.com/shogan50/p1_navigation/blob/master/eps_ptone.PNG)

## Learning Algorithm
The Banana agent learning algorithm is basic Deep Q learning, although the network isn't very deep at three total layers.  It utilizes experience replay and periodic updates. I haven't explored this, but apparently in the absense of some sort of mechanisms like experience replay and periodic updates, DQN's haven't been very successful.  The idea is that you decouple network update step from the experience step.  To perform experience replay, the agent's experiences are stored at each time step in a buffer.  During learning, Q-Learning updates are applied to the samples of experiences drawn uniformly from the buffer.  The weights of the network are periodically updated.  in this case every 4th step.

## Hyperparameters
What was enlightening to me is that, really low epsilon values result in the fastest training.  I guess that since this environment isn't like a maze, this agent learns best with a greedy policy rather than an epsilon greedy policy. By not being like a maze, I mean the environment doesn't have fixed geometry that the agent really needs to learn to navigate, for instance like cliff walking.  The elments of the environment that the agent needs to navigate, so to speak (location of bananas), are randomly placed during each episode. 

In order to save myself time finding good hyperparameters, I did some training in loop, randomly selecting some hyper parameters including epsilon start and let it run un-attended over night. The starting epsilon value was one of the parameters. The minimum starting epsilon was clipped at .1 and all of the trials that started at that value achieved the requisite 13+ mean over 100 episodes in about 300 episodes.  

Setting epsilon to zero however slowed learning.  Once it reach some threshold, around 500 episodes, its learning seemed to take of at the same rate as epsilon_start = 0.1.  The following plot of rewards was run with epsilon at zero. (layer one nodes: 243, layer 2 nodes: 19)  I don't have an explanation for why some randomness is necessary.  It is possible that it is related to that network architecture, but my first guess is that non-zero epsilon helps overcome the weights initialization.  It would be interesting to play with two things: 1. other initializations, perhaps all zeros.  2. Staring with a high epsilon, but substantially decaying say to approaching zero in 100 episodes.

![Epsilon Zero](https://github.com/shogan50/p1_navigation/blob/master/eps_zero.PNG)

Other hyper parameters were played with manually (not in the loop described above), but in the end left the same as in the exercise they came from.  All changes seemed to decrease learning.
```
BUFFER_SIZE = int(1e5)  # replay buffer size
BATCH_SIZE = 64         # minibatch size
GAMMA = 0.99            # discount factor
TAU = 1e-3              # for soft update of target parameters
LR = 5e-4               # learning rate 
UPDATE_EVERY = 4        # how often to update the network
```

## Model Architecture
Not much else interesting came out of the random hyper parameters except that it is pretty insensitive to the size of the two fully connected layers.

The model consists of 3 linear layers.  The first layer takes the 37 inputs and consists of about 64 nodes.  As of this writing random numbers of nodes up to 256 in the first layer and the same in the second layer are running, but initial results show that it is pretty insensitive.  The second layer is similar.  The third layer has 4 ouputs. I will turn in this proejct with the number of nodes to 81 and 64 for the first and second linear layers respectively.

## Other observations
It was interesting for me to discover that my GPU was running at 1% during training and that while I din't do a head to head comparison, training on my laptop with a GPU and on my Linux workstation without a GPU were not obviously taking different amounts of time to learn.  I hypothesize that this rather small network doesn't benifit from the GPU and that the unity environment is the bottleneck.  One thing that would be intresting to try, is to see if it is possible to run two agents and instantiate two environments in parallel training only consumes about 50% of my CPU resources. 

## Potential Improvements
1. Explore the reason that a low, but finite epsilon value yields the best results.
2. Try prioritized experience replay.
3. Try Dualing DQN.
