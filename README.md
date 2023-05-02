Download Link: https://assignmentchef.com/product/solved-comp590-homework-4-panoramic-image
<br>



In this project, you will implement a system to combine a series of horizontally overlapping photographs into a single panoramic image. I will give you the harris corner feature detector, descriptor and matching function. You will use them to first detect discriminating features in the images and find the best matching features in the other images. Then, using RANSAC, you will automatically align the photographs (determine their overlap and relative positions) and then blend the resulting images into a single seamless panorama.

The skeleton code provided have three files main.py h_matrix.py h_matrix_ransac.py

The main.py only takes a pair of images as input, the same as your HW3 with the additional part that compute pairwise homography using RANSAC function. It is a good starting point to work on the RANSAC and homography computation function and check your result. But the final goal of this project is to stitch a series of images together. Therefore, you need to modify the main.py or write your own file that read a series of images and compute their transformations and stitch them together.

To stitch a series of images, e.g., image ​<em>a, b, c</em>​ and ​<em>d</em>​, first you pick one image as the base image that all other images warp into. Suppose you pick ​<em>b</em>​ as the base image, then you compute homography​<em> h</em>​<em>ab</em><sub>​</sub><em>, h</em>​<em>cb</em><sub>​</sub><em>, h</em>​<em>dc</em><sub>​</sub>. Afterwards, you can warp ​<em>a</em>​ into ​<em>b</em>​ by ​<em>h</em>​<em>ab</em><sub>​</sub><em>*a</em>​, <em>c</em>​ into ​<em>b</em>​ by ​<em>h</em>​<em>cb</em><sub>​</sub><em>*b</em>​, and d into b by ​<em>h</em>​<em>cb</em><sub>​</sub><em>*h</em>​<em>dc</em><sub>​</sub><em>*d</em>​.

You need to figure out the new coordinates for where to put ​<em>b</em>​ and then inversely warp every other images into the panorama image.




<h2>2.1 Compute initial feature matching</h2>

You can use your own harris corner detection code, or the provided code, or any library functions for computing the feature detection and matching. The goal of this part is to get a list of coordinates of the matched feature points.

Since our harris corner detection code is not a multi-scale feature detector, for different testing images you will need to modify the parameters, e.g., non-maximum suppression window size, feature descriptor window size, sigma or corner detection window size, in order to achieve better initial feature detection and matching. If there are too many outliers, then the RANSAC algorithm will fail.




<h2>2.2 RANSAC</h2>

Implement the RANSAC algorithm discussed in class by filling in the skeleton code h_matrix_ransac.py. The explanation and pseudo code for RANSAC are in lecture 10’s slides.




<h3>2.2.1 Fitting the homography</h3>

Within the RANSAC algorithm you will solve for the homography using the matrix operations discussed in class to solve equations like ​M a = b​

Filling in the estimate_homography() function in h_matrix.py for this part.




<h3>2.2.1 Calculate model inliers</h3>

Figure out how many matches are inliers to a model. Fill in compute_h_matrix_inliers() function to project all points using the homography, and check if a projected point is within some threshold distance of the actual matching point in the other image.










<h1>2.3 Combine the images with a homography​</h1>

Now you have to stitch the images together using the estimated homography from the RANSAC algorithm.




The first step is to figure out the bounds to the image. To do this you can project the corners of image ​b​ back onto the coordinate system of image ​a​ using ​Hinv​. Then you can figure out the “new” coordinates and where to put ​image a​ on the canvas. Paste ​a​ in the appropriate spot in the canvas.




Next you loop over pixels in the canvas that might map to ​image b​, perform the mapping to see if they do, and if so fill in the pixels with the appropriate color from ​b​. The mapping will likely land between pixel values in ​b​ so use bilinear interpolation to compute the pixel value.