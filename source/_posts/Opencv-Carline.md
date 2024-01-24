---
title: Opencv_Carline
date: 2024-01-24 15:28:57
tags: PYTHON
categories:
    - Python
---
# OpenCV를 이용한 차선 검출

- 원본 사진

![](/image/138913710-66237810-b79b-4615-8248-8d881c432cf8.png)

- opencv 코드

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

    # white 뽑아내기
    white_thr = 200
    lower_white = np.array([white_thr, white_thr, white_thr])
    upper_white = np.array([255, 255, 255])
    white_mask = cv2.inRange(frame, lower_white, upper_white)
    white_image = cv2.bitwise_and(frame, frame, mask=white_mask)

    # yellow 뽑아내기
    img_hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
    lower_yellow = np.array([20, 100, 100])
    upper_yellow = np.array([40, 255, 255])
    yellow_mask = cv2.inRange(img_hsv, lower_yellow, upper_yellow)
    yellow_image = cv2.bitwise_and(img_hsv, img_hsv, mask=yellow_mask)

    mixed_img = cv2.addWeighted(white_image, 1.0, yellow_image, 1.0, 0.0)

    cv2.imshow("line", mixed_img)


capture.release()
cv2.destroyAllWindows()
```

- 컬러 값을 조정하여 내가 원하는 부분만 검출해내기

![](/image/138913915-648553c1-fe43-460a-90a5-46c912fb6243.png)