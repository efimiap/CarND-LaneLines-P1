## CarND-LaneLines-P1
The goal of this project is to build a robust pipeline that detects lane lines on the road. 


### Pipeline Description:
 My pipeline consists of the following steps, which were the result of different testing approaches. 
First, I convert the image to grayscale and apply Gaussian Smoothing with kernel size = 7. To detect the differences in intensity of the neighbour pixels in the images, I apply Canny Edge Detection with rate: 1:3 [low_threshold, high_threshold] = [50, 150]. For outliers’ rejection, I apply a mask at the region of interest, which identifies the region in which I expect to find the lines. As a final step for the detection, I apply Hough Transform line detection to find the line segments on the road. For solid line detection and as requested, I connect/average/extrapolate the line segments in the function draw_lines.   

In order to draw a single line on the left and right lanes, first I calculated the slope of the line segments in hough space, using the function (y2-y1)/(x2-x1). For the corner cases, where x2=x1, I applied a large slope, to avoid division by 0. Based on the sign of the slope and the position of the x-points in the image compared to the center point, I classified the lines to left and right and distinguished the x and y points in two different arrays for each side. Finally I applied linear regression to the points using the numpy polyfit function and got the m and b parameters of the linear function y = mx +b. After calculating the x and y points for each linear function, I draw the right and left lines on the image. The y points are the min and max values of the applied region of interest and the x are calculated based on the linear function: x = (y-b)/m   

Note: The expected argument of the cv2.line function is integer. For this reason, I converted the inputs to int32   

The final output is the result of all the above mentioned steps and the function weighted_image which draws semitransparent lines on the original image, based on the output of the hough_lines   

Results in: test_images_output folder and test_videos_output of the zip file.  

### Potential Shortcomings

 - A potential shortcoming of the current pipeline could be a misidentification of the lines because of the determined region of interest. In case of an unexpected movement of the vehicle, e.g. if it leaves from the middle of the lines, the algorithm won’t identify the lines and the path planning will fail to bring the car back on track.
 - Another shortcoming could be that this pipeline might identify random lines or signs written on the road. In the UK the signs SLOW and STOP are written in the asphalt with white paint. My algorithm might identify those as lane lines. 
 - Another shortcoming could be the computational speed of the algorithm, as for lanes detection is scans every pixel of each frame. 

### Possible Improvements

 - Regarding the computational speed, we could possibly drop frames in order for the detection to be faster or we could crop a region of interest from the beginning, e.g. I could crop out the sky from the images 
 - Another improvement is outliers’ rejection, as there is a large drift on the detected lines on some frames
 - Another improvement could be a more generic detection of lines of every color. 
 - Finally we could use as prior the information we have from each previous frame for applying lane tracking

### Result
