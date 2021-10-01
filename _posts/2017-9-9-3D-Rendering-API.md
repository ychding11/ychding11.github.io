---
layout: post
title: "Graphics API" 
date: 2017-09-09
---

This post sumarize key points of graphic API.


## D3D11
### Input Layout
- How to understand input layout object in D3D11 and  how does GPU Hardware use them ?
  - *D3D11_INPUT_ELEMENT_DESC*, describes each element's property, including data format, binding slot, offset, and stride.
  - `IASetVertexBuffers()`, binds vertex buffer to input assembler stage. including the beginning slot, number of buffers, array of buffer address, stride and offset.
  - with input layout object describing each element of each slot and each buffer's stride, GPU hardware should know how to read vertex data correctly.

### GPU resources and views
- Vertex buffer, index buffer, constant buffer, stream output buffer, these four kinds of buffers can be bind to pipe directly.
- Other buffers require a resource view in order to bind to pipe.
	- render target view(RTV), pipeline output buffer.
	- depth-stencil view(DSV), pipeline output buffer.
	- shader resource view(SRV), bind to shader stage, read only.
	- unordered access view(UAV), only bind to pixel shader, read and write by lots of thread.
- *ID3D11DeviceChild::SetPrivateData()* with GUID "WKPDID_D3DDebugObjectName" defined in d3dcommon.h can assign a debug name to GPU resource.
- How to understand GPU Buffer like *ID3D11Buffer* ? 
  - What information does the object hold ? A pointer to GPU buffer? 
  - CPU can access GPU's memory by mapping ? 
  - What does mapping mean ? 
  - Add page table entry of CPU ?

### Geometry Shader & Subdivision
Geometry Shader is different from Vertex shader.
- It is a per-primitive level processing. For example, point, line and triangle.
- It eats in a primitive and output zero or multiple primitive.
- New algorithms based on primitive can be applied. For example it can subdivide a surface.

In DX11 a geometry shader example is as following:

```
  //--------------------------------------------------------------------------------------
  // Geometry Shader
  //--------------------------------------------------------------------------------------
  [maxvertexcount(12)]
  void GS( triangle GSPS_INPUT input[3], inout TriangleStream<GSPS_INPUT> TriStream )
```

- key word **maxvertexcount**, limit the output vertex number.
- key word **triangle**, specify the operated primitive type.*GSPS_INPUT*, specify the vertex format, maybe user-defined structure.
- identifier *input*, specify that 3 vertex will be eat in.
- key word **TriangleStream**, specify the output will be *triangle strip*.
- After assemble an triangle, *TriStream.RestartStrip()* function call will cut *triangle strip* into *triangle list*.

