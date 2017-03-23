---
layout: post
title: "Ray and shape intersection algorithms" 
date: 2016-04-21
---
This post give a simple summary of ray intersection with simple shapes.

## ray reflection angle

![reflection model]({{ site.url }}/images/shapeIntersection/reflection-angle.jpg "Reflection Model")   
The image demostrates how to calculate the reflection vector by incident vector.

## line segment and 2D rectangle intersection algorithms

[Cohen-Sutherland algorithm](https://en.wikipedia.org/wiki/Cohen%E2%80%93Sutherland_algorithm)
is a very efficient algorithm for testing lots of lines intersection with 2D rectangle.   
The wikipedia page contains the very detailed description about algorithm and gives example code.

![line segment and rectangle intersection region]({{ site.url }}/images/shapeIntersection/intersection-line-rectangle-region.png  "Line and Rectangle intersection region division")   
![line segment and rectangle intersection case]({{ site.url }}/images/shapeIntersection/intersection-line-rectangle-case.png  "Line and Rectangle intersection case summary")   

## ray and axis-aligned bounding-box intersection algorithms

The algorithm is excerpted from *Physically Based Rendering - From Theory to Implementation 3rd edition*.     
The main idea is to compute 3 slabs intersection with ray independently in 3D space.
Each slab is the space determined by two planes. [Other materials](http://people.csail.mit.edu/amy/papers/box-jgt.pdf).    

Given plane: $$ x = c $$ and its normal is: $$ \overrightarrow{n}=(1,0,0)$$.

![ray and slab intersection]({{ site.url }}/images/shapeIntersection/intersection-slab.png  "ray and slab intersection")   

Suppose axis-aligned bounding box is determined by: $$ Box_{min} = (x_1, y_1, z_1), Box_{max} = (x_2, y_2, z_2) $$.     
  ray formula:  $$ ray = o + t * d $$.     
  plane formula: $$ a * x + b * y + c * z + d = 0 $$.    
  intersection param: $$ t_{1,2}$$.    
   $$ t_1 = min \left\{(x_1-o_x)/d_x,(y_1-o_y)/d_y,(z_1-o_z)/d_z \right\} $$  
   $$ t_2 = max \left\{(x_2-o_x)/d_x,(y_2-o_y)/d_y,(z_2-o_z)/d_z \right\} $$  
   
## point-triangle test in 2D

This section is about  how to judge whether a point is within a triangle in 2D space. Following are definitions:    
edges: $$ e0, e1, e2 $$, vertex: $$ p0=(x_0,y_0), p1=(x_1,y_1), p2=(x_2,y_2) $$, testing vertex: $$ p=(0,0) $$.     
edge function for $$ e_0 $$: $$ f(e_0)=(p-p_1)\bigotimes(p_2-p_1)$$.    
edge function for $$ e_1 $$: $$ f(e_1)=(p-p_0)\bigotimes(p_2-p_0)$$.    
edge function for $$ e_2 $$: $$ f(e_2)=(p-p_0)\bigotimes(p_1-p_0)$$.    

After simplification.    
 $$ f(e_0)=p \bigotimes (p_2-p_1) + (p_2 \bigotimes p_1)$$.    
 $$ f(e_1)=p \bigotimes (p_2-p_0) + (p_2 \bigotimes p_0)$$.    
 $$ f(e_2)=p \bigotimes (p_1-p_0) + (p_1 \bigotimes p_0)$$.    
If $$ p=(0,0)$$ then $$ f(e_0)= y_1 * x_2 - x_1 * y_2$$.    

According to property of cross product:     

1. if all edge function returns the same sign, the vertex is within triangle.
2. if one edge function returns 0, the vertex is on the edge of triangle.

## ray-triangle intersection test in 3D

The algorithm is excerpted from *Physically Based Rendering - From Theory to Implementation 3rd edition*.    
Main steps list as following.
1. transform *ray* and *triangle vertex* from *world space* into *ray space*.
2. convert 3D intersection into judging of 2D point $$ (0,0) $$ with a triangle.

