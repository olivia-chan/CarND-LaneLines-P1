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
[image3]: ./test_images/solidYellowCurve_edges.jpg
[image4]: ./test_images/solidYellowCurve_houghlines.jpg
[image5]: ./test_images/solidYellowCurve_final.jpg
---

### Reflection

###1. Pipeline Description

As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 

Firstly, I defined the region of interest where the lanes are likely to be detected. I chose to use a trapezium shape instead of a triangle to avoid detection of incorrect objects far away in front of the vehicles. In defining the region, instead of using fixed end points value, I have used ratios of the input image size (i.e. bottom boundaries set at ~ 8% of image size from image edge), so that it would work for videos of various size.

![alt text][image2]

Next, I converted the images to grayscale and applied the a GaussianBlur filer with a kernel size of 5 to smoothing out the image.

Following the filtering, I used the Canny Edge Detection function, with low and high threshold set at 40 and 100. These two values are found to give good results in detected lane lines of interest, not omiting too much details and at the same time, not giving too many false lane line detections. 

![alt text][image3]

Once the edges were detected, I used the HoughLines function to detect straight lines. Multiple parameters were attempted and good results were obtained when the min_line_length and max_line_gap was set to 20 and 10 respectively. With this setting, the function was able to return short lines that may be presented as part of a dashed lane line but not too many irrelavent lines.

![alt text][image4]

Now, given that multiple segments of lines were detected, the next step was to be able to define only 1 straight line on the left hand side of the vehicle and 1 straight line on the right.  

In order to do so, my approach was to group the left lines and the right lines and compute the average gradient and average 'y-intercept'. I also performed some filtering to remove lines of gradient that obviously did not represent the lane lines. 

Once this is done, the left line could be defined as a line with the equation of "y=mx+b".
I defined two fixed y positions: one being the bottom of the image and around 65% from the top of image. Then the corresponding x position of the end points were calculated.

Finally, instead of modifying the draw_lines, I used the cv2.line function to draw the single lines. 

![alt text][image5]




###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


###3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...