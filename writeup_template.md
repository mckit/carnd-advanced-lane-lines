## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

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

[image1]: ./output_images/undistorted_image.png "Undistorted"
[image2]: ./output_images/warped_image.png "Road Transformed"
[image3]: ./output_images/threshold_image.png "Binary Example"
[image4]: ./output_images/warped_perspective_transform_image.png "Warp Example"
[image5]: ./output_images/sliding_windows_image.png "Fit Visual"
[image6]: ./output_images/road_and_info_image.png "Output"
[video1]: ./project_video_output.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

My code for doing the camera calibration is under the Part 1 header in my project file. The camera calibration is done by getting object points, looping through the calibration images and finding chessboard corners, then adding those points to the object and image point arrays. Lastly, the object points, image points, and grayscale image shape are used to calibrate the camera. 

![Undistorted Image][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

In the calibration class I use the camera calibration matrix (mtx) and distortion coefficient (dist) to apply distortion correction to iamges. 

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

In part 2 of my code I create a binary thresholded image. My Binary_Threshold class combines two different types of thresholding. I use a Sobel threshold and an HLS threshold to create my combined binary image. When creating the combined binary image I also apply a region of interest mask to show the part of the image that is more relevant for lane line detection. 

![Threshold Image][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

Part 3 of my code is where I perform the perspective transform. Using the image size I create source and destination points for the transform. Then, I calculate the perspective transform matrix M and the inverse matrix Minv

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![Warped Image][image2]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Part 4 of my code is where I identified lane line pixels and fit their positions with a polynomial. I take in a warped image and use a histogram of the bottom half ot the image to find where the lane lines should be. I then iterate through the range of windows and find the nonzero pixels in each. If I find more than my minpix threshold I recenter the window on their position and continue. 

![Sliding Windows Image][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I also did my curvature calculation in part 4 of my code. The get_curvature function is part of my Line class. I take in the warped image and fit x and y polynomials to real world space. These polynomials are used to calculate the radius of curvature (R_curve), which the function then returns. 

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

Using the Line class and helper functions in part 4 of my code, plus the other methods previously detailed I created an image pipeline that returns an image of the road with the lane area, curvature, and vehicle position left or right from center illustrated. Below is an example output image from the pipeline. 

![Pipeline Output Image][image6]

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The biggest challenge I faced with this project was organizing how I wanted to implement the features required by the rubric. The majority of the actual code had been covered in detail during the previous three lessons about computer vision. In order to try to keep things organized and simple to troubleshoot when necessary I opted to organize most of my code into various classes depending on what their purpose was. I think this served me well throughout this process. Implementing part 4 was particularly challenging for me as even though most of the code was not new and had been introduced in the lessons, organizing it so that the functions fed properly into one another took a lot of time. 

I think that my pipeline could be more robust in part 2, where I implement thresholding. I opted to only use an HLS color threshold, perhaps including other forms of color thresholding would be helpful in the future. 
