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
[image2]: ./test_images/solidYellowCurve_region.jpg

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 

Firstly, I defined the region of interest where the lanes are likely to be detected. I chose to use a trapezium shape instead of a triangle to avoid detection of incorrect objects far away in front of the vehicles. In defining the region, instead of using fixed end points value, I have used ratios of the input image size (i.e. bottom boundaries set at ~ 8% of image size from image edge), so that it would work for videos of various size.

![alt text][image2]

Next, I converted the images to grayscale and applied the a GaussianBlur filer with a kernel size of 5 to smoothing out the image.

Following the filtering, I used the Canny Edge Detection function, with low and high threshold set at 40 and 100. These two values are found to give good results in detected lane lines of interest, not omiting too much details and at the same time, not giving too many false lane line detections. 



First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


###3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...