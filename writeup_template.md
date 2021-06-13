# **Finding Lane Lines on the Road** 


### Reflection

My pipeline consists of the following steps : 
1) Convert the image to grayscale.
2) Find Canny Edge transform of the image.
3) Identify the region of interest with respect to the car using a trapezoidal quadrilateral of fixed vertices.
4) Apply hough transform to the image and identify lines.
5) Filter and remove lines with slope between 0.5 and -0.5 as they are remnants of irrelevant edges from Canny transform.
6) Seperate the remaining lines into positiveLines and negativeLines corresponding to their slopes.
7) Find the average slope and average x-coordinate of the intercept to the bottom of the image.
8) Find the x intercept to the top of the vertice taking average slope and average bottom intercept as reference. Extrapolate this line seperately for positiveLines and negativeLines.

Steps 6-8 are implemented in the draw_lines() function. HyperParameters for Canny Edge Detection, HoughTransform are determined using a visual tool I developed using tkinter which can be found at the last section of the notebook.

### 2. Potential Shortcomings with my pipeline

I found several shortcoming while tuning hyperparameters. I logged them as failCase scenarios to analyse. Some of them are : 

1) Objects closer to the lane (such as cars) couldn't be trimmed by Canny Edge Transform and they were identified as lines in the hough transform. So I implemented a slope filter to remove these lines.
2) Increasing the maxLineGap caused lines to be formed between the two edges of the same lane such as this : 
![Problem with increasing maxLineGap](examples/problematicScenario1.jpg)
I took the hint from a stackoverflow thread to increase minLineLength as much as we want and to decrease maxLineGap as much as we can in order to get "true" lines from the transform. 
3) Finding the average slope and intercept was extremely sensitive to vanishing lane images. So I tried to use the nearest lane marking to extrapolate. This increase sensitivity even worse that once the nearest lane marking crossed, the lines shifted by a huge margin taking the furthest marking as reference. I was able to tweak the Hough parameters to make the average extrapolation stable.
4) The other shortcoming is obviously evident in the challenge.mp4. The algo doesn't work for curved lines and fixed vertices. Changing the sides of the vertices based on lanes from previous images can help resolve this problem.


### 3. Possible improvement to my pipeline

A possible improvement would be to make vertice edges dynamic. And using a transform that converts points to a parabolic (or a similiar curve equation) space could also work.
