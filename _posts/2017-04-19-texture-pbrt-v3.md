---
layout: post
title: "Texture"
date: 2017-04-19
---

The post summarizes Texture technique in pbrt book<sup>600</sup>.


## Texture sampling rate

Texture maybe the source of high frequency in final image and therefore cause texture aliasing.
Because texture function is always available in a convenient analytic form. It is possible to
remove high frequencies be sampling or avoid to introduce high frequency when evaluating the function.
First, we need to know how to calculate texture sampling rate in texture space<sup>602</sup>.

After getting texture sampling rate, it may need to apply filtering to texture function to remove high
frequencies beyond Nyquist limit.

### Generate texture coordinate
It is an active research area of computer graphics to create smooth parameterization of complex mesh with
little distortion<sup>612</sup>.

