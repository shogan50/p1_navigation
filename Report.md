[//]: # (Image References)

[image1]:
"C:\Users\scotth\Google Drive\deep-reinforcement-learning\p1_navigation\eps_zero.PNG" "Epsilon Zero"


# Project 1 - Navigation Report

## Learning Algorythm
The Banana agent learning algorithm is basic Deep Q learning, although the network isn't very deep at three total layers.  

## Hyperparameters
What was enlightening to me is that, more or less by accident, I discovered that really low epsilon values result in the fastest training.  I guess that since there is nothing to explore in this evironment, this agent learns best with a greedy policy rather than an epsilon greedy policy. By nothing to explore, I mean the environment isn't fixed like, for instance like cliff walking.  The environment is randomly   In order to save time, I put the learning in a loop, randomly selecting some hyper parameters including epsilon start and let it run un-attended over night.   The minimum starting epsilon was clipped at .1 and all of the trials that started at that value achieved the requisite 13+ mean over 100 episodes in about 300 episodes.  

Setting epsilon to zero however slowed learning.  Once it reach some threshold, it seemed to take off.  The following plot of rewards was run with epsilon at zero. (layer one nodes: 243, layer 2 nodes: 19)  I don't have an explanation for why some randomness is necessary.  It is possible that it is related to that network architecture.

![Epsilon Zero][image1]

Not much else interesting came out of the random hyper parameters except that it is pretty insensitive to the size of the two fully connected layers.  

## Model Architecture
The model consists of 3 linear layers.  The first layer takes the 37 inputs and consists of about 64 nodes.  As of this writing random numbers of nodes up to 256 in the first layer and the same in the second layer are running, but initial results show that it is pretty insensitive.  The second layer is similar.  The third layer has 4 ouputs. 

## Other observations
It was interesting for me to discover that my GPU was running at 1% during training and that while I din't do a head to head comparison, training on my laptop with a GPU and on my Linux workstation without a GPU were not obviously taking different amounts of time to learn.  I hypothesize that this rather small network doesn't benifit from the GPU and that the unity environment is the bottleneck.  One thing that would be intresting to try, is to see if it is possible to run two agents and instantiate two environments in parallel training only consumes about 50% of my CPU resources. 

#Plot of rewards



