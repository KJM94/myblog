---
title: opencv_base
date: 2021-06-26 13:29:49
tags: python
categories:
    - Python
---

# OpenCV 기본 셋팅 작성

- 이미지

```python
import cv2

# 이미지 불러오기 (상대경로)
# "C:\\Users\\hkit\\PycharmProjects\\OpenCvProj\\balloon.jpg"
# "C:/Users/hkit/PycharmProjects/OpenCvProj/balloon.jpg"
img_src = cv2.imread("balloon.jpg", cv2.IMREAD_COLOR)

cv2.imshow("Source", img_src)
cv2.waitKey()
cv2.destroyAllWindows()
```

- 동영상

```python
import cv2

capture = cv2.VideoCapture('aespa.mp4')

while cv2.waitKey(33) < 0:
    if(capture.get(cv2.CAP_PROP_POS_FRAMES) ==
            capture.get(cv2.CAP_PROP_FRAME_COUNT)):
        capture.get(cv2.CAP_PROP_POS_FRAMES, 0)
    ret, frame = capture.read()
    cv2.imshow("Video", frame)

capture.release()
cv2.destroyAllWindows()
```
