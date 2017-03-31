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

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I applied a gaussian blur on the images to reduce noise.
This prepared the image for the canny edge detection algorithim. The canny edge detection algorithim works by essentially taking a derivative of the pixels. The areas in which the most change occured could be identified as edges. Of course the threshold for the derivative is user defined. I found that a min of 150 and a max of 200 worked for me, but I did not experiment much with this threshold. After the canny edge detection, I cropped the contents of the image that are of most interest to me. I took a general area that was a little outside of the road. It is crucial to do this step after the canny algorithm. If you crop before the edge detection step, your edge detector will identify the bounds of your newly cropped image as an edge. You don't want that! Now that we have the edges in point form, I used the hough transformation to translate these points into lines. I chose a relatively small intersection threshold of 20 because some of the lane lines were botchy and small. I had a min line length of 10. I found that this number was small enough to pick up the beaten up lane lines but large enough to filter out cars and random noise. My max line gap was also a small 10, because I wanted to filter out random islands of noise from other cars and splotches on the road.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first splitting the lines into left and right lines. I mistakenly thought that left lines would have positive slopes and vice versa, but since the origin is at the top, the left lines have a negative slope. I also added a horizontal threshold of 480 in order to filter out random noise and crazy outliers. 

After categorizing the x y points of the lines, I fit them to one line with `np.polyfit` and computed one line for each lane.


###2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when you're steering your car on a curvy road. This would break my draw lines method because left lanes could have both positive and negative slopes. Also drawing the lines as straight lines would no longer work. 

Another shortcoming could be driving at night, or driving uphill with a different horizon point. Driving under rainy or snowy conditions would also mess with this algorithm, as the edge detector might not work as well.

###3. Suggest possible improvements to your pipeline

A possible improvement would be to have some sort of a better filtering mechanism for determining whether a lane is left or right, perhaps a combination of slope and position would mitigate the problems associated with curved roads. To draw the actual line I could fit the points to a higher degree polynomial which would allow me to draw a curved line instead of a straight line. 

Other possible improvements could be to use a machine learning model to identify lanes to complement a more mathematical approach like we used in this project. 