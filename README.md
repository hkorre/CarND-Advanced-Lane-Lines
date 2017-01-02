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

## Find points on Lane Lines
We develop a window of a subset of the height of the image. We take a histogram in that area. From the histogram we identify the center of the lines and log the points on the left and right. We slide the window up the image and use previously identified points to constrain where we can look for the next points.

### Curve Fitting

