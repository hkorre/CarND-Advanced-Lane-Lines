# Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

## Goals
The goals / steps of this project are the following:  

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply the distortion correction to the raw image.  
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view"). 
* Detect lane pixels and fit to find lane boundary.
* Determine curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

---

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames.  The video called `project_video.mp4` is the video your pipeline should work well on.  `challenge_video.mp4` is an extra (and optional) challenge for you if you want to test your pipeline.

If you're feeling ambitious (totally optional though), don't stop there!  We encourage you to go out and take video of your own, calibrate your camera and show us how you would implement this project from scratch!

## Pipeline

### Camera Calibration
The camera is calibrated using the standard OpenCV/Chessboard method. This results in camera matrix (mtx) and distortion coefficients (dist). We use this to undistort the each image.

### Color Masks
Yellow and white features are then highlighted in the image. This is done by separate white and yellow filters. Then we threshold and combine the intermedite images.

### Edge Detection
First we color transform the undistorted image into HLS, then we isolate the L and S channels.
We then do sobel operations in the x and y directions for L and S channels. We calculate the sobel magnitude and direction for L and S channels. Lastly we threshold and combine these separate images.

### Combining Color and Edge
We then combine the color masks and the edge detections into a single binary image.

### Region of Interest
Then we crop out a region of interest.

### Perspective Transform
We use an image with "straight" lanes to develop a transform to go from the original image to a birds-eye view of the lanes.
We then apply this to our region of interest.

### Find points on Lane Lines
We develop a window of a subset of the height of the image. We take a histogram in that area. From the histogram we identify the center of the lines and log the points on the left and right. We slide the window up the image and use previously identified points to constrain where we can look for the next points.

### Curve Fitting
We define a Line class that will:
* Accept new points and low-pass filter them
* Fit the points to a curve
* Calculate the line curvature
* Generate evenly spaces points to use for lane overlay
We apply this Line class to our image to find curves for the left and right lane lines.

### Path Overlay
We then use the points from the Line class to overlay a colored patch over the lane.

### Text Overlay
Lastly we print onto the image/frame text showing the lane curvature and the position of the vehicle relative to the center of the lane.

## Room for Improvement and Cases for Failure
The system has issues in the following situations:
* Changes is lighting still make it difficult to separate the lanes since we rely on color filtering
* Changes in ground color are difficult since we rely on edge detection
* Thin lane lines are difficult to identify as they only get skinnier as the image gets processed
* When the ground is discolored (e.g. fixing potholes), this create patches that can look like a lane dash and then can really throw off the line fit, since the fit does not throw out outliers.


