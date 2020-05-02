# **Finding Lane Lines on the Road** 

## Writeup - Jacob Ososke

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I defined a kernel size of 5 pixels and applyed a Gaussian smoothing function. After that I defined a low to high threshold of 50:150 for the Canny edge detector, which was the recommended ratio by John Canny of 1:3 and applyed Canny edge detection. With the edges detected in the image, I created a mask appropriate to the lane size. Witht he masked image I defined the Hough transform parameters based on paramter tuning methods. I implemented a hough transform, which drew lines based on the detected edges.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by taking an average value of all of the lines from the hough transform that I thought would be on the left or right side based on the slope:

(0.5 < m < 1.0) or (-1.0 < m < -0.5)

The slope was calculated using the equation of a line:

 m = (y2-y1)/(x2-x1)
 b = y1 - m * x1
 
I also kept track of the length or each line so that I could use that later to calculate a weighted average, which would kill off shorter line segments and allow longer line segments to dominate. 


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the lines are different colors. This algorithm is optimized for white lines, while yellow lines are still bright, and as can be seen by the output video, this algorithm is able to handle yellow lines, other line colors may prove more toublesome.

Another potential shortcoming of this algorithm is shadows, as can be seen by the challenge test case, this algorithm has trouble identifying images with shadows.

Another potential shortcoming that can be seen by the challenge test case is that this algorithm has trouble with very bendy lines.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to convert the image to HSV image space, this would allow the edge detection method to work regardless of the color of the line and only based on the intensity of the image.

Another potential improvement could be to keep a running average of line length and create a predition and estimation step for line detection, an example of this would be to implement a kalman filter, this would get rid of massive jumps in lines detected and help with bendy roads. 
