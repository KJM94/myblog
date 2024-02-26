---
title: Keras
date: 2024-02-26 09:31:03
tags: python
categories:
    - Python
---
# Keras

- 딥러닝 모델 구축, 학습 인터페이스
- Tensorflow와 함께 사용되며, 간편한 구성과 확장성 제공

ex) 선형회귀 예시(입력 변수와 출력 변수 간의 선형 관계 모델링)

```python
import numpy as np
import tensorflow as tf
from tensorflow import keras

# 예시 데이터
x = np.random.rand(100, 1)
y = 5 * x + 5 * np.random.rand(1)

# Keras의 Sequential 모델 생성
model = keras.Sequential([
    keras.layers.Dense(32, activation=tf.nn.relu, input_shape=(1,)),
    keras.layers.Dense(32, activation=tf.nn.relu),
    keras.layers.Dense(1)
])

# 모델 컴파일
model.compile(optimizer='adam', loss='mse', metrics=['mse', 'binary_crossentropy'])

# 학습
history = model.fit(x, y, epochs=500, batch_size=100)

# 시각화
import matplotlib.pyplot as plt
plt.scatter(x, y, label='y_true')
plt.scatter(x, model.predict(x), label='y_pred')
plt.legend()
plt.show()
```

![](/image/ke.PNG)