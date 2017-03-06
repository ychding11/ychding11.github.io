---
layout: post
title: "Ray and objects intersection algorithms" 
date: 2015-04-21
---
This post give a simple summary of ray intersection with simple shapes.

## ray reflection angle

![reflection model]({{ site.url }}/images/shapeIntersection/reflection-angle.jpg "Reflection Model")   
The image demostrates how to calculate the reflection vector by incident vector.

## line segment and 2D rectangle intersection algorithms

Cohen-Sutherland algorithm is a very efficient algorithm for testing lots of lines intersection 
with 2D rectangle.    
![line segment and rectangle intersection region]({{ site.url }}/images/shapeIntersection/intersection-line-rectangle-region.png  "Line and Rectangle intersection region division")   
![line segment and rectangle intersection case]({{ site.url }}/images/shapeIntersection/intersection-line-rectangle-case.png  "Line and Rectangle intersection case summary")   

