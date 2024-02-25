# Lab 6 - Recognition and Classification
*_Peter Cheung, version 1.0, 29 Feb 2024_*

This laboratory session is designed to support the contents of Lectures 10 and 11 of the module.  

## Task 1: Image resizing

The following image is the famous painting by van Gogh called 'Cafe Terrace at Night', which can be found in the file **_'cafe_van_gogh.jpg'_** in the assets folder.  

<p align="center"> <img src="assets/cafe_van_gogh.jpg" /> </p>

Write a Matlab program to read this file and build the image pyramid by resize the image by a factor of 1/2, 1/4, 1/8, 1/16 and 1/32 by drop every other rows and columns.  Then display all six images as a montage of size [2 3]. 

To drop every other rows and columns in an image, you can use a power Matlab syntax: (start: increment: end) for the next.  Try this Matlab command:

```
1:2:10
1:3:10
```
The first command returns the values: 1, 3, 5, 7 and 9.
The second command returns the values: 1, 4, 7, 10.

Matlab provides a proper image resizign function **_imresize(I, scale)_** where I is the input image and scale is the factor to resize.  So 0.5 means the image is reduced by a factor of 2. This function first filter the image by a lowpass filter (Gaussian) that remove the high frequency components before subsampling by skipping pixels.  This prevent aliasing and the introdduction of artifacts.

Repeat the above exercise by adding code to properly resize the image with the **_imresize_** function.

## Task 2: Pattern Matching with Normalized Cross Correlation

In this task, we will examine how to use Matlab's normalized cross correlation (NCC) function **_normxcorr2( )_** to match a template in file **_'assets/template1.tif'_** to that of the image **_'salvador_grayscale.tif'_**.

The following code will compute the NCC function and plot it as a 3D plot:

```
clear all; close all;
f = imread('assets/salvador_grayscale.tif');
w = imread('assets/template2.tif');
c = normxcorr2(w, f);
figure(1)
surf(c)
shading interp
```

Try this code and explore the correlation between the template and the image.  You should be able manually locate the position of the template in the image. This will be the location that the normalized cross correlation value = 1.0, i.e. an exact match.

Now we want to detect the peak location automatically. This is achieve with:

```
[ypeak, xpeak] = find(c==max(c(:)));
yoffSet = ypeak-size(w,1);
xoffSet = xpeak-size(w,2);
figure(2)
imshow(f)
drawrectangle(gca,'Position', ...
    [xoffSet,yoffSet,size(w,2),size(w,1)], 'FaceAlpha',0);
```

Find out for yourself what the Matlab function **_find_** does.  Comment on the results.

Test this procedure again with the second template image **_'template2.tif'_**.

It is clear that NCC is only useful if the match between template and image is exact or nearly exact. 



https://uk.mathworks.com/help/vision/ug/point-feature-types.html#bukg_4d
