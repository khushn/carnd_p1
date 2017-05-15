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
 ####1. Divide the lines into candidate left lane line and right lane lines
  
  I saw there were around 35/40 lines on each frame. I applied a simple heuristic of -ve slope values become potentially left lane lines and +ve ones to the right lane line. Also I applied a filter of absolute value of slope between .5 and .9. This is to reject horizontal(ish) lines. 
  
 ####2. Combine the lines based on slope value. 
 
  Keeping a tolerance value like.05, i.e if slope difference is within this range then probably lines belong to a feature (e.g. lane)
  
 ####3. Selecting the lines based on max frequency 
   For the first frame, and for subsequent frames if needed, I select the lines based on no. of occurences for a given slope.
   
 ####4. Filter of low deviation from previous frame to minimize jitter
   For noisy images (e.g transition between shadow to light and vice versa) as in the case of [challange video](./challenge.mp4), it helps if we move the line gradually between frames. For this I have a weighted averaging method, which gives more weightage to the prev frame lines, and moves them slightly in the direction of the current frame lines, if they are too much away. This helped in reducing jitter and also the noise.
  
 I was able to get reasonably good markings for all the [images](./test_images/out) and for all the videos, as well. 
 * [Solid white right](./solidWhiteRight.mp4) processed output is at [white right](./white.mp4)
 * [Solid Yello left](./solidWYellowLeft.mp4) processed output is at [yellow left](./yellow.mp4)
 * [Optional Challenge](./challenge.mp4) processed output is at [challenge out](./extra.mp4)

 
 Link to the [Jupyter notebook](./P1.ipynb)
 
###2. Potential shortcomings

####1. Tradeoffs 

 I figured there are tradeoffs, for e.g. in order to reduce the jitter of lane lines, we also may end up having a nice looking marking but more noisy. This was particularly evident in the 3rd challenge video. 

####2. Global variable
 
 I had to keep a prev frame line info, the slope m, and offset c, to compare with the next frame, to reduce jitter. Keeping global varianles is not a good idea in general.

###3. Possible improvements

One possible improvement, which I can do, is select lines in a frame which compare the slope m and offset c, and select lines with a higher frequency which are also the closest. This can make the markings in the challenge clip better, especially in the area, where we come out of the shadow and its quite bright.

I need to explore the moviepy library's usage in more detail, particularly around VideoClipFile's fl_image() function, as to the possibility of passing some extra data. 

