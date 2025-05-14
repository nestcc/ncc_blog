---
title: Neuroinformatics and Neurotechnologies
date: 2020-12-07 10:41:53
tags: master's course
---



# 1.Science “Neuroinformatics” -  what is it?

modern theory about principles and new mathematical models of information processing, which based on the biological prototypes and mechanisms of human brain activities





# 2.Artificial intellect in technical  systems.

A set of scientific branches which aims to modeling of reasonable intellectual action in human mind.  

1. **Representation and accumulation of knowledge,** 
2. **The ability of reasoning to retrieve relevant information**
3. **The ability of adaptation by learning and self-organization**
4. **Optimal team behavior for goal achievement or hierarchy of targets, role models**
5. **Meaning analysis, semantic, crisp logic, rules of inference, emotions**





# 3.Paradigms of Neuroinformatics.



**Pattern Manipulation Paradigm in Brain Computer**

The main difference between human and traditional computer comes from the fact that humans perceive everything as a pattern, whereas for a machine everything is digitals data.  





# 4. Neurocomputer and artificial neural networks

## Brain-like computer 

is a mathematical model of humane-brain principles of computations. This computer consists of those  elements which can be called the biological neuron prototypes, which are interconnected by direct links called connections and which cooperate to perform parallel distributed processing (PDP) in order to solve a desired computational task.

Neurocomputer may be described as  a mathematical model which has been created in the image and likeness of human brain. Mathematicians suggest dramatically new high-performance models of massive parallelism in data processing and data storage, ways of analysis and calculus results interpretation, formal models creation and knowledge data, artificial intelligent systems. 



## ANN

ANN is an assembly of a simple processing elements – neurons which are interconnected by direct links and which cooperate in order to solve a desired computational task.

Every neuron of ANN can be interpreted as an elementary processing unit with multiple inputs and non-linear transformation of bioelectrical signal excitations .



# 5.Formal neurons and its biological prototype.

  More general math model of ANN theory is known as the basic or **Formal neuron**. It can be describe as a multi-input nonlinear transformer with weighted interconnections, also called synaptic weights or strengths (*w**1**,w**2**, …,**w**n*  ). The cell body (soma) is represented by a nonlinear limiting or threshold activation function . 



1. **Soma** **or** **body cell** **-** is a large, round central body in which almost all the logical functions of the neuron are realized.

2. **The axon** **(output****)**, is a nerve fibre attached to the soma which can serve as a final output channel of the neuron. An axon is usually highly branched.

3. **The dendrites** **(inputs)-** represent a highly branching tree of fibres. These long irregularly shaped nerve fibres (processes) are attached to the soma. 

4. **Synapses** are specialized contacts on a neuron which are the termination points for the axons from other neurons. 



# 6. Mathematical interpretation of excitation, nonlinear activation and dynamic behavior of artificial neuron.

In artificial neural networks, the **activation function** of a node defines the output of that node given an input or set of inputs. A standard integrated circuit can be seen as a digital network of activation functions that can be "ON" (1) or "OFF" (0), depending on input. This is similar to the behavior of the linear perceptron in neural networks.

## excitation

![Snipaste_2020-12-07_10-17-52](../neuro-test/index/Snipaste_2020-12-07_10-17-52.png)



## nonlinear activation

![Snipaste_2020-12-07_10-17-52](../neuro-test/index/Snipaste_2020-12-07_10-17-52.png)



## dynamic behavior

![Snipaste_2020-12-07_10-24-29](../_img/Snipaste_2020-12-07_10-24-29.png)



# 7.Types of artificial neuron models

Depending on the specific model used they may be called a **semi-linear unit**, **Nv neuron**, **binary neuron**, **linear threshold function**, or **McCulloch–Pitts** (**MCP**) **neuron**.







# 8.Artificial Neural networks (ANN): linear and non-linear models.

Linear models are usually some linear transformation applied to the input whose parameters needs to be learned, like classification whose decision boundary looks like the hyper-plane, i.e. labels are [nearly] linearly separable.



In the neural nets, you apply such transformation to the input, and then right after that, you pass the resulting vector to the non-linear function. Why do we do a such thing? Most of the processes in nature are very complex for various reasons. One such reason is that there are always some hidden variables that we don’t know about (or simply can’t observe), which have great impact on the output of the process. Because of all this, we can’t only use matrix multiplications in our neural networks to solve this problem. That’s why we use some non-linear functions on top of linear transformations (feeding the weight-input product to the sigmoid, ReLU, etc.) to create non-linear decision boundaries in hope to fit and generalize at the same time.



# 9. Topology of ANN.

> [Full](https://towardsdatascience.com/the-mostly-complete-chart-of-neural-networks-explained-3fb6f2367464)

1. Perceptron
2. Feed forward neural networks
3. DFF neural networks
4. Recurrent Neural Networks
5. LSTM (long/ short term memory)





# 10.Learning paradigms of ANN.

>  [Link](https://medium.com/swlh/learning-paradigms-in-neural-networks-30854975aa8d)

## Supervised Learning

In supervised learning, we have a very clear and defined goal to predict something using our data. In this manner, we usually use this approach to solve problems of classification or regression.

## Unsupervised Learning

In unsupervised learning, the goal is to discover patterns and trends within a dataset and then use those patterns to make predictions about new data. Within the context of a neural network, the goal is to determine the organization of the dataset it is fed.

## Reinforcement Learning

This style of learning is distinct from the other two because it has a specific goal that it is pushing the machine to move towards. There is a specific outcome that is expected, the machine chooses and learns the most optimal path to achieve it. The way this is accomplished is through providing the machine with rewards and punishments based on its performance that culminate in a long-term end-goal. Rewards and punishment signals are only received by the machine when the machine achieves a new state. Instantaneous feedback is not a part of this paradigm of learning.



