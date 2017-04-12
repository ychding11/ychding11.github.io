---
layout: post
title: "Sampling and Monte Carlo integration"
date: 2017-04-10
---

The post summarizes sampling and monte carlo integration in computer graphics.


## Monte Carlo Integration 

Why Monte Carlo integration is a must in computer graphics? It tries to estimate integral value by random 
sampling. The number of sample is independent of dimensionality of integral. A Monte Carlo estimator is defined by:

$$ F_N=\frac{1}{N} \displaystyle \sum_{i=1}^{N} \frac{f(X_i)}{p(X_i)}$$

$$ f(x) $$ is evaluated function, $$ p(x) $$ is PDF of random variable $$ X_i $$.
We can easily understand that expect value of random varible $$ F_N $$ is equal to $$ \int_0^\infty f(x) \,\mathrm{d}x $$.   

$$ E[F_N] $$ = $$ E[\frac{1}{N} \displaystyle \sum_{i=1}^{N} \frac{f(X_i)}{p(X_i)}] $$   

$$ E[F_N] $$ = $$ \frac{1}{N} \displaystyle \sum_{i=1}^{N} \int_0^\infty \frac{f(x)}{p(x)} p(x) \, \mathrm{d}x $$   

$$ E[F_N] $$ = $$  \int_0^\infty f(x) \, \mathrm{d}x $$   


How many samples does it need to estimate integral value correctly or with mimimal error? There is no detailed 
explaination in pbrt book. I am confused.

Given a PDF, how to draw samples from it, the book<sup>754</sup> gives steps:
1. compute CDF from PDF $$p(x)$$, $$P(x)=\int_0^x f(y) \, \mathrm{d}y$$. 
2. compute inverse function $$P(x)^{-1}$$.
3. get uniform distributed random varible $$\xi$$.
4. compute $$X_i$$ = $$ P(\xi)^{-1}$$

If PDF $$p(x)$$ is similar to $$f(x)$$ in shape, estimator converges more quickly. It is explained in pbrt book<sup>793</sup> 
*Importance Sampling*. It is a common technique to reduce sampling variance in rendering. But it still has problems. In many
cases, the integrand is the product of more than one function. How to find a PDF similar to integrand in such cases? For 
example in computer graphics.   

$$L_o(p,\omega_o)=\int_{\mathcal{s}^2} f(p,\omega_i,\omega_o) L_i(p,\omega_i) \cos(\theta) \, \mathrm{d}\omega $$.
$$\approx \frac{1}{N} \displaystyle\sum_{j=1}^{N} \frac{f(p,\omega_j,\omega_o) L_i(p,\omega_j) \cos(\theta_j) }{p(\omega_j)} $$


