

# **Finding Lane Lines on the Road** 

<i>Clark Hochgraf
<br>Dec 29, 2019</i>

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road from a color image.
* Reflect on the performance of the pipeline and potential enhancements

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./test_images/solidWhiteCurve.jpg "Solid White Curve"


---

## Reflection

### 1.Description of Pipeline, including modification of the draw_lines() function.
    
My pipeline consisted of 6 steps. At each stage the image output was renamed for clarity e.g. for the conversion to grayscale, the image became img_g. 

* The image was converted from color to grayscale (img_g). 
* Next, the image was blurred with a gaussian filter with 9x9 kernel (img_gb). 
* Canny edge detection was applied with a low threshold of 100, and high of 200 (img_gbc). 
* The vertices of a region of interest were found and applied to mask of the region where lane lines are expected while ignoring other regions of the image (img_gbcr). 

* Then a hough transform was used to find lines in the image (img_gbcrl).
* Finally, the average lines were then overlayed on the original image (img_gbcrlw).

In order to draw a single line on the left and right lanes, I modified the <strong>draw_lines()</strong> function so that only lines that fit into a model of "left lane" or "right lane" were kept. First the slope and offset of each line was found, lines with slopes and offsets that were not "left lane" or "right lane" were discarded. The average slope and offset was found for the remaining left lane lines and then the right lane lines. 


---



### 2. Shortcomings with current pipeline


A few noticeable shortcoming of the current pipeline include:
1. In the road videos, the lane marker line overlay is jittery, jumping around from frame to frame. While not inaccurate, it is distracting.
2. When the lane contrast is low, such as in the challenge video where a yellow lane goes over a concrete bridge, the hough transform can fail to detect the lane. This can be noticed as the lane line hold a fixed position for  number of video frames and then jumping to the correct lane location once the lane image has sufficient contrast.
3. The lane markers are at times longer than the lane in the image. 
4. The lane markers are straight lines and do not match the curvature of the lane.
5. The thresholds for region of interest, canny, are hardcoded.
6. If no lines are detected in the image, a run-time error NONETYPE will occur in the draw_lines() function


### 3. Possible improvements to pipeline

To address these shortcomings, the following improvements could be made:
1. For the jittery lane markers, adding a low pass filter to smooth the lane marking locations could improve the visual display of the lane marker and also reduce jitter in the steering wheel command later during self-driving. The filter will have to respond fairly quickly so as to not impair the steering response as the car drifts in the lane
2. Improved contrast through analysis in the color space might make it easier to detect a faded yellow lane on concrete.
3. The lane marker length could be fine tuned based on information such as image size/shape, output of canny and hough transforms
4. The lane markers could be modified to be curved lines or segmented lines.
5. The thresholds could be parameterized for more convenient tuning.
6. Add error handling for the case where no lines are detected in an image.










# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project you will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

To complete the project, two files will be submitted: a file containing project code and a file containing a brief write up explaining your solution. We have included template files to be used both for the [code](https://github.com/udacity/CarND-LaneLines-P1/blob/master/P1.ipynb) and the [writeup](https://github.com/udacity/CarND-LaneLines-P1/blob/master/writeup_template.md).The code file is called P1.ipynb and the writeup template is writeup_template.md 

To meet specifications in the project, take a look at the requirements in the [project rubric](https://review.udacity.com/#!/rubrics/322/view)





#**Background Material**

The Project
---

## If you have already installed the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) you should be good to go!   If not, you should install the starter kit to get started on this project. ##

**Step 1:** Set up the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) if you haven't already.

**Step 2:** Open the code in a Jupyter Notebook

You will complete the project code in a Jupyter notebook.  If you are unfamiliar with Jupyter Notebooks, check out [Udacity's free course on Anaconda and Jupyter Notebooks](https://classroom.udacity.com/courses/ud1111) to get started.

Jupyter is an Ipython notebook where you can run blocks of code and see results interactively.  All the code for this project is contained in a Jupyter notebook. To start Jupyter in your browser, use terminal to navigate to your project directory and then run the following command at the terminal prompt (be sure you've activated your Python 3 carnd-term1 environment as described in the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) installation instructions!):

`> jupyter notebook`

A browser window will appear showing the contents of the current directory.  Click on the file called "P1.ipynb".  Another browser window will appear displaying the notebook.  Follow the instructions in the notebook to complete the project.  

**Step 3:** Complete the project and submit both the Ipython notebook and the project writeup

## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).

