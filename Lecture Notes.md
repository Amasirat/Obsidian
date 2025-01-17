The author's of Being the Center of Attention or BCA for short, aim to predict the personality of each individual based on video information. They propose that using information related to the context and environment of the scene can greatly increase accuracy of the result.

An important challenge in trying to quantify a nebulous concept such as Personality is identifying the right framework for personality analysis. Fortunately, many researches in the field of psychology have produced a popular, useful, and accurate framework for personality characterization called the Big-Five Personality Trait Model.
# Personality

The Big-Five Personality Trait model or Five-Factor model is comprised of five factors of personality that can describe a general assesment of an individual's personal inclinations with how they react in an environment.

## Extraversion

Captures sociability and energeticness in social situations. People with high extraversion thrive in social situations, where as people with low extraversion tend to prefer non-social situations.
## Neuroticism

Measures emotional reactivity and resilience. Low Neuroticism results in more emotional stability and high Neuroticism is characterized by mood swings or massive spikes in emotional stress. 
## Openness to Experience

Measures tendency for creativity and openness to new ideas. Individuals with high openness are often open-minded and adventurous where as individuals with lower levels of openness tend to prefer structured and traditional environments or methodologies.
## Conscienciousness

Measures how responsible, forward thinking, or goal oriented a person is. High levels of conscienciousness results in a person with high responsiblity, while low levels describe people who procrastinate, hate structure, and are more spontaneous people.
## Agreeableness

It reflects how much people prioritize the needs and feelings of others over their own and how much they value social bonds.
- **High Agreeableness:** Empathetic, altruistic, trustworthy, kind, and warm. These individuals are often well-liked and good at resolving conflicts. They're likely to avoid confrontation and strive for social harmony.
- **Low Agreeableness:** More skeptical, competitive, or even antagonistic. They may prioritize their own needs and desires over others, and they're more willing to engage in arguments or challenge social norms.

Many studies have been conducted around studying the relation of individual behaviors and their peronality labels. A great example is how pedestrians behave in the case of navigating a crowded area. Individuals are considered aggressive for example if their walking path is not influenced by the crowd. 
# Applications of the Study

This study proposes a few applications of its work.

