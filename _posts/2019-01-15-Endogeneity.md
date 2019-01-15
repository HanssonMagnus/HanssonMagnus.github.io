---
layout: post
title: Endogeneity
---

## Introduction
This post considers the econometric concept of endogeneity in which the explanatory variable in a regression in correlated with the error term. A short explanation is given, followed by a numerical example in R using indtrumental variable (IV) estimation.

## Short theory
Consider the OLS estimator,

\begin{equation}
    \hat{\beta} = (X'X)^{-1}X'y \iff
\end{equation}

\begin{equation}
	\hat{\beta} = (X'X)^{-1}X'(X \beta +u) \iff
\end{equation}
	
\begin{equation}
	\hat{\beta} = (X'X)^{-1}(X'X) \beta + (X'X)^{-1}X'u \iff
\end{equation}

\begin{equation}
	\hat{\beta} = \beta + (X'X)^{-1}X'u
\end{equation}

Thus, in order for the estimator to be unbiased $$\mathbb{E}((X'X)^{-1}X'u)=0$$ needs to hold. The standard OLS assumption $$\mathbb{E}(u \bar X)=0$$ yields unbiasedness,

\begin{equation}
	\mathbb{E}(\hat{\beta}) = \beta + \mathbb{E}((X'X)^{-1}X'u) \iff
\end{equation}

\begin{equation}
	\mathbb{E}(\hat{\beta}) = \beta + \mathbb{E}(\mathbb{E}((X'X)^{-1}X'u|X)) \iff
\end{equation}

\begin{equation}
	\mathbb{E}(\hat{\beta}) = \beta + \mathbb{E}((X'X)^{-1}X' \mathbb{E}(u|X)) \iff
\end{equation}

\begin{equation}
	\mathbb{E}(\hat{\beta}) = \beta
\end{equation}

by using the law of iterated expectations. (For consistency this needs to hold asymptotically.) This means that if endogeneity is present, i.e. $$X$$ is correlated with $$u$$, the estimator is biased and inconsistent.

## The IV estimator
\begin{equation}
	\hat{\beta} = \frac{(z'z)^{-1}z'y}{(z'z)^{-1}z'x} = (z'x)^{-1}z'y
\end{equation}


## Numerical example
\begin{equation}
	y = 0.5x + u
\end{equation}

\begin{equation}
	x = z + v
\end{equation}

where $$z$$ is distributed $$\mathcal{N}(2,1)$$ and $$(u,v)$$ are distributed by a multivariate normal distribution with means $$0$$, variances $$1$$ and correlation $$0.8$$. This means that $$x$$ is correlated with $$u$$, hence the regression of $$y$$ on $$x$$ is inconsistent and biased.

The $$z$$ variables is an instrument since it is correlated with $$x$$ but uncorrelated with $$u$$.

We start by defining a linear regression function,

```R
lin_reg <- function(x, y, z = 0, intercept = TRUE, instrument = FALSE){
  
  x = as.matrix(x)
  y = as.matrix(y)
  
  output = list()
  
  if (intercept){
    x = cbind(1,x)
    z = cbind(1,z)
  }
  
  if (instrument){
    output[['coeffs']] = solve(t(z) %*% x) %*% t(z) %*% y
  } else {
    output[['coeffs']] = solve(t(x) %*% x) %*% t(x) %*% y
  }
  
  output[['preds']] = x %*% output[['coeffs']]
  output[['resids']] = output[['preds']] - y
  output[['mse']] = mean(output[['resids']]^2)
  
  return(output)
}
```

Simulation of data,

```R
library(MASS)

set.seed(42)

z <- rnorm(10000, 2, 1)

sigma <- matrix(data = c(1,0.8,0.8,1), nrow = 2, ncol = 2)
uv <- mvrnorm(n = 10000, mu = c(0,0), Sigma = sigma)

u <- uv[,1]
v <- uv[,2]

x <- z + v
y <- 0.5*x + u
```
