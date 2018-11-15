# **Finding Lane Lines on the Road** 

## Writeup

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./writeup_images/solidWhiteRight.jpg "Example Output"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. Steps are explained below:

1. Colorspace conversion
In this phase, original colored images are transormed to grayscale color space. This is required because we do not need color information at all.

2. Gaussian Blur
Gaussian blur filter is applied on image in order to remove noises that cause canny edge detector to find small edges. 

3. Canny Edge Detector
Canny Edge Detector algorithm finds edges on given gray scale image. Canny edge detector can easily find lane lines as edges since lane lines make a great change in image (for example 0 -> 255 value change in extreme case).

4. ROI Definition
A ROI is defined within the image in order to remove edge information that is not required for lane lane detection. ROI Definition simply defines a region that is required for finding lanes  lines that the car is moving between.

5. Hough Line Transformation
Hough line transformation gets the image and transforms it to Hough Space. This transformation is required since slope for vertical lines cannot be defined.  After the transformation lines are filtered according to given threshold value. 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function so that lane lines  that are belonging to left and right can be separated. I implemented this feature by calculating the slope of every line. Slope will be negative for left lines and positive for right lines. 

Here is an example output for overall pipeline:

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline

One possible shortcoming can be unable to detect edges for a given image. We assuming that lane lines generate edges on black road however lane lines could be erased in real life scenario. This situation causes Canny Edge Detector to not detect any edges at all and this breaks the whole pipeline. 

Another possible shortcoming can be related when switching across the lanes. The pipeline classifies lanes as left or right lanes according to the slopes of the lines. However left/right classification cannot be performed when switching lanes. 


### 3. Suggest possible improvements to your pipeline

As you can see in output videos pipeline sometimes generates lane lines that is originated from a left lane point but terminated at a right lane point or vice versa. I think the possible problem is slopes are converging to zero for lane lines that are far away from the vehicle. I implemented a small filter in order to remove this problem. The filter simply classifies a lane line as left if slope is smaller than -0.2 and classifies it as right if slope is bigger than 0.2. According to the tests, pipeline works better when mentioned filter is used however it cannot the solve problem totally. 

A better way could extracting lane lines in a smaller roi (roi with smaller height) and drawing lane lines in a bigger area. These lines can be drawn using the slopes of extracted lane lines. In this method, I am expecting to get lane lines that are not totally fit to the lanes. However I think it worths to give a try to the method. 