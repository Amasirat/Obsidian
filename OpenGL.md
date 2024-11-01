OpenGL is a Graphics API that can be used to create graphical applications.

It is not a framework, It is simply a standard created by the Khronos Group that different graphics manufacturers create implementations for the functions.

## State machine

OpenGL is a giant **State Machine**, a collection of variables that specify how OpenGL should currently operate. The sate of OpenGL is called **The Context**.

OpenGL has in general two kinds of functions:

* **State Changing Functions,** change the context.
* **State Using Functions,** that do operations based on the current context.

Everything that we do in OpenGL is in some way just trying to alter this giant state machine to draw the particular pixels that we want on screen.
## Objects

An object is a collection of options that represents a subset of OpenGL's state.

We can define more than one object in our applications, set their options and whenever we use the OpenGL's state, we bind desired objects with our preferred settings. 

## Setup and Initialization Boilerplate

```C++
//init boilerplate
glfwInit();
glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
constexpr int w_height{800};
constexpr int w_width{600};
const char* w_title{"triangle"};
GLFWwindow* window{glfwCreateWindow(w_height, w_width, w_title, nullptr, nullptr)};
if(window == nullptr){
std::cout << "glfwCreateWindow returned null address\n";
glfwTerminate();
return -1;
}
glfwMakeContextCurrent(window);
if(!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress)){
std::cout << "Glad was not initialized\n";
glfwTerminate();
return -1;
}
```

glViewport() adjusts the size of the render window. In order to calibrate it incase the user changes the window size, we have to call this function everytime a change happens. To do that we can create a framebuffer_size_callback() function and give that to glfwSetFramebufferSizeCallback() function as a function pointer.

```C++
void framebuffer_size_callback(GLFWwindow* window, int width, int height)
{
	glViewport(0, 0, width, height);
}

int main() 
{
...

framebuffer_size_callback(window, w_width, w_height);
glfwSetFramebufferCallback(window, framebuffer_size_callback);

}

```

## Hello Triangle

Everything in OpenGL is in 3Dspace, but the window is a 2D array of pixels. A lot of OpenGL's work is to translate 3D coordinates to 2D pixels and that is done by the **graphics pipeline**.

Graphics Pipeline is a complex and beast of a pipeline that does many complex things with multiple steps and programs.

Small programs that specify graphical data and are executed by the graphics card are called **Shaders**.

A **vertex** is a collection of data per a 3D coordinate. It could be anything, for example it could only consist of a 3D position and color values or much more complex things.

The graphics pipeline can be divided into several steps where each step requires the output of the previous step:

* Vertex Shader
* Geometry Shader
* Shape assembly
* Tests and Blending
* Fragment Shader
* Rasterization

A **fragment** in OpenGL is all the data required to draw a single pixel on screen like color values and such.

OpenGL only processes vertices if they are between -1.0 and 1.0 which we say the vertex is in **Normalized Device Coordinates(NDC)**.
Any vertex outside this range will be outside the OpenGL window.

Note: In computer graphics, color is represented as an array of 4 values: red, green, blue, alpha (opacity) which is abbreviated to **RGBA**. each value can get a number between 0.0 and 1.0.
## Rendering a triangle

We have to first define an array containing three points of the traingle called vertices.
```C++
float vertices {
	-0.5f, -0.5f, 0.0f,
	 0.5f, -0.5f, 0.0f,
	 0.0f,  0.5f, 0.0f,
};
```

After defining the vertices of a triangle, they have to be sent to the first stage of the graphics pipeline. To do that, we have to create memory on the GPU and store the vertices there. 

Sending Data to the GPU is slow, therefore we need to do that all in one batch and not do it a lot continously. It's like when you have to bring all the bags of fruit your mom bought into the house. It'll be much faster if you bring as much as possible with you to the house even though you will be spent in the process. If you don't do that, it'll be less strain but it'll take more time.

In OpenGL, we create **Vertex Buffer Objects** that store vertices, so that we can send large amounts of vertex data at once.

Objects are created and accessed through handles which are of the unsigned int type or Gluint.
An example of creating a vertex buffer object:

```C++
unsigned int VBO;
glGenBuffers(1, &VBO);//1 means creating only 1 buffer and we send a reference to VBO which is a handle for our Buffer
glBindBuffer(GL_ARRAY_BUFFER, VBO);//first parameter is the type of buffer
```

Because OpenGL is a state machine, you won't be drawing an "object" but more so you'll have the buffers in memory and simply select which buffers to load by the glBindBuffer function. the unsigned int is going to be the handle for a specific buffer you had generated.

