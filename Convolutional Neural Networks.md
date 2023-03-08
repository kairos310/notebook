
### History
*LeNet* architecture (1990s):
	Yann LeCun - developed neural network architecture, turing award in 2018
	designed to classify a picture into categories

### Convolutional pipeline
Reduce an image into pixels, into numbers as input
1920 x 1080 x 3 pixels, every thing becomes an input into the neural network one for every single possible value of the image.
alternate pooling and convolution, every layer recognizes more and more high level features

Has **Location invariance** preserves spatial relationships between the pixels
**Convolution** extracts features from an image, don't need to specify all features. 
no longer need to do feature engineering
perform an operation on an image using a kernel, can use edge detection 

### Convolutional filters
all pixels go into *convolutional filters*, generating other images, number of filters is *depth* number of pixels moe the filter matrix each time is *stride*
*zero-padding*, make post convolution makes images the same size as pre-convolution images. 

##### parameters
The numbers in the convolutional filter matrices are learned through back-propogation. 
Others like (depth, stride, size of matrix are set ahead of time)

##### activation function
typically use sigmoid, but use *RELU* in  convolutions

##### pooling
applies mathematical operator into a small region, but unlike convolution, it's done in segments and not continuous. 
reduces dimensionality of images
*subsampling, down-sampling*
*max-pooling* get the max in a region, make an image using the maxs
*sum-pooling*
*average-pooling*

