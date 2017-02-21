
#**Finding Lane Lines on the Road** 

###1 Description

This project consists of a simple python script (Jupiter notebook) to detect lane lines from an image for self-driving purpose. It uses Image Recognition techniques to identify lane lines and draw it over the original image. The script itself can work over one or a sequence of images (i.e. video).

The main goals of this project is to develop a sequence of operations (pipeline) that gets as input a raw image  and outputs an annotate image where the lane lines are marked. Figures 1 and 2 depict examples of the input and output images.

[//]: # (Image References)
[image1]: ./test_images/solidWhiteRight.jpg "Figure 1. Input"
[image2]: ./test_images/copy_solidWhiteRight.jpg "Figure 2. Output"
[image3]: ./test_images/part_solidWhiteRight.jpg "Figure 3. Partial Image with Lines"

---

![alt text][image1]
#####Figure 1. Input Image

![alt text][image2]
#####Figure 2. Output Image

---

The sequence of operations is presented at Section 2. Possible shortcoming for the techniques used and improvements are presented at Sections 3 and 4, respectively.

###2 Pipeline: Image Manipulation 

The process to identify line lanes and draw it over the original image can be divided into a sequence of operations that manipulates the input image in order to extract its key features. This pipeline of operations is implemented at the "add_lines" function (see notebook) and its operations are presented below following the execution order and highlighting which helper function the operation is part of.

####1.1 Color Selection and Gray scale conversion: 
The helper function grayscale apply a color filter followed by a gray scale conversion. The color filter threshold is adjusted to filter dark colors but keep bright colors such as yellow and white. After that, the image color depth RGB is downscaled to Gray scale.

####1.2 Gaussian Smoothing: 
As a prior step before applying the Candy Transform, the pipeline smooths the image using Gaussian smoothing with five kernels at the gaussian_bluer helper function.

####1.3 Canny Transform: 
The helper function canny performs the canny transformation to detect edges at the image. Low and High thresholds are set empirically as 80 and 100, respectively.

####1.4 Region Mask: 
It temoves non interesting areas of the image using the geometric form trapezium, all pixel outside of the trapezium are set to black. This operations is coded at the region_of_interest helper function.

####1.5 Hough Transform and Line drawing: 
The helper function hough_lines applies the hough transform and draws the lines. The parameters for the hough transform are also set empirically. The line drawing follows a straightforward implementation where first, for both sides, we pick two points for each line. The key point is to choose the pair of points that best describes the slope and constant of the lane line. For this, we have used the idea of choosing the closest and farthest point from the bottom. After that, for each line, we compute the line function (slope and constant) in order to estimate new points that best fit the lane. With the line function in hand, we consider the first point of the line to have y equal to the image height and x is obtained from the line function; for the second point we use the farthest y we have found previously and x is also obtained from the line function. This procedure is done for both sides, left and right lines. Figure 3. depicts an example of a image with lines.

---

![alt text][image3]
#####Figure 3. Image with Lines

---

####1.6 Stack Lines: 
The lines are stacked over the original image at the last step of the pipeline.

###3 Shortcomings

Possible shortcomings may arise for different situations such as image illumination, type of the road, if it is raining, snowing, etc. This problem is related to the parameters used for some of the operations of the pipeline, i.e. the parameters used for the canny and the houg transform. These parameters are set empirically and the results obtained are strongly tied to the nature of images used, in other words, the results depends that the image to be analyzed are similar to the ones used to set these parameters. Becuase of this, I believe neural networks are better suited for this type of problem.


###4 Improvements

A possible improvement should be to work a better color filter to avoid light gray without removing the yellow lanes. The color filter used failed to remove the light gray colors, resulting in some errors for the "challenge" video. Another improvement should be to use small segments of lines instead of only one line per side, allowing a more natural draw for corners. Finally, I believe that using neural networks would greatly improve the results.
