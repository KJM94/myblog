---
title: 'CAN_arduino_f'
date: 2021-08-12 09:39:07
tags: c++
categories:
    - C++
---

CAN통신을 응용한 아두이노 학습

- red led, green led 순차점멸

```
String str_1 = "Choose your led : ";
int num;

void setup()
{
   Serial.begin(9600);
   pinMode(10,OUTPUT);
   pinMode(9,OUTPUT);
}
void loop()
{
  Serial.print(str_1);
   // 시리얼 통신으로 입력이 될때까지 기다림
  while(Serial.available() == 0){
  }
  Serial.println(num);
  num = Serial.parseInt();
  
  if(num == 1){
    digitalWrite(10,HIGH);
    Serial.println("RED LED ON");
  }
  else if(num ==2){
    digitalWrite(9,HIGH);
    Serial.println("GREEN LED ON");
  }
  else{
    digitalWrite(10,LOW);
    digitalWrite(9,LOW);
  }
  delay(200); 
}
```
