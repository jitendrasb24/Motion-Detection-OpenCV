# Motion-Detection-OpenCV

#### Motion Detection technology in Commonly used in our daily life. This feature is used in CCTV Cameras to detect any kind of motion in the video frame. In this repository, we are going to make a motion detection script using OpenCV in Python.

# Installation

First of all make sure you have Python installed in your system.

Secondly you have to install pip. It can be installed easily using command prompt.

    python -m pip install

The last requirement is the OpenCV module. It can be installed using pip.

    pip install opencv-python

# STEPS FOR MOTION DETECTION USING OPENCV AND PYTHON

**STEP 1:-** ***Capturing Real-time video from a camera or Reading recorded video.***

**STEP 2:-** ***Read two frames from the video source.***

**STEP 3:-** ***Find Out the Difference between the next frame and the previous frame.***

**STEP 4:-** ***Apply Image manipulations like Blurring, Thresholding, finding out contours, etc.***

**STEP 5:-** ***Finding Area of Contours to detect Motion.***

**STEP 6:-** ***Drawing Rectangle on the Motion Detected Areas.***

**STEP 7:-** ***Showing the video in the window.***

# CAPTURING VIDEO

We use this command in openCV to Capture Video, in which you have to detect movement.

    cap = cv2.VideoCapture(0)

If:

    cv2.VideoCapture(0) ---- Captures video from your webcam.

    cv2.VideoCapture("video path") ---- It uses the video stored in the given path.

    cv2.VideoCapture(1) ---- Captures video from external Camera i.e: Usb camera.

# READING TWO FRAMES

In Order to Detect motion in a frame we also need to have the previous frame with us, so we can say there is any kind of movement in the next frame or not.

    ret, frame1 = cap.read()

    ret, frame2 = cap.read()

# FINDING THE DIFFERENCE BETWEEN THE FRAMES

**absdiff()** function is used to find the absolute difference between frame1 and frame2. As the Difference can’t be negative in this case, so absolute difference is taken.

    diff = cv2.absdiff(frame1, frame2)

The difference between the two frames is stored in diff variable and the next process will be held on the difference frame.

# IMAGE MANIPULATIONS

Now it is time for image manipulation techniques on the different frames.

First of all the “difference” frame is converted from coloured to grayscale image using **cvtColor()** function in OpenCV.

    diff_gray = cv2.cvtColor(diff, cv.COLOR_BGR2GRAY)

**The diff_gray grayscaled image is then blurred using Gaussian Blur, using a 5×5 Kernel.**

The blurring method removes noise from an image and thus good for edge detection.

    blur = cv2.GaussianBlur(diff_gray, (5, 5), 0)

The Blurred image is then thresholded using the **cv.THRESH_BINARY**. Basically the image now contains either 255 or 0 in the matrix.

This threshold function takes a grayscale image and also takes the min and max threshold values.

**cv.THRESH_BINARY** – returns 0 if the color value of that pixel is below the min threshold value, and returns max threshold value if the pixel is greater than the min threshold value. And thus the image contains only low or high value.

    _, thresh = cv2.threshold(blur, 20, 255, cv.THRESH_BINARY)

The thresholded image is then dilated. Dilation means Adding pixels to the boundaries of objects in an image. The total number of iterations is 3 in this case, which means the same function will be repeated 3 continuous times.

    dilated = cv2.dilate(thresh, None, iterations=3)

# FINDING CONTOURS

The dilated image is then used for finding out contours. 

**findCotours()** use **cv.RETR_TREE** and **cv.CHAIN_APPROX_SIMPLE** technique for finding out contours in the dilated image.

**findContours()** returns a list of contours.

    contours, _ = cv2.findContours( dilated, cv.RETR_TREE, cv.CHAIN_APPROX_SIMPLE)

contours variable is a list of all the contours that were found using **findContours()** function.

    for contour in contours:

            (x, y, w, h) = cv2.boundingRect(contour)
	    
            if cv2.contourArea(contour) < 900:
	    
                continue
		
            cv2.rectangle(frame1, (x, y), (x+w, y+h), (0, 255, 0), 2)
	    
            cv2.putText(frame1, "STATUS: {}".format('MOTION DETECTED'), (10, 60), cv2.FONT_HERSHEY_SIMPLEX,
	    
                        1, (217, 10, 10), 2)
			
**boundingRect()** function returns the coordinates and width and height of the bounding rectangle.

**cv.contourArea(contour)** takes contour as an argument and returns the area bound by the contour.

If the area of the contour is above 900 in this case. Then a rectangle is drawn covering that object, showing that the object moved when compared to the last frame, and the area covered by the motion was above 900.

Text is also put on the video frame “STATUS: MOTION DETECTED” when there is motion detected in the video frame.

# Author

### ***JITENDRA SINGH BISHT***
