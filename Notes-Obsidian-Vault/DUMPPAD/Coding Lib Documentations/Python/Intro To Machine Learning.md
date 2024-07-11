## Table of Contents

    - [Methods of learning in Machine Learning](#Methods\of\learning\in\Machine\Learning)
- [Introduction to Machine Learning](#introduction\to\machine\learning)
  - [Accessing the Machine](#Accessing\the\Machine)
  - [Understanding AI and Machine Learning (ML)](#Understanding\AI\and\Machine\Learning\(ML))
  - [Learning Styles in ML](#Learning\Styles\in\ML)
  - [Basic Structure of a Neural Network](#Basic\Structure\of\a\Neural\Network)
  - [Node Function in Hidden Layer](#Node\Function\in\Hidden\Layer)
  - [Training the Neural Network](#Training\the\Neural\Network)
  - [Dataset Splits](#Dataset\Splits)


- **Genetic Algorithm**: ML Structure that aims to mimic the process of natural selection and evolution. Uses rounds of "offsprings" and mutations based on a specified criteria provided, the structure aims to create the "strongest children" through the "survival of the fittest".
- **Particle Swarm**: This ML structure aims to mimic the process of how birdws flock and group together at specific points. By creating a swarm of particles, the structure aims to move all the particles to the optimal's grouping point
- **Neural Networks**: Most popular and aims to mimic the process of how neurons work in the brain. These neurons receive various inputs that are then transformed before being sent to the next neuron. These neurons can then be "trained" to perform the correct transformations to provide the correct final answer


### Methods of learning in Machine Learning
- Supervised learning: This learning style guides the neural network to the answers we want it to provide. we ask the neural network to give us an answer and then provide it feedback on how close it was to the correct answer. This way we have more control over the overall outcome of the model's performance.

- **Unsupervised learning:** In this learning style, we take a bit more of a hands-off approach and let the neural network do its own thing. While this sounds very strange, the main goal is to have the neural network identify "interesting things". Humans are quite good at most classification tasks – for example, simply looking at an image and being able to tell what colour it is. But if someone were to ask you, "Why is it that colour?" you would have a hard time explaining the reason. Humans can see up to three dimensions, whereas neural networks have the ability to work in far greater dimensions to see patterns. Unsupervised learning is often used to allow neural networks to learn interesting features that humans can't comprehend that can be used for classification. A very popular example of this is the restricted Boltzmann machine. Have a look [here](https://scikit-learn.org/stable/modules/neural_networks_unsupervised.html) at the weird features the neural network learned to classify different digits.


**Basic Structure**

Next on our list of ML basics to learn is the basic structure of a neural network. Sticking to the very basics of ML, a neural network consists of various different nodes (neurons) that are connected as shown in the animation below:

                        Input      Hidden
Height                0                0  0
Width                 0                 0  0
Length               0                  0  0
Colour Scheme 0                   0  0
Checker Elf ID   0                    0  0



s shown in the animation, the neural network has three main layers:

- **Input layer:** This is the first layer of nodes in the neural network. These nodes each receive a single data input that is then passed on to the hidden layer. This means that the number of nodes in this layer always matches the network's number of inputs (or data parameters). For example, if our network takes the toy's length, width, and height, there will be three nodes in the input layer.
- **Output layer:** This is the last layer of nodes in the neural network. These nodes send the output from the network once it has been received from the hidden layer. Therefore, the number of nodes in this layer will always be the same as the network's number of outputs. For example, if our network outputs whether or not the toy is defective, we will have one node in the output layer for either defective or not defective (we could also do it with two nodes, but we won't go into that here).
- **Hidden layer:** This is the layer of nodes between the neural network's input and output layers. With a simple neural network, this will only be one layer of nodes. However, for additional learning opportunities, we could add more layers to create a deep neural network. This layer is where the neural network's main action takes place. Each node within the neural network's hidden layer receives multiple inputs from the nodes in the previous layer and will then transmit their answers to multiple nodes in the next layer.
# Introduction to Machine Learning

## Accessing the Machine

- **Start the Target Machine:** Click on the green "Start Machine" button on the top-right of the task.
- **Wait Time:** Allow three minutes for the VM to open on the right-hand side.
- **Split View:** If the machine is not visible, press the blue "Show Split View" button at the top of the room.

## Understanding AI and Machine Learning (ML)

- **AI vs ML:**
    - AI is a broad term, often used incorrectly.
    - ML is a specific process of creating systems that mimic real-life intelligence.
- **Key ML Structures:**
    - **Genetic Algorithm:** Mimics natural selection and evolution.
    - **Particle Swarm:** Mimics flocking behaviors of birds.
    - **Neural Networks:** Mimics neuron functions in the brain.

## Learning Styles in ML

- **Supervised Learning:**
    - Guiding the neural network towards desired answers.
    - Requires a labeled dataset for feedback.
- **Unsupervised Learning:**
    - Allows the neural network to identify patterns independently.
    - Used for complex classification tasks.

## Basic Structure of a Neural Network

1. **Input Layer:**
    - Receives individual data inputs.
    - Number of nodes matches the number of inputs.
2. **Output Layer:**
    - Sends the final output from the network.
    - Number of nodes matches the number of outputs.
3. **Hidden Layer:**
    - Located between input and output layers.
    - Performs the main data processing.

## Node Function in Hidden Layer

- **Data Processing:**
    - Inputs are multiplied by a weight value.
    - Output is determined by an activation function.

## Training the Neural Network

- **Feed-Forward Loop:**
    - Normalizes inputs.
    - Propagates data through the network.
    - Reads the output from the network.
- **Back-Propagation:**
    - Calculates the difference between received and expected outputs.
    - Updates the weights of the nodes.
    - Propagates the difference back to other layers.

## Dataset Splits

- **Training Data:** 70–80% for training the network.
- **Validation Data:** 10–15% for validating network training.
- **Testing Data:** 10–15% for final network performance evaluation.

