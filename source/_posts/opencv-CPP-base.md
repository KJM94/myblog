---
title: opencv_CPP_base
date: 2021-06-27 20:36:41
tags: c++
categories:
    - C++
---

# OpenCV C++ 기본 셋팅


- 이미지
```
#include <opencv2/opencv.hpp>
#include <iostream>

using namespace cv;
using namespace std;

int main()
{	
	Mat img_src;
	img_src = imread("chese.jpg", IMREAD_COLOR);

	pyrDown(img_src, img_src);

	int height = img_src.rows;
	int width = img_src.cols;

	int w_q1 = width / 4;
	int h_q1 = height / 4;
	int w_q2 = width / 4;
	int h_q2 = width / 4;

	Mat img_crop1, img_crop2;
		
	img_crop1 = img_src(Rect(30, 180, 200, 440)).clone();
	img_crop2 = img_src(Rect(570, 320, 160, 280)).clone();

	cvtColor(img_crop1, img_crop1, COLOR_BGR2GRAY);
	cvtColor(img_crop1, img_crop1, COLOR_GRAY2BGR);

	cvtColor(img_crop2, img_crop2, COLOR_BGR2GRAY);
	cvtColor(img_crop2, img_crop2, COLOR_GRAY2BGR);

	img_crop1.copyTo(img_src(Rect(30, 180, 200, 440)));
	img_crop2.copyTo(img_src(Rect(570, 320, 160, 280)));


	imshow("crop", img_src);
	waitKey();
	destroyAllWindows();


	//Mat img_src;
	//img_src = imread("chese.jpg", IMREAD_COLOR);
	//int height = img_src.rows;
	//int width = img_src.cols;

	//int w_q1 = width / 4;
	//int h_q1 = height / 4;

	////img_crop = img_src[h_q1:h_q3, w_q1 : w_q3].copy()
	//Mat img_crop;
	//img_crop = img_src(Rect(w_q1, h_q1, width / 2, height / 2)).clone();

	//cvtColor(img_crop, img_crop, COLOR_BGR2GRAY);
	//cvtColor(img_crop, img_crop, COLOR_GRAY2BGR);

	//img_crop.copyTo(img_src(Rect(w_q1, h_q1, width / 2, height / 2)));


	//imshow("src", img_src);
	//waitKey();
	//destroyAllWindows();

	/*Mat img_src;

	img_src = imread("balloon.jpg", IMREAD_COLOR);

	int height = img_src.rows;
	int width = img_src.cols;

	Mat img_dst1;
	Mat img_dst2;

	pyrDown(img_src, img_dst1);
	pyrUp(img_src, img_dst2, Size(width * 2, height * 2));

	resize(img_src, img_dst1, Size(width, height), INTER_LINEAR);
	resize(img_src, img_dst2, Size(), 0.5, 0.5);

	imshow("src", img_src);
	imshow("dst1", img_dst1);
	imshow("dst2", img_dst2);
	waitKey();
	destroyAllWindows();*/


	/*Mat img_src;
	img_src = imread("balloon.jpg", IMREAD_COLOR);

	Mat img_dst;
	flip(img_src, img_dst, 1);

	imshow("src", img_src);
	imshow("flip", img_dst);


	waitKey();
	destroyAllWindows();*/


	//Mat img_src;
	//img_src = imread("balloon.jpg", IMREAD_COLOR);
	//int height = img_src.rows;
	//int width = img_src.cols;

	//Mat matrix = getRotationMatrix2D(
	//	Point(width / 2, height / 2), // 회전할 때 중심점
	//	45, //회전 각도
	//	1); // 이미지 배율

	//Mat img_dst;
	//warpAffine(img_src, img_dst, matrix, Size(width, height));

	//imshow("src", img_src);
	//imshow("rotation Result", img_dst);
	//waitKey();
	//destroyAllWindows();




	//Mat frame;
	//VideoCapture cap("aespa.mp4");

	//if (!cap.isOpened())
	//{
	//	cout << "동영상을 열 수 없습니다." << endl;
	//	return -1;
	//}

	//Mat frame_gray;

	//while (1)
	//{
	//	bool ret = cap.read(frame); // frame 은 img_src 동일
	//	// 이미지 처리를 해주면 됨
	//	cvtColor(frame, frame_gray, COLOR_BGR2GRAY);
	//	//
	//	imshow("Video", frame);
	//	imshow("Video-Gray", frame_gray);
	//	
	//	int key = waitKey(33);

	//	if (key == 27) // ESC 키
	//		break;
	//}

	//cap.release();


	//Mat img_src;
	//img_src = imread("balloon.jpg", IMREAD_COLOR);

	//// 이미지를 그레이영상으로 바꿈
	//Mat img_gray;
	//cvtColor(img_src, img_gray, COLOR_BGR2GRAY);

	//imshow("Src", img_src);
	//imshow("Gray", img_gray);

	//waitKey(0);
	//destroyAllWindows();

	// cout << "OpenCV Version : " << CV_VERSION << endl;
}
```


- 동영상

```
#include <opencv2/opencv.hpp>
#include <iostream>

using namespace cv;
using namespace std;

int main()
{
	Mat frame;
	VideoCapture cap("aespa.mp4");

	if (!cap.isOpened())
	{
		cout << "동영상을 열 수 없습니다." << endl;
		return -1;
	}

	Mat frame_gray;

	while (1)
	{
		bool ret = cap.read(frame); // frame 은 img_src 동일
		// 이미지 처리를 해주면 됨
		cvtColor(frame, frame_gray, COLOR_BGR2GRAY);
		
		//
		imshow("Video", frame);
		imshow("Video-Gray", frame_gray);
		
		int key = waitKey(33);

		if (key == 27) // ESC 키
			break;
	}

	cap.release();

	waitKey(0);
	destroyAllWindows();
}
```

