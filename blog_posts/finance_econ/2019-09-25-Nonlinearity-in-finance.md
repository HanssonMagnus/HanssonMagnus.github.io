---
layout: post
title: Nonlinearity in finance [finance]
---
Foundation for discussion:

In engineering the underlying statistical distributions that describe the data that is observed empirically are generally constant. If a certain engine part behaves in a certain way under certain conditions today, it will most likely do so tomorrow too. If the behavior of a photon is observed today and an approximation of the underlying quantum wave function is made, these results can be extrapolated to tomorrow's research.

This is however not generally true in the financial markets, the mere fact that an analysis is conducted will change the underlying data is sometimes argued. People are not machines that behave in a precise predictible fashion. In engineering over-fitting of historical/observed data is not a problem, since the physical objects that one is trying to model will behave in the same manner tomorrow. This means that nonlinear methods are extremely popular in engineering and the only real trade-off is nonlinearity versus computing power.

In finance very nonlinear methods can be problematic since one does not want to over-fit yesterday's data when trying to predict today.

The reason why this is important to understand is because this fact by itself limits the use of "high degrees of freedom methods", e.g. artificial neural networks, in economics and finance. Furthermore, it gives one an insight in which areas machine learning could excel in economics and finance, and one of those areas is preprocessing of data.

One example is text analysis or what is in computer science called natural language processing (NLP). The reason is of course that the regular statistical methods are not able to process text data, and first in order to analyze text data one has to transform the text into numerical values that are meaningful.

To end on a philosophical note, the nonlinear nature of financial markets makes nonlinear method themselves hard to use successfully in a prediction setting.
