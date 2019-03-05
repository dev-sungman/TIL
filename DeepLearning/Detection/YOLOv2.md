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