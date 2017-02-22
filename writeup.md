#**Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Pipeline description

Overall my pipeline has 5 steps.
 * Grayscale
 * Canny edge detection
 * Filtering out the region of interest
 * Using Hough transform to draw the potential lane lines
 * Superimpose the lane lines on the original image
 
 For the [test images](./test_images) just tuning of the parameters like threshold, rho, theta, min and max gap was sufficient. And so it was to get the [broken lane markings](./white_dashed_out.mp4). 

 But for making the lane lines continuous, I had to make significant improvements to the draw_line() method. To list the enhancements: 
 * Divide the lines into candidate left lane line and right lane line, based on slope (-ve goes to left, and +ve goes to right)
 * 
  

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


###3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
