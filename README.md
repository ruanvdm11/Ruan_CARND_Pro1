#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

---

### Reflection

###1. Pipeline Discussion

The pipeline that I created in order to successfully complete the necessary task is sequentially explained in the following tables. However a brief explanation will also be given as to how the lines of the Hough transform were utilised in the creation of the road lines.

---
| 1. At first it was necessary to import the correct image, whether it be a still image or a frame from a clip.   | 2. Then, using the openCV conversion tool, the initial image was converted to grayscale.   |
| :------------- | :------------- |
| ![alt text](https://github.com/ruanvdm11/Ruan_CARND_Pro1/blob/master/Pipeline_Image1_Import.jpg?raw=true)      | ![alt text](https://github.com/ruanvdm11/Ruan_CARND_Pro1/blob/master/Pipeline_Image2_Gray.jpg?raw=true) |

---
| 3. The Canny edge function was then applied in order to determine the largest colour gradients. It is at these large colour gradients that we expect the lines.  | 4. Seeing as we do not care too much about what happens off of the road, a spatial constraint was applied and implemented on the Canny edges image.   |
| :------------- | :------------- |
| ![alt text](https://github.com/ruanvdm11/Ruan_CARND_Pro1/blob/master/Pipeline_Image3_Canny.jpg?raw=true)   |   ![alt text](https://github.com/ruanvdm11/Ruan_CARND_Pro1/blob/master/Pipeline_Image4_Canny_in_Border.jpg?raw=true)   |

---
| 5. Now that we have loacated the lane lines we can implement the Hough function. The Hough transform transforms points in **image space** to sine waves in **Hough space**. The paramaters that correspond to line intersections can be used to create lines ie. theta and roh.   | 6. The original image can then be overlayed with the border,green, (The area in which the Canny function is applied) and the created Hough lines, red.  |
| :------------- | :-------------|
| ![alt text](https://github.com/ruanvdm11/Ruan_CARND_Pro1/blob/master/Pipeline_Image5_Houghlines.jpg?raw=true)   |   ![alt text](https://github.com/ruanvdm11/Ruan_CARND_Pro1/blob/master/Pipeline_Image6_Overlay_Houghlines_Border_On_Original.jpg?raw=true)   |

---
| 7. The Hough lines are then used to determine prominent lines which can be used to extrapolate a straight line that fits the road line |
| :------------- |
| ![alt text](https://github.com/ruanvdm11/Ruan_CARND_Pro1/blob/master/Pipeline_Image7_Resultant_Extrapolated_Lines.jpg?raw=true)

---
####2. The utilisation of the Hough transform data occured as follows:
* Firstly the minumum and maximum gradients were determined from the obtained points. The reason for this is because due to how the camera sees the road it will have the effect that the left road line will have a positive incline and the right line will have a negative incline. Obtaining the largest and smallest (largest in the negative direction) inclines will give the points to be used.
* Now that we have inclines we can calculate the linear line running through these points and extrapolate where necessary.
* The boundaries for the extrapolation were decided to be:
    * The bottom of the image
    * The top of the search boundary. (As indicated by the green line in step 6 above)
* Thus, only x values for the plot had to be calculated and this was done by using the incline, offset, and y values that were obtained.
* The offset and incline values was then stored in a matrix so that previous values can be called for averaging purposes.

####The following shows the test images that were obtained.
| 1. Solid White Curve   | 2. Solid White Right   |
| :--- | :--- |
| ![alt text](https://github.com/ruanvdm11/Ruan_CARND_Pro1/blob/master/SolidWhiteCurve.jpg?raw=true)   | ![alt text](https://github.com/ruanvdm11/Ruan_CARND_Pro1/blob/master/SolidWhiteRight.jpg?raw=true)   | 

---
| 3. Solid Yellow Curve  | 4. Second Solid Yellow Curve  |
| :--- | :--- |
| ![alt text](https://github.com/ruanvdm11/Ruan_CARND_Pro1/blob/master/solidYellowCurve.jpg?raw=true)   | ![alt text](https://github.com/ruanvdm11/Ruan_CARND_Pro1/blob/master/solidYellowCurve2.jpg?raw=true)   | 

---
| 5. Solid Yellow Left  | 6. White Car Lane Switch   | 
|:--- |:--- |
| ![alt text](https://github.com/ruanvdm11/Ruan_CARND_Pro1/blob/master/solidYellowLeft.jpg?raw=true)   | ![alt text](https://github.com/ruanvdm11/Ruan_CARND_Pro1/blob/master/whiteCarLaneSwitch.jpg?raw=true)   |


###2. Potential shortcomings with the current pipeline


One possible shortcoming of my pipeline is that at this stage the lines being created seem 'shaky'. If the averaging is increased it is seen that the pipeline does not predict the road line well. Therefore, in its current configuration is predicts the line well but with some flutter.

In the 'challenge' clip the excessive colour gradients definitely puts the pipeline through its paces. Therefore, another shortcoming is that when shadows (many clour gradients) are detected the pipeline might deviate slightly from its prediction.


###3. Possible improvements to the pipeline

A definite improvement is to concatenate the code. At this stage it seem bulky even though it would be capable of calculating a 33 FPS clip real time.  I inserted a timer to see how long the script runs per clip to estimate this value. The reason this is important criteria is because the faster an image can be processed, the faster a decision can be made which is crucial in this environment.


The sensitivity to edges found by the Canny function and the lines created by the Hough transform can be adjusted. During testing of the pipeline it was clear that there are many, unnecessary, edges created that can possibly confuse the the rest of the analyses done by the pipeline.
