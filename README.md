# Architectures for Image Classification

<p aling="center">
  <img alt="" src="https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/u-net-architecture.png">
</p>


In the  field of Deep Learning there is a list of powerful architectures for image classification that have become de-facto standard approaches to many  computer vision or related problems:
  1. AlexNet
  2. VGG-16, VGG-19
  3. Inception Nets
  4. ResNet
  5. BN - AlexNet
  6. ResNet-101, ResNet-152, ResNet-50, Resnet-34
  7. Inception-V4, Inception-V3
  8. BN-NIN
  9. GoogLeNet
  10. ENet 
  #### 11. U-Net [Newest]
 
## [Neural Network Architectures](https://towardsdatascience.com/neural-network-architectures-156e5bad51ba) 
<p aling="center">
  <img alt="" src="https://chaosmail.github.io/images/deep-learning/top1-param.png">
</p>
                                                                                                                           
<p aling="center">
  <img alt="" src="https://cdn-images-1.medium.com/max/1600/1*n16lj3lSkz2miMc_5cvkrA.jpeg">
</p>


U-NET is considered one of standard architectures for image classification tasks, when we need not only to segment the whole image by its class, but also to segment areas of image by class, i.e. produce a mask that will separate image into several classes.

## [Code](https://github.com/zhixuhao/unet/blob/master/model.py)

## 1. U-NET's capabilities
The easiest way to judge the neural network is by its:

  1. Performance on various real-life tasks
  2. Computational difficulty (how many and which GPUs you need, how long it will train)
  3. Availability of examples in the community
  
 [read more...](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/)
 
 
 
 ## 2. Model
The U-Net architecture is built upon the Fully Convolutional Network and modified in a way that it yields better segmentation in medical imaging. Compared to FCN-8, the two main differences are:
  1. U-net is symmetric,
  2. the skip connections between the downsampling path and the upsampling path apply a concatenation operator instead of a sum. These skip connections intend to provide local information to the global information while upsampling.
 
 
Because of its symmetry, the network has a large number of feature maps in the upsampling path, which allows to transfer information. By comparison, the basic FCN architecture only had number of classes feature maps in its upsampling path.

The U-Net owes its name to its symmetric shape, which is different from other FCN variants.

U-Net architecture is separated in 3 parts:

  1. The contracting/downsampling path
  2. Bottleneck
  3. The expanding/upsampling path



## Contracting/Downsampling path

The contracting path is composed of 3 blocks. Each block is composed of:

  1. 3x3 Convolution Layer + activation function (with batch normalization)
  2. 3x3 Convolution Layer + activation function (with batch normalization)
  3. 2x2 Max Pooling
  


## Expanding /Upsampling path:

The expanding path is also composed of 4 blocks. Each of these blocks is composed of

  1. Deconvolution layer with stride 2
  2. Concatenation with the corresponding cropped feature map from the contracting path
  3. 3x3 Convolution layer + activation function (with batch normalization)
  4. 3x3 Convolution layer + activation function (with batch normalization)



## Bottom Neck path

This part of the network is between the contracting and expanding paths. The bottleneck is built from simply 2 convolutional layers (with batch normalization), with dropout.



## Advantages

  1. As we see from example, this tool is really versatile and can be used for any reasonable image masking task
  2. High accuracy given proper training, adequate dataset and training time
  3. This architecture is input image size agnostic since it does not contain fully connected layers
  4. This also leads to smaller model weight size (for 512x512 U-NET - 89mb)
  5. Can be easily scaled to have multiple classes
  6. Code samples are abundant (though none of them worked for me from the box, given that the majority was for keras >1.2.        Even keras 2.0 required some layer cropping to launch properly)
  7. Pure tensorflow black-box implementation also exists (did not try it)
  8. Relatively easy to understand why the architecture works, if you have basic understanding of how convolutions work


## Disadvantages

  1. The architecture initially was designed to overcome the ad hoc choice of 'rolling window' / patches hyper-parameters.        To have decent results the size of U-NET that you will be using must be comparable with the size of features and its          surroundings (i.e. you should include some context for the model to work properly)
  2. Because of many layers takes significant amount of time to train
  3. Relatively high GPU memory footprint for larger images:
      1. 640x959 image => you can fit 4-8 images in one batch with 6GB GPU
      2. 640x959 image => you can fit 8-16 images in one batch with 12GB GPU
      3. 1280*1918 => you can fit 1-2 images in one batch with 12GB GPU
  4. Is less covered in blogs / tutorials than more conventional architectures
  5. The majority of Keras implementations are for outdated Keras versions
  6. Is not standard to have pre-trained models widely available (it's too task specific)
