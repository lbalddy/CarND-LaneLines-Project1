#**Finding Lane Lines on the Road** 

##Writeup - Luis B. Salgado

**Files given with this project**

- This writeup
- P1.ipynb, contains the code.
- Result videos can be seen at the end of this write up.


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./Image_1_solved.png

---

### Reflection

###1. Pipeline

The pipeline I used was really simple and strightforward, using material we seen this first week. 

1. Convert the image to grayscale
2. Apply a Gaussian filter using a 5x5 mask. 
3. Apply Canny algorithm to find edges. 
4. Apply two masks. Each mask will correspond to the region of the image where the line is more likely to be located. Each mask is a four sides irregular poligon.
5. Apply hough to these masks separately. This is better because I can use draw lines without the need to differiantate right or left by the slope. 
6. Combine both Hough transforms with the image.

  ![image2]

###2. Draw lines modification

I modified draw lines just to give one pair of points as a line. This pair of points is calculated finding the median slope, and median intersect of all the lines given by HoughLinesP.

Because the Hough transform was applicated separately over the right line and the left line I didn't needed to select right and left lines using slope sign.

###3. Shortcomings

The first and worst shortcoming I see in my pipeline is curves. Because ROI for finding lines is static, any change in the line can destroy the way I calculate the ROI.

While I didn't need to differentiate colors in the image with yellow lines, a yellow in different light conditions could become indistinguishable from the lane because of gray transformation.

In one or two seconds of the yellow video, the left line doesn't fits correctly the actual line. This is easy solvable using thresholds to admit lines. However, while this actually should work, I preferred not to do that. That approach looks really problematic to deal with curves.

###4. Suggest possible improvements to your pipeline

##### Dinamic mask generation #####

I think that the mask that we generate to find the lane should be generated dinamycally taking in account the lines on the road. This is the best way to takle with any curve. 

##### Camera correction ######

We need to correct the distorsions created by the lens of the camera

##### Yellow treatment #####

Yellow should be treated in any way. For example, changing it for white. 

##### View transformation #####

We should transformate the view in a way that the lines in the road are parallel. This means, that we should transform the view like seeing the road from the heights.

##### Ponderate lines by their length #####

One of the fellow students made a moving average based on the length of the lines that he detected with Hough algorithm. This is a good idea for this problem. 

###5. Videos ###

White lines
https://youtu.be/b94uSp7SVMg

Yellow lines
https://youtu.be/_VaE--x05VA

 