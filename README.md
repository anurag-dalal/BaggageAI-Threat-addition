# BaggageAI-Threat-addition
You are given two sets of images:- background and threat objects. Background images are the background x-ray images of baggage that gets generated after passing through a X-ray machine at airport. Threat images are the x-ray images of threats that are prohibited at airport while travelling.
Your task is to cut the threat objects, scale it down, rotate with 45 degree and paste it
into the background images using image processing techniques in python.
* Threat objects should be translucent, means it should not look like that it is cut pasted. It should look like that the threat was already there in the background images. Translucent means the threat objects should have shades of background where it is pasted.
* Threat should not go outside the boundary of the baggage. **difficult**
* If there is any background of threat objects, then it should not be cut pasted into the background images, which means while cutting the threat objects, the boundary of a threat object should be tight-bound.


## Requirements
```bash
pip install opencv-python==4.5.2.52
pip install numpy==1.19.5
pip install matplotlib==3.4.2
```

## Process
The process followed are discussed below:
* Read the threat images, and background images using the read_images() function.
* We will select each threat image and process them as follows:
    * Convert the image to grayscale.
    * Dialate the image with 5x5 matrix of ones, with iterarion 2. The bright area of the threat dilates around the background. It helps to smooth out the image.
    * Then we create a mask for the threat object using a threshold level for white and the inRange() function of OpenCV.
    * Then we crop the threat object to a square using the form_square() function.
    * Then we pad the image dynamically, so that when the threat is rotated all part of the threat image is conserved.
    * Next, we rotate the image by 45 degrees.
    * Next we select all the background images and run a loop for all the back ground images.
        * The threat image dynamically with respect to the background image using the autoscaler() function.
        * Next, we find the centre co-ordinate of the largest contour found in the background image using the get_xy() function.
        * We then properly fix x and y position in background image using the size of threat object.
        * Next we place the threat in the background image using the place_threat() function.
        * The final task is to save the image in a location.

## Functions
* read_images(path): This function reads the .jpg files from a specific location and returns a list of images as numpy array and the number of images read.
* form_square(image): This function takes in a image(threat, with the background set to black using the inRange() OpenCV function) and finds the left, right, top, and bottom of the threat object, therby removing the extra background. However the threat object is not guarrented to be a square. So this function also checks the image for the height and width of the cropped threat image and pad black portion in top-buttom of left-right making it a square image. This function returns this final square image.
* pad_image(image): This function calculates the diagonal length of the image and set the height and width of the image equal to diagonal length, basically it is used so that when the threat object is rotated, no portion of the image go out of bounds.
* get_xy(background): This function craeates a binary image of the baggage using inRange() function of OpenCV and then inverts it. Next it finds the contours in the binary image. Then the contour with maximum area is selected and the center of the countour is found using moments() function of OpenCV. This center co-ordinate is returned.
* place_threat(background, threat, x=0, y=0): This function places the threat image in the background image in (x, y) location on the background
  

## Configuration
The configuration used are:
```python
# location of threat images and background images
threat_path = 'threat_images/'
background_path = 'background_images/'
save_locaion = 'generated_images/'
rotate_angle = 45
# threshold level for white
lower_white = np.array([245,245,245])  
upper_white = np.array([255,255,255])
# percent factor for resizing threat
percent_factor = 0.4
```
