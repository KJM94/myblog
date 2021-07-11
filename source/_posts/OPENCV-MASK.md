---
title: OPENCV-MASK
date: 2021-07-12 00:03:26
tags: python
categories:
    - Python
---

도로 위의 차선을 따라가기

```python
import cv2
import numpy as np

capture = cv2.VideoCapture('race.mp4')

while cv2.waitKey(33) < 0:
    if capture.get(cv2.CAP_PROP_POS_FRAMES) == capture.get(cv2.CAP_PROP_FRAME_COUNT):
        capture.get(cv2.CAP_PROP_POS_FRAMES, 0)
    ret, frame = capture.read()

    trap_bottom_width = 0.85
    trap_top_width = 0.075
    trap_height = 0.43

    def region_of_interest(frame):
        frame_mask = np.zeros_like(frame)

        if frame.ndim > 2:  # color영상이면
            channel_count = frame.shape[2]
            ignore_mask_color = (255, 255, 255)
        else:
            ignore_mask_color = 255

        imshape = frame.shape
        vertices = np.array([[ \
            ((imshape[1] * (1 - trap_bottom_width)) // 2, imshape[0]), \
            ((imshape[1] * (1 - trap_top_width)) // 2, imshape[0] - imshape[0] * trap_height), \
            (imshape[1] - (imshape[1] * (1 - trap_top_width)) // 2, imshape[0] - imshape[0] * trap_height), \
            (imshape[1] - (imshape[1] * (1 - trap_bottom_width)) // 2, imshape[0])]] \
            , dtype=np.int32)

        cv2.fillPoly(frame_mask, vertices, ignore_mask_color)

        frame = cv2.bitwise_and(frame, frame_mask)
        return frame

    if __name__ == '__main__':
        frame = cv2.imread("line.png", cv2.IMREAD_COLOR)


        frame_gray = cv2.cvtColor(frame, cv2.COLOR_RGB2GRAY)
        frame_gray = cv2.GaussianBlur(frame_gray, (5,5), 0)

        frame_canny = cv2.Canny(frame_gray, 95, 200)
        frame_canny = region_of_interest(frame_canny)

        rho = 2
        theta = 1 * np.pi/180
        threshold = 15
        min_line_len = 10
        max_line_gap = 20

        lines = cv2.HoughLinesP(frame_canny, rho, theta, threshold, np.array([]), \
                                minLineLength=min_line_len, maxLineGap=max_line_gap)

        for i, line in enumerate(lines):
            cv2.line(frame, (line[0][0],line[0][1]),(line[0][2],line[0][3]), \
                     (0,255,0), 2)

    cv2.imshow("src",frame)

capture.release()
cv2.destroyAllWindows()
```
