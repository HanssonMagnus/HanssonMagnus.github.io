---
layout: post
title: No arbitrage [finance]
use_math: true
---

## Farkas' lemma
[Farkas' lemma](https://en.wikipedia.org/wiki/Farkas%27_lemma) states,

let $$\textbf{A} \in \mathrm{R}^{m \times n}$$ and $$\textbf{b} \in \mathrm{R}^{m}$$. Then exactly one of the following two statements is true:

1. There exists an $$\textbf{x} \in \mathrm{R}^{n}$$ such that $$\textbf{Ax} = \textbf{b}$$ and $$\textbf{x} \geq 0$$.
2. There exists a $$\textbf{y} \in \mathrm{R}^{m}$$ such that $$\textbf{A}^T \textbf{y} \geq 0$$ and $$\textbf{b}^T \textbf{y} < 0$$.

Where, $$\textbf{x} \geq 0$$ means $$x_i \geq 0$$ $$\forall i$$. (There's a version of Farkas' lemma called Stiemke's lemma in which the relationship hold with strict inequality.)

If we imagine $$\textbf{A}$$ to be a payoff matrix of an asset, $$\textbf{b}$$ a corresponding price vector and $$\textbf{y}$$ portfolio weights, we can think about the theorem in the following way. (1.) implies no arbitrage and (2.) implies arbitrage, and only one can be true.

## Arrow-Debreu security
Suppose that the world exists of two time periods, today and tomorrow, in which tomorrow is stochastic. Tomorrow the world might be in $$S$$ different states of nature. An asset that pays $$1$$ tomorrow if state $$s \in S$$ occurs is called an A-D security, and the price of this security is called state price $$q_s$$.

Imagine an arbitrary asset $$i$$ that pays $$X_{i,s}$$ in state $$s$$. The price of an arbitrary asset can be described in terms of A-D securities in the following way,

\begin{equation}
    P_i = \sum_s q_s X_{i,s} \tag{1}
\end{equation}

## The lemma in finance
In a [complete market](https://en.wikipedia.org/wiki/Complete_market) with no arbitrage, Stiemke's lemma implies that the equation $$\textbf{Xq} = \textbf{P}$$ has a solution $$\textbf{q} > 0$$. We rewrite this as equation (1), $$P_i = \sum_s q_s X_{i,s}$$.

This equation states that the price of asset $$i$$, is equal to the sum of the assets state payoffs times the state prices. No arbitrage (Farkas'/Stieke's lemma) implies (in both directions) a positive state-price vector, this is sometimes called the fundamental theorem of asset pricing.

## Implications
Consider equations (1) again and multiply with the probability that state $$s$$ occurs,

\begin{equation}
    P_i = \sum_s \pi_s \frac{q_s}{\pi_s} X_{i,s} \tag{2}
\end{equation}

$$\frac{q_s}{\pi_s} \equiv M$$ which is a random variable. $$M$$ is in financial theory called the stochastic discount factor (SDF). Further,

\begin{equation}
    P_i = \sum_s \pi_s M X_{i,s} \iff \tag{3}
\end{equation}

\begin{equation}
    P_i = \mathrm{E}[ M X_{i,s} ] \tag{4}
\end{equation}

This tells us that the price of any asset $$i$$ is equal to the expected value of the payoff times the SDF. Henceforth, we drop the s in the notation for convenience. Dividing both sides by the price $$P_i$$ yields what is called the fundamental equation of asset pricing,

\begin{equation}
    1 = \mathrm{E}[ M R_{i} ] \tag{5}
\end{equation}
where the return $$R_i$$ is the payoff divided by the price.

Equation (5) hold for all assets. If we assume that the return, $$R_i$$ is non-stochastic, i.e. risk-free (and can thus be taken out of the expectation),


\begin{equation}
    1 = \mathrm{E}[ M R_{rf} ] \iff \tag{6}
\end{equation}

\begin{equation}
    1 = R_{rf} \mathrm{E}[ M ] \iff \tag{7}
\end{equation}

\begin{equation}
    \frac{1}{\mathrm{E}[M]} = R_{rf} \tag{8}
\end{equation}


Thus, the risk-free rate is equal to the inverse of the SDF. This can also be shown from our initial definition of SDF,

\begin{equation}
    \mathrm{E}[M] = \sum_s \pi_s \frac{q_s}{\pi_s} = \sum_s q_s \tag{9}
\end{equation}

where $$\sum_s q_s$$ implies that you have bought all A-D securities and thus have secured a payoff of $$1$$ tomorrow, and thus the risk-free return is,

\begin{equation}
    R_{rf} = \frac{1}{\sum_s q_s} = \frac{1}{\mathrm{E}[M]} \tag{10}
\end{equation}

## Asset pricing models
Now we have the tools to formulate the core of "complete market no arbitrage asset pricing theory". Consider equation (4),

\begin{equation}
    P = \mathrm{E}[ M X ]
\end{equation}

By using the fact that $$Cov(M, X) = \mathrm{E}[M X] - \mathrm{E}[M] \mathrm{E}[X]$$,

\begin{equation}
    P = Cov(M, X) + \mathrm{E}[M] \mathrm{E}[X] \iff \tag{11}
\end{equation}

\begin{equation}
    P = \frac{1}{R_{rf}} \mathrm{E}[X] + Cov(M, X) \tag{12}
\end{equation}

This, says that the price of any asset is equal to its dicounted future payoff plus a risk premium. We can now rearrage equation (12) and then divide each side by the price,

\begin{equation}
    \mathrm{E}[X] = P R_{rf} - R_{rf} Cov(M, X) \iff \tag{13}
\end{equation}

\begin{equation}
    \mathrm{E}[R] = R_{rf} - R_{rf} Cov(M, R) \tag{14}
\end{equation}

Equation (14) is a fundamental result and says that the return of any asset depends on the risk-free rate and the covaraince with the SDF. Rearranging this into excess return,

\begin{equation}
    \mathrm{E}[R] - R_{rf} = - R_{rf} Cov(M, R) \tag{15}
\end{equation}

Furthermore, multiplying and dividing by $$Var(M)$$ and substituting back $$R_{rf} = \frac{1}{\mathrm{E}[M]}$$,

\begin{equation}
    \mathrm{E}[R] - R_{rf} = \frac{Cov(M, R)}{Var(M)} \cdot - \frac{Var(M)}{\mathrm{E}[M]} \tag{16}
\end{equation}

Equation (16) is called a beta pricing model and sometimes written as,

\begin{equation}
    \mathrm{E}[R] - R_{rf} = \beta_{M,R} \cdot \lambda_{M} \tag{17}
\end{equation}

$$\beta_{M,R}$$ is the quantity of risk in the asset. Note that $$\lambda_{M}$$ does not directly depend on the individual return but on the first and second moments of the SDF.

A large literature in theoretical asset pricing is about finding the SDF and its properties. The theory of SDF relates several independent asset pricing models, some commonly known examples are:

* In the CAPM the SDF is a linear combination of the market portfolio.
* In consumption (micro-founded) models the SDF is marginal rate of substitution of utility today and in the future.
* In Arrow-Debreu pricing the SDF is a function of aggregate demand.



