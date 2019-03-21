# YOLO9000:Better, Faster, Stronger

Joseph Redmon, Ali Farhadi

University of Washington, Allen Institude for AI


## Introduction

* We propose a new method to harness the large amount of classification data we already have and use it to expand the scope of current detection systems.



## Better

* we focus mainly on improving recall and localization while maintaining classification accuracy.

* Instead of scaling up our network, we simplify the network and then make the representation easier to learn.

* **Batch Normalization**

  * Batch normalization leads to significant improvements in convergence while eliminating the need for other forms of regularization.
  * By adding batch normalization on all of the convolutional layers in YOLO we get more than 2% improvement in mAP.
    

* **High Resolution Classifier**

  * YOLO trains the classifier network at 224 x 224 and increases the resolution to 448 for detection.
  * we first fine tune the classification network at the full 448x448 resolution for 10 epochs on ImageNet.
  * The high resolution classification network gives us an increase of almost 4% mAP.
    

* **Convolutional With Anchor Boxes.**

  * Using only convolutional layers the region proposal network(RPN) in Faster R-CNN predicts offsets and confidences for anchor boxes.
  * Since the prediction layer is convolutional, the RPN predicts these offsets at every location in a feature map.
  * Predicting offsets instead of coordinates simplifies the problem and makes it easier for the network to learn.
  * <u>remove the fully connected layers</u> from YOLO and use anchor boxes to predict bounding boxes.
  * <u>eliminate one pooling layer</u> to make the output of the network's convolutional layers higher resolution. 
  * We also shrink the network to operate on 416 input images instead of 448 x 448. Because we want an odd number of locations in our feature map so there is a single center cell.
    * Objects, especially large objects, tend to occupy the center of the image so it's good to have a single location right at the center to predict these objects instead of four locations.
    * YOLO's convolutional layers downsample the image by a factor of 32 so by using an input image of 416 we get an output feature map of 13 x 13.
  * when we move to anchor boxes we also decouple the class prediction mechanism from the spatial location and instead predict class and objectness for every anchor box.
  * YOLO only predicts 98 boxes per image but with anchor boxes out model predicts more than a thousand. Without anchor boxes out intermediate model gets 69.5 mAp with a recall of 81%. With anchor boxes our model gets 69.2mAP with a recall of 88%.
    

* **Dimension Clusters**

  * we encounter two issuses with anchor boxes when using them with YOLO.

    * The first is that the box dimensioins are hand picked. The network can learn to adjust the boxes appropriately but if we pick better priors for the network to start with we can make it easier for the network to learn to predict good detections.

    * Instead of choosing priors by hand, we run k-means clustering on the training set bounding boxes to automatically find good priors.

    * If we use standard k-means with Euclidean distance larger boxes generate more error than smaller boxes. However, what we really want are priors that lead to good IOU scores, which is independent of the size of the box. Thus for our distance metric we use:
      
      $$
      d(box, centroid) = 1 - IOU(box, centroid)
      $$
      
      

    * The cluster centroids are significantly different than hand-picked anchor boxes.
      

* **Direct location prediction**

  * we encounter a. econd issue: model instability, especially during early iterations. Most of. he instability comes from predicting the $$(x,y)$$ locations for the box. In region proposal networks the network predicts values $$t_x$$ and $$t_y$$ and the $$(x,y)$$ center coordinates are calculated as:
    
    $$
    x = (t_x * w_a) - x_a
    $$

    $$
    y = (t_y * h_a) - y_a
    $$

    
    

  * This formulation is unconstrained so any anchor box can end up at any point in the image, regardless of what location predicted the box.
    

* **Fine-Grained Features**

  * simply adding a passthrough layer that brings features from an earlier layer at 26 x 26 resolution.
  * The passthrough layer concatenates the higher resolution features with the low resolution features by stacking adjacent features into different channels instead of spatial locations, simliar to identity mapping in ResNet. This turns the 26 x 26 x 512 feature map into a 13 x 13 x 2048 feature map, which can be concatenated with the original features.
    

* **Multi-Scale Training**

  * Every 10 batches our network randomly chooses a new image dimension size. {320, 352, …, 608}. Thus smallest option is 320 x 320 and the largest is 608 x 608.
  * 