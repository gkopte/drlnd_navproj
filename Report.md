
# Design Choices

The approach for this project was to employ a simple, standard form of Deep Q-Learning with a straightforward replay buffer. The architecture of the neural networks used in this project is simple and includes two hidden layers with ReLU activation functions and an output layer with no activation function. Given the relatively uncomplicated nature of the problem and the modest size of the state space, there was no apparent need for implementing advanced variants of Deep Q-Learning or specialized forms of experience replay.

# Agent

**<u>act</u>**: selects an action using an epsilon-greedy policy. With probability epsilon it chooses a random action, and otherwise it chooses the action with the highest Q-value according to the local Q-Network.

**<u>learn</u>**: it uses Q-Learning updates to adjust the parameters of the local Q-Network. It applies a variant of the Q-Learning update called Fixed Q-Targets, where two networks are used to decouple the target from the parameters being updated for more stable learning. The Q-values of the next state are obtained from the target Q-Network.

**<u>soft_update</u>**: gradually blends the parameters of the local and target Q-Networks, ensuring that learning is stable and that the target values are consistent with the policy being learned.

**<u>step</u>**: stores the latest experience tuple into the replay buffer and occasionally triggers a learning step.

# Q-Network

The agent uses a neural network for the function approximation of the Q-values. It includes two instances of the Q-Network, a local model qnetwork_local and a target model qnetwork_target. The local model is the one being trained, and the target model is used to compute the target Q-values during the learning step.

The network architecture is fairly simple and consists of two hidden layers with ReLU activation functions and an output layer without an activation function. The reason for not having an activation function at the output layer is because in Q-learning, we want to directly predict the Q-value of each action, which can theoretically be any real number, so we don't want to limit the output range with an activation function.

The Q-Network uses only fully connected layers and doesn't include more complex layer types like convolutional layers because the state representation is a relatively small, flat vector of features. In our case, the state size is 37, which is a moderate-sized vector that can be effectively processed using fully connected layers. 

# Replay Buffer


A buffer is created to store the interactions between the agent and the environment. This buffer is populated with the experiences of the agent, each experience being a tuple consisting of the state, action, reward, the next state, and a 'done' flag. When the buffer reaches its capacity, it begins to overwrite old experiences with new ones. No version of the prioritized Replay Buffer has been implemented in this solution.