There is a DX11 sample to implement exploding model by geometry shader only.
1. calculate surface normal of the triangle.
2. calculate the center position of the triangle.
3. generate three triangles with center point with position extruding towards surface normal.
4. complete geometry shader on [github](https://github.com/ychding11/directx-sdk-samples/blob/master/Direct3D11TutorialsFX11/Tutorial13/Tutorial13.fx).


### Tessellation
GPU Hardware includes three stages: **hull shader stage**, **fixed-function tessellator** and **domain shader stage**. Hull Shader process control patch. HW Tessellator generates lots of samples in domain field by a predefined rule. Domain Shader converts domain samples into vertex by control patch processed by Hull Shader.

- Hull Shader accepts control patch from vertex shader. It can be divided into two phases,
	1. Hull Shader processes once per **output control point** and its attribute independently, controlled by the **outputcontrolpoints** attribute of Hull Shader.
	2. Hull Shader processes once per **patch**, controlled by the**patchconstantfunc** attribute of Hull Shader. 
- Output control points pass on to domain shader directly. **pathconstantfunc** output tessellator factors.  
- Hull Shader would be regarded as a "data setup stage" of tessellation, accepting standard control patch and converting its format according to application configuration by APIs.
- Tessellator accepts tessellator factors and partition mode domain then generates uvw coordinates and output primitives. Tessellator does not care about control points at all.
- Domain Shader accepts Control patch, uv coordinates and generates final vertex.

Bezeir surface is good example to demonstrate this ideas, because it is very simple and easy to understand.
code on [github](https://github.com/ychding11/directx-sdk-samples/blob/master/SimpleBezier11/SimpleBezier11.hlsl)

- sematic value *SV_InsideTessFactor*, defines tessellation mount within a patch surface. [details](https://msdn.microsoft.com/zh-cn/library/windows/desktop/ff471572(v=vs.85).aspx)
- sematic value *SV_TessFactor*,  defines tessellation mount on patch edge.[details](https://msdn.microsoft.com/zh-cn/library/windows/desktop/ff471574(v=vs.85).aspx)
- sematic value *SV_DomainLocation*, it is an output value uvw coordinates by tessellator, defines the location on the hull of current domain point.
- *SV_InsideTessFactor* and *SV_TessFactor* are required input of Domain shader, Why? 

### rasterizer
1. How many HW rasterizers in GPU ? It handles a primitive one time.
2. Before rasterize begin, the primitive should have transformed from world space into clip space, divide by w and mapped into render target by viewport. Rasterizer determines primitive covered square in render target. How about a "pixel square" shared by multiple primitives? What if MSAA applies?
3. HW rasterizer just handles 3 kinds primitives: point, line and triangle. What's the rules for these primitives? 
4. In D3D11, *D3D11_FILL_MODE* only support two modes: *D3D11_FILL_WIREFRAME* and *D3D11_FILL_SOLID*. [details](https://msdn.microsoft.com/en-us/library/windows/desktop/ff476131(v=vs.85).aspx)

### what  does it require to draw a mesh ?
"Each draw method renders a single topology type. During rendering, incomplete primitives are silently discarded." 
What happens to Render Pipe state ? 

A typical render function does following things:
1. update word, view, projection information from user input.
2. Update per-frame information in constant buffer.
3. clear render target and depth stencil.
4. bind constant buffers of all stages.
5. set shaders of each stage.
6. set rasterizer state.
7. bind vertex data and set primitive type.
8. Draw() call

### Compile Shader
*D3DCompileFromFile()* compiles hlsl code into byte code for specified target, for example, hs_5_0. The second parameter **in_opt  const D3D_SHADER_MACRO pDefines** can add user-defined macro into compiler.

- What happens from byte code to specified shader program ?
- The compiling is done by API, is it possible to compile in multi-thread ?

### environment map
It is a GPU programming hack to implement **specular reflective surface** by texture sampling.
- Generate a cube map to represent the environment irradiance. Specular effect depends on view. It is either only for static surroundings or dynamically generating texture .
- It is Pixel Shader to sample cube map normally. The sampling vector is reflected by view vector.
- In order to reduce computing, sampling vector can be calculated in Vertex Shader in view space. Then interpolate in screen space.

A good example is preferred.

### shadow map
It is to generate shadow by two pass render.
1. Render a depth map in light camera.  Z value in which space is better ?
2. Pixel Shader determine whether a pixel is in shadow by sampling the depth map.
   - convert pixel(x, y) into world space, then convert into light camera space.
   - compare newly generated z with stored in shadow map to determine whether it is in shadow.
   - caution for z flighting because of precision issue.

### reference
- [MSDN Subscription Download](https://msdn.microsoft.com/en-us/subscriptions/downloads/)
- [Windows USB DVD Download Tool](https://www.microsoft.com/en-us/download/windows-usb-dvd-download-tool)
- [DX 11 Tutorials](http://www.rastertek.com/tutdx11.html)
- [Where is DX SDK 2015 version](https://blogs.msdn.microsoft.com/chuckw/2015/08/05/where-is-the-directx-sdk-2015-edition/)
- [DX SDK Sample](https://blogs.msdn.microsoft.com/chuckw/2013/09/20/directx-sdk-samples-catalog/)
- [What is WARP](https://msdn.microsoft.com/en-us/library/windows/desktop/gg615082%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)
- [HLSL Shader Model 5](https://msdn.microsoft.com/en-us/library/windows/desktop/ff471419(v=vs.85).aspx)
- [msbuild  /p:Configuration=Release FaceOSC.vcxproj](http://code.dblock.org/2009/02/13/how-to-do-a-debug-release-or-both-builds-with-msbuild.html)

