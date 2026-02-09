Image Classification: Task of assigning an input image one label from a fixed set of categories.

* Images are usually stored as 8-bit per channel, hence it represents 2^8 = 256 values, so starting from 0, the range goes up to 255 for each pixel

Challenges:

* Viewpoint variation. A single instance of an object can be oriented in many ways with respect to the camera.
* Scale variation. Visual classes often exhibit variation in their size (size in the real world, not only in terms of their extent in the image).
* Deformation. Many objects of interest are not rigid bodies and can be deformed in extreme ways.
* Occlusion. The objects of interest can be occluded. Sometimes only a small portion of an object (as little as few pixels) could be visible.
* Illumination conditions. The effects of illumination are drastic on the pixel level.
* Background clutter. The objects of interest may blend into their environment, making them hard to identify.
* Intra-class variation. The classes of interest can often be relatively broad, such as chair. There are many different types of these objects, each with their own appearance.

**Data-driven approach**: Instead of trying to specify what every one of the categories of interest look like directly in code, we’re going to provide the computer with many examples of each class and then develop learning algorithms that look at these examples and learn about the visual appearance of each class. This approach is referred to as a data-driven approach, since it relies on first accumulating a training dataset of labeled images.

Basic flow of this goes as: Input a set of N images (training data) labelled as one of K categories, learning what every one of the classes looks like, and evaluating the quality of the classifier by asking it to predict labels for a new set of images and comparing it with the ground truth.

**Nearest neighbour classifier**: One of the simplest possibilities is to compare the images pixel by pixel and add up all the differences. In simple words, for each test image find a training image that has the distance-value nearesest to the one of test image and classify it accoridngly. Now the distance here can be defined in many ways, sum of absolute difference in each pixels (L1 norm), sqrt of sum of squared difference in each pixels (L2 norm) etc.

**k - Nearest Neighbor Classifier**: Instead of calculating and comparing the distance-value to find just one training image closest, we can extract a set of k-training images for each test image. In particular, when k = 1, we recover the Nearest Neighbor classifier. Intuitively, higher values of k have a smoothing effect that makes the classifier more resistant to outliers.

The k-nearest neighbor classifier requires a setting for k, which poses an interesting probelm to solve. Additionally, we saw that there are many different distance functions we could have used: L1 norm, L2 norm, there are many other choices we didn’t even consider (e.g. dot products). These choices are called **hyperparameters** and they come up very often in the design of many Machine Learning algorithms that learn from data. It’s often not obvious what values/settings one should choose.

Whenever you’re designing Machine Learning algorithms, you should think of the test set as a very precious resource that should ideally never be touched until one time at the very end. Otherwise, the very real danger is that you may tune your hyperparameters to work well on the test set, but if you were to deploy your model you could see a significantly reduced performance. In practice, we would say that you overfit to the test set. But if you only use the test set once at end, it remains a good proxy for measuring the generalization of your classifier

there is a correct way of tuning the hyperparameters and it does not touch the test set at all. The idea is to split our training set in two: a slightly smaller training set, and what we call a **validation set**.

In cases where the size of your training data (and therefore also the validation data) might be small, people sometimes use a more sophisticated technique for hyperparameter tuning called cross-validation. Working with our previous example, the idea is that instead of arbitrarily picking the first 1000 datapoints to be the validation set and rest training set, you can get a better and less noisy estimate of how well a certain value of k works by iterating over different validation sets and averaging the performance across these. For example, in 5-fold cross-validation, we would split the training data into 5 equal folds, use 4 of them for training, and 1 for validation. We would then iterate over which fold is the validation fold, evaluate the performance, and finally average the performance across the different folds.

One advantage is that it is very simple to implement and understand. Additionally, the classifier takes no time to train, since all that is required is to store and possibly index the training data. However, we pay that computational cost at test time, since classifying a test example requires a comparison to every single training example. They are very expensive to train, but once the training is finished it is very cheap to classify a new test example.

## Distadvantages of k-Nearest Neighbour classifier:

* The classifier must remember all of the training data and store it for future comparisons with the test data. This is space inefficient because datasets may easily be gigabytes in size.
* Classifying a test image is expensive since it requires a comparison to all training images.




