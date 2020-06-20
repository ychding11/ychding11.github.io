---
layout: post
title: "Building tools" 
date: 2016-11-08
---
This post list links of widely used building tools.

## CMake 
+ [cmake useful variables](https://cmake.org/Wiki/CMake_Useful_Variables). predefined cmake varialbles.
- [cmake document](https://cmake.org/cmake/help/v3.5/manual/cmake-buildsystem.7.html)
- `cmake -DCMAKE_BUILD_TYPE=Debug/Release  path-to-CMakeList.txt`
+ `cmake -h` checks available generator. `cmake -G generator_name` specifies the generator name.
- [include()](https://cmake.org/cmake/help/v3.0/command/include.html) run cmake code frome an 
  external file. Search path priorty is listed in link.
- [add_subdirectory()](https://cmake.org/cmake/help/v3.0/command/add_subdirectory.html) add a 
  submodule into build. This submodule always resident in a independent folder.
- judge a varialbe in CMake. `if(NOT DEFINED VAR_NAME) and if(NOT DEFINED ${VAR_NAME})` . The first one is for varialbe, the second is for contents of the variable.
- [condition syntax](https://cmake.org/cmake/help/latest/command/if.html#condition-syntax) It is how to judge a statement is true or false.
- [set normal variable & cache entry](https://cmake.org/cmake/help/latest/command/set.html)

## MSBuild
- Build release version, `MSBuild.exe mySolution.sln /p:Configuration=Release /p:DebugSymbols=false`.

## CMakeLists.txt sample
