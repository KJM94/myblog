---
title: MATLAB_move
date: 2021-07-07 02:01:02
tags: python
categories:
    - Python
---

MATLAB을 이용한 좌표이동

```python

import cv2
import numpy as np

img_src = cv2.imread('figure/fruits_gray.png',cv2.IMREAD_COLOR)
height, width = img_src.shape[:2]

img_zero = np.zeros((height, width, 3), dtype=np.uint8)

img_dst = cv2.resize(img_src, (int(width*0.5), int(height*0.5)))
img_zero[0:int(height/2), 0:int(width/2),:] = img_dst

M  = np.array([[1,0,int(width/4)],[0,1,int(height/4)]], dtype=float)
img_dst = cv2.warpAffine(img_zero, M, (width, height))

angle = 45
center = (int(width/2), int(height/2))
rotation_matrix = cv2.getRotationMatrix2D(center, angle, 1)
img_dst2 = cv2.warpAffine(img_dst, rotation_matrix, (width, height))

M2  = np.array([[1,0,0],[0,1,0]], dtype=float)
img_dst3 = cv2.warpAffine(img_dst2, M2, (width, height))

#img_result = cv2.hconcat([img_src, img_zero])
cv2.imshow("dst", img_dst3)
cv2.waitKey()
cv2.destroyAllWindows()
```