# Keypoint-Detection

## Overvirew 
In this repository, we explore various CNN architectures that can be used to detect keypoints in an image. We worked with Kaggle's CAT dataset.

## Dataset

The CAT dataset includes over 9,000 cat images. For each image, there are annotations of the head of cat with nine points, two for eyes, one for mouth, and six for ears. We will use the keypoints corresponding to the nose, left and right eyes.

## Architecture

Keypoint detection, like object detection, can be thought of as a regression problem. In terms of architecture, the question is whether the network should 
1) predict keypoint positions directly or 
2) predict an intermediary representation from which keypoints can be inferred.

## Heatmap Regression
keypoint information can be combined together with the raw image to form a heatmap. Each pixel location (i,j) in the heatmap stores a value representing the probability of that location being a keypoint. Therefore, the input to a network is no longer a WxHx1 grayscale image but instead a WxHx15 heatmap tensor, where each channel along the input tensor's depth corresponds to a unique keypoint h. W, H are the image width and Hight respectively.

To convert the training images to heatmaps, use the following procedure:

1) For the nth keypoint, create a gaussian kernel centred at the (i,j) location of that keypoint.
2) Apply the kernel to every (i,j) location in the original image.
3) Repeat steps 1-2 Nkeypoints times, updating the gaussian kernel with a new centre location. For each iteration, concatentate the resulting single-channel heatmap with the previous heatmaps.

Example of ground-truth image and its corresponding heatmaps are shown below:


![plot](https://github.com/SaraFattouh/Keypoint-Detection/blob/main/Heatmap%20sample.png)

Heatmap prediction is conceptually similar to semantic segmentation, in which a network inputs and outputs a mask tensor. The main difference with heatmap prediction is that the channels of a typical segmentation output tensor correspond to object classes, whereas our prediction will correpsond to individual keypoint heatmaps.

The predicted heatmaps need to be transformed back into (i,j) coordinates. A simple method to do this is to search each heatmap channel for the (i,j) indices corresponding to the maximum heatmap value. However, to account for outlier predictions by the network, the keypoint coordinates can be calculated as the  average of all heatmap values / (i,j) index pairs.

## Training
The CAT dataset was divided into train-val-test sets (ratio: 60-20-20)

## Prepare dataset

* Download the Cats dataset. We will only use a subset of the original dataset, the CAT_00 folder. Here you can find more information about the dataset: https://www.kaggle.com/crawford/cat-dataset
* Preprocess the data. You can find some help here: https://github.com/kairess/cat_hipsterizer/blob/master/preprocess.py
  * Following the steps in the link above, read and resize the images to be 128x128.
  * Keep only the left eye, right eye and nose coordinates.
  * Create a keypoint heatmap from the keypoints. A 128x128x3 channel image, where the first channel corresponds to left eye heatmap, the sencond channel corresponds to the right eye heatmap and the third channel corresponds to the nose heatmap. To do this:
    1. At each keypoint, draw a circle with its corresponding color. For this, use the following method: `cv2.circle(<heatmap>, center=<keypoint_coord>, radius=2, color=<keypoint_color>, thickness=2)`
    2. Then smooth the heatmap with a 5x5 Gauss filter: `<heatmap> = cv2.GaussianBlur(<heatmap>, (5,5), 0)` 
  * Then crop each image and heatmap:
    1. Define the bounding box, select the min and max keypoint coordinates: `x1, y1, x2, y2`.
    2. Add a 20x20 border around it: `x1, y1, x2, y2 = x1-20, y1-20, x2+20, y2+20`.
    3. Then crop the image and the heatmap using the extended bounding box.  
  * Finally, resize the images and the heatmaps to be 64x64.
* Split the datasets into train-val-test sets (ratio: 60-20-20), without shuffling.
* Print the size of each set and plot 5 training images and their corresponding masks.
* Normalize the datasets. All value should be between 0.0 and 1.0. 
* *Note: you don't have to use standardization, you can just divide them by 255.*



![plot](https://github.com/SaraFattouh/Keypoint-Detection/blob/main/Keypoint%20Detection.png)
