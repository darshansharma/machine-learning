LeNet was one of the very first convolutional neural network which helped propel the field of DeepLearning. We call this architecture as LeNet architecture.

There are 4 main operations in the CNN - 
1. Convolution
2. Non Linearity (ReLU)
3. Pooling or sub-sampling
4. Classification (Fully Connected Layer)

The above operations are the basic building block for every CNN model.

**Preprocessing of Image**
Channel is a conventional term used to reefer certain component of an image. An image from a standard digital camera will have three channels - red. gree, blue - we can imagine those three 2d-matrices stacked over each other(one for each color) each having pixel values in the range 0 to 255.
The grayscale image has only one channel and that is what will go as an input to our cnn model. So conversion of image/preprocessing is required before passing the input to model.


**Convolution**
The primary purpose of convolution is to extract features from the input image. Convolution preserves the spatial relationship between pixels by learning image features using small squares of input data. 
Consider a 5 * 5 image whose pixel values are only 0 or 1  - 

![](images/img-matrix.png?raw=true)

Also consider another 3X3 matrix  - 

![](images/kernel-matrix.png?raw=true)

Then the convolution of 5X5 image and 3X3 matrix is - 

![](images/convolved-image.png?raw=true)

The output matrix is called Convolved feature or feature map. We slide the orange matrix over our original image (green) by 1 pixel (also called stride) and for every position we compute the element wise multiplication(between the two matrices) and add the multiplication output to get the final integer which formsa single element of output matrix. 

We call this 3X3 matrix "filter" or  "kernel" or "feature detector" and the matrix formed by sliding the filter over the image and computing the dot product is called "Convolved Feature"  or "Activation map" or the "feature map".

Different values of feature detector will produce different feature map for the same input image. We can perform the operations such as Edge Detection, Sharpen, and Blur just by changing the numeric values of our filter matrix before the convolution operation.

![diff-fitlers](images/diff-filters.png?raw=true)

Important point to note here - is that CNN learns the value of these filters on its own during the training process(though we need to specify parametrs such as number of filters, filter size, architecture of the network etc before the training process.) The more number of features we have, the more image features get extracted and the better our network becomes at recognizing patterns.

The size of the feature map is controlled by 3 parameters - 
1. Depth
2. Stride 
3. Zero-padding

**NON-LINEARITY (ReLU)**
ReLu stands for rectified linear unit and is non linear operation. Its output is given by - 

Output = max(zero, Input)

ReLu is an element wise operation and replaces all negative pixel values in the feature map by zero. The purpose of the relu is to introduce non linearity in our ConvNet, since most of the real world data we would want our CNN to learn is non linear (convolution is linear operation - element wise multiplication ,addition..)
Other non linear functions such as tanh or sigmoid can also be used instead of ReLU but Relu has been found to peform better in most situations.

**POOLING**

Pooling reduces the dimensionality of each feature map but retains the most import information. Spatial pooling can be of different types - max, average, sum etc.
In case of the max pooling we define a 2X2 window and take the largest element from the rectified feature map within that window. Instead of taking the largest element we could also take the average (Average Pooling) or sum of all the elements in that window. Max pooling in practice has shown to perfom better.

![](images/max-pooling.png?raw=true)

We slide our 2X2 window by 2 cells (stride) and take the maximum value in each region. 

  ------ 


It is important to understand that these layers are the basic builiding blocks of any CNN. 

![](images/simple-cnn-model.png?raw=true)

In the above figure we have 2 sets of the convolution , ReLU & pooling layers - the 2nd convolution layer perform convolution on the putput of the first pooling layer using 6 filters to produce a total of 6 feature maps. ReLU is then applied individually to all these 6 feature maps. We then perform max pooling operation seprately on each of the 6 recitfied feature map.

The output of the 2nd pooling layer acts as an input to the fully connected layer.

**FULLY CONNECTED LAYER**

The fully connected layer is the traditional multi layer perceptron that uses a softmax function at the output layer (SVM can also be used). 
The output from the convolution and pooling layers represent high level features of the input image. The purpose of the FCL is to use these features for classifying the input image into various classes based on the training dataset.

The softmax at output layer takes a vector of arbitrary real-valued scores and squashes it to a vector of value between zero and one that sum to one.


**BACKPROPAGATION**

The convolution + pooling layers acts as feature extractors from the input image while FCL acts as a classifier. 

The overall process of training the convolution Neural network is summarized as - 

1. Initialize all filters and parameters/weights with random values.
2. Network takes training image as input, goes the through the forward propragation step (convolution, ReLU, and pooling operations along with forward propagtion if FCL) and the finds the output probabilities for each class.
3. Calcualte the total error at the output layer 
4. Use backpropagation to calculate the gradients of the error with repect to all weights in the network and use gradient descent to update all filter values/weights and parameter values to minimize error.
5. Repeat steps 2-4 with all the images.


Note that above we used two sets of alternating convolution and pooling layers. Please be aware that these operations can be repeated any number of times in a CNN model. In fact some of the best models today have 10s of convolution and pooling layers.
