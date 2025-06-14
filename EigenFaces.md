This paper outlines a method of recognizing faces using neural networks inspired by concepts from information theory. Its goal is to find a simple and practical way of detecting faces.

The approach is to transform face images into a small set of characteristic feature images called *eigenfaces*.

## Eigenfaces for recognition

Previous works had ignored the issue of understanding what aspects of the face are important to face recognition and just assumed that it would be similar to how we recognize faces (nose, eyes, forhead, etc)

This paper takes inspiration from information theory so that it can encode and decode relevent information efficiently and compare them with other face informations. Here they don't assume which information is more useful than others.

The paper treats the issue of face recognition as a principal component analysis. The steps of training such a program goes like this:

For M amount of test images, first each image matrix is flattened into N<sup>2</sup> vectors. Then the mean vector of these vectors is calculated and subtracted from each vector. 
$$
avg = \frac{1}{M}\sum_{k=1}^M a_{i}
$$

$$
meanVector_{i} = v_{i} - avg
$$

Each meanVector then is the columns of a new matrix called A. Now we find the covariance matrix.

$$
C = \frac{1}{M} \sum_{k=1}^M A_{i}A_{i}^{T}
$$
We then find the eigenvalues and eigenvectors of the covariance matrix, by ranking from the top eigenvalues, we take their associated eigenvectors and form the face space, the space of basis vectors that can form a face image. 

Any new image has to be projected onto this new face space. Its distance from each vector is calculated and if that distance is too much, it will conclude that it does not know this face, otherwise it does know it.


