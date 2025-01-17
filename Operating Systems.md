
An Operating System is software that bridges the user with the hardware in order to do more efficient computing.

Drivers are also software that introduce the hardware to the operating system. 

A computer system is roughly divided into 4 components:

* **Hardware**: Provides basic computing resources
* **Application Programs**: Define how resources in the computer are used. 
* **Operating System**: Controls the hardware and coordinates its use among the various application programs.
* **User**

**An operating system is simliar to a government, it provides an environment within which programs can do useful work**

An operating system contains:

* **Kernel**
* **Middleware Frameworks**
* **System Programs**
# Computer System Organization

For controlling external devices like Keyboards, mouse, etc, a computer has a device controller and provides the Operating System with a uniform interface for controlling the device.

![[2024-12-04_17-51.png]]

Operating systems typically have a **device driver** for each device controller.
## Interrupts

Through a typical computer operation, the computer requires communication between different controllers. How a CPU knows the state of a controller's operation for example, when it's doing an I/O operation, interrupts are signals that it uses.

Hardware may trigger an interrupt at any time by sending a signal to the CPU. When an interrupt is recieved, the CPU stops what it is doing and immediately transfers execution to a fixed location where the starting address of where the interrupt service routine instructions are located.

Once an interrupt occurs, the CPU starts processing a service routine related to that interrupt signal. A straightforward way of doing this is to devise a generic service routine to call the appropriate interrupt routine.

Since interrupts need to be handled quickly,

## I/O Structure








