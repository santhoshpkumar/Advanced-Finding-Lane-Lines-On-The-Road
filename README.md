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

[image16]: ./output_images/challenge_output.png "Output"
[image17]: ./output_images/challenge_output.gif "Output" 

[image18]: ./output_images/hard_output.png "Output"
[image19]: ./output_images/harder_challenge_output.gif "Output" 

[image20]: ./output_images/project_output.png "Output"
[image21]: ./output_images/project_output.gif "Output" 

[image22]: ./output_images/sliding_window_preprocess1.png "Output"
[image23]: ./output_images/sliding_window_postprocess1.png "Output" 
[image24]: ./output_images/sliding_window_preprocess2.png "Output"
[image25]: ./output_images/sliding_window_postprocess2.png "Output" 
[image26]: ./output_images/smooth1.png "Output"
[image27]: ./output_images/smooth2.png "Output" 

[image29]: ./output_images/straight_lines1_histogram.png "Output"
[image30]: ./output_images/straight_lines2_histogram.png "Output" 
[image31]: ./output_images/test2_histogram.png "Output"

[image32]: ./output_images/straight_lines1_transformed.png "Output"
[image33]: ./output_images/straight_lines2_transformed.png "Output" 
[image34]: ./output_images/test1_transformed.png "Output"
[image35]: ./output_images/test2_transformed.png "Output"

[image36]: ./output_images/straight_lines1_processed.png "Output" 
[image37]: ./output_images/straight_lines2_processed.png "Output"
[image38]: ./output_images/test1_processed.png "Output"
[image39]: ./output_images/test2_processed.png "Output"

[video1]: ./project_video_output.mp4 "Video"
[video2]: ./challenge_video_output.mp4 "Video"
[video3]: ./harder_challenge_video_output.mp4 "Video"

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

#### 4. Identified lane-line pixels and fit their positions with a polynomial

#### Histogram
The peaks in the histogram tells us about the likely position of the lanes in the image.

![alt text][image29]

![alt text][image30]

![alt text][image31]

#### Sliding Window Search

I then perform a sliding window search, starting with the base likely positions of the 2 lanes, calculated from the histogram. I have used 10 windows of width 100 pixels.

The x & y coordinates of non zeros pixels are found, a polynomial is fit for these coordinates and the lane lines are drawn.

![alt text][image22]

![alt text][image23]

![alt text][image24]

![alt text][image25]

Then finally i fit my lines with a 2nd order polynomial kinda like this:

![alt text][image15]

#### Searching around a previously detected line.

Since consecutive frames are likely to have lane lines in roughly similar positions, in this section we search around a margin of 50 pixels of the previously detected lane lines.

![alt text][image26]

![alt text][image27]

#### 5. Computing the radius of curvature and center offset.

The radius of curvature is computed according to the formula and method described in the classroom material. Since we perform the polynomial fit in pixels and whereas the curvature has to be calculated in real world meters, we have to use a pixel to meter transformation and recompute the fit again.

The mean of the lane pixels closest to the car gives us the center of the lane. The center of the image gives us the position of the car. The difference between the 2 is the offset from the center.

#### 6. Inverse Transform

In this block of code we:
- Paint the lane area
- Perform an inverse perspective transform
- Combine the processed image with the original image.

![alt text][image32]

![alt text][image33]

![alt text][image34]

![alt text][image35]


### Tying up all together 

#### Figuring out bad frames
There will be some frames where no lanes will be detected or the lanes might not make sense. We determine the bad frames if any of the following conditions are met:

- No pixels were detected using the sliding window search or search around the previously detected line.
- The average gap between the lanes is less than 0.7 times or greater than 1.3 times the globally maintained moving average of the lane gap.

#### What to do if a bad frame is detected?

- Perform a sliding window search again (this is done in the `brute_search` method in the code block below
- If this still results in a bad frame then we fall back to the previous well detected frame.

#### Averaging lanes

The lane for each frame is a simple average of 12 previously computed lanes. This is done in the `get_averaged_line` method.

![alt text][image36]

![alt text][image37]

![alt text][image38]

![alt text][image39]

---

### Pipeline (video)

#### 1.  Project video 

![alt text][image21]

![alt text][image20]

#### 2.  Challenge video 

![alt text][image17]

![alt text][image16]

#### 3.  Hard Challenge  video 

![alt text][image19]

![alt text][image18]

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
