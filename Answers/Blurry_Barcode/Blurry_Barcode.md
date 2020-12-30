# Read Blurry Barcode in Python

> This answer for this [Question](https://stackoverflow.com/questions/64111254/read-blurry-barcode-in-python-with-pyzbar/64175996) in StackOverflow.

---

## How to Read Blurry Barcode in Python

> **Theory:** The **barcode** consists of **vertical black zone** to represent **each bar**, so I did the **summation of the rows**, and **thresholded** the whole image depending on an empirical value near the mean of the summation.

---

> Python Code

```py
#========================
# Import Libraies
#========================
import numpy as np
import cv2 as cv
import matplotlib.pyplot as plt 
from pyzbar import pyzbar

#------------------------
# Read Image
#========================
img = cv.imread('barcode_example.jpg', cv.IMREAD_GRAYSCALE)

# #------------------------
# # Morphology
# #========================
# # Closing
# #------------------------
closed = cv.morphologyEx(img, cv.MORPH_CLOSE, cv.getStructuringElement(cv.MORPH_RECT, (1, 21)))

# #------------------------
# # Statistics
# #========================
print(img.shape)
dens = np.sum(img, axis=0)
mean = np.mean(dens)
print(mean)

#------------------------
# Thresholding
#========================
thresh = closed.copy()
for idx, val in enumerate(dens):
    if val< 10800:
        thresh[:,idx] = 0

(_, thresh2) = cv.threshold(thresh, 128, 255, cv.THRESH_BINARY + cv.THRESH_OTSU)

#------------------------
# plotting the results
#========================
plt.figure(num='barcode')

plt.subplot(221)
plt.imshow(img, cmap='gray')
plt.title('Original')
plt.axis('off')

plt.subplot(224)
plt.imshow(thresh, cmap='gray')
plt.title('Thresholded')
plt.axis('off')

plt.subplot(223)
plt.imshow(thresh2, cmap='gray')
plt.title('Result')
plt.axis('off')

plt.subplot(222)
plt.hist(dens)
plt.axvline(dens.mean(), color='k', linestyle='dashed', linewidth=1)
plt.title('dens hist')

plt.show()

#------------------------
# Printing the Output
#========================
barcodes = pyzbar.decode(thresh2)
print(barcodes)
```

### Result

![Input Image](https://i.stack.imgur.com/zxDwH.jpg "Blurry Barcode")

![Result Image](https://i.stack.imgur.com/Gs8xk.png "Result Image")

> Output: 

```
[Decoded(data=b'00004980072868003004',
 type='CODE128',
 rect=Rect(left=34, top=0, width=526, height=99),
  polygon=[Point(x=34, y=1), Point(x=34, y=99),
   Point(x=560, y=98), Point(x=560, y=0)])]
```
