---
**Advanced Lane Finding Project**
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

[image1]: ./writeup_images/Undistorted_Chess.png "Undistorted_chessboard"
[image2]: ./writeup_images/Undistorted_test.png "Undistorted_test_image"
[image3]: ./writeup_images/binary.png "binary"
[image4]: ./writeup_images/lane_detection.png "lane_detection"
[image5]: ./writeup_images/warp.png "warp"
[image5]: ./writeup_images/Formula.png "Formula"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

All de code of this project is in the file called "code.py", you can found it here [link to code.py](./code.py)

###Camera Calibration

####1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

In cells one to three of the IPython notebook is computed the camera calibration matrix and distortion coefficients given a set of chessboard images.

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.

We can see that 20 images are fed to the code but 3 of then are not valid for dis process becuase doesn´t appear the whole chessboard.

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

###Pipeline (single images)

####1. Provide an example of a distortion-corrected image.
In cell 4 we can see the distortion correction to one of the test images like this one:
![alt text][image2]

####2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

In order to create a thresholded binary image I used a combination of thresholds:
* Magnitude threshold (Cell 5): Image is transformed to HLS color space the Magnitude threshold is applied to "H" channel and to "S" channel, this is because in H channel yellow lanes are well detected whilst in S channel the white ones. This magnitude threshold consists in obtain sobelX and sobelY gradiend and compute their magnitude.
* Direction threshold (Cell 8): Direction threshold is computed also in "H" and "S" channel, sobelX and sobelY are obtained, values less than threshold are omited, then is computed the direction, and is applied direction threshold.
* S channel threshold (Cell 7): In this threshold image is transformed also to HLS color space and are extracted in two steps elements with yellow color and elements with white color.  
* R channel threshold (Cell 9): A threshold is applied to R channel of RGB image to obtain lanes.

This thresholds are composed in cell 10, and a binary image is generated, also a stacked image is generated for debugging.

![alt text][image3]

####3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `image_unwarp()`, in cell 11.  This function takes as inputs an image (`img`), and returns the image warped and the inverse M matrix to unwarp later.

```
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
| 590, 450      | 200, 0        | 
| 700, 450      | 1080, 0      |
| 1133, 680     | 1080, 720      |
| 200, 680      | 200, 720        |

Here we can see an example:

![alt text][image5]


####4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Lanes are detected using method of --- explained in udacity classroom

![alt text][image6]

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image4]

####5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

####6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

###Pipeline (video)

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

