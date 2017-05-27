---
layout: post
title: "Building tools" 
date: 2016-11-08
---
This post list links of widely used building tools.

## CMake 
+ [cmake useful variables](https://cmake.org/Wiki/CMake_Useful_Variables) 
- [cmake document](https://cmake.org/cmake/help/v3.5/manual/cmake-buildsystem.7.html)
- `cmake -DCMAKE_BUILD_TYPE=Debug/Release  path-to-CMakeList.txt`
+ `cmake -h` checks available generator. `cmake -G generator_name` specifies the generator name.
- [include()](https://cmake.org/cmake/help/v3.0/command/include.html) run cmake code frome an 
  external file. Search path priorty is listed in link.
- [add_subdirectory()](https://cmake.org/cmake/help/v3.0/command/add_subdirectory.html) add a 
  submodule into build. This submodule always resident in a independent folder.

## MSBuild
- Build release version, `MSBuild.exe mySolution.sln /p:Configuration=Release /p:DebugSymbols=false`.

## CMakeLists.txt sample
