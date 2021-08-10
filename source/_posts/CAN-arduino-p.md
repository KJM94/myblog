---
title: CAN_arduino_p
date: 2021-08-11 00:03:37
tags: c++
categories:
    - C++
---

CAN통신을 응용한 아두이노 학습

- base led 점멸

```
int pinArr[] = {13,12,11,10};
int pinSize = sizeof(pinArr) / sizeof(int);
int delayTime = 200;
void setup()
{
   for(int i=0; i<pinSize; i++)  
      pinMode(pinArr[i], OUTPUT);
}

void loop()
{
  for(int i=0; i<pinSize; i++){ 
     digitalWrite(pinArr[i], HIGH);
     delay(delayTime);
     digitalWrite(pinArr[i], LOW);
     delay(delayTime);
  }
}
```