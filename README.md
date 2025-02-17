# Thresholding and Binary Morphology

## Thresholding

## What is Thresholding?

The simplest segmentation method. 

Application example: Separate out regions of an image corresponding to objects which we want to analyze. This separation is based on the variation of intensity between the object pixels and the background pixels.

To differentiate the pixels we are interested in from the rest (which will eventually be rejected), we perform a comparison of each pixel intensity value with respect to a threshold (determined according to the problem to
solve).

Once we have separated properly the important pixels, we can set them with a determined value to identify them (i.e. we can assign them a value of 0 (black), 255 (white) or any value that suits your needs).

![](https://docs.opencv.org/2.4/_images/Threshold_Tutorial_Theory_Example.jpg)

## Simple Thresholding

Here, the matter is straight forward. If pixel value is greater than a threshold value, it is assigned one value (may be white), else it is assigned another value (may be black). The function used is `cv2.threshold`. First
argument is the source image, which should be a grayscale image. Second argument is the threshold value which is used to classify the pixel values. Third argument is the ` maxVal ` which represents the value to be given if pixel
value is more than (sometimes less than) the threshold value. OpenCV provides different styles of thresholding and it is decided by the fourth parameter of the function. Different types are:

- cv2.THRESH_BINARY
- cv2.THRESH_BINARY_INV
- cv2.THRESH_TRUNC
- cv2.THRESH_TOZERO
- cv2.THRESH_TOZERO_INV

To illustrate how these thresholding processes work, let’s consider that we have a source image with pixels with intensity values $` src(x,y) `$. 
The plot below depicts this. The horizontal blue line represents the threshold $` thresh `$ (fixed).

![](https://docs.opencv.org/2.4/_images/Threshold_Tutorial_Theory_Base_Figure.png)

The documentation clearly explains what each type is meant for. [Please check out the
documentation](http://docs.opencv.org/doc/tutorials/imgproc/threshold/threshold.html).

## Task 3

Using OpenCV, load the image `images/apple.jpg` as a **grayscale** image. Perform simple **binary** thresholding in two ways: 1) using the OpenCV function mentioned above, and 2) using NumPy by setting all pixels above a certain value to 255 and others to 0. Display the thresholded image.

## Otsu Binarization

**Binarization** of an image is the process of converting the image into a format where each pixel can only be one of two possible values. For `uint8` images, these values are usually `0` (black) and `255` (white). For `float` images, the values are `0` (black) or `1.0` (white). **Binarization** is often a precursor to **thresholding**, where the image is divided into completely white and black regions, and then only the parts of the original image that are completely white in the binary image are retained. Mathematically, by multiplying the original and binary images, the pixels that are completely white in the binary image remain unchanged, while those that are completely black are multiplied by 0, resulting in a completely black pixel in the product image.

In the previous example, you manually determined the threshold. Otsu binarization is a more advanced method that determines the optimal threshold based on the **histogram** of the image, which best separates the pixels. A histogram is a graph that shows the frequency of each value in a data set. In the case of an image, the histogram shows, for each color value, how many pixels are of that color.

Let's take a look at the histogram of the `apple.jpg` image.

![image](https://github.com/user-attachments/assets/4e8c4963-191e-4f68-903d-8117ca5e8a9c)

From the histogram, it is evident that most pixels are grouped around the values 255 and 50. By approximation, we see that the optimal way to separate the pixels into two groups would be with a threshold between 150 and 200, as this threshold effectively separates the two largest groups of pixels.

## Task 4

According to the following link, implement Otsu binarization for the `apple.jpg` image. Display the resulting binary image **using Matplotlib**. Print the optimal threshold value determined by the Otsu method to the console.

Such a binary image can be used as a **mask** for the original image. A mask is a binary image where the value is `0` for all pixels that should not be visible, and the maximum value (`1.0` or `255`) for pixels that should be visible.

## Task 5

Using the Otsu binary image as a mask, apply the function `img_thresholded = cv.bitwise_and(img, img, mask=mask)` where `img` is the original grayscale image of the apple, and `mask` is the Otsu binary image.

## Improving Masks with Binary Morphology

Morphological transformations are some simple operations based on the image shape. It is normally performed on binary images. It needs two inputs, one is our original image, second one is called structuring element or kernel which decides the nature of operation. Two basic morphological operators are erosion and dilation. Then its variant forms like opening, closing, gradient etc. also comes into play.

Morphological operations such as erosion, dilation, closing and opening are common tools used to improve masks after they are generated by thresholding. They can be used to fill small holes, remove noise, increase or decrease the size of an object, or smoothen mask outlines.

Most morphological operations are once again simple kernel functions that are applied at each pixel of the image based on their neighborhood as defined by a `structuring element (SE)`. For example, `dilation` simply assigns to the central pixel the maximum pixel value within the neighborhood; it is a maximum filter. Conversely, `erosion` is a minimum filter. Additional options emerge from combining the two: `morphological closing`, for example, is a `dilation` followed by an `erosion`. This is used to fill in gaps and holes or smoothing mask outlines without significantly changing the mask's area. Finally, there are also some more complicated morphological operations, such as `hole filling`.

### Erosion

The basic idea of erosion is just like soil erosion only, it erodes away the boundaries of foreground object (Always try to keep foreground in white). So what does it do? The kernel (SE) slides through the image (as in 2D convolution). A pixel in the original image (either 1 or 0) will be considered 1 only if all the pixels under the kernel is 1, otherwise it is eroded (made to zero).

### Dilation

It is just opposite of erosion. Here, a pixel element 1 is if atleast one pixel under the kernel is 1. So it increases the white region in the image or size of foreground object increases. Normally, in cases like noise removal, erosion is followed by dilation. Because, erosion removes white noises, but it also shrinks our object. So we dilate it. Since noise is gone, they won’t come back, but our object area increases. It is also useful in joining broken parts of an object.

### Opening

The morphological opening is nothing but erosion followed by dilation. It is useful in removing noise.

### Closing

The morphological closing  is nothing but dilation followed by erosion. It is useful in removing noise. It is useful in closing small holes inside the foreground objects, or small black points on the object.

### Morphological Gradient

It is the difference between dilation and erosion of an image.

### Tophat

Tophat is the equivalent of subtracting the result of performing a morphological opening operation on the input image from the input image itself.

### Blackhat

It is the difference between the closing of the input image and input image.

### SEs of different shapes

OpenCV provides built-in functions for creating SEs of custom shapes like circle, ellipse, cross, etc. They turn out to be useful for dufferent purposes.



