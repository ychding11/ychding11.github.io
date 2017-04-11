---
layout: post
title: "Camera in computer graphics" 
date: 2017-04-08
---
This post give a summary of camera sampling in Computer Graphics.

## Depth of field.

Len coordinate system setting: len is at $$ z=0 $$, film plane is at right side and 
scene at left side.

need a picture here to demostrate.
![focal plane]({{ site.url }}/images/camera/focal-plane.png "focal plane")  

$$ z'=\frac{zf}{z+f}$$

According to this fomular object at distance is focused. The range of distance from len in which
objects appears focused is called len's *depth of filed*.
A point that is not focused on focal plane as a point is imaged as a disk. The boundary of this 
disk is called *Circle of confusion*. The bigger its value the more blurriness on focal plane.
The following picture demostrate how to caculate its value by similar triangle. 

![circle of confusion]({{ site.url }}/images/camera/circle-of-confusion.png "circle of confusion")  


$$ d_c=\frac{fd_l}{z} $$
