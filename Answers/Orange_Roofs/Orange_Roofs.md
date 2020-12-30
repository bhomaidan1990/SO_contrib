# Orange Roofs segmentation

> This answer for this [Question](https://stackoverflow.com/questions/65396244/how-can-i-obtain-a-representation-of-the-roofs-from-a-aerial-image-using-rgb-dif/65400191#65400191) in StackOverflow.

---

## How to segment Orange Roofs in Python

> **Theory:** Thresholding in **HSV** Space.
> Reference [Link](https://stackoverflow.com/a/48367205)

![Theory](https://i.stack.imgur.com/gyuw4.png "HSV")

---

> Python Code
> Reference [Link](https://www.pyimagesearch.com/2014/08/18/skin-detection-step-step-example-using-python-opencv/)

```py
import numpy as np
import matplotlib.pyplot as plt 
import cv2

# Read image
img = cv2.imread('roofs.jpg')
converted = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)

# Reference: https://www.pyimagesearch.com/2014/08/18/skin-detection-step-step-example-using-python-opencv/
# define the upper and lower boundaries of the HSV pixel
# intensities to be considered 'skin'
lower = np.array([0, 48, 80], dtype = "uint8")
upper = np.array([12, 255, 255], dtype = "uint8")

skinMask = cv2.inRange(converted, lower, upper)
# apply a series of erosions and dilations to the mask
# using an elliptical kernel
kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (7, 7))
skinMask = cv2.morphologyEx(skinMask, cv2.MORPH_CLOSE, kernel, iterations = 1)
# blur the mask to help remove noise, then apply the
# mask to the img
skinMask = cv2.GaussianBlur(skinMask, (5, 5), 0)
skin = cv2.bitwise_and(img, img, mask = skinMask)
# show the skin in the image along with the mask
cv2.imshow("images", np.hstack([img, skin]))
# waits for user to press any key 
# (this is necessary to avoid Python kernel form crashing) 
cv2.waitKey(0) 
  
# closing all open windows 
cv2.destroyAllWindows() 
```

---

### Input

![Input Image](https://i.stack.imgur.com/D62OO.jpg "Input Image")

### Output

![Output Image](https://i.stack.imgur.com/KcFzP.jpg "Result Image")