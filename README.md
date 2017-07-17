[//]: # (Image References)

[test_original]: ./test_run/original.png "Original Image"
[test_roi_boundary]: ./test_run/roi_boundary.png "ROI Boundary"
[test_color_masked]: ./test_run/color_masked.png "Color Masked for Yellow and White"
[test_blurred]: ./test_run/blurred.png "Blurred Image"
[test_roi_masked]: ./test_run/roi_masked.png "Masked to Region of Interest"
[test_canny]: ./test_run/canny_edge.png "Canny Edges"
[test_hough]: ./test_run/hough_lines.png "Hough Lines Overlay"
[test_final]: ./test_run/final.png "Final Lane Overlay"


## Finding Lane Lines on the Road


### 1. Overview

My pipeline is broken into two parts, the first is a preprocessing step that does the following:

**1. Preprocessing Step**
1. Mask all colors except white and yellow
1. Apply a Gaussian blur to the masked image
1. Mask all except that within the region of interest
1. Run Canny edge detection on the ROI
1. Return the edges


**2. Estimation Step**
1. Runs Hough lines on the Canny edge output
1. Separates the left from right lane line segments
1. Polyfits each set of points for the left and right lanes
1. Draws extended lane lines from top to bottom of the region

For debugging and testing, I have a function that just takes output of Canny edge detection step and draws the Hough lines onto the original image, showing outlines where lanes are detected.

I created a few extra functions that handle the Hough lines output, separate the left from right lanes, run polyfit and draw the extended lines.

**Samples of the pipeline on a test image:**

![alt text][test_original]
![alt text][test_roi_boundary]
![alt text][test_color_masked]
![alt text][test_blurred]
![alt text][test_roi_masked]
![alt text][test_canny]
![alt text][test_hough]
![alt text][test_final]

The test image outputs for drawing Hough lines are here: `./test_images_output_p1`. 

The final extended line outputs are here: `./test_images_output_p2`.

Here are the final video outputs: `./test_videos_output`



### 2. Assumptions

- When separating the left from right lanes, the slope of the line was not enough to filter the line segments. I added a crude check for lines on the left and right half of the image frame, but this could cause issues with curved roads where the left or right lane curves into the opposite half of the image.

- The frame size and position of the camera are fixed so the region of interest could
be hard coded, assuming that the horizon is static in all videos. When testing on the 
curved road video, I had to adjust the region boundaries to accomodate. This would also be an issue when approaching a downhill or uphill section of road.

- All videos/images are from daytime driving. Adjustments to the Canny edge and Hough
 lines would be needed for different times of days. Or if the sun.


### 3. Possible Improvements

- For curved roads a high order polynomial fit would improve the lane line estimates, but
could lead to more error in the estimate for very windy roads.

- Mounting the camera higher might help capture more lane lines in the distance, which
would improve estimation of the full lane.

- More averaging could smooth out the lane line estimates, currently the line extension jitters which could create issues downstream during processing where a controller might overreact to fast changes in lane position.
