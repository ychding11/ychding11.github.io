---
layout: post
title: "Graphics resources collection" 
date: 2016-10-20
---

This post lists a collection of usefull graphic resources.

## Useful Papers 
---

- [Computer Graphics Paper collection](http://kesen.realtimerendering.com/) 
- [Casting curved shadows on curved surface](http://cseweb.ucsd.edu/~ravir/274/15/papers/p270-williams.pdf) This is the original idea 
    of shadow mapping which widely used in shadow computation. No math formula in paper, it is very good for beginners.
- [Simulation of Wrinkled Surfaces](http://research.microsoft.com/pubs/73939/p286-blinn.pdf) The paper presents normal mapping ideas first time. 
- [Multi-View Intrinsic Images of Outdoors Scenes with an Application to Relighting](http://www-sop.inria.fr/reves/Basilic/2015/DRCLLPD15/)
- [Real-Time Polygonal-Light Shading with Linearly Transformed Cosines](https://eheitzresearch.wordpress.com/415-2/)
- [A System for Rapid Exploration of Shader Optimization Choices](http://graphics.cs.cmu.edu/projects/spire/)
- [The Sketchy Database: Learning to Retrieve Badly Drawn Bunnies](http://sketchy.eye.gatech.edu/)
- [Reconstruction of Personalized 3D Face Rigs from Monocular Video](http://gvv.mpi-inf.mpg.de/projects/PersonalizedFaceRig/)
- [Realtime 3D Eye Gaze Animation Using a Single RGB Camera](http://faculty.cs.tamu.edu/jchai/projects/projects.htm)
- [Approximating Catmull-Clark Subdivision Surfaces with Bicubic Patches](http://www.cs.cmu.edu/afs/cs/academic/class/15869-f11/www/readings/loop08_accpatches.pdf)
- [A Language for Shading and Lighting Calculations](http://www.cs.cmu.edu/afs/cs.cmu.edu/academic/class/15869-f11/www/readings/hanrahan90_rsl.pdf)
- [State of the Art in Monte Carlo Ray Tracing for Realistic Image Synthesis](https://inst.eecs.berkeley.edu/~cs294-13/fa09/lectures/course29sig01.pdf)
- [A Framework for Realistic Image Synthesis](http://www.graphics.cornell.edu/pubs/1997/GTS+97.pdf)


## Computer Graphics Courses & Useful Homepage 
---

### courses
- [Visual Computing Systems, CMU](http://graphics.cs.cmu.edu/courses/15869/fall2014/)
- [Parallel Computer Architecture and Programming, CMU](http://15418.courses.cs.cmu.edu/spring2016/home)
- [Real-Time High Quality Rendering, UCSanDiego](http://cseweb.ucsd.edu/~ravir/274/15/274.html) The course has some advanced topics.
- [Graphics Course Collection, CMU](http://graphics.cs.cmu.edu/?page_id=16) Lots of materials for you to start CG explore.
- [Graphics Course Collection, Stanford](http://graphics.stanford.edu/courses/) Slides Papers and Assignments   
- [Computer Graphics Course Modeling, UBC](http://www.cs.ubc.ca/~sheffa/dgp/sylabus.html)
- [Introduction to 3D Image Generation](http://web.cse.ohio-state.edu/~hwshen/781/Site/Main.html)

### homepages
- [Stanford Matt homepage](https://graphics.stanford.edu/~mdfisher/index.html)
- [Pixar Engineer Prideout github page](http://github.prideout.net/)
- [hardware github repo](https://github.com/hardware)

## misc
- [Lighting Model](http://huanzhewu.github.io/2015/08/06/%E3%80%90CG%E8%AF%AD%E8%A8%80%E3%80%91%E5%B8%B8%E8%A7%81%E5%85%89%E7%85%A7%E6%A8%A1%E5%9E%8B%E8%A7%A3%E6%9E%90.html)

## OpenGL
---

- [OpenGL3.x Specification](https://www.opengl.org/registry/doc/glspec32.core.20091207.pdf) This is an OpenGL specifications 
    containing lots of detailed description of OpenGL design. Perhaps We should regard it as a dictionary for lookup when in need.  
- [Next Generation OpenGL](https://www.khronos.org/assets/uploads/events/Next-Generation-OpenGL-Dec14.pdf)The slide has a very
    concrete description about 3D API of Khonoros group. It provides a family tree clearly depicting the relationship between
    OpenGL, OpenGL ES, WebGL. It is very helpful to new beginners. Next generation API design is the main focus of slide, a
    comparison list about traditional API and next-generation API design can tell lots of stories. Platform diversity and
    radically changed GPU architecture drive such changes. 
- [Ambient Diffuse Specular Lighting Model](http://www.learnopengl.com/#!Lighting/Basic-Lighting), The link explains basic ideas of ADS.  
- [Graphics Gems PDF on github](https://github.com/tl3shi/books/tree/master/GameDev/Graphics), It is a collection of Graphics development books on github.  
- [OpenGL Shader tutorials](https://www.opengl.org/sdk/docs/tutorials/TyphoonLabs/Chapter_4.pdf), It talks about some advancted lighting techniques,including bump mapping, displacement mapping and so on.
- [glsl Debugger on github](http://glsl-debugger.github.io/)
- [OGL Extensions](https://www.opengl.org/archives/resources/features/OGLextensions/)

## Bump Mapping, Normal Mapping and Displacement mapping
----

- [Bump Normal Displacement mapping comparison](http://blog.digitaltutors.com/bump-normal-and-displacement-maps/)
    This blog post gives a comparison about bump mapping, normal mapping, displacement mapping.
    It is helpful for beginners to construct overview concepts about those techniques. 
- [Displacement Mapping Slide](https://perso.limsi.fr/jacquemi/OGL-4/OGL-4-slides.pdf)
    I thinks first things we need to know in those techniques is what info is stored and
    in what format. Then how these info is used or applied to what, light calculation?
    geometry modification? or others? These differences are key to identify them and that
    would enhance our understanding. Today I explore on google and find a nice slide about
    displacement mapping by accidently. So I put it here.

## Geometry Shader & Subdivision
----

- [Pixar Open Subdiv](http://graphics.pixar.com/opensubdiv/docs/subdivision_surfaces.html)
- [Maya NURBS Modeling](https://courses.cs.washington.edu/courses/cse459/06wi/help/mayaguide/Complete/NURBS.pdf)
    It has an detailed info about nurbs modeling. nurbs modeling is an comparison to subdivision surface. When modeling complex object, it 
    need lots of nurbs and deforming the object lead to cracks at seams.
- [Standford graphic course about Subdivision](http://graphics.stanford.edu/courses/cs468-10-fall/LectureSlides/10_Subdivision.pdf)
    It is a very good material about subdivision, containing background introduction, basic ideas and comparison.
- [Catmull-Clark Subdivision demo by example](http://www.rorydriscoll.com/2008/08/01/catmull-clark-subdivision-the-basics/)
- [GPU Subdiv on Windows on github repo](https://github.com/astrolagrange/GPU-based-feature-adaptive-rendering-of-Loop-subdivision-surfaces)
- [Planet LOD on github repo](https://github.com/sp4cerat/Planet-LOD)

## Parametric surface

The surface is based on math equation.

- [Survey on NURBS](http://design.osu.edu/carlson/history/PDFs/Piegl-NURBS-91.pdf)
- [OpenGL Programming about NURBS](http://www.glprogramming.com/red/chapter12.html)
- [Splines Surface Math Representation](http://mrl.nyu.edu/~perlin/courses/spring2009/splines4.html), it is clear and easy to understand.
- [Deformation Styles on github repo](https://github.com/sp4cerat/Deformation-Styles-using-Spline-Skinning)

## BRDF
---

### useful links
- [Disney BRDF analysis Tool](http://www.disneyanimation.com/technology/brdf.html)
- [Survey on BRDF Representation](http://www.cs.princeton.edu/~smr/cs348c-97/surveypaper.html)
- [BRDF Database](http://people.csail.mit.edu/wojciech/BRDFDatabase/)

The MERL BRDF database contains reflectance functions of 100 different materials.
Each reflectance function is stored as a densely measured Bidirectional Reflectance Distribution Function (BRDF)
The data is stored in $$ (\theta_h, \theta_d, \phi_d) $$ format which is in "half angle and difference" coordinate.
It is convert from $$ (\theta_i, \phi_i, \theta_o, \phi_o)$$ to $$ (\theta_h, \theta_d, \phi_d) $$.


## Tessellation
---

### useful links
- [Nvida](http://www.nvidia.com/object/tessellation.html) 
- [Math forum](http://mathforum.org/sum95/suzanne/whattess.html) 
- [Math fun](https://www.mathsisfun.com/geometry/tessellation.html) 
- [Cool Math](http://www.coolmath.com/lesson-tessellations-1) 
- [Intel Vtune](https://software.intel.com/en-us/node/596501) 
- [OpenGL wiki about Tessellation](https://www.opengl.org/wiki/Tessellation)
- [Tessellation on CodeFlow](http://codeflow.org/entries/2010/nov/07/opengl-4-tessellation/)
- [Tessellation Shader on github repo](https://github.com/NCCA/TessellationShader)
- [Tessellate Bezier Surface dynamicly on github repo](https://github.com/Jakub-Ciecierski/TessellationGFX)
- [Terrain Tessellation on github repo](https://github.com/hardware/TerrainTessellation)
- [Maya Subdiv Plugin](https://github.com/dnkv/MayaTSubdiv)
- [pyglet wiki](https://bitbucket.org/pyglet/pyglet/wiki/Home)
- [pyglet on pip](https://pypi.python.org/pypi/pyglet)
    

## Maya
---

- [Training Material of Maya Plugin](https://github.com/ADN-DevTech/Maya-Training-Material)
- [Maya plugins on github](https://github.com/illmillrig?tab=repositories)
- [Maya plugins](https://github.com/appleseedhq)
- [Maya Face Tracking Plugins](https://github.com/oscarwestberg/Face-Tracking-Maya)
- [Kara Blog](http://www.karajensen.com/)
- [Tree Generator Plugin](https://github.com/karajensen/tree-generator)
- [Maya Plugin](https://github.com/illmillrig/SurfaceAttach)
- [Maya NURBS Patch](http://www.3dtutorials.michaelorourke.com/tutorials/Modeling/Basics/NurbsPatchsIntro12.pdf)    
- [material X](http://www.materialx.org/)

### Maya Mel

-  string $result = getenv("MAYA_VP2_DEVICE_OVERRIDE");    print $result;
-  string $result = `optionVar -query vp2RenderingEngine`; print $result;
-  *radioButtonGrp* creates a radio button group UI.
-  *editorTemplate -addControl* specifiy which attribute you want to control. It will generate a UI element automatically.
-  *pluginInfo -q -loaded "dgProfiler"*

## Nvida Sample Code
---

- [NVIDIA Direct3D SDK 10 Code Samples](http://developer.download.nvidia.com/SDK/10.5/direct3d/samples.html#InstancedTessellation)
- [GPU GEMS 3 Articles](https://developer.nvidia.com/gpugems/GPUGems3/gpugems3_ch01.html)
- [GPU GEMS 2 CODE](http://http.download.nvidia.com/developer/GPU_Gems_2/CD/Index.html)
- [DX Samples](https://developer.nvidia.com/gameworks-directx-samples)
- [nVidia's GPU logical pipeline](https://developer.nvidia.com/content/life-triangle-nvidias-logical-pipeline)
- [nVidia's DX homepage](https://developer.nvidia.com/directx)
- [nVidia's DX11 Overview pdf](http://www.nvidia.com/content/nvision2008/tech_presentations/game_developer_track/nvision08-direct3d_11_overview.pdf)
- [How to Use DX11](https://msdn.microsoft.com/en-us/library/windows/desktop/ff476330(v=vs.85).aspx)
- [Introduce to Parallel Computing](http://courses.cs.washington.edu/courses/cse558/11wi/lectures/06-introToParallelProgramming_forWeb.pdf)
- [Windows Download](https://developer.microsoft.com/en-us/windows/downloads)

## Windows DirectX3D
---

### resources
vertex buffer, index buffer, constant buffer, stream output buffer, these four buffers can be bind to pipeline directly.
Other buffers need a resource view to bind to pipeline.
- render target view, pipeline output buffer.
- depth-stencil view, pipeline output buffer.
- shader resource view, bind to shader stage, read only.
- unordered access view, only bind to pixel shader, read and write by lots of thread.

### rasterizer
1. How many HW rasterizers in GPU? It handles a primitive one time.
2. Before rasterize begin, the primitive should have transformed from world space into clip space,
   divide by w and mapped into render target by viewport. Rasterizer determines primitive covered
   square in render target.
3. HW rasterizer just handles 3 kinds primitives: point, line and triangle. What's the rules for these
   primitives? How about a "pixel square" shared by multiple primitives? What if MSAA applies?


### reference
- [MSDN Subscription Download](https://msdn.microsoft.com/en-us/subscriptions/downloads/)
- [Windows USB DVD Download Tool](https://www.microsoft.com/en-us/download/windows-usb-dvd-download-tool)
- [DX 11 Tutorials](http://www.rastertek.com/tutdx11.html)
- [Where is DX SDK 2015 version](https://blogs.msdn.microsoft.com/chuckw/2015/08/05/where-is-the-directx-sdk-2015-edition/)
- [DX SDK Sample](https://blogs.msdn.microsoft.com/chuckw/2013/09/20/directx-sdk-samples-catalog/)
- [What is WARP](https://msdn.microsoft.com/en-us/library/windows/desktop/gg615082%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)
- [HLSL Shader Model 5](https://msdn.microsoft.com/en-us/library/windows/desktop/ff471419(v=vs.85).aspx)
- [msbuild  /p:Configuration=Release FaceOSC.vcxproj](http://code.dblock.org/2009/02/13/how-to-do-a-debug-release-or-both-builds-with-msbuild.html)


## Ray Tracing
---

- [Stanford course](http://candela.stanford.edu/cs348b-14/doku.php), It introduces the basic ideas about ray tracing.
- [99 ines ray tracer](http://www.kevinbeason.com/smallpt/)
- [Ray tracing overview](http://www.scratchapixel.com/lessons/3d-basic-rendering/ray-tracing-overview)
- [pixar explanation about tray tracing](https://renderman.pixar.com/view/raytracing-fundamentals)
- [ray tracing tutorial](https://www.ics.uci.edu/~gopi/CS211B/RayTracing%20tutorial.pdf)
- [light transport simulation algorithms](http://iliyan.com/publications/VertexMerging), This page includes paper, code, results and other usefull materials.
- [Monte Carlo Method](http://www.scratchapixel.com/lessons/mathematics-physics-for-computer-graphics/monte-carlo-methods-in-practice/monte-carlo-integration)
- [Random number generator](http://www.agner.org/random/?e=0,34)
- [Random number generator](http://www.maths.manchester.ac.uk/~ahazel/VBAC++_coursework3.pdf)
- [Monte Carlo Simulation](http://ww2.odu.edu/~agodunov/teaching/notes/Cp01_random.pdf)
- [PBRT Document](http://pbrt.org/users-guide.html)

## 3D Model Repo
---

- [3D Scans, a free 3D Model repo](http://threedscans.com/)

## GPU Hardware 
---

- [GPGPU-Sim](http://gpgpu-sim.org/) provides a detailed simulation model of a contemporary GPU running CUDA and/or OpenCL workloads.
- [softart](http://www.cnblogs.com/gongminmin/archive/2009/12/09/SoftArt.html)
- [GPU memory type comparision](https://www.microway.com/hpc-tech-tips/gpu-memory-types-performance-comparison/), it points out that *SHared Memory* is a very 
   important type of memory and introduce some internals in it.
- Texture memory is global memory. It is accessed by a dedicated pipe and caches. [reference on stackoverflow](https://stackoverflow.com/questions/8767059/texture-memory-in-cuda-concept-and-simple-example-to-demonstrate-performance)
- Bound Const Memory is accessed as c[bankIdx][offset], offset is 4 bytes aligned, bankIdx is the bank index.
- Bindless Const Memory is access as cx[memoryHeader][offset], offset is 4 bytes aligned. Memory header is consist of a Uniform register pair, totaly 64 bits.
  19 bits MSB is 16 bytes aligned for bank size with max size 64KB, 45 bits LSB is 16 bytes aligned for virtual address.


