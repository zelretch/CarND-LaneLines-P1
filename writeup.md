#**Finding Lane Lines on the Road** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, 
then I applied gaussian smooth with kernal size 5
then I find edges with canny edge detection
then I applied a four sided polygon mask
finnaly I run hough on edge detected image and draw lines overlayed with original one
Most parameters used in the pipeline come from the hough quiz.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by  
trying to group lines into left lane and right lane group, and then run cv2.fitline on them. Initially I was grouping them just by checking if their slope is > or < than 0, the result wasn't very good. Then I tried to throw out the outliers by imposing a min and max slope and also a slope variance parameter to throw out lines with slope too far away from the average slope. This parameter is called slope_threshhold in the code. The result looks ok in the white lane video but not so good in the yellow lane video. What's annoying is the short white lane next to the right yellow lane. So I introudced a color filter to try to throw out the lines with blue color too far away from the group average(because yellow and white mainly differed in blue). However, I tried hours of adjusting the blue threshhold, but still couldn't get rid of the white short line when calculating the line fit for the yellow lane. In the end I had to adjust the hough parameter to throw out shorter lines to make the yellow line video works

![alt text][image1]


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the dotted lane is too short, then we might see big varations from frame to frame. 

Another shortcoming could be that if the road has high curvature, i.e. when the car is turning in the challenge video, the line it draws will be wrong


###3. Suggest possible improvements to your pipeline

A possible improvement would be try to fit a curve instead of a straight line to handle the situation when the car is turnning. 
