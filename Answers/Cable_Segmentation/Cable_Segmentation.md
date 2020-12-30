# Cable Segmentaion in Python

> This answer for this [Question](https://stackoverflow.com/questions/64145295/object-extraction-and-construction/64166586) in StackOverflow.

---

## How to segment coloured cable in Python

> **Theory:** Thresholding in **HSV** Space.
> Reference [Link](https://stackoverflow.com/a/48367205)

![Theory](https://i.stack.imgur.com/gyuw4.png "HSV")

> Morhpological Closing
> [Theory Link](https://towardsdatascience.com/image-processing-class-egbe443-6-morphological-filter-e952c1ec886e)

![Input Image](https://homepages.inf.ed.ac.uk/rbf/HIPR2/figs/closebin.gif "Noisy Text Image")

> [Morpho closing Image source](https://homepages.inf.ed.ac.uk/rbf/HIPR2/close.htm)

---

> Python Code

```py
#========================
# Import Libraies
#========================
import numpy as np 
import matplotlib.pyplot as plt 
import cv2 as cv 

#------------------------
# Read Image
#========================
img = cv.imread("img.jpg")
imgHSV = cv.cvtColor(img, cv.COLOR_BGR2HSV)
img = cv.cvtColor(img, cv.COLOR_BGR2RGB)

#------------------------
# Threshold Image
#========================
## mask of red
mask1 = cv.inRange(imgHSV, (0, 30, 0), (10, 255,255))
mask2 = cv.inRange(imgHSV, (170, 30, 0), (180, 255,255))

mask = cv.bitwise_or(mask1, mask2)

mask = np.tile(mask, (3,1,1))
mask = np.swapaxes(mask , 0, 1)
mask  = np.swapaxes(mask , 1, 2)

print(mask.shape)

th1 = cv.bitwise_and(img,mask)

#------------------------
# Morphology
#========================
kernel1 = np.ones((7,7),np.uint8)
kernel2 = np.zeros((70,70),np.uint8)
kernel2[10:60, 10:60] = 1

img_opn = cv.morphologyEx(th1 ,cv.MORPH_OPEN ,kernel1)
img_cls = cv.morphologyEx(img_opn, cv.MORPH_CLOSE, kernel2)

#------------------------
# Results Visualization
#========================
plt.figure(num = "Red Cable")

plt.subplot(221)
plt.imshow(img)
plt.title('Original')
plt.axis('off')

plt.subplot(222)
plt.imshow(th1)
plt.title('Thresholded')
plt.axis('off')

plt.subplot(223)
plt.imshow(img_opn)
plt.title('Opening')
plt.axis('off')

plt.subplot(224)
plt.imshow(img_cls)
plt.title('Result')
plt.axis('off')

plt.show()
#------------------------
```

---

### Result

![Input Image](https://i.stack.imgur.com/h4x9T.jpg "Noisy Text Image")

![Result Image](https://i.stack.imgur.com/krg30.png "Result Image")
