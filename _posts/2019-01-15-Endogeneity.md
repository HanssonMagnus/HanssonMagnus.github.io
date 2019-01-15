---
layout: post
title: Endogeneity
---

## Introduction
This post considers the econometric concept of endogeneity in which the explanatory variable in a regression in correlated with the error term. A short explanation is given, followed by a numerical example in R using instrumental variable (IV) estimation.

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

Thus, in order for the estimator to be unbiased $$\mathbb{E}((X'X)^{-1}X'u)=0$$ needs to hold. The standard OLS assumption $$\mathbb{E}(u \mid X)=0$$ yields unbiasedness,

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

by using the law of iterated expectations. (For consistency this needs to hold asymptotically). This means that if endogeneity is present, i.e. $$X$$ is correlated with $$u$$, the estimator is biased and inconsistent.

One might note that $$(X'X)^{-1}X'u)$$ is simply another regression. However, what is observed is not $$u$$ but $$\hat{u}$$ and $$\mathbb{E}(X \hat{u})$$ is by construction of the estimator (FOC of MSE) equal to $$0$$, thus the bias cannot be measured. This can be seen by,

\begin{equation}
	(X'X)\hat{\beta} = X'y \iff
\end{equation}

\begin{equation}
	(X'X)\hat{\beta} = X'(X \hat{\beta} + \hat{u})
\end{equation}

\begin{equation}
	(X'X)\hat{\beta} = (X'X)\hat{\beta} + X'\hat{u}
\end{equation}

\begin{equation}
X'\hat{u} = 0
\end{equation}

This means that the observed correlation between $$X$$ and $$\hat{u}$$ is $$0$$. It also means that the sum of $$\hat{u}$$ is equal to $$0$$ if we have an intercept in the model, since that represents a column of $$1$$'s in the $$X$$ matrix.

## The IV estimator
\begin{equation}
	\hat{\beta} = \frac{(z'z)^{-1}z'y}{(z'z)^{-1}z'x} = (z'x)^{-1}z'y
\end{equation}

For more information see e.g. [wiki](https://en.wikipedia.org/wiki/Instrumental_variables_estimation)

## Numerical example
\begin{equation}
	y = 0.5x + u
\end{equation}

\begin{equation}
	x = z + v
\end{equation}

where $$z$$ is distributed $$\mathcal{N}(2,1)$$ and $$(u,v)$$ are distributed multivariate normal with means $$0$$, variances $$1$$ and correlation $$0.8$$. This means that $$x$$ is correlated with $$u$$, hence the regression of $$y$$ on $$x$$ is inconsistent and biased.

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

Running the regressions,

```R
reg_xy <- lin_reg(x, y, intercept = TRUE)
```

The estimated coefficients, where the first element is the intercept and the second element the slope,

```R
> reg_xy[['coeffs']]
           [,1]
[1,] -0.8003143
[2,]  0.8978703
```

Running the IV estimation,

```R
reg_xzy <- lin_reg(x, y, z, intercept = TRUE, instrument = TRUE)
```

```R
> reg_xzy[['coeffs']]
          [,1]
[1,] 0.0139782
[2,] 0.4887241
```

By observing the results the IV estimator is accurate in estimating the true dgp whereas the biased/inconsistent estimator is not.

## How does the bias depend on the correlation?
One question that comes to mind is how does the biased/inconsistent estimator perform under different correlations? In the previous example a quite large correlation between $$u$$ and $$v$$ was imposed.

The following piece of code simulates data from $$0$$ correlation between $$u$$ and $$v$$ to correlation $$1$$. Then it plots the coefficients and we can observe how the estimated coefficients change when the correlation increases.

```R
set.seed(42)
z <- rnorm(10000, 2, 1)

coeffs_a<- list()
coeffs_b <- list()

for (i in seq(0, 1, by=0.01)){
  sigma <- matrix(data = c(1, i, i, 1), nrow = 2, ncol = 2)
  uv <- mvrnorm(n = 10000, mu = c(0,0), Sigma = sigma)
  u <- uv[,1]
  v <- uv[,2]
  
  x <- z + v
  y <- 0.5*x + u
  
  reg_xy <- lin_reg(x, y, intercept = TRUE)
  
  coeffs_a[[toString(i)]] <- reg_xy[['coeffs']][1,]
  coeffs_b[[toString(i)]] <- reg_xy[['coeffs']][2,]
}
```

![Fig 1](/images/beta_alpha.png)

The beta coefficient starts at the true value when the correlation is $$0$$ (as does the intercept) and then linearly deviates. This seems bad, however what happens if we plot the $$x$$ and $$y$$ data together with different regression lines corresponding to different correlations? How off are they? To give "less" advantage to the biased/inconsistent regression, I plot simulated data where $$u$$ and $$v$$ has zero correlation.

![Fig 2](/images/diff_corr.png)

Note that you need to evalute the intercept at $$(0,0)$$.

Although, the red line with higher bias has a poorer fit it might be an okay approximation depending on the task at hand.

## Conclusion
Endogeneity is a problem in academic econometrics, it distorts the interpretation of the coefficients and the explanatory power of the model. However, the biased estimator still seems to capture the overall correlation direction and from a modelling point of view it might be used with caution. To end with a cliche, 'all models are wrong...'.