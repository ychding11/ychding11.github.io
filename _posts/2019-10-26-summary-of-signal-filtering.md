---
layout: post
title: "Summary on Signal Filter" 
date: 2019-10-26
---

summary on digital signal filtering, especially on 2D.

- wider filter function means narrow frequency spectrum of that function.
  - example : f(x/2) is wider than f(x) in time domain.
- filter kernel function becomes wider, low frequency magnititude will become higher. 
  - wider in time means slower change, slower change means low frequency component becomes major one.

- "Gibbs Phenomenon" : 
  - When reconstruct squre signal by sinc(x), it oscillates at discontinuities point.

- fetch a value by "sample point" in a texture. filter several texels around "sample point". 
  - "point filter" by box function (sinc() in frequency domain, sinc() is smoother and wider, thus significant postaliasing) 
  - "triangle filter" by triangle function (sinc2() in frequency domain)
  
