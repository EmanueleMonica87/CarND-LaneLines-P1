# **Finding Lane Lines on the Road** 

The objective of this project is to find and print lanes on road images using computer vision tecqniques explained on the first part of the Self-Driving Car Nanodegree Course.

In the later stage, the same functions will be applied to videos of both white only and white-yellow lanes.
Limitations to this basic approach will be presented as well as illustrated with the last video which shows curved lanes

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* See the results in different images, as well as videos


[//]: # (Image References)

[image1]: ./test_images_output/output_solidWhiteCurve.jpg "solid White Lane"
![alt text][image1]

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

I used the basic steps explained during the course. I first applied a grayscale conversion and gaussian blurring to prepare the image for a Canny transformation

[image1]: ./for_markup/gray/output_solidWhiteCurve.jpg "grayscale"
![alt text][image2]

Then the Canny transform applied to the grayscale image outputs big differences in gradient, as shown below

[image1]: ./for_markup/canny/output_solidWhiteCurve.jpg "canny"
![alt text][image3]

As next step, we need to define a polygonal mask to filter out everything that is not in the region of possible lanes. In our case, we are interested in the ego lane, so we can build a trapezium where the horizon sets the maximum height and the vertices of the image set the base.

[image1]: ./for_markup/target/output_solidWhiteCurve.jpg "lines"
![alt text][image4]

[image1]: ./for_markup/lines/output_solidWhiteCurve.jpg "lines"
![alt text][image5]

As last step, we will use the Hough transform to identify linear segments in the image. 

[image1]: ./test_images_output/output_solidWhiteCurve.jpg "lines"
![alt text][image6]

Each line is represented by 4 points, which we can use to define a function called draw_lines(), that extrapolate, interpolate them to form one, unique line. The function would work in the following way:
* we calculate the slope and the intercept to the y axis for each line
* we populate 2 empty arrays with the found values, dividing them into "left line" lines (positive slope) and "right line" lines (negative slope)
* to filter out what may not be acceptable as lane, we use a threshold finally average the value for slopes and intercepts 
* we can take the y_min of all the lines as the y_min for the final line
* finally, we can use the line equation to find the missing x values from the values of the mean intercept, the mean slope, the y_min and the y_max which we set equal to the bottom of the image. We use this final lines with the source image to blend it in the final output.

### 2. Potential shortcomings

The shortcoming of this pipeline is the robustness to different light conditions. As this can be seen from the final challenge video, shadows and curved road change the sun direction and this impacts the detection of the lines a lot.

Another shortcoming will definitely be the usage of a fixed shape for the masking. In case of a lane change, we will soon lose one of the lanes and will have to rely on a single source to execute the function.


### 3. Possible improvements

Conversion to HSV colorspace would help solve the problem, as observed from similar projects.

Another potential improvement, in the case of videos, could be to smooth the lanes using previous frames output, in a sort of moving average. 
