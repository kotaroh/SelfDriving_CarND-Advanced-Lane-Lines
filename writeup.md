## Writeup 

**Advanced Lane Finding Project**

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

[image1]: ./output_images/undistorted_output.png "Undistorted"
[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./output_images/binary_combo_example.png "Binary Example"
[image4]: ./output_images/warped_straight_lines.png "Warp Example"
[image5]: ./output_images/color_fit_lines.png "Fit Visual"
[image6]: ./output_images/example_output.png "Output"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  

This document is the write up exiplaining thow I addressed the rubric points.

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "./ouptut/example.ipynb".

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps in the second code sell of the notebook).  Here's an example of my output for this step. The code is in the section 2 of the jupyter notebook.

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform is described in the section 3 of the IPython notebook.  The section starts from loading an image to be used as a input for calculation of matrix for transformation of perspective. Source (`src`) and destination (`dst`) points are also defined in this section.  I chose the hardcode the source and destination points in the following manner.

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 220, 720      | 220, 720        | 
| 595, 450      | 220, 0      |
| 685, 450      | 1000, 0      |
| 1100, 720     | 1000, 0720        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I fit my lane lines with a 2nd order polynomial like this:

![alt text][image5]

The code is in the section 4 of the notebook. The transformation I applied are;
1. Remove the distortion from the target image
2. Apply the image filter and get a binary image, to detect lanes easier
3. Detect the line by using sliding windows and get dots of the middle of the line areas
4. Fit a second order polynomial to the right set of dots and left set of dots
5. Visualize them

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

This is described in the section 5 of jupyter notebook. The 2nd order polynomial is already identified in the previous section, and the radius of curvature of the lane is calcurated as explained in the class.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in the section 6 of jupyter notebook.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./output/lanedetection.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The implementation is based on the sessions of this section.Overall the lane detection works well even with some shadows and vehicles are in the sceen of the project video.

However, the video output by my implementation shows some distruption because of shadows. They can be avoided by tuning parameters for threasholding.
Also, the video shows lanes very cleanly.In the actual scenarios, more factors could make lane detection harder, such as weather (rain, snow, etc.), other traffic, bad road condition, etc. More parameter tuning of image thretholding and other data captureed by other sensors would be needed to make the lane detection more robust.
