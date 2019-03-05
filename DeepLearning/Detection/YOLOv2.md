# YOLO9000:Better, Faster, Stronger

Joseph Redmon, Ali Farhadi

University of Washington, Allen Institude for AI


## Introduction

* We propose a new method to harness the large amount of classification data we already have and use it to expand the scope of current detection systems.



## Better

* we focus mainly on improving recall and localization while maintaining classification accuracy.
* Instead of scaling up our network, we simplify the network and then make the representation easier to learn.
* Batch Normalization
  * Batch normalization leads to significant improvements in convergence while eliminating the need for other forms of regularization.
  * By adding batch normalization on all of the convolutional layers in YOLO we get more than 2% improvement in mAP.
    
* High Resolution Classifier
  * YOLO trains the classifier network at 224 x 224 and increases the resolution to 448 for detection.
  * we first fine tune the classification network at the full 448x448 resolution for 10 epochs on ImageNet.
  * The high resolution classification network gives us an increase of almost 4% mAP.
    
* Convolutional With Anchor Boxes.
  * Using only convolutional layers the region proposal network(RPN) in Faster R-CNN predicts offsets and confidences for anchor boxes.
  * Since the prediction layer is convolutional, the RPN predicts these offsets at every location in a feature map.
  * Predicting offsets instead of coordinates simplifies the problem and makes it easier for the network to learn.
  * <u>remove the fully connected layers</u> from YOLO and use anchor boxes to predict bounding boxes.
  * <u>eliminate one pooling layer</u> to make the output of the network's convolutional layers higher resolution. 
  * We also shrink the network to operate on 416 input images instead of 448 x 448. Because we want an odd number of locations in our feature map so there is a single center cell.
    * Objects, especially large objects, tend to occupy the center of the image so it's good to have a single location right at the center to predict these objects instead of four locations.