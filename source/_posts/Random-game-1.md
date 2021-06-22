---
title: Random game 1
date: 2021-03-23 21:55:42
tags: C++
categories:
    - C++
---

## 가위바위보

- 1을 입력하면 가위
- 2를 입력하면 바위
- 3을 입력하면 보
- 컴퓨터가 내는 수는 판마다 랜덤(난수)
- 0을 입력하면 종료
- 0, 1, 2, 3을 제외한 숫자 입력시 예외처리
- 게임결과 출력

```c++
#include <iostream>
#include <string>
#include <random>

using namespace std;

string text_temp(int num)
{
	if (num == 1)
	{
		return "가위";
	}
	else if (num == 2)
	{
		return "바위";
	}
	else
	{
		return "보";
	}
}

int main()
{
	int Count = 1;
	int player;

	cout << "가위바위보" << endl;

	random_device rd;
	mt19937 gen(rd());

	uniform_int_distribution<int> dis(1, 3);

	string com_out;

	while (1)
	{
		int com = dis(gen);

		cout << "가위(1)바위(2)보(3)종료(0) : "; cin >> player;

		Count++;

		if (player == 0)
		{
			cout << "종료" << endl;
			break;
		}

		else if (player == com)
		{
			cout << "비겼습니다." << endl;
			cout << "상대(" << text_temp(com) << ") : 플레이어(" << text_temp(player) << ")" << endl;
		}

		else if ((player == 1 && com == 3) || (player == 2 && com == 1) || (player == 3 && com == 2))
		{
			cout << "이겼습니다." << endl;
			cout << "상대(" << text_temp(com) << ") : 플레이어(" << text_temp(player) << ")" << endl;
		}

		else if (player > 3)
		{
			cout << "숫자는 1, 2, 3중에서만 입력해주세요." << endl;
		}
		
		else
		{
			cout << "졌습니다." << endl;
			cout << "상대(" << text_temp(com) << ") : 플레이어(" << text_temp(player) << ")" << endl;
		}
	}

	return 0;
}
```
