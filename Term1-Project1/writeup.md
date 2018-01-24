# **Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[comparison]: ./writeup_assets/Comparison.png "Comparison"
[s1]: ./writeup_assets/s1.png "Step 1: Original Image"
[s2]: ./writeup_assets/s2.png "Step 2: Gray scale"
[s3]: ./writeup_assets/s3.png "Step 3: Gaussian Blur"
[s4]: ./writeup_assets/s4.png "Step 4: Canny Edge Detection"
[s5]: ./writeup_assets/s5.png "Step 5: Masking off the lane lines"
[s6]: ./writeup_assets/s6.png "Step 6: Masking off the individual side"
[s7]: ./writeup_assets/s7.png "Step 7: Probabalistic Hough Lines"
[s8]: ./writeup_assets/s8.png "Step 8: Linear Regression and Final Output"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The image processing pipeline involves a series of steps operating on matric containing image data. This pipeline has steps that involve changing the color mapping, detecting lines, extrapolating from those lines, and finally overlaying the predicted lines onto the original image. Beginning with the color conversion, we need to grayscale the image because this algorithm works off the intensity of the image data, not the colors. Next, we apply a gaussian blur removes noise in the picture, which might eliminate finer details, but this is okay because we care about the 'coarse' lines. Filtered data is then masked off into an area of interest, where the lane lines are most likely to be, and fed into the hough probabalistic line detection algorithm. This function looks for all line segments matching our parameters in the hough space, which can then be translated into normal space.

But, before applying the hough equations, I masked off the left and right sides of the image separately to get clear definitions of all lines on the left side of the image (and car), and the right side. With these two sets of lines, I run a linear regression algorithm to determine their probable direction. The predicted lines are finally superimposed onto the original image, and we have a clear picture of where the lanes are.

Below is a comparison of the hough lines superimposed upon the original image, and my single predicted lane line.

![alt text][comparison]

Here is a step-by-step view that describes the above process. Hover over the picture for more detail.

![alt text][s1]
![alt text][s2]
![alt text][s3]
![alt text][s4]
![alt text][s5]
![alt text][s6]
![alt text][s7]
![alt text][s8]

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when lines are detected in various places across the X axis that do not constitute a lane line. Because I'm taking every line that I find on each side of the image, and morphing them into a single line, any stray lines will skew the linear regression and create lane lines that are nowhere close to correct.

Another shortcoming could be heavy curves on the road. My lines are made to be linear, and they will always be straight in one direction. If a sharp curve were detected, the generated line would most likely be heavily clipping the actual lane lines, taking the car into a potentially dangerous situation.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to filter the lane lines more effectively, perhaps by dropping or ignoring lines with significantly different slopes than the majority of detected lines. Furthermore, it seems that because lane lines are usually detected in pairs (for each long side of the line itself, because they're actually rectangular items), I can verify that a detected line is real only if there's a 'similar' line near it. 

Another potential improvement could be to match based on color of the surronding pixels of detected lines. Lane markings are made to be high contrast, and thus it is probably a good idea to look at a color gradiant of the image near detected lines as another filtering pass in the pipeline.
