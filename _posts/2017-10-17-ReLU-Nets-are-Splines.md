---
layout: post
title: ReLU nets are splines
---

### Introduction

Artificial Neural Networks has been increasingly popular during the last years. With API's such as [TensorFlow](https://www.tensorflow.org/) deep learning is easily accessible not only for machine learning experts. This leads to a lot of practice and hands on experience but it might also lead to a lack of theoretical knowledge about the approximation methods.

In this post I will try to shed some light upon how a neural network with the increasingly popular ReLU activation function actually makes its approximations.

The theorems and proofs in this blog post can be found in my [bachelor's thesis in mathematics/numerical analysis](link). When writing the thesis, [my thesis partner](https://github.com/dachrillz) and I got inspiration from [Chen 2016](https://arxiv.org/pdf/1611.09448.pdf).

### ReLU nets are linear splines

The neural network model that will be investigated has one single hidden layer, with an arbitrary number of nodes, the [rectified linear unit](https://en.wikipedia.org/wiki/Rectifier_(neural_networks)) as activation function for the hidden layer and the unit function (i.e. $$g(a) = a$$) as activation function for the final output layer. The network maps one input value to one output value. Hence, $$y: \mathbb{R} \rightarrow \mathbb{R}$$. Thus the network can be expressed as:


\begin{equation}
    y = \sum_{i=1}^n w_i^{(2)} z_i + b^{(2)} \tag{1}
\end{equation}


where,


\begin{equation}
    z_i = g(w_i^{(1)}x + b_i^{(1)}) \tag{2}
\end{equation}


The ReLU function is defined as,


\begin{equation}
    g(a) \equiv \max(0,a) \tag{3}
\end{equation}


Already when looking at the ReLU function one can see that it looks like a linear spline. It has two "line parts" and one discontinuity at 0. By looking at equation 2 one can see how $$z$$ is just an affine transformation of the input with a wrapper, i.e. $$g()$$. Since $$g()$$ is defined as the the ReLU function the hinge will be where the affine transformation is equal to zero.

Solving for where the transformation is equal to zero,


\begin{equation}
    w_i^{(1)}x + b_i^{(1)} = 0 \Leftrightarrow x = -\frac{b_i^{(1)}}{w_i^{(1)}} \tag{4}
\end{equation}


Thus the knots of the linear spline is defined by the fraction of the weights and biases. This means that the position of the knots of the linear spline is adjusted when the weights and biases are updated, i.e. when the network is optimized/trained.

### Upper bound of knots
In fact, one can prove that there is an upper bound on the number of knots produced by the ReLU network's approximation. When looking at a ReLU net with one hidden layer the number of knots is bounded by the number of nodes in the network. The relationship is $$k \leq n$$, where $$k$$ is the number of inner knots of the linear spline and $$n$$ is the number of nodes of the network. The reason that it can be less is because theoretically two or more knots of the approximation can coincide, however this is not very likely in practice.


### Numerical experiment
In order to investigate this property a one hidden layer network with ReLU activation is set up in Keras with TensorFlow backend and tested on [Runge's function](https://en.wikipedia.org/wiki/Runge%27s_phenomenon), i.e. $$\frac{1}{1+25x^2}$$. The network approximation is compared to a linear spline with equidistant placed knots.

The network plotted below has 3 inner nodes plus 2 endpoints, yielding 5 knots.
![Fig 1](/images/3nodes1.png)

Furthermore, the position of the knots are plotted by solving for $$x$$, i.e. $$-\frac{b_i^{(1)}}{w_i^{(1)}}$$,

![Fig 2](/images/3nodes2.png)


### Theorems and proofs
The theorems and proofs presented below is taken from [my thesis](link). For a generalization see [Chen 2016](https://arxiv.org/pdf/1611.09448.pdf).


In order to show that the ReLU net is a linear spline it is helpful to first define what a linear spline is.


\newtheorem*{mydef3}{Definition of linear spline}
\begin{mydef3}
A function, $S$, is called a \textit{linear spline} or a \textit{spline of degree 1} if for a finite set of knots $x_0,x_1,...,x_n$ the following two conditions hold
\begin{enumerate}
    \item on each interval $[x_{i-1},x_i]$, $S$ is a polynomial of maximal degree $1$
    \item $S$ is continuous
\end{enumerate}
\end{mydef3}


