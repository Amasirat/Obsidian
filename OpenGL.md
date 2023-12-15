OpenGL is a Graphics API that can be used to create graphical applications.

It is not a framework, It is simply a standard created by the Khronos Group, that different graphics manufactureres create implementations for the functions.

## State machine

OpenGL is a giant **State Machine**, a collection of variables that specify how OpenGL should currently operate. The sate of OpenGL is called **The Context**.

OpenGL has in general two kinds of functions:

* **State Changing Functions,** change the context.
* **State Using Functions,** that do operations based on the current context.


## Objects

An object is a collection of options that represents a subset of OpenGL's state.

We can define more than one object in our applications, set their options and whenever we use the OpenGL's state, we bind desired objects with our preferred settings. 

## Hello Triangle

Everything in OpenGL is in 3Dspace, but the window is a 2D array of pixels. A lot of OpenGL's work is to trnslate 3D coordinates to 2D pixels and that is done by the **graphics pipeline**.

Graphics Pipeline is a complex and beast of a pipeline that does many complex things with multiple steps and programs.

Small programs that specify graphical data and are executed by the graphics cards are called **Shaders**.

A **vertex** is a collection of data per a 3D coordinate. It could be anything, for example it could only consist of a 3D position and color values or much more complex things.

The graphics pipeline can be divided into several steps where each step requires the output of the previous step:

* Vertex Shader: takes in a single vertex, 
* Geometry Shader
* Shape assembly
* Tests and Blending
* Fragment Shader
* Rasterization


A **fragment** in OpenGL is all the data required to draw a single pixel on screen.


Note: In computer graphics, color is represented as an array of 4 values: red, green, blue, alpha (opacity) which is abbreviated to **RGBA**. each value can get a number between 0.0 and 1.0.

