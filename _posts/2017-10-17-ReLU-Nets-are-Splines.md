---
layout: post
title: ReLU nets are splines
---

### Introduction

Artificial Neural Networks has been increasingly popular during the last years. With API's such as [TensorFlow](https://www.tensorflow.org/) deep learning is easily accessible not only for the machine learning experts. This leads to a lot of practice and hands on experience but it might also lead to a lack of theoretical knowledge about the approximation methods.

In this post I will try to shed some light upon how a neural network with the increasingly popular ReLU activation function actually makes its approximations.

The theorems and proofs in this blog post are taken from my bachelor's thesis in mathematics/numerical analysis. When writing the thesis [my thesis partner](https://github.com/dachrillz) and I got inspiration from [Chen 2016](https://arxiv.org/pdf/1611.09448.pdf).

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

In order to show that the ReLU net is a linear spline it is helpful to first define what a linear spline is.


$$\newtheorem*{mydef3}{Definition of linear spline}$$
$$
\begin{mydef3}
A function, $S$, is called a \textit{linear spline} or a \textit{spline of degree 1} if for a finite set of knots $x_0,x_1,...,x_n$ the following two conditions hold
\begin{enumerate}
    \item on each interval $[x_{i-1},x_i]$, $S$ is a polynomial of maximal degree $1$
    \item $S$ is continuous
\end{enumerate}
\end{mydef3}
$$

$$
\newtheorem*{mydef4}{Theorem (1): Single hidden layer neural networks with ReLU activation functions are linear splines}
\begin{mydef4}
Let $y$ be a neural network defined by Equation 23 and 24 with the ReLU activation function defined as in Equation 25. $y$ is a function  $\mathbb{R} \rightarrow \mathbb{R}$ satisfying the conditions in the definition of the linear spline and is thus a linear spline. The knots are defined by the networks's weights and biases.
\end{mydef4}
$$

Proof: 
$$
\begin{enumerate}
\item Let g(a) be the ReLU function. g(a) is a linear spline with knots $X = \{ a_{min}, 0, a_{max} \} $
\item Let z be an inner node of the neural network $y$ defined as $z = g(wx + b)$. z is a linear spline with knots $X = \{\frac{x_{min} - b}{w}, \frac{-b}{w}, \frac{x_{max} - b}{w} \} $
\item The whole network $y$ is an affine transformation of the inner network nodes $z_1,z_2,...,z_n$ Such that: $y(x) = w^{(2)}_1g(w_1^{(1)} x + b^{(1)}_1) + w^{(2)}_2g(w_2^{(1)} x + b^{(1)}_2) + ... + w^{(2)}_n g(w_n^{(1)} x + b^{(1)}_n) + b^{(2)}$
\end{enumerate}
$$

By creating the set of knots $$X_y$$ for $$y$$ as the union of all the knots of the inner network nodes, $$X_1,X_2,X_n$$ such that: $$X_y = X_1 \cup X_2 \cup ... \cup X_n$$. Then $$y$$ is defined by a set of knots. As y consists of a linear combination of $$z_i$$'s the two conditions for linear splines are preserved in $$y$$. Combining these results one sees that $$y$$ is a linear spline. $$\qed$$

### Upper bound of knots in one layer network.

$$
\newtheorem*{mydef8}{Theorem (2): The number of inner spline knots is bounded by the number of inner network nodes}
\begin{mydef8}
The number of inner knots, $k$, of a spline, $y$, defined by Equation 23 and 24 with activation function as defined in Equation 25, is bounded by the number of inner network nodes, $n$. That is, the number of inner spline knots satisfies the relation $k \leq n$.
\end{mydef8}
$$

Proof: Writing Equation 23 as $$y = w_1^{(2)} z_1 + w_2^{(2)} z_2 + ... + w_n^{(2)} z_n + b^{(2)}$$, where $$z_i$$ is defined as in Equation 24. The derivative of $$y$$ with regard to $$x$$ is,  $$y^\prime=w_1^{(2)} z_1^\prime w_1^{(1)} +...+ w_n^{(2)} z_n^\prime w_1n^{(1)}$$. The derivative consists of $$n$$ terms $$z_1,z_2,...z_n$$. The terms can at most be discontinuous at one point. That is when the argument for $$z_i(x)$$ is $$x = \frac{-b_i^{(1)}}{w_i^{(1)}}$$ Hence, there are at most $$n$$ discontinuities in $$y^\prime$$.

The relation is $$k \leq n$$ due to the fact that two knots can overlap one another if $$(\frac{-b_i^{(1)}}{w_i^{(1)}}) = (\frac{-b_j^{(1)}}{w_j^{(1)}})$$ for two different network nodes $$z_i$ and $z_j$$. The knots can also lie outside of the range of $$[x_{min},x_{max}]$$ if  $$\frac{-b_i^{(1)}}{w_i^{(1)}} < x_{min}$$ or $$\frac{-b_i^{(1)}}{w_i^{(1)}} > x_{max}$$ $$\qed$$

Using Theorem 1 one can see that if two or more of the $$n$$ terms $$y^\prime$$ have their discontinuity at the same position their knots will coincide. That is if $$\frac{b_i^{(1)}}{w_i^{(1)}} = \frac{b_k^{(1)}}{w_k^{(1)}}$$ happens to coincide for the two network nodes $$z_i$$ and $$z_k$$. If $$w^{(1)}_i$$ also is small or if $$b^{(1)}_i$$ is large the knots might lie outside of the set of knots $$X$$ as this set is bounded. Since $$y$$ is defined in a finite interval then that knot will not exist for $$y$$ as it is defined by it's knots.
