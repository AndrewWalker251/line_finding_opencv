# **Finding Lane Lines on the Road** 

## Writeup
---

**Finding Lane Lines on the Road**


The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

[//]: # (Image References)

[image2]: ./examples/canny.jpg "Edge Detection"
[image4]: ./examples/extrap.jpg "Line Extrapoliation"


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 main steps. First, I converted the images to grayscale.

Then I added some gaussian blur, which simplifies the image and reduces noise and detail so that the detector can pick out the key features. The third step was to use canny edge detection, which uses gradients to find edges. 

![alt text][image2]


This was followed by a step to only retain the part of the image that was of interest. Step 5 then applied a hough transform to find the main lines within the edges that had been detected.  

In order to draw a single line on the left and right lanes, I modified the draw_lines() function. For all the lines from the output of the hough transform I either rejected the line, added it to lines belonging to the right, or lines belonging to the left line. Any line with a absolute gradient less than 0.2 or greater than 10 was rejected completely. This helped the model deal with the additional last challenge where the bonnet of the vehicle is present.Right lines were found from those with a gradient above 0, the remaining ones belonged to the left hand side. 

I then found the average gradient and center of the right hand/ left hand lines. 

Using this information and y2-y1 = m(x2-x1) I was then able to calculate the x values for the top and bottom of each line. The y values were simply the bottom of the image and the point 0.58*height from the top. 

![alt text][image4]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when you go round a bend. The lines are currently creating straight lines.  

Another shortcoming could be how the process would work if a car was in the lane ahead. How would this confuse the line finding? 

Although a relatively easy fix for this one case, the extra assignment with the bonnet showed that this approach can be easily corrupted. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to include a way of looking for curved lines at bends. Could this be done by fitting a move complex function than a straight line to the hough transform.

Another potential improvement could be to test with images of cars infront and find a way to exclude these from the image before trying to draw the lines. 
