
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




# Neural Networks

**terminology**
	numbers of layers = L , indexed from 0
		layer 0 inputs, layer L = output layer

$\vec{X}$ = vector of inputs, X is always 1. 
$W^{[\ l\ ]}$: matrix of weights, connecting layer 1-1→ laya I •Dimensions are n[e] x n' [1-1] , where n denotes the 2

number of neurons in layer I.

Z vector of dot products for layar I, of size no aces vector of activations for layer I, of size n in 1: vector do inputs for layer I. cel

*activation functions*

![[Drawing 2023-02-23 09.41.05.excalidraw]]

[[Convolutional Neural Networks]]

![[Drawing 2023-03-09 09.38.57.excalidraw]]

## Backpropogation 
![[Drawing 2023-03-09 09.46.04.excalidraw]]
In both regression and classification, all layers will use sigmoid except the last one. 
The last one is the *identity* function. 
	a = z                -> last one
	a = $\sigma{(z)}$
	
### Squared Error
$$\frac{d}{dy}\Biggr[\frac{1}{2}(a-y)^{2}\Biggr]=a-y$$
### Cross Entropy
$$\frac{d}{dy}\Biggr[-y log(a)-(1-y)log(1-a)\Biggr]=\frac{a-y}{a (1 - a)}$$
### Sigmoid
$$\frac{d}{dz}\Bigr[ \sigma(z) \Bigr]=\sigma(z)(1-\sigma(z))$$
### Stochastic gradient descent
- backpropogation gives is derivatives for the gradient for the cost function for each weight
- 