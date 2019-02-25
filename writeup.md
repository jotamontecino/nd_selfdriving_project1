# **Finding Lane Lines on the Road**

---

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

#### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consist of 6 steps:
  * Convert the image to grayscale
  * Apply a gaussian blur
  * We use the canny edge detection
  * Select an  isosceles trapezoid region in front of the "vehicle"
  * We detect and draw filtered lines
  * We apply the image containing the filtered lines to the initial image

In order to discriminate left and right lane lines, we use the slope of each line and which side of the screen they are.
As the axis starting point is on the left top corner, a positive slope means the vector direction goes toward the starting point (top left). So a line with a positive slope should be on the right side of the screen.
I also filter the lines, trying to reduce possible noise, by selecting only the lines having a possible slope for a road lane (between 0.5 and 0.9).

For the selected lines, we append the slope and intercept into an array (leftFit and rightFit), to calculate the average slope and intercept for each line.
Finally we draw the right and left lane, by calculating the start (bottom end of the image) and end (40% of the actual height starting at the top) point using the formula `y = mx + b`.

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be the incapacity to handle sharp curves. We actually use lines to define the lanes, but they should be defined as splines.

Another shortcoming would be the possible noise in the line detection. If there is a big contrast between materials on the road, they could be selected as possible lane lines thus creating noise and inaccuracy.

Finally, the last shortcoming I could think of is the non-presence of the center lane, due to normal wear.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to create splines, and trying to extrapolate their continuation to another spline thus creating more accurate lanes and excluding noisy splines.

Another potential improvement could be to calculate the normal distance (with the standard deviation) between two lanes and applying it to validate our splines.

Another potential improvement could be to use data from the last couple of frames. We could then ensure the continuity of the lanes and have a more accurate prediction for our splines.
