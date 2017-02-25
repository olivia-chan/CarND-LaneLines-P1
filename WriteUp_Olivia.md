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

In order to do so, my approach was to group the left lines and the right lines and compute the average gradient and average 'y-intercept'. I also performed some filtering to remove lines of gradient that obviously did not represent the lane lines. As the lines could be defined as a line with the equation of "y=mx+b", I defined two fixed y positions: one being the bottom of the image and around 65% from the top of image. Then the corresponding x position of the end points were calculated. If the line intercept is found to be without the right region, i.e. the lines cuts the bottom of the image within the left and right corner, I would then calculate the average gradient and intercept of the lines satisfying the above conditions.

Finally, instead of modifying the draw_lines, I used the cv2.line function to draw the final single lines. 

![alt text][image5]



###2. Potential shortcomings with current pipeline
With the approach described above, there are certain limitations. 

- Edges detection from a converted grayscale image is the primary way to detect lanes". However, with only this approach, colours are not taken into account. There are possibiltiies that the system will detect lines from irrelvant edges like shadows. If such lines were detected, that will affect the average gradient and intersect outcome, making the final line inaccurate compared to the actual lane marking.

- Also, with the current approach, the system does not keep track of the line defined in the last frame. As a result, there is possibiltiy to detect a line in the current frame which is very much different from the line defined in the last frame, which is in reality not feasible.

###3. Suggest possible improvements to pipeline

A possible improvement to address the above shortcoming is to incorporate colour filtering (i.e. only consider lines in certain colours e.g. white or yellow colour) together with edge detection. In this case, lines detected from shadows would be filted out. 

Another possible improvement is to perform additional filtering, e.g. only consider lines with a plausible gradient and intercept range. For example, if a vertical line appears in the middle of the image, that should not be considered as the lane. 

Lastly, other possible improvements could be to give more weights to lines that are longer in length and within the right gradient range. Also, given the high refresh rate of the the video, lane mark can only be changing in gradient and intercept gradually. We can keep track of the current line, and give more weight to the lines that are within certain tolerance of the previous detected line to further filter out irrevant lines to increase accuracy of the final result.

