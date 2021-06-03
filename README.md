# Keypoint-Detection

Cat eyes and nose keypoint detector model using tf.keras. 
We use an autoencoder like architecture, which first encodes the data, then decodes it to its original size. 
Information about the dataset: https://www.kaggle.com/crawford/cat-dataset.

We Keep only left eye, right eye and node. 

Create a keypoint heatmap from the keypoints. A 128x128x3 channel image, where the first channel corresponds to left eye heatmap,
the sencond channel corresponds to the right eye heatmap and the third channel corresponds to the nose heatmap.
To do this:
  1- At each keypoint, draw a circle with its corresponding color.
  2- Then smooth the heatmap with a 5x5 Gauss filter.
  
Then crop each image and heatmap
  1- Define the bounding box, select the min and max keypoint coordinates: x1, y1, x2, y2.
  2- Add a 20x20 border around it: x1, y1, x2, y2 = x1-20, y1-20, x2+20, y2+20.
  3- Then crop the image and the heatmap using the extended bounding box.
  
Finally, resize the images and the heatmaps to be 64x64
