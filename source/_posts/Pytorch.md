---
title: Pytorch
date: 2024-02-27 10:38:48
tags: python
categories:
    - Python
---
# Pytorch

- 딥 러닝 오픈 소스 라이브러리
- 동적 계산 그래프 제공
- 신경망 훈련 및 딥 러닝 앱 작업
- 텐서 계산, 자동 미분 지원

ex) 신경망 생성

```python
import torch
import torch.nn as nn
import torch.optim as optim
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import accuracy_score

# 아이리스 데이터셋 불러오기
iris = load_iris()
X = iris.data
y = iris.target

# 훈련, 학습 데이터 나누기
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 피쳐
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

# 데이터 텐서 계산
X_train_tensor = torch.tensor(X_train, dtype=torch.float32)
y_train_tensor = torch.tensor(y_train, dtype=torch.long)
X_test_tensor = torch.tensor(X_test, dtype=torch.float32)

# 신경망
class SimpleNN(nn.Module):
    def __init__(self, input_size, hidden_size, num_classes):
        super(SimpleNN, self).__init__()
        self.fc1 = nn.Linear(input_size, hidden_size)
        self.relu = nn.ReLU()
        self.fc2 = nn.Linear(hidden_size, num_classes)

    def forward(self, x):
        x = self.fc1(x)
        x = self.relu(x)
        x = self.fc2(x)
        return x

# 모델 및 손실률
input_size = X.shape[1]
hidden_size = 8
num_classes = len(set(y))
model = SimpleNN(input_size, hidden_size, num_classes)
criterion = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=0.001)

# 모델 학습
num_epochs = 100
for epoch in range(num_epochs):
    optimizer.zero_grad()
    outputs = model(X_train_tensor)
    loss = criterion(outputs, y_train_tensor)
    loss.backward()
    optimizer.step()

# 테스트셋으로 예측
with torch.no_grad():
    model.eval()
    predictions = torch.argmax(model(X_test_tensor), dim=1).numpy()

# 정확도
accuracy = accuracy_score(y_test, predictions)
print(f'Accuracy: {accuracy}')
```
![](/image/ta.PNG)

- 시각화

```python
import matplotlib.pyplot as plt

losses = []

for epoch in range(num_epochs):
    optimizer.zero_grad()
    outputs = model(X_train_tensor)
    loss = criterion(outputs, y_train_tensor)
    loss.backward()
    optimizer.step()
    
    losses.append(loss.item())

plt.plot(range(1, num_epochs + 1), losses, label='Training Loss')
plt.xlabel('Epochs')
plt.ylabel('Loss')
plt.title('Training Loss Over Epochs')
plt.legend()
plt.show()
```
![](/image/tl.PNG)