Gathering real time infromation of a person's personality in a non-interactive system. There have been methods of gathering information about a user's inclinations and personality through their engagement with the system (For example, youtube, TikTok, etc detecting their users' video watching preferences) which are standardly called "active interaction", however by improving methods of **non-interactive** or **passive interaction**, the system can adapt to the user's current mood or personality by simply observing the environment and make the system more "human-like".

Behavioral monitoring can also be a useful application. For example, the dyanmics of a scene for bank's surveilence systems can help predict problematic individual and could even possibly prevent any heist in advance.

Automated personality recognition can help psychologists find out more about an individual without them having to fill out a questionare in advance.
# CNN

A CNN or Convolutional Neural Network is a specialized form of a Neural Netwok designed for **object detection** and **Image Classification**.

A general CNN architecture will be composed of these general parts:

* **Convolutional Layers**: A series of layers that perform convolutions on each pixel of the input, the purpose of these layers is to extract features from the image input.
* **Pooling Layer**: Or **sub-sampling** layers, these layers are for reducing the dimensions of the convolved image input. It uses two methods of resizing the input, **Max Pooling** where the max pixel value of a section is chosen and **Average Pooling** where the average of all pixel values in a section (4x4 or 2x2) are merged into one output image map.
* **Flattening**: Its job is to transform the nxm matrix into a 1-Dimensional vector of size n+m to be able to feed the next layer's input.
* **Fully-Connected Layers**: For integrating the features and facilitating learning.

## Convolutional Layers

Convolutional layers are comprised of three components:

* **Kernel**: It's a small filter that is convolved through the pixels of the input image. The process of convolution is this: The kernel matrix moves through the pixels and does an element-wise multiplication and produces an output image.
* **Padding**: we can put extra pixels around the sides of the input, so that the filter can convolve through the edges of the image better
* **Stride**: By how much does the kernel move through the image

For images with three or more channels, each channel can have its own kernel. 

## Pooling Layers

Pooling or subsampling layers that **reduce the dimension of the image**. There are two methods:

* Max Pooling: Where the maximum among a patch of pixel values is chosen to produce the output
* Average Pooling: Where the average of the numbers is taken.

## Flattening Layer

Its only job is to convert a matrix into a one-dimensional vector, so that it can be used as an input for the Fully Connected Layer.
## Fully Connected Layers

This layer's purpose is to integrate all the features extracted in the previous layers and to help facilitate learning for the model. Each node has a ReLU activation function and at the end it also has a **softmax** activation function that gives it the ability to classify images.

Each node is connected to all the nodes of the next layer with a weight of w. The activation value of each node is thereby calculated by:

$$
NodeActivation = ReLU((\sum_{k=0}^n a_{k}w_{k}) + bias)
$$

# Architecture

For any sequence of images such as video called a scene, the process of feature extraction is proposed to look something like this:

* **Individual Person Motion Describtor**: Values describing the features describing each individual in the scene.
* **Group Motion Describtor**: Describing the motion of individuals that are detected to have formed a **social group**
* **Proxemics Descriptor**: Describing the global positions of objects in the scene, it comes from the idea that human behavior is affected by their environment and proxemity to other individuals.

![[2024-12-08_10-55.png]]

Because this model aims to be a general framework for personality recognition, it can also describe non-social scenes as well. In that case, the **Group Motion Describtor** will be 0 and proxemics will refer to regions of interest in the scene deemed relevant to the individual. 
## Process of Feature Extraction: Social Scene

### Person Describtor

For every frame, a pose estimation algorithm is performed, in the case that a few joints were not detected, the missing joints are estimated by refering to its surrounding joints. Then a tracking algorithm is run through to track the relative motion of these joints over time. 4 reference joints are selected and every joint's distance to the reference joints. A matrix of values with dimensions of t x Jref-1 is built. For compactness sake, these data are converted to a cylindrical coordinates.

* **Ro**: The euclidean distance of each joint to its reference joint
* **Theta**: The angle of that distance
* **Z**: The vertical difference of each joint

Each row of the matrix represents the values of one timestep.

Then the three descriptor values are converted into values between 0-255.
### Social Group Descriptor

A Social group is detected using an approach of statistical analysis of F-formations. Once discovered a similar process as the process of obtaining individual descriptors is applied to every member of the group except the target individual. Since the members of a social group can be variable, a pooling strategy is used. Average pooling of these descriptors have been shown to have higher accuracy in most cases.
## Proxemics

The global distance between each individual in the scene is calculated. Z is 0 here because a vertical difference is not relevant.

Based on studies done by psychologists, it is discovered that proxemics or proxemity to others has some correlation with their personality and behavior. For instance, people that are shy, tend to be further away from the rest of the group.

![[2024-12-08_11-08.png]]

## Process of Feature Extraction: Non-Social scene

The same process for extracting individual motion descriptors is similar to a social scene. However Social Group Descriptors are set to zero and proxemics is proxemity to notable objects in the scene.

The idea is that because of the social nature of humans, individuals tend to Anthropomorphisize objects around them, this process can intensify in the case of extreme lonely situations (Show an example image of the movie Cast Away). Therefore, the way individuals interact with their surrounding scene can help gather latent information for helping to deduce their personality labels.

The surrounding regions of the individuals are discovered in an unsupervised way. The scene is divided into patches and it is assumed that arm motions of the individual are the most important data, however since overall motion can skew arm motions, the total motion of the individual is subtracted.

For locating regions where interactions occur a Guassian Mixture Model is used.
# Feature Learning

A pretrained VGG19 CNN trained on ImageNet is used. A CNN architecture is used because of their ability to find common latent representations inside input data.

Because it would require more training to be able to send the data collected from the features that we are concerned with, the data was to be put as descriptors and scaled to a series of pixel values from 0 to 255, forming a gray scale image. This way we can rest assured that the architecture is modular enough to accomodate any sort of training model.

After features from the three descriptors are extracted by the convolutional layers, they're fused together and converted into one single feature vector and fed into the FC.

Fusion is used because it can improve recognition performance, because it combines relevant information like person motion and context motion into one data.
# Qualitive and Quantitive Results

Three datasets are used to train this model.

* Salsa Poster Presentation: A presentation for a group of scientists
* Salsa Party: During coffee break of the presentation, where subjects are free to converse with each other.
* A series of scenes recorded by the team themselves, where subjects were tasked to accomplish specific tasks inside a room in isolation.

Two testing methods were used:
* **Leave One Out (LOO)**: Left the data related to one subject out of the testing.
* **K-Cross Validation(CV)** with k = 10: 
### Ablation Study

In order to test the contribution of each descriptor to the result, tests were taken place with different combinations of descriptor data. The results are then compared by their accuracy with available state of the art models.

![[2024-12-08_17-25.png]]

As shown in the above picture, the results given by a combination of descriptor data far exceeded other combinations. Although it is important to note, how the addition of the proxemics descriptor did not increase accuracy dramatically in the PosterSession dataset, however it did increase accuracy in the CocktailParty scenario. A reason for that disparity could be because of the limitations a poster session, which has certain social rules and naturally forming roles that all individuals present must adhere to posed, especially in the proxemity of people. Therefore data related to proxemics were not as usefull as a scenario where the proxemity of individuals was able to shift and change freely.

![[2024-12-10_16-29.png]]


![[2024-12-10_16-29_1.png]]
Interesting to note that the model's accuracy on discovering the E trait was very low in case of non-social scenes, however it did detect with higher accuracy traits belonging to A, C, and OE. 

Quote: "When it comes to engagement with the scene, searching for objects, deliberate scanning
strategies or curiosity are attributes related to C and OE traits, which can be matched to behaviors
observed in the nonsocial dataset"

![[2024-12-08_17-26.png]]

Here is an example of a correctly identified case of an individual's personality (given by the red color), belonging to a high A trait.

As you can see, the values of the Person Descriptor is high at timestep 3, while it decreases to below the Group Descriptor when the group takes turns in converation at timestep 9.


![[2024-12-10_16-24.png]]

This is however an example of a misclassified sequence. Because the interactions between them is limited, the system could not accurately predict a pattern of a high A trait, the model is therefore limited in cases where motion is very low. The study's proposed solution is to introduce more multi-modal types of data such as audio cues and facial features.

