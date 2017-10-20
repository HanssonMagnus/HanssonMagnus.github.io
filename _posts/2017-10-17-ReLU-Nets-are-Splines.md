---
layout: post
title: ReLU nets are splines
---

### Introduction

Artificial Neural Networks has been increasingly popular during the last years. With API's such as [TensorFlow](https://www.tensorflow.org/) deep learning is easily accessible not only for the machine learning experts. This leads to a lot of practice and hands on experience but it might also lead to a lack of theoretical knowledge about the approximation methods.

In this post I will try to shed some light upon how a neural network with the increasingly popular ReLU activation function actually makes its approximations.

The theorems and proofs in this blog post are taken from my bachelor's thesis in mathematics/numerical analysis. When writing the thesis my thesis partner and I got inspiration from [Chen 2016](https://arxiv.org/pdf/1611.09448.pdf).

### ReLU nets are linear splines

The neural network model that will be investigated has one single hidden layer, with an arbitrary number of nodes, and the rectified linear unit as activation function for the hidden layer and the unit function as final layer. The network maps one input value to one output value. Hence, $$y: \mathbb{R} \rightarrow \mathbb{R}$$. Thus the network can be expressed as:

$$
\begin{equation}
    y = \sum_{i=1}^n w_i^{(2)} z_i + b^{(2)}
\end{equation}
$$

where,

$$
\begin{equation}
    z_i = g(w_i^{(1)}x + b_i^{(1)})
\end{equation}
$$

The ReLU function is defined as follows,

$$
\begin{equation}
    g(a) \equiv \max(0,a) \text{ and } g^{\prime}(a) =         \begin{cases}
            1 \text{ if } a > 0 \\
            0 \text{ otherwise}
        \end{cases}
\end{equation}
$$
