##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.


###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the 2nd and 3rd code cell of the IPython notebook.  

I started by reading in all the `vehicle` and `non-vehicle` images.  

Extract Histogram of Oriented Gradients (HOG) for a given image from "hog" method from skimage.feature.  

Visualization of a hog image example is at 4th code cell.

####2. Explain how you settled on your final choice of HOG parameters.

I pick the HOG parameters by experimenting.

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

At code cell 5th I am exctracting HOG features from dataset. At 6th code cell I extract features for input datasets and combine, define labels vector, shuffle and spli. I am using colorspace YUV and following parameters:
11 orientations 16 pixels per cell and 2 cells per block
The training itself it happening at code cell 7th. The result of the training is:
1.69 Seconds to train SVC...
Test Accuracy of SVC =  0.9823
0.027 Seconds to predict 10 labels with SVC

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

In code cell 8th I am implementing my find cars function. I rescale an images as they are in png format and rescale to 1.0
Then I change color space if different than RGB. For sliding window I am using cells steps to slide the cell. After extracting a hog features, I am doing test prediction with trained SVC and found rectangles.
Then test this function on test images printing how many rectangles are found in each image.

---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
There are 2 outputs at this Project:
1) video_project.mp4 through vehicle detection pipeline -> project_video_output.mp4 (23th cell)
2) project_video_lane.mp4 which is the result of the last project through vehicle detection pipeline -> project_video_out_lane_and_vehicles.mp4 (25th)

The pipeline is the following (code cell 21):
1) I am defining a class to store separately detected data from each video
2) process each image from the video through find_cars algorithm with defined different start stop searching positions and scale using the trained svc data.
3) then produces a heat map based on rectangle locations (additive with overlap) 
4) At the end I am drawining the rectangles on the image



---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

1) The project wasn't hard as most of the functions are direct copy paste or 95% reuse from lessons functions.
2) Based on tests seems that a scale with value less than 1.0 could produce a lot of false positives.
3) Based on some frames in the video, I believe that start and stop positions and scales could be improved, depending on video performance.