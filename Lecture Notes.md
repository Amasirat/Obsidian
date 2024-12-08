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


## Agreeableness



Many studies have been conducted with the relation of individual behaviors and their peronality labels. A great example is how pedestrians behave in the case of navigating a crowded area. Individuals are considered aggressive for example if their walking path is not influenced by the crowd.
# Applications of the Study

This study proposes a few applications of its work.

Gathering real time infromation of a person's personality in a non-interactive system. There have been methods of gathering information about a user's inclinations and personality through their engagement with the system (For example, youtube, TikTok, etc detecting their users' video watching preferences) which are standardly called "active interaction", however by improving methods of **non-interactive** or **passive interaction**, the system can adapt to the user's current mood or personality by simply observing the environment and make the system more "human-like".

# CNN

A CNN or Convolutional Neural Network is a specialized form of a Neural Netwok designed for **object detection** and **Image Classification**.

A general CNN architecture will be composed of these general parts:

* **Convolutional Layers**: A series of layers that perform convolutions on each pixel of the input, the purpose of these layers is to extract features from the image input.
* **Pooling Layer**: Or **sub-sampling** layers, these layers are for reducing the dimensions of the convolved image input. It uses two methods of resizing the input, **Max Pooling** where the max pixel value of a section is chosen and **Average Pooling** where the average of all pixel values in a section (4x4 or 2x2) are merged into one output image map.
* **Flattening**: Its job is to transform the nxm matrix into a 1-Dimensional vector of size n+m to be able to feed the next layer's input.
* **Fully-Connected Layers**: For integrating the features and facilitating learning.

## Convolutional Layers



## Fully Connected Layers


# Architecture

For any sequence of images such as video called a scene, the process of feature extraction is proposed to look something like this:

* **Individual Motion Describtor**: Values describing the features describing each individual in the scene.
* **Group Motion Describtor**: Describing the motion of individuals that are detected to have formed a **social group**
* **Proxemics Descriptor**: Describing the global positions of objects in the scene, it comes from the idea that human behavior is affected by their environment and proxemity to other individuals.

![[2024-12-08_10-55.png]]

Because this model aims to be a general framework for personality recognition, it can also describe non-social scenes as well. In that case, the **Group Motion Describtor** will be 0 and proxemics will refer to regions of interest in the scene deemed relevant to the individual. 
## Process of Feature Extraction: Social Scene

### Individual Describtor

For every frame, a pose estimation algorithm is performed, in the case that a few joints were not detected, the missing joints are estimated by refering to its surrounding joints. Then a tracking algorithm is run through to track the relative motion of these joints over time. 4 reference joints are selected and every joint's distance to the reference joints. A matrix of values with dimensions of t x Jref-1 is built. For compactness sake, these data are converted to a cylindrical coordinates.

* **Ro**: The euclidean distance of each joint to its reference joint
* **Theta**: The angle of that distance
* **Z**: The vertical difference of each joint

$$
Jref=0:
\begin{bmatrix}
	    x_{00}       & x_{01} & x_{02} & \dots & x_{0n} \\
	    x_{10}       & x_{11} & x_{12} & \dots & x_{1n} \\
	    x_{d0}       & x_{d1} & x_{d2} & \dots & x_{dn}
\end{bmatrix}
$$
$$
	Jref = 1
	\begin{bmatrix}
	    x_{11}       & x_{12} & x_{13} & \dots & x_{1n} \\
	    x_{21}       & x_{22} & x_{23} & \dots & x_{2n} \\
	    x_{d1}       & x_{d2} & x_{d3} & \dots & x_{dn}
	\end{bmatrix}

$$
$$
	Jref = 2
	\begin{bmatrix}
	    x_{11}       & x_{12} & x_{13} & \dots & x_{1n} \\
	    x_{21}       & x_{22} & x_{23} & \dots & x_{2n} \\
	    x_{d1}       & x_{d2} & x_{d3} & \dots & x_{dn}
	\end{bmatrix}

$$
$$
	Jref = 3
	\begin{bmatrix}
	    x_{11}       & x_{12} & x_{13} & \dots & x_{1n} \\
	    x_{21}       & x_{22} & x_{23} & \dots & x_{2n} \\
	    x_{d1}       & x_{d2} & x_{d3} & \dots & x_{dn}
	\end{bmatrix}

$$
Each row of the matrix represents the values of one timestep.

Then the three descriptor values are converted into values between 0-255.
### Social Group Descriptor

A Social group is detected using an approach of statistical analysis of F-formations. Once discovered a similar process as the process of obtaining individual descriptors is applied to every member of the group except the target individual. Since the members of a social group can be variable, a pooling strategy is used. Average pooling of these descriptors have been shown to have higher accuracy in most cases.
## Proxemics

The global distance between each individual in the scene is calculated. Z is 0 here because a vertical difference is not relevant.

![[2024-12-08_11-08.png]]

## Process of Feature Extraction: Non-Social scene

The same process for extracting individual motion descriptors is similar to a social scene. However Social Group Descriptors are set to zero and proxemics is proxemity to notable objects in the scene.

The idea is that because of the social nature of humans, individuals tend to Anthropomorphisize objects around them, this process can intensify in the case of extreme lonely situations (Show an example image of the movie Cast Away). Therefore, the way individuals interact with their surrounding scene can help gather latent information for helping to deduce their personality labels.

The surrounding regions of the individuals are discovered in an unsupervised way. The scene is divided into patches and it is assumed that arm motions of the individual are the most important data, however since overall motion can skew arm motions, the total motion of the individual is subtracted.

For locating regions where interactions occur a Guassian Mixture Model is used.
# Feature Learning

A pretrained VGG19 CNN trained on ImageNet images is used. However 



# Qualitive and Quantitive Results

![[2024-12-08_17-25.png]]






![[2024-12-08_17-26.png]]