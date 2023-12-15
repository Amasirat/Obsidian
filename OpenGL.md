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