There are many types of buffers in OpenGL and GL_ARRAY_BUFFER is the type of a vertex buffer object, because at the end of the day our triangle is just an array of float numbers.

Then finally we copy the vertices we defined into our buffer object by making a call to glBufferData()

```C++
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
```

The first parameter is the type of buffer object, second is the size of the vertex data, the third is the vertex data itself and the fourth parameter is the drawing method.

OpenGL has three ways of dealing with vertex data in buffers:

* GL_STREAM_DRAW: The data is set only once and is used at most a few times
* GL_STATIC_DRAW: The data is set only once, and used many times.
* GL_DYNAMIC_DRAW: The data changes a lot and is also used a lot.

A usuage of GL_DYNAMIC_DRAW will ensure the data is placed in a way that reading and writing into it is faster. It could be useful for drawing the player in videogames for example.

Next we have to create both a **vertex** and **fragment** shader that can process the array of vertices we put in the GPU's memory.

For drawing there are a few functions that handle that and we can use in our render loop.

```cpp
glDrawArrays(GL_TRIANGLES, 0, 3);//Start from 0, then pass in the amount of 2D vertices
```

But it can't draw anything if a shader is not defined and the program doesn't know what the data in the buffers actually mean.

## Tell the program what the data in the buffers mean.

Using the glVertexAttribArray(), we can give context to the data we have given to the buffer.

[This is the documentation for this function](https://docs.gl/gl4/glVertexAttribPointer)

The vertices in the buffer are indexed much like an array. each index contains the information for one vertex.

* index: From which index should it be read. From 0, means start from the starting vertex.
* size: this means how many points of data one vertex is, if a vertex contains only 2 positions, then this will be 2. Size can only be an integer between 1 and 4.
* stride: How many bytes are dedicated for each vertex in the buffer
* pointer: define the offset from which each type of vertex is separated. you can use offsetof() macro to do this dynamically.

# Shaders

A shader is basically a program that runs on the GPU.

*Shaders are written in GLSL(OpenGL Shading language)*

The two most popular types of shaders are:

* Vertex Shaders: Runs for every vertex of a shape and determines the position.
* Fragment Shaders: Fills in the pixels between the vertices. For a triangle, it's similar to coloring in the outline. It runs for every pixel between the vertices. It usually determines the texture or color.

**Because fragment shaders can run hundreds or thousands of times, they are important thresholds for optimizations and performance bottlenecks.**

**Rasterization** is basically the process of rendering a shape on screen.

Because OpenGL is a state machine, Shaders are also enabled or disabled inside the context.

Data is sent to a shader in the form of a **uniform**.

Here is a simple vertex shader for our traingle:

```GLSL
#version 330 core
layout (location = 0) in vec3 aPos;
void main()
{
	gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);
}
```

# Index Buffers

*OpenGL renders everything based on triangles, for example a rectangle will basically be two triangles stuck together*

It'll be wastefull to repeat vertices that we already have inside the buffer just to draw them again, therefore we can use index buffers.

```C++
float positions[] = {
	-0.5f, -0.5f, // 0
	 0.5f, -0.5f, // 1
	 0.5f,  0.5f, // 2
	-0.5f,  0.5f, // 3
};
// we still load in the buffers the same as always, however when it comes to drawing we use an index buffer to indicate a particular part of the buffer to the GPU
unsigned int buffer;
// generate a buffer and assign it to traingle_buffer
glGenBuffers(1, &buffer);
// bind the id
glBindBuffer(GL_ARRAY_BUFFER, buffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(positions), positions, GL_STATIC_DRAW);
  
glVertexAttribPointer(0, 2, GL_FLOAT, GL_FALSE, sizeof(float) * 2, 0);
glEnableVertexAttribArray(0);

unsigned int indices[] = {
	0, 1, 2,
	2, 3, 0
};

unsigned int ibo;
glGenBuffers(1, &ibo);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, ibo);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(unsigned int) * 6, indices, GL_STATIC_DRAW);

// render loop
while(...)
{
	glDrawElements(GL_TRIANGLE, 6, GL_UNSIGNED_INT, nullptr);
}
```

**The indices array HAS to be unsigned int**

# Error Checking

There are two main ways to check for errors in OpenGL.

* glError: returns an error flag if one has occured. repeated calling will give back more error flags if they exist.
* glDebugMessageCallback: It's a new edition to OpenGL from 4.2 onwards. It gives more detailed information about its errors.

# Uniforms

A way for us to pass in data from the code to the shader.
















