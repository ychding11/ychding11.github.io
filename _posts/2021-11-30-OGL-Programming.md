---
layout: post
title: "OpenGL Programming" 
date: 2021-11-30
---

It would introduce OpenGL programming by a simple example.

## OpenGL Programming

Most of 3D models are presented by triangles. Those triangles make the surface of 3D objects. Rendering is the process to transform 3D models into 2D image. It maybe done by an offline path tracer or a real time renderer. The topic today is about real time rendering by OpenGL. It is grouped into four parts : Geometry data preparation, Shading surface, Show shading result, and Manipulation.

OpenGL is not an API, a specification, developed and maintained by the [Khronos Group](http://www.khronos.org/).  Khronos publicly hosts all specification documents for all the OpenGL versions. The OpenGL specification specifies exactly what the result/output of each API should be and how it should perform. It is up to vendors to do the implementation. OpenGL specification does not give implementation details, Vendors may have different implementations. But they shall comply with the specification (and are thus the same to the user).

OpenGL's core-profile is a division of OpenGL's specification that removed all old deprecated functionality(fixed function pipeline). When using OpenGL's core-profile, OpenGL forces to use modern practices. Whenever we try to use one of OpenGL's deprecated functions, OpenGL raises an error and stops drawing. 

OpenGL is by itself a large state machine: a collection of variables on which OpenGL currently operate. The OpenGL libraries are written in C generally. 

### Geometry data prepare 

### Shading

### Show shading result

### Manipulation

## Screen Shot



## Reference

- [OpenGL - The Industry Standard for High Performance Graphics](https://www.opengl.org/)
- [OpenGL registry](https://www.opengl.org/registry/),  the OpenGL specifications and extensions for all OpenGL versions.
- [solid wireframe]( https://developer.download.nvidia.com/whitepapers/2007/SDK10/SolidWireframe.pdf)

