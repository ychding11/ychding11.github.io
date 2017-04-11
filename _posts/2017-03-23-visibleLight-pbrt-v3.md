---
layout: post
title: "Visible light"
date: 2017-03-23
---

The post summarizes how to analyse visible light in compute graphics. 


## spectral power distribution

Visible light wavelength is between $$[380nm, 780nm]$$ and human visual system is very sensitive 
to $$[400nm, 700nm]$$.[spectral power distribution](https://en.wikipedia.org/wiki/Spectral_power_distribution)
is a function of wavelength describing ligth energy distribution on each wavelength. So SPD $$S(\lambda)$$ exist 
in an infinite dimensional space. Researchers developed a method to represent it with *basis functions and coefficients*.
This concept is introduced in pbrt book<sup>326</sup>.

How to sample SPD and what is proper sample rate?

[Spectral sensitive curve](https://en.wikipedia.org/wiki/Spectral_sensitivity) can be regard as normal human visual
system response to visible light. Three spectral sensitive peaks can be represented by three dimensional space. It 
is called [LMS color space](https://en.wikipedia.org/wiki/LMS_color_space). What color space does is to map phycially
generated colors into description of human sensation. XYZ color space is device invariant and used as a base reference
for other color space. 

## Radiometric Quantities

A photon's energy is $$Q=\frac{hc}{\lambda}$$. It's obvious that high frequency photon has high energy.
Radiant flux is represented by $$\Phi=\frac{\mathrm{d}Q}{\mathrm{d}t}$$. Its unit is watts(W). Visit pbrt
book<sup>346</sup> for details.   

Most frequently used quantity is irradiance(E) which is area density of radiant flux arriving at a surface.
So its unit is $${W}/{m^2}$$ and represented by E. A point light, its irradiance is $$ E=\frac{\Phi}{4\pi r^2}$$.
If light has a angle with surface normal then $$ E=\frac{\Phi cos(\theta)}{A}$$. It is called Lambert's Law,
on book<sup>348</sup>.   

Intensity is the angular density of emmited power, denoted by I, with unit $$W/sr$$. It is defined by solid angle.
$$I=\frac{\mathrm{d}\Phi}{\mathrm{d}\omega}$$. The difference is that E is defined by area.   
The most important radiometric quantity<sup>350</sup> is *Radiance*, L. It measures irradiance or radiant exitance with respcect to
solid angle. It does not care about light leaving or arriving. Its definition:

$$L(p,\omega)=\frac{\mathrm{d}E_{\omega}(p)}{\mathrm{d}\omega}$$.

$$E_{\omega}(p)$$ denotes irradiance at the surface which is perpendicular to solid angule $$\omega$$.
So we can regard radiance as flux density per unit area, per unit solid angle. It can also be defined as:
$$L=\frac{\mathrm{d}\Phi}{\mathrm{d}\omega \mathrm{d}A^\perp} $$.

$$A^\perp$$ is the projected area of $$A$$ on a hypothetical surface perpendicular to $$\omega$$.
Since Radiance does not care about light leaving or arriving, it is possible to make a distinction between 
radiance arriving at a point and radiance leaving that point. $$L_i(p,\omega)$$ and $$L_o(p,\omega)$$.

## reflection equation

