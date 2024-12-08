
Neural Networks are in basic terms, functions that learn what parameters gives correct outputs.

Take for instance the classic Digit Recognition problem.

You have images of 28x28 pixel resolutions that have hand-written digits written on them. Without the use of a neural network, it would be incredibly difficult do this problem.

A **Node** holds numbers from 0 to 1, with 1 being the color white and 0 being black and the range of values between them dictates how close their grayscale is to either black or white. This number is also called the **activation**.

We use the series of 28x28 pixels, 784 pixels as 784 nodes that contain a 0 to 1 number that dictates what color they are. These nodes are called the **Outer Layer** and at the end the output layer will update with the activation number of each of them. The highest activation will most likely be the answer to what digit it was.

Aside from these layers, there are a few hidden layers in between that have the job translating the 784 inputs into the 10 inputs that we desire. These **hidden layers** can be designed in any way however in this example, Each layer will have 16 nodes. But how are these layers connected to each other?

In the real world, we recognize these digits by a few **characteristic features**. Like a curve and a line for the digit '9' or two curves with the digit '8'. By being inspired by this fact, we can design layers that light up the characteristic that is the most correct (light up here means nodes with higher activation numbers).

The issue will be this: Each connection to these nodes will have a particular weight, these weights are **unknown** and the process of learning is basically finding the weights that correctly assume the correct digit.

You can think of each node connection as a many to one function that takes in inputs and gives out a number between 0 and 1.

Every input is multiplied by its connection weight and summed together, basically a linear summation. We assume a certain bias as well and sum it with the result(why?)
However this summation does not give us a **number between 0 and 1**. An old school way is to squish the result by the sigmoid function.

$$

Node Activation = sigmoid(a0w0 + a1w1 + a2w2 + ... + anwn + bias)

$$
Nowadays it is more common to use the ReLU function however.

# How these NNs learn

What we mean by a Neural Network learning, is it minimizing soe sort of **cost function**



# CNN: Convolutional Neural Networks

