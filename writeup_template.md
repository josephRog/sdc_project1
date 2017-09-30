# **Finding Lane Lines on the Road** 

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipeline I designed consisted of 7 discrete stages. There are as follows...
#### 1) Filter the image by pixel value.

I started by using cv2.inRange() to throw away all of the pixels that were below a certain threshold. I specifically ignored throwing away data related to the blue channel. This was so that I could keep yellow pixels included.

#### 2) Convert the image to grayscale.

#### 3) Blur the image.

#### 4) Apply the Canny function.

#### 5) Split the image into left and right hand trapezoids.

I split the image into left and right side during part of the pipeline so that I could have the possibility of using different Hough Transform parameters on different types of lanes. My thoughts were that this might be necessary given that some lanes are dashed and some are solid. For this project at least I managed to use only one set of parameters for the Hough Transform.

#### 6) Apply the Hough Transform to each side and use that data in draw_lines() to draw on the extended lane lines.

I modified the draw_lines() function to keep track of the average values (x1,y2,x2,y2) to get some representation of what the "average" line was for on side of the image. I then used these average points to find and average slope for one of the lanes. Then, using the magic of (y = mx + b) I calculated the intercepts of where the average line would hit if extended to the middle and bottom of the image frame. I then took these intercept points and used then in cv2.line() to draw my extrapolated lane lines.

#### 7) Combine the left and right sides back together to form one image and then composite this on the original image.


### 2. Identify potential shortcomings with your current pipeline
Since only strait lines are being used. This pipeline would not be sufficient to work on roads that curved more sharply. It's also likely that the pipeline is over-tuned for just these type of lanes. Lanes that are actually created as a series of dots on the road may not work at all. Or what if the road has no lanes?

I was also not able to get my code working properly for the challenge video. I'm not sure why this is the case. I made several still frames from the video and tested my pipeline on those discrete images to great success! However using the whole video things were not even close to correct. They are so wrong that I suspect something more complicated (or simple maybe) is going on than just tuning parameters differently. I don't see why it should work so well on single frames from that video but not the video itself. Oh Well! I will have to investigate that more later.

To demonstrate this, an example frame from the challenge video that the code works fine with had been added in the test_images folder as "hard.jpg". The code has already been set to select this one by default.


### 3. Suggest possible improvements to your pipeline
When playing through the videos, the lane lines are a little noisy and skip about some. Perhaps this could be smoothed out by keeping data from several frames at a time and averaging the lane positions. It may also be possible to do this by taking several Hough Transforms with different parameters and averaging those to smooth out the movement. 

It is possible to know when NO lines are detected when doing the Hough Transform. Perhaps if this happens then we should try again but with some different parameters until SOME lines are actually detected.

It also needs to be made more generic so that it could run on an arbitrary set of image sizes and formats.


