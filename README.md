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

[image14]: ./examples/warped_straight_lines.jpg "Warp Example"
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

#### Distortion-correction 

To demonstrate this step, I have applied distortion correction to some of the test images:

![alt text][image4]

![alt text][image5]

![alt text][image6]

#### Gradients and color transforms

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

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warper()`, which appears in lines 1 through 8 in the file `example.py` (output_images/examples/example.py) (or, for example, in the 3rd code cell of the IPython notebook).  The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32(
    [[(img_size[0] / 2) - 55, img_size[1] / 2 + 100],
    [((img_size[0] / 6) - 10), img_size[1]],
    [(img_size[0] * 5 / 6) + 60, img_size[1]],
    [(img_size[0] / 2 + 55), img_size[1] / 2 + 100]])
dst = np.float32(
    [[(img_size[0] / 4), 0],
    [(img_size[0] / 4), img_size[1]],
    [(img_size[0] * 3 / 4), img_size[1]],
    [(img_size[0] * 3 / 4), 0]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 320, 0        | 
| 203, 720      | 320, 720      |
| 1127, 720     | 960, 720      |
| 695, 460      | 960, 0        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image14]

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
