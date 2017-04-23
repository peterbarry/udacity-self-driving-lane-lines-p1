#**Finding Lane Lines on the Road**

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps.
  * I converted the images to grayscale
  * Then ran a Gaussian blur filter with blur 5
  * Ran a canny filter - to extract edges in the image. (1:2 ratio for threshold)
  * Generated a region of interest to using a polygon. selecting approx 40% bottom of image tapered from lowest part to cropped area at approx 40%
  * Ran hough algorithm to select lines in region of interest.
  * Called generate lines from hough output.
  * Blend original image with generated lines.


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...
  * Generate the slope of all lines output by the hough algorithm.
  * Bin slopes of lines into left and right slopes < 0  or > 0.
  * Remove slopes not in direction of road lines ( in many cases hough generated 4 lines for a white dashed line) removing the horizontal component of these lines was critical.
  * Get the mean of all slopes of the remaining lines ( do left and right separately)
  * Keep track of lowest point in an image for left and right lines.
  * draw a line from the lowest point to a target y point ( eg 40% of image) with median slope. ( for left and right)
  * For cases where the lowest point of line in an image is not at the bottom of image, project a line from that point back to the base of the image using the median slope.



###2. Identify potential shortcomings with your current pipeline

The pipeline does not deal with curved roads very well. The mean of the lines is altered by the points further up on the image.

The selection of edges to include is very basic and could be upset due to vehicles in the lanes


###3. Suggest possible improvements to your pipeline

Improvement 1:
 It would be better to use a binning histogram to select the collection of slopes to include in the lines, rather than simple thresholds.
Improvement 2:
 Generate a poly curve instead of a line, the curve would be fit using the segments of lines identified on left and right side.
Improvement 3:
 The starting x value of the line ( y is always max value at end of image) should be selected to be in the middle of the thick road lines. There is some jitter in the images due to the x value moving around a little.

Improvement 4:
  Introduce some frame to frame averaging, to reduce jitter.

Improvement 5:
  If there is wide range of slopes ( curved road), generate a weighed mean slop with higher contribution from lines nearest.
