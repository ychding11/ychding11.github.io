---
layout: post
title: "Visible light"
date: 2017-03-23
---

The post summarizes how to analyse visible light in compute graphics. 


## spectral power distribution

Visible light wavelength is between $$ [380nm, 780nm] $$ and human visual system is very sensitive 
to $$ [400nm, 700nm] $$.[spectral power distribution](https://en.wikipedia.org/wiki/Spectral_power_distribution)
is a funtion of wavelength describing ligth enery distribution on each wavelength. So SPD $$ S(\lambda)$$ exist 
in an infinite dimensional space. Researchers developed a method to represent it with *basis functions and coefficients*.

How to sample SPD and what is proper sample rate?

[Spectral sensitive curve](https://en.wikipedia.org/wiki/Spectral_sensitivity) can be regard as normal human visual
system response to visible light. Three spectral sensitive peaks can be represented by three dimensional space. It 
is called [LMS color space](https://en.wikipedia.org/wiki/LMS_color_space). What color space does is to map phycially
generated colors into description of human sensation. XYZ color space is device invariant and used as a base reference
for other color space. 

## reflection equation

