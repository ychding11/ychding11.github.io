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
We can easily understand that the expect value of random varible $$ F_N $$ is equal to $$ \int_0^\infty f(x) \,\mathrm{d}x $$.
The derivation is as following:

$$ E[F_N] $$ = $$ E[\frac{1}{N} \displaystyle \sum_{i=1}^{N} \frac{f(X_i)}{p(X_i)}] $$

$$ E[F_N] $$ = $$ \frac{1}{N} \displaystyle \sum_{i=1}^{N} \int_0^\infty \frac{f(x)}{p(x)} p(x) \, \mathrm{d}x $$

$$ E[F_N] $$ = $$  \int_0^\infty f(x) \, \mathrm{d}x $$

How many samples does it need in order to estimate integral value correctly or with mimimal error? There is no detailed explaination in pbrt book.
So I am a little confused.

*Given a PDF, how to draw samples from it?* The book<sup>754</sup> gives steps:

1. compute CDF from PDF $$p(x)$$, $$P(x)=\int_0^x f(y) \, \mathrm{d}y$$. 
2. compute inverse function $$P(x)^{-1}$$.
3. get uniform distributed random varible $$\xi$$.
4. compute $$X_i$$ = $$ P(\xi)^{-1}$$

If PDF $$p(x)$$ is similar to $$f(x)$$ in shape, estimator converges more quickly which is explained in pbrt book<sup>793</sup> 
*Importance Sampling*. It is a common technique to reduce sampling variance in rendering. But it still has problems. In many
cases, the integrand is the product of more than one function. How to find a PDF similar to integrand in such cases? For
example in computer graphics, the following formula.   

$$L_o(p,\omega_o)=\int_{\mathcal{s}^2} f_r(p,\omega_i,\omega_o) L_i(p,\omega_i) \cos(\theta_i) \, \mathrm{d}\omega_i $$.
$$\approx \frac{1}{N} \displaystyle\sum_{j=1}^{N} \frac{f(p,\omega_j,\omega_o) L_i(p,\omega_j) \cos(\theta_j) }{p(\omega_j)} $$

### Russian Roulette
It is a technique used to increase Monte Carlo estimator efficiency. When calculating estimator, there are samples
contributing a little and cost a lot. In pbrt book<sup>786</sup>, there is an example to calculate reflected radiance
with direct lighting.
$$L_o(p,\omega_o)=\int_{\mathcal{s}^2} f_r(p,\omega_i,\omega_o) L_d(p,\omega_i) \cos(\theta_i) \, \mathrm{d}\omega_i $$.
When $$f_r(p,\omega_i,\omega_o)$$ is very small and $$\omega_i$$ is close to the horizon, the integrand's value is very
small and contribute little in final result.   

*Russian Roulette* solves this problem. It introduce a termination probability $$q$$ and some constant value $$c$$.
When termination condition is meet, use constant value $$c$$ instead of evaluating integrand with sample. So there 
are two cases, one is to evaluate integrand, another is constant value $$c$$. New estimator is combined by both with
wight $$\frac{-q}{1-q} $$ and $$\frac{1}{1-q}$$. New estimator is written as.
$$
F'=\begin{Bmatrix} 
\frac{F-qc}{1-q} \quad \xi>q \\
\, c             \quad \quad \text{otherwise} \\
\end{Bmatrix}
$$.

The expect value of new estimator is the same with the origin one. $$ E[F']=(1-q)(\frac{E[F]-qc}{1-q})+qc=E[F]$$.   
Note that Russian Roulette cannot reduce variance but increase efficiency. A properly chesen termination is very 
important to generate visually acceptable result.

### Splitting
Splitting<sup>788</sup> is another technique to increase Monte Carlo integration efficiency. It can use different sample number 
for different dimension. For example it may increase light sample and decrease image sample to get better visual
result without increasing total sample number.

## Metropolis Sampling

This technique is in book<sup>761</sup>. 

### Transform between distributions
We have a sample $$X$$ from PDF $$p_x(x)$$, but we want get a sample $$Y$$ from another PDF $$p_y(y)$$. 
All we needed is ensure CDFs are equal, $$P_x(x)=P_y(y)$$. So we can get the relation. 
$$p_x(x)\mathrm{d}x = p_y(y)\mathrm{d}y, \quad  p_x(x)=p_y(y)\frac{\mathrm{d}y}{\mathrm{d}x}$$.   
The transform formula is as.  $$\quad p(y)={P_y}^{-1} (P_x(x))$$.
This result is listed on book<sup>771</sup>, but no details about how this formula is get.

$$X$$ is a n-dimensional random varible and has PDF $$p_x(x)$$. For transform $$Y=T(X)$$, it has 
$$p_y(y)=p_y(T(x))=\frac{p_x(x)}{J_T(x)}$$.  $$\quad \boldsymbol T(x)=(T_1(x),T_2(x),\dotsc,T_n(x))$$. $$\quad J_T(x)$$ is 
T's Jacobian matrix.

$$\boldsymbol J_T(x)=
\begin{pmatrix}
\frac{\partial T_1}{\partial x_1} & \cdots & \frac{\partial T_1}{\partial x_n} \\
\vdots    & \ddots & \vdots    \\
\frac{\partial T_n}{\partial x_1} & \cdots & \frac{\partial T_n}{\partial x_n} \\
\end{pmatrix}
$$.


### Consine Weighted Hemisphere Sampling 
It is better to generate hemishpere samples close to the top of hemisphere. It means a smaller $$\theta$$ and a bigger
$$cos(\theta)$$ part of integral<sup>778</sup>. So the PDF of $$\omega$$ should meet the requirement.   

    $$p(\omega) \propto cos(\theta)$$.

According to Malley's method, the cosine weighted hemishere can be get by sampling on a unit disk uniformly
and projecting it up to the hemishpere above it<sup>778<sup>. 

### Importance Sampling
If PDF $$p(x)$$ is similar to $$f(x)$$ in shape, estimator converges more quickly. It is explained in pbrt book<sup>793</sup>.
