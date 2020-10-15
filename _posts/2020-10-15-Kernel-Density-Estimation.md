---
layout: post
title: Kernel Density Estimation [ml]
---

# Introduction
If one has a data sample with an unknown distribution, then there are non parametric ways to
estimate the distribution. One is by making a histogram, another, slightly more sophisticated
solution, would be to use KDE (kernel density estimation).

# KDE
KDE can be used to estimate the PDF (probability density function). Consider,

\begin{equation}
    \hat{f}_h (x) = \frac{1}{n} \sum_{i=1}^n K_h (x - x_i)
\end{equation}

\begin{equation}
    = \frac{1}{nh} \sum_{i=1}^n K \left( \frac{x-x_i}{h} \right)
\end{equation}

where $$x_i$$, $$i = 1, 2, ..., n$$, is a univariate i.i.d. variable from an unknown distribution
with density $$f \forall x$$, and $$K$$ is a kernel with bandwidth $$h$$. For each data point a
kernel is fitted, then summed up. Note that the kernel is a function of $$x$$, thus programming
wise, we need to create a linspace. For kernels such as the Gaussian, where the Pdf goes from
$$- \infty$$ to $$\infty$$, one has to make a decision regarding what range one should include in
the estimation, and henceforth the graphical analysis.

The kernel function, also called window or tapering function, is a non negative weighting function.
Common kernels include; uniform, triangular, Epanechnikov, cosine, and Gaussian. The choice of
kernel function is not terribly important in most applications. What matters more is often the
choice of the bandwidth, $$h$$, which is a parameter of smoothness.

When choosing $$h$$ there is a
trade off between a too low $$h$$, leading to noise, and a too high $$h$$ leading to over
smoothening. One can think of the optimization of $$h$$ as similar to choosing the bin width in a
histogram, or degree when fitting a polynomial. This trade off is easily simulated, e.g., draw from
a standard normal, plotting the true distribution, and some KDEs with different bandwidth. A decent
guess of $$h$$ can be obtained from cross validation or certain rules given some assumptions. One
commonly used rule, for one dimensional data, is Silverman's rule, for a generalization see Scott's
rule. Although it assumes that the data is normally distributed
it usually performs okay for unimodal distributions (one peak) and is a good initial guess, however
it performs not as well for multimodal distributions.

Silverman's rule chooses $$h$$ such that,

\begin{equation}
    h = \left( \frac{4 \hat{\sigma}^5}{3n} \right)^{\frac{1}{5}} \approx 1.06 \hat{\sigma}
    n^{-\frac{1}{5}}
\end{equation}

# Example
In this particular piece of code I wanted to estimate the distribution of the length of documents,
more precisely the number of tokens after pre-processing, in a text corpus.

```Python3
# Import packages.
import pickle
import numpy as np
import matplotlib.pyplot as plt

# Load docs.
infile = open('docs.p', 'rb')
docs = pickle.load(infile)
infile.close()

# List of lengths.
len_total = []
for i in range(len(docs)):
    len_total.append(len(docs[i]))

len_total = np.array(len_total)

# Gaussian function. h is standard deviation.
def gaussian(x, x_0=0, sd=1):
    gaussian = np.exp(-(x-x_0)**2/(2*sd**2))/(sd*np.sqrt(2*np.pi))
    return gaussian

# Smoothing/bandwidth parameter with Silverman, h is sd if Gaussian kernel.
h = 1.06*np.std(len_total)*len(len_total)**(-1/5.0)

# The Gaussian curve goes from + to - infinity.
stepsize = 1000
start = 0
stop = 7000 # Decides the x-axis of the plot
X = np.linspace(start, stop, stepsize)

# KDE function.
def kde(X, data, h):
    n = len(data)
    m = len(X)
    density = np.zeros(m)
    for i in range(n):
        density += gaussian(X, data[i], h)
    density = density / n
    return density

density = kde(X, len_total, h)
mean_len = np.round(np.mean(len_total), 2)

# Plot for total length.
plot1 = plt.plot(X, density, label='Density estimate')
plot2 = plt.plot([np.mean(len_total), np.mean(len_total)], [0, 0.00035], '--',
        label=f'Mean: {mean_len}')
plt.legend()
plt.grid()
plt.title(f'Gaussian Kernel Density Estimation: h= {np.round(h,2)}')
plt.xlabel('Words per document')
plt.ylabel('Probability')
plt.show()
```

<img src="/images/kde_50.png" width="600" height="600">
<img src="/images/kde_175.png" width="600" height="600">
<img src="/images/kde_500.png" width="600" height="600">

Looking at the figures with different bandwidth, it is clear that a too low $$h$$ creates noise, whereas
a too high $$h$$ creates too much smoothness. The larger $$h$$ the more mass is around $$0$$, this
is not desirable since in reality not many observations are close to $$0$$.
In this application Silverman's rule seems to perform well.

The distribution looks like a Poisson distribution. This makes sense since the number of words in a
document is not symmetric around zero. Depending on the type of documents it is natural, as in my
case, that there are few documents with very few words.
