---
layout: post
title: "Graphics Resources Collection" 
date: 2016-10-20
---

## SIGGRAPH 

- [Casting curved shadows on curved surface](http://cseweb.ucsd.edu/~ravir/274/15/papers/p270-williams.pdf) This is the original idea 
    of shadow mapping which widely used in shadow computation. No math formula in paper, it is very good for beginners.
- [Simulation of Wrinkled Surfaces](http://research.microsoft.com/pubs/73939/p286-blinn.pdf) The paper presents normal mapping ideas first time. 
- [Computer Graphics Paper collection](http://kesen.realtimerendering.com/) 
- [Multi-View Intrinsic Images of Outdoors Scenes with an Application to Relighting](http://www-sop.inria.fr/reves/Basilic/2015/DRCLLPD15/)
- [Real-Time Polygonal-Light Shading with Linearly Transformed Cosines](https://eheitzresearch.wordpress.com/415-2/)
- [A System for Rapid Exploration of Shader Optimization Choices](http://graphics.cs.cmu.edu/projects/spire/)
- [The Sketchy Database: Learning to Retrieve Badly Drawn Bunnies](http://sketchy.eye.gatech.edu/)
- [Reconstruction of Personalized 3D Face Rigs from Monocular Video](http://gvv.mpi-inf.mpg.de/projects/PersonalizedFaceRig/)
- [Realtime 3D Eye Gaze Animation Using a Single RGB Camera](http://faculty.cs.tamu.edu/jchai/projects/projects.htm)

## Graphic Course & Homepage 

- [Real-Time High Quality Rendering](http://cseweb.ucsd.edu/~ravir/274/15/274.html) The course has some advanced topics.
- [CMU Graphics Course Collection](http://graphics.cs.cmu.edu/?page_id=16) Lots of materials for you to start CG explore.
- [Stanford Graphics Course Collection](http://graphics.stanford.edu/courses/) Slides Papers and Assignments   
- [UBC Computer Graphics Course Modeling](http://www.cs.ubc.ca/~sheffa/dgp/sylabus.html)
- [Introduction to 3D Image Generation](http://web.cse.ohio-state.edu/~hwshen/781/Site/Main.html)
- [Stanford Matt homepage](https://graphics.stanford.edu/~mdfisher/index.html)
- [Pixar Engineer Prideout github page](http://github.prideout.net/)
- [hardware github repo](https://github.com/hardware)

## OpenGL

- [OpenGL3.x Specification](https://www.opengl.org/registry/doc/glspec32.core.20091207.pdf) This is an OpenGL specifications 
    containing lotsof detailed description of OpenGL design. Perhaps We should regard it as a dictionary for lookup when in need.  
- [Next Generation OpenGL](https://www.khronos.org/assets/uploads/events/Next-Generation-OpenGL-Dec14.pdf)The slide has a very
    concrete description about 3D API of Khonoros group. It provides a family tree clearly depicting the relationship between
    OpenGL, OpenGL ES, WebGL. It is very helpful to new beginners. Next generation API design is the main focus of slide, a
    comparison list about traditional API and next-generation API design can tell lots of stories. Platform diversity and
    radically changed GPU architecture drive such changes. 
- [Ambient Diffuse Specular Lighting Model](http://www.learnopengl.com/#!Lighting/Basic-Lighting) The web link may help to understand 
    basic ideas of ADS.  
- [Graphics Gems PDF Version](https://github.com/tl3shi/books/tree/master/GameDev/Graphics) It is a very good collection of Graphics 
    related books.  
- [OpenGL Shader tutorials](https://www.opengl.org/sdk/docs/tutorials/TyphoonLabs/Chapter_4.pdf) talks about some advancted lighting 
    techniques widely used in OpenGL, including per-pixel illumination model, bump mapping, displacement mapping and so on.
- [glsl Debugger](http://glsl-debugger.github.io/)

## Bump Mapping, Normal Mapping and Displacement mapping

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

The creation of the surface is based on equation.

- [Survey on NURBS](http://design.osu.edu/carlson/history/PDFs/Piegl-NURBS-91.pdf)
- [OpenGL Programming about NURBS](http://www.glprogramming.com/red/chapter12.html)
- [Splines Surface Math Representation](http://mrl.nyu.edu/~perlin/courses/spring2009/splines4.html)
    clear to understand.
- [Deformation Styles on github repo](https://github.com/sp4cerat/Deformation-Styles-using-Spline-Skinning)

## BRDF

- [Disney BRDF analysis Tool](http://www.disneyanimation.com/technology/brdf.html)
- [Survey on BRDF Representation](http://www.cs.princeton.edu/~smr/cs348c-97/surveypaper.html)

## Tessellation

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

- [Coding Training Material](https://github.com/ADN-DevTech/Maya-Training-Material)
- [Maya plugins on github](https://github.com/illmillrig?tab=repositories)
- [Maya plugins](https://github.com/appleseedhq)
- [Maya Face Tracking Plugins](https://github.com/oscarwestberg/Face-Tracking-Maya)
- [Kara Blog](http://www.karajensen.com/)
- [Tree Generator Plugin](https://github.com/karajensen/tree-generator)
- [Maya Plugin](https://github.com/illmillrig/SurfaceAttach)
- [Maya NURBS Patch](http://www.3dtutorials.michaelorourke.com/tutorials/Modeling/Basics/NurbsPatchsIntro12.pdf)    

## Nvida Sample Code

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

## Windows

- [MSDN Subscription Download](https://msdn.microsoft.com/en-us/subscriptions/downloads/)
- [Windows USB DVD Download Tool](https://www.microsoft.com/en-us/download/windows-usb-dvd-download-tool)
- [Dx 11 tutorial](http://www.rastertek.com/tutdx11.html)
- [Where is DXSDK 2015 version](https://blogs.msdn.microsoft.com/chuckw/2015/08/05/where-is-the-directx-sdk-2015-edition/)
- [DXSDK Sample](https://blogs.msdn.microsoft.com/chuckw/2013/09/20/directx-sdk-samples-catalog/)

## Ray Tracing

- [Stanford course](http://candela.stanford.edu/cs348b-14/doku.php) It introduces the basic ideas about ray tracing.
- [99 lines ray tracer toy](http://www.kevinbeason.com/smallpt/)
- [Ray tracing overview](http://www.scratchapixel.com/lessons/3d-basic-rendering/ray-tracing-overview)
- [pixar explanation about tray tracing](https://renderman.pixar.com/view/raytracing-fundamentals)
- [ray tracing tutorial](https://www.ics.uci.edu/~gopi/CS211B/RayTracing%20tutorial.pdf)
- [light transport simulation algorithms](http://iliyan.com/publications/VertexMerging)
    This page includes paper, code, results and other usefull materials.
- [Monte Carlo Method](http://www.scratchapixel.com/lessons/mathematics-physics-for-computer-graphics/monte-carlo-methods-in-practice/monte-carlo-integration)
- [Random number generator](http://www.agner.org/random/?e=0,34)
- [Random number generator](http://www.maths.manchester.ac.uk/~ahazel/VBAC++_coursework3.pdf)
- [Monte Carlo Simulation](http://ww2.odu.edu/~agodunov/teaching/notes/Cp01_random.pdf)
