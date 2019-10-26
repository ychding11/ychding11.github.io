---
layout: post
title: "Summary on Signal FIlter" 
date: 2017-09-09
---

- wider filter function means narrow frequency spectrum of that function. f(x/2) is wider than f(x) in time domain.
- filter kernel becomes wider, low frequency magnititude will become higher. Wider means slower change, slower change means strong low frequency component.
- "Gibbs Phenomenon" : reconstruct squre signal by sinc(x). It oscillates at discontinuities point.

- fetch a value by "sample point" in a texture. filter several texels around "sample point". 
  - "point filter" by box function (sinc() in frequency domain, sinc() is smoother and wider, thus significant postaliasing) 
  - "triangle filter" by triangle function (sinc2() in frequency domain)
  
