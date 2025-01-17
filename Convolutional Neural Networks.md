They are a specialized kind of [[Neural Networks]] designed to process and classify images.

What's different about a CNN then?

## Convolutional Layers

Convolutional Layers are the fundamental building blocks of CNNs. They perform a **Convolution**....duh. **Their main objective is extract features from images**.

They also use a special filter called a kernel. It is basically a nxn matrix (usually small numbers of n) that traverse through an image pixel by pixel. They then perform element-wise multiplication on the whole. The digits of the kernel are decided based on what you want to do. 

It's basically a convolution operation, multiplying the pixel values by the kernel values, then summing up the results.

0 1 2              0  1
3 4 5      \*      2  3          =   19   25
6 7 8                                  37  43

(0\*0) + (1\*1) + (3\*2) + (4\*3) = 19

The output matrix of this process is known as the **feature map**.

There are 3 essential components inside convolutional layers:

* **Channels**
* **Stride**
* **Padding**
### Channels

Digital images are composed of three channels: Red(R), Green(G), Blue(B), or RGB for short, each of them being represented as a separate matrix. For an RGB image, there might be three separate kernels for each color channel.

A depth of a layer refers to the **number of kernels it contains**. Each one produces a separate feature map. The collection of these feature maps completes the output of the layer.

There are typically three channels for each color channel however **there can be more, each one representing a feature it wants to extract.**
### Stride

It refers to how the kernel moves through an input image. The smaller the stride, the kernel will moves by less amount of pixels each time. 

This also affects the output size as well. **The greater the stride, the smaller the output matrix**.

We usually want to increase the stride of the filter to gather more global features of the image, while decreasing the stride to capture more fine-grained and minute details.

In additon, incrasing the stride will make computation more efficient (As there does not need to be as many multiplications and the output size will decrease)

### Padding

It refers to addition of extra pixels around the edge of an input image. On 0 padding, the filter traverses the edges of the input image fewer times than the center. Therefore by increading the padding of the image with extra pixels, we can capture more information from the borders of the image. **It increases the size of the output**

Output size is calculated like this:

$$
OutputSize = \frac{Input Size+2 \times Padding - KernelSize}{Stride} + 1
$$
Padding does not have to be symmetrical, padding could just mean padding the pixels in *one* direction, perhaps just the left side.

________________________________________________________________________

There are two more layers inside CNNs after the Convolutional Layers.

* Pooling Layers
* Flattening Layers

## Pooling Layers

It is also called the subsmapling layers and their objective is **Dimensionality Reduction**. It goes through the image with a size of the kernel, and it then uses two methods to resize it:

* Max Pooling: When it chooses the max value inside a part of the image it checks
* Average Pooling: When it averages all the numbers inside the section its checking.

For example a 4x4 feature map is resized to 2x2 throughout this procedure.

**Pooling Layers do not reduce the number of channels, each operation is applied independently to each channel of the input data.**

After convolution layers and then pooling layers finish what they're doing, then it is time for the **Flattening Layers** to do their job.

## Flattening Layers

Their job is to transform a matrix of nxn into a single vector of 2n length. They do not make any changes to the data, they just **flatten** the matrix in basic terms.

This is done because the Fully connected layers in the later stages do not support multi-dimensional data like the ones the convolutional layers produce. So this is done to make the data compatible for the Fully Connected or Dense layers.

We need these layers because they are essential for **Integrating features gathered by convolutinoal Layers into predictions**

# Activation Functions

The **Fully Connected Layers** and **Convolutional Layers** have activation functions which create non-linearity and can make training the CNN easier.

The **pooling** and **flattening** layers do not have activation functions.

* **ReLU**: The most common activation function. It allows for positive values and turns negative values into 0.

* **Modified ReLU**: A modified ReLU that allows small non-zero values for inactive nodes in order to not have dead neurons.

* **Sigmoid**: An old-fashioned activation function, restricts the value between 0 to 1. 

* **Tanh**: restricts value from -1 to 1. Gives great performance for some problems.


