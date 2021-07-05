---
title: MATLAB_scale
date: 2021-07-05 09:53:13
tags: python
categories:
    - Python
---

MATLAB을 이용한 크기 조정하기

```python

import cv2
import numpy as np

img_src = cv2.imread('figure/fruits_gray.png',cv2.IMREAD_COLOR)
height, width = img_src.shape[:2]

img_zero = np.zeros((height, width, 3), dtype=np.uint8)

img_dst = cv2.resize(img_src, (int(width), int(height)))
img_zero[0:int(height), 0:int(width),:] = img_dst

M  = np.array([[1,0,int(width/4)],[0,1,int(height/4)]], dtype=float)
img_dst = cv2.warpAffine(img_zero, M, (width, height))

#img_result = cv2.hconcat([img_src, img_zero])
cv2.imshow("dst", img_dst)
cv2.waitKey()
cv2.destroyAllWindows()
```
