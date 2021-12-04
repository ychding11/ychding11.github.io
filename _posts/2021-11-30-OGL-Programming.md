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

A **Vertex Array Object** (VAO) is an [OpenGL Object](https://www.khronos.org/opengl/wiki/OpenGL_Object) that stores all of the state required for vertex data . It appears since Core profile 3.0.  In OpenGL compatibility  profile, VAO object 0 is a default object. But in OpenGL core profile it makes VAO object 0 not an object at all. 
- The format of the vertex data （vertex attribute）
  - Each attribute can be enabled or disabled 
  - If access is disabled, any reads to that attribute would return a constant value (SPEC specify ?)
  - A newly-created VAO disable  all attributes

- Buffer Object(VBO) storing the vertex data
- Map between Vertex buffer binding and vertex attribute

 VAO have normal creation, destruction, and binding functions like other OpenGL Object. The following is just an example.

```c
        glGenVertexArrays(1, &m_vao_id);
        glBindVertexArray(m_vao_id); //< only one target, No explicit specify

        glGenBuffers(1, &m_vbo_id);
        glBindBuffer(GL_ARRAY_BUFFER, m_vbo_id);
        glBufferData(GL_ARRAY_BUFFER, m_vb_size_in_bytes, m_vertices.data(), GL_STATIC_DRAW);

        glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, vertexStride, 0);
        glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, vertexStride, (GLvoid*)normalOffset);
        glVertexAttribPointer(2, 3, GL_FLOAT, GL_FALSE, vertexStride, (GLvoid*)tangentOffset);
        glVertexAttribPointer(3, 2, GL_FLOAT, GL_FALSE, vertexStride, (GLvoid*)texcoordOffset);

        glEnableVertexAttribArray(0);
        glEnableVertexAttribArray(1);
        glEnableVertexAttribArray(2);
        glEnableVertexAttribArray(3);

        glBindVertexArray(0);
```



### Shaders & Shading

Shaders are the code which can be running on certain programmable stages of modern GPU for some purpose, such as visual computing, AI model training. OpenGL shaders are written in the [OpenGL Shading Language](https://www.khronos.org/opengl/wiki/OpenGL_Shading_Language). Modern GPU supports the following shader type. Each of them serves different purpose.

- Vertex shader
- Tessellation control shader
- Tessellation evaluation shader
- Geometry shader
- Fragment shader
- Compute shader

A [Program Object](https://www.khronos.org/opengl/wiki/Program_Object) can contain the executable code for all of the [Shader](https://www.khronos.org/opengl/wiki/Shader) stages. Building programs requires  two steps : Compile & Link.

### shader compile

- shader source code is first fed into a compiler, it would generate a *shader object*
  -  Shader compilation failure is not an [OpenGL Error](https://www.khronos.org/opengl/wiki/OpenGL_Error)
  - developer need to check for it manually
- one or more shader objects are linked into a program object

An example of Shader code compiling is like following.

```c

	//< creates an empty shader object for the shader stage given by given type
	GLuint shaderID = glCreateShader(type); 
    char const* tempPtr = shaderCode.c_str();
	//< Feed shader source code into OpenGL
	//< here we feed 1 string
	//< NULL, OpenGL will assume all of the strings are NULL-terminated
    glShaderSource(shaderID, 1, &tempPtr, NULL);
	//< Compiles the given shader
    glCompileShader(shaderID);

	GLint success = 0;
    glGetShaderiv(shaderID, GL_COMPILE_STATUS, &success);
	if (success == GL_FALSE) //< the most recent compilation failed
	{
		//< query how many bytes to allocate, the length includes NULL terminator.
		GLint maxLength = 0;
		glGetShaderiv(shaderID, GL_INFO_LOG_LENGTH, &maxLength);
		std::vector<GLchar> errorLog(maxLength);
		//< NULL, don't care actually written bytes
		glGetShaderInfoLog(shaderID, maxLength, NULL, &errorLog[0]);
		Err("{}", std::string(errorLog.begin(), errorLog.end()));
		return 0;
	}
```

### shader link

- create a program object

  - ```c
    GLuint glCreateProgram();
    ```

- attach shader objects into it

  - ```c
    void glAttachShader(GLuint program, GLuint shader);
    ```

  - It is OK to attach multiple shader objects for one shader type

  - But only one of those shader objects can have a **main** function

- linking

  - Each shader stage's code is separate from others. They have their own global variables, functions ...
  - Certain definitions are considered shared between shader stages
    - [uniforms](https://www.khronos.org/opengl/wiki/Uniform_(GLSL))
    - [buffer variables](https://www.khronos.org/opengl/wiki/Shader_Storage_Buffer_Object)
    - buffer-backed interface blocks

  - linking may fail because of
    - mismatch between shader stage
    - violation of shader stage limitations
    - call to not defined functions

  - linking errors can also be detected and fetched just like compiling errors

- clean up

  - After linking , it is a good idea to detach all shader objects 
  - If the shader objects are NOT used in future, delete it


### Program Introspection

Program Introspection is a mechanism to querying information about a program object, so as to interact with it. For example, setting or getting a uniform variable.

- The introspection API is built around basic types, see following example

  ```glsl
  struct A_Struct
  {
    vec4 first;
    vec2 second;
    vec2 third[3];
  };
  uniform A_Struct unif;
  //< struct itself cannot be queried via the introspection API
  //< but its member such as unif.first can be
  ```
  
  

### Show shading result

GLFW

### Manipulation

Camera

## Screen Shot



## Reference

- [OpenGL - The Industry Standard for High Performance Graphics](https://www.opengl.org/)
- [OpenGL registry](https://www.opengl.org/registry/),  the OpenGL specifications and extensions for all OpenGL versions.
- [VAO](https://www.khronos.org/opengl/wiki/Vertex_Specification#Vertex_Array_Object)
- [OpenGL Shader](https://www.khronos.org/opengl/wiki/Shader)
- [OpenGL Shader Compilation](https://www.khronos.org/opengl/wiki/Shader_Compilation)
- [Program Introspection](https://www.khronos.org/opengl/wiki/Program_Introspection)
- [solid wireframe]( https://developer.download.nvidia.com/whitepapers/2007/SDK10/SolidWireframe.pdf)

