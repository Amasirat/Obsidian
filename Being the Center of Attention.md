A novel study, featuring a CNN model that learns to differentiate human personalities based on video information.

Humans show consistent behavioral patterns which are **Personality Traits**.

[[The Big-Five Personality Trait Model]] defines five general dimensions of human personality:
* **Extroversion**
* **Aggreeableness**
* **Conscientiousness**
* **Neuroticism**
* **Openness**
[Personality Computing]() is where machine learning is combined with psychology to understand personalities.

This model combines **individual behaviour** and **contextual information** to map a personality label.  
We need three high-level parameters for the CNN:
* Context Proxemics: Relation between **all** persons in environment
* Person Descriptor: Individual motion descriptor
* Social Group Descriptor: Describing people in conversation groups
This model also works in a one-person-context.

Although human behavior is complex, some papers have shown that a few attributes can describe many behaviors. For example, a pedestrian that walks in a way divorced from surrounding crowd, is shown to have a more aggressive personality.

# Problems it tries to fix

Field of personality computing has progressed far but still has some challenges:
* datasets **with** personality labels is limited.
* findings about personality are restricted to **one** field

# Related Works

* **Skeleton motion features**
* **Personality Recognotion**
* **Social and nonsocial interaction**

# Method

## Architecture

Given **n** total subjects (skeleton joints) either in a social or non-social scene, it will map each Person and Context onto a personality Label (The big-five traits).

For every frame of video data, they are run through a **Pose Estimation Algorithm**. The poses that were not correctly gained, can be estimated by existing joint positions. Each frame is also run through a frame-by-frame tracking algorithm.

Skeleton-pose estimations use reference joints such as hips or shoulders to gain data about the movement of each joint.

In order to make the data gathered useful, it creates a **Cylindrical Descriptor** for each feature.

A Cylindrical Descriptor has three parameters:
* Radius: The distance between a reference joint and another joint
* Theta: The angle between those
* Z: Which is the y difference of those two: yref - y

Then these are pooled using one of the pooling algorithms and given to the input layer of the CNN.

**The reason a cylindrical descriptor is used, is to give the same backbone structure to the data being fed to an FC input layer.**
### Person Motion

They create temporal skeleton clips, clips of skeletons over time. For every frame in a sequence, relative position of all joints that have been detected are computed with respect to some selected reference joints (Like hip joints, it's a common thing in motion detection, ref is also set to 4 here)

Each computation is stored **vertically**, each row representing all distances for x and y in **one** time sequence.

{
	{(2,3, 4), (1.2,3.2, 2), (3.2,0.56, 0.2), ....}
	{(2.1, 2.9, 0.6), (2.3, 0.12), ....}
}

The matrix is a 3D one because rach item inside it has 3 properties: x, y, and z.

This 3D temporal matrix is then transformed into cylindrical coordinates.

After all of that, the three descriptors are converted into a value between 0 - 255 and fed into a CNN.
### Group Motion
In NonSocial situations, this is set to zero.

Otherwise, code from \[15] is used to detect members of a social group. They compute motion dynamics on every member of the group. (The process simillar to the previous step)

Since groups vary their number of members, a pooling strategy is used to codify this data.

Two conventional pooling strategies exist:
* Average Pooling: takes an average of **all** motions
* Max Pooling: Takes the highest motion values in the group

It was shown that average pooling provided higher accuracy in most experiments.

**Interestingly, its performance was not that different for Conscientiousness and Neuroticism.**

The paper postulates that individuals with high Conscientiousness and Neuroticism are less likely to stand out, will result in no particular di


### Context (Proxemics)

### For Social Proxemics:

Each subject's distance to one another is computed, giving values to Ro and Theta descriptors (z is irrelevant because vertical axis has no effect on proxemics)
For consistency, they are converted to a number between 0 - 255.

Two advantages to these descriptors:
* global position of every subject with respect to all other subjects is represented, reflecting behaviors that have already been correlated with personality display.
* Due to common cylindrical coordinates structure, they can be fused with other generated features to optimize the CNN.
### For Non-Social Proxemics:

Psychological theorem suggests that humans interact with objects similarly to engaging with humans, especially in "lonely" situations.

* Semantic regions of the scene are discovered in an unsupervised way
* A GMM is used to find out the most active and relevant regions to the individual.
# Feature Learning

