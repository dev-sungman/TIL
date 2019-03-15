#ASTER: An Attentional Scene Text Recognizer with Flexible Rectification

Baoguang Shi, Mingkun Yang, Xinggang Wang, Pengyuan Lyu, Cong Yao, Xiang Bai



## INTRODUCTION

* Previous methods do not explicitly address the problem of irregular text.

* The model comprises two parts: the rectification network and the recognition network.

  

  <img src="../images/ASTER/architecture.png" width="1000px" height="200px">

* Given an input image, the rectification network transforms the image to rectify the text in it.

  * The transformation is parameterized Thin-Plate Spline(TPS).

    

* During inference,

  * The rectification network first predicts the <u>TPS parameters</u> from the image.
  * Based on <u>STN, rectification network ca be trained</u> purely by the gradients back propagated by the recognition network, hence requiring no human annotations.
  * <u>The recognition network</u> predicts a charater sequence from the rectified image in an <u>attentional sequence to sequence manner.</u>

* In particular, ASTER enables a horizontal text detector to detect oriented text.

* Contributions

  * Tackle the problem of irregular text recognition with an explicit rectification mechanism, which significantly <u>improves recognition performance without extra annotations.</u>
  * Introduce attentional sequence-to-sequence model.
  * Propose a method for enhancing text detectors.



## MODEL

### Rectification Network

<img src="../images/ASTER/structure_rectification.png" width="80%" height="50%">

* Adpot Thin-Plate-Spline(TPS) as the transformation.
  * It is more flexible compared to other simpler 2D transformations.
  * <u>TPS performs non-rigid deformation on images, handling a variety of distortions.</u>

<img src="../images/ASTER/rectification_network.png" width="50%" height="50%">

* **Localization Network**
  * A TPS transformation is determined by two sets of control points of equal size, denoted by $$K$$.
  * When the <u>control points on the input image are predicted</u> along the upper and lower text deges, the resulting <u>TPS transformation outputs a rectified image with regular text.</u>
  * 



