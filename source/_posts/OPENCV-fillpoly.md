---
title: OPENCV_fillpoly
date: 2021-07-09 01:00:07
tags: python
categories:
    - Python
---

OpenCV를 이용한 다각형 그리기

- 차선을 따라 그려 나머지 부분은 검은색으로 채우기

```python
import cv2
import numpy as np

capture=cv2.VideoCapture("race.mp4")
trap_bottom_width=0.9
trap_top_width=0.2
trap_height=0.3


while cv2.waitKey(33)<0:
    if(capture.get(cv2.CAP_PROP_POS_FRAMES)==capture.get(cv2.CAP_PROP_FRAME_COUNT)):
        capture.set(cv2.CAP_PROP_POS_FRAMES,0)
    ret,frame=capture.read()


    img_mask = np.zeros_like(frame)

    if frame.ndim > 2:
        channel_count = frame.shape[2]
        ignore_mask_color = (255,255,255)
    else:
        ignore_mask_color = 255

    imshape = frame.shape
    vertices = np.array([[ \
        ((imshape[1] * (1 - trap_bottom_width)) // 2, imshape[0]), \
        ((imshape[1] * (1 - trap_top_width)) // 2, imshape[0] - imshape[0] * trap_height), \
        (imshape[1] - (imshape[1] * (1 - trap_top_width)) // 2, imshape[0] - imshape[0] * trap_height), \
        (imshape[1] - (imshape[1] * (1 - trap_bottom_width)) // 2, imshape[0])]] \
        , dtype=np.int32)

    cv2.fillPoly(img_mask, vertices, ignore_mask_color)

    img_load = cv2.bitwise_and(frame, img_mask)
    img_onload=cv2.addWeighted(frame,1.0,img_load,1.0,0)

    cv2.imshow("Video",img_load)


capture.release()
cv2.destroyAllWindows()
```
