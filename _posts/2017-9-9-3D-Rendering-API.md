---
layout: post
title: "3D Graphics Library" 
date: 2017-09-09
---

This post lists a key points of graphic Library.


## DirectX3D
---

### resources
vertex buffer, index buffer, constant buffer, stream output buffer, these four buffers can be bind to pipeline directly.
Other buffers need a resource view to bind to pipeline.
- render target view, pipeline output buffer.
- depth-stencil view, pipeline output buffer.
- shader resource view, bind to shader stage, read only.
- unordered access view, only bind to pixel shader, read and write by lots of thread.

### Geometry Shader & Subdivision
Geomery Shader is different from Vertex shader.
- It is a per-primitive level processing. For example, point, line and triangle.
- It eats in a primitive and output zero or multiple primitive.
- New algorithms based on primitive can be applied. For example it can subdivison a surface.

In DX11 a geometry shader example is as following:

{% highlight c++ %}

//--------------------------------------------------------------------------------------
// Geometry Shader
//--------------------------------------------------------------------------------------
[maxvertexcount(12)]
void GS( triangle GSPS_INPUT input[3], inout TriangleStream<GSPS_INPUT> TriStream )

{% endhighlight %}

- key word *maxvertexcount*, limit the output vertex number.
- key word *triangle*, specify the operated primitive type.*GSPS_INPUT*, specify the vertex format, maybe user-defined structure.
  identifier *input*, specify that 3 vertex will be eat in.
- key word *TriangleStream*, specify the output will be *triangle strip*.
- After assemble an triangle, *TriStream.RestartStrip()* function call will cut *triangle strip* into *triangle list*.

There is a DX11 sample to implement exploding model by gemomery shader only.
1. calculate surface normal of the triangle.
2. calculate the cector position of the triangle.
3. generate three triangles with center point with position extruding towards surface normal.


### tessellation
It converts control patch into surface patch. GPU do it in parallel. It includes three stage: hull shader stage, fixed-function tessellator and
domain shader.
- Hull Shader accecpts control patch from vertex shader. It process every control point and its attribute indepently.
- Constant Hull Shader processes every control patch indepently. 
- Tessellator generates uv coordinates.
- Domain Shader accepts Control patch, uv coordinates and generates final vertex.

Bezeir surface is good example to demostrate this ideas, because it is very simple and easy to understand.

### rasterizer
1. How many HW rasterizers in GPU? It handles a primitive one time.
2. Before rasterize begin, the primitive should have transformed from world space into clip space,
   divide by w and mapped into render target by viewport. Rasterizer determines primitive covered
   square in render target.
3. HW rasterizer just handles 3 kinds primitives: point, line and triangle. What's the rules for these
   primitives? How about a "pixel square" shared by multiple primitives? What if MSAA applies?

### environment map
It is a GPU programming hack to implement specular refelctive surface.
- Generate a cube map to represent the eviroment irradiance. It depends on camera view, so it needs to be dynamically generated.
- It is in Pixel Shader to sample cube map to get the reflected irradiance. The sample vector is key point.
- In order to reduce compute, sample vector can be calculated in Vertex Shader in view space. Then interpolate in screnn space.

A goode example is preferred.

### reference
- [MSDN Subscription Download](https://msdn.microsoft.com/en-us/subscriptions/downloads/)
- [Windows USB DVD Download Tool](https://www.microsoft.com/en-us/download/windows-usb-dvd-download-tool)
- [DX 11 Tutorials](http://www.rastertek.com/tutdx11.html)
- [Where is DX SDK 2015 version](https://blogs.msdn.microsoft.com/chuckw/2015/08/05/where-is-the-directx-sdk-2015-edition/)
- [DX SDK Sample](https://blogs.msdn.microsoft.com/chuckw/2013/09/20/directx-sdk-samples-catalog/)
- [What is WARP](https://msdn.microsoft.com/en-us/library/windows/desktop/gg615082%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)
- [HLSL Shader Model 5](https://msdn.microsoft.com/en-us/library/windows/desktop/ff471419(v=vs.85).aspx)
- [msbuild  /p:Configuration=Release FaceOSC.vcxproj](http://code.dblock.org/2009/02/13/how-to-do-a-debug-release-or-both-builds-with-msbuild.html)

