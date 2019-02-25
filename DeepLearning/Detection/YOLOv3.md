# YOLO v3 implementation scratch.

ref: https://blog.paperspace.com/how-to-implement-a-yolo-object-detector-in-pytorch/

## Interpreting the output

In YOLO, the predictions is done by using a convolutional layer which uses 1x1 convolutions.
Since we have used 1x1 convolutions, the size of the prediction map is exactly the size of the feature map before it.
In YOLO v3, the way you interpret this prediction map is that each cell can predict a fixed number of bounding boxes.

Depth-wise, we have (B x (5 + C)) entries in the feature map.

* B: the number of bounding boxes each cell can predict.
* each bounding boxes have 5 + C attributes, which describe the center coordinates, the dimensions, the objectness score and C class confidences for each bounding box.

YOLO v3 predict 3 bounding boxes for every cell.

** each cell of the feature map to predict an object through one of it's bounding boxes if the center of the object falls in the receptive field of that cell **


