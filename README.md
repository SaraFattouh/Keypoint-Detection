# Keypoint-Detection

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
* Normalize the datasets. All value should be between 0.0 and 1.0. *Note: you don't have to use standardization, you can just divide them by 255.*


![plot](https://github.com/SaraFattouh/Keypoint-Detection)
