## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

This repository contains code for a project I did as a part of [Udacity's Self Driving Car Nano Degree Program](https://www.udacity.com/drive). The goal is to write a software pipeline to identify the road lane boundaries in a video.  

The Project
---

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/calibration2_undistorted.png "Undistorted"
[image2]: ./output_images/calibration_samples.png "Calibration Samples"
[image3]: ./output_images/corner_samples.png "Corners Identified"

[image4]: ./output_images/straight_lines1_undistorted.png "Road Transformed"
[image5]: ./output_images/test1_undistored.png "Road Transformed"
[image6]: ./output_images/test6_undistorted.png "Road Transformed"

[image7]: ./output_images/straight_lines1_thresholded.png "Binary Example"
[image8]: ./output_images/test1_thresholded.png "Binary Example"
[image9]: ./output_images/test6_thresholded.png "Binary Example"

[image10]: ./output_images/straight_lines1_vertices.png "Warp Example"
[image11]: ./output_images/straight_lines2_vertices.png "Warp Example"
[image12]: ./output_images/straight_lines1_warped.png "Warp Example"
[image13]: ./output_images/straight_lines2_warped.png "Warp Example"

[image15]: ./examples/color_fit_lines.jpg "Fit Visual"
[image16]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"


The Steps
---

The steps of this project are listed below. You can have a look at [advanced_lane_lines.ipynb](advanced_lane_lines.ipynb) for the code.

### Camera Calibration

The images for camera calibration are stored in the folder called camera_cal. 

![alt text][image2]

We will compute the camera calibration using the abovechessboard images. We will step through the list and search for chessboard corners. 

![alt text][image3]

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Distortion-correction 

To demonstrate this step, I have applied distortion correction to some of the test images:

![alt text][image4]

![alt text][image5]

![alt text][image6]

#### 2. Gradients and color transforms

We use 2 kinds of gradient thresholds:

- Along the X axis.
- Directional gradient with thresholds of 30 and 90 degrees.

This is done since the lane lines are more or less vertical.

We then apply the following color thresholds:

- R & G channel thresholds so that yellow lanes are detected well.
- L channel threshold so that we don't take into account edges generated due to shadows.
- S channel threshold since it does a good job of separating out white & yellow lanes.

![alt text][image7]

![alt text][image8]

![alt text][image9]

#### 3. Perspective transform 

The code for my perspective transform includes a function called `warp_image()`. I chose to hardcode the source and destination points but checking them on the sample images.

![alt text][image10]

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 570, 470      | 320, 1        | 
| 220, 720      | 320, 720      |
| 1110, 720     | 920, 720      |
| 722, 470      | 920, 1        |

This was consistent when tried on the second sample image as well.

![alt text][image11]

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image12]

![alt text][image13]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image15]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image16]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
