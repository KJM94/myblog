---
title: classification
date: 2021-04-13 16:58:58
tags: python
---


# 설정

- matplotlib 그래프를 인라인으로 출력
- 그림을 저장하는 함수

```python
# 파이썬 ≥3.5 필수
import sys

assert sys.version_info >= (3, 5)

# 사이킷런 ≥0.20 필수
import sklearn

assert sklearn.__version__ >= "0.20"

# 공통 모듈 임포트
import numpy as np
import os

# 노트북 실행 결과를 동일하게 유지하기 위해
np.random.seed(42)

# 깔끔한 그래프 출력을 위해
% matplotlib
inline
import matplotlib as mpl
import matplotlib.pyplot as plt

mpl.rc('axes', labelsize=14)
mpl.rc('xtick', labelsize=12)
mpl.rc('ytick', labelsize=12)

# 그림을 저장할 위치
PROJECT_ROOT_DIR = "../../../classification"
CHAPTER_ID = "classification"
IMAGES_PATH = os.path.join(PROJECT_ROOT_DIR, "images", CHAPTER_ID)
os.makedirs(IMAGES_PATH, exist_ok=True)


def save_fig(fig_id, tight_layout=True, fig_extension="png", resolution=300):
    path = os.path.join(IMAGES_PATH, fig_id + "." + fig_extension)
    print("그림 저장:", fig_id)
    if tight_layout:
        plt.tight_layout()
    plt.savefig(path, format=fig_extension, dpi=resolution)
```

# MNIST
- 손으로 쓴 7만개의 숫자 이미지를 모은 데이터셋

MNIST 데이터셋 내려받는 코드


```python
from sklearn.datasets import fetch_openml
mnist = fetch_openml('mnist_784', version=1, as_frame=False)
mnist.keys()
```




    dict_keys(['data', 'target', 'frame', 'feature_names', 'target_names', 'DESCR', 'details', 'categories', 'url'])



사이킷런에서 읽어 들인 데이터셋들은 딕셔너리 구조를 갖고 있음.

- 데이터셋을 설명하는 DESCR 키
- 샘플이 하나의 행, 특성이 하나의 열로 구성된 배열을 가진 data 키
- 레이블 배열을 담은 target 키


```python
X, y = mnist["data"], mnist["target"]
X.shape
```




    (70000, 784)



이미지가 7만개 있고 각 784개의 특성을 갖고 있음.
이미지가 28 x 28 픽셀이기 때문
특성은 0~255까지의 픽셀 강도를 나타냄.

이미지 한개 확인하기
- 샘플의 특성 벡터를 추출해서 28 x 28 배열로 크기를 바꾸고 matplotlib의 imshow() 함수를 사용


```python
y.shape
```




    (70000,)




```python
28 * 28
```




    784




```python
%matplotlib inline
import matplotlib as mpl
import matplotlib.pyplot as plt

some_digit = X[0]
some_digit_image = some_digit.reshape(28, 28)
plt.imshow(some_digit_image, cmap=mpl.cm.binary)
plt.axis("off")

save_fig("some_digit_plot")
plt.show()
```

    그림 저장: some_digit_plot




![output_9_1](https://user-images.githubusercontent.com/59479116/121621117-7e765c00-caa6-11eb-9c13-b99a7eb4d3d5.png)
    


숫자 5 이미지의 실제 레이블


```python
y[0]
```




    '5'



레이블은 문자열이며, 머신러닝 알고리즘은 숫자 인식이 필요하기 때문에 y를 정수로 변환함.


```python
y = y.astype(np.uint8)
```


```python
def plot_digit(data):
    image = data.reshape(28, 28)
    plt.imshow(image, cmap = mpl.cm.binary,
               interpolation="nearest")
    plt.axis("off")
```


```python
# 숫자 그림을 위한 함수
def plot_digits(instances, images_per_row=10, **options):
    size = 28
    images_per_row = min(len(instances), images_per_row)
    images = [instance.reshape(size,size) for instance in instances]
    n_rows = (len(instances) - 1) // images_per_row + 1
    row_images = []
    n_empty = n_rows * images_per_row - len(instances)
    images.append(np.zeros((size, size * n_empty)))
    for row in range(n_rows):
        rimages = images[row * images_per_row : (row + 1) * images_per_row]
        row_images.append(np.concatenate(rimages, axis=1))
    image = np.concatenate(row_images, axis=0)
    plt.imshow(image, cmap = mpl.cm.binary, **options)
    plt.axis("off")
```


```python
plt.figure(figsize=(9,9))
example_images = X[:100]
plot_digits(example_images, images_per_row=10)
save_fig("more_digits_plot")
plt.show()
```

    그림 저장: more_digits_plot




![output_16_1](https://user-images.githubusercontent.com/59479116/121621140-8930f100-caa6-11eb-91d0-e9aa605f5868.png)
    



```python
y[0]
```




    5




```python
X_train, X_test, y_train, y_test = X[:60000],X[60000:],y[:60000],y[60000:]
```

# 이진 분류기

ex) 숫자 5만 식별하기
5와 5가 아닌 것 두개의 클래스를 구분 하는 것

타깃 벡터 만들기


```python
y_train_5 = (y_train == 5)
y_test_5 = (y_test == 5)
```

max_iter와 tol 같은 일부 매개변수는 사이킷런 다음 버전에서 기본값이 바뀝니다. 버전이 업데이트 되더라도 결과가 바뀌지 않도록 아예 나중에 바뀔 기본값을 사용해 명시적으로 지정합니다.

## 확률적 경사 하강법
Stochastic Gradient Descent (SGD)
- 매우 큰 데이터셋을 효율적으로 처리하는 방법
- 한 번에 하나씩 훈련 샘플을 독립적으로 처리함.


```python
from sklearn.linear_model import SGDClassifier

sgd_clf = SGDClassifier(max_iter=1000, tol=1e-3, random_state=42)
sgd_clf.fit(X_train, y_train_5)
```




    SGDClassifier(alpha=0.0001, average=False, class_weight=None,
                  early_stopping=False, epsilon=0.1, eta0=0.0, fit_intercept=True,
                  l1_ratio=0.15, learning_rate='optimal', loss='hinge',
                  max_iter=1000, n_iter_no_change=5, n_jobs=None, penalty='l2',
                  power_t=0.5, random_state=42, shuffle=True, tol=0.001,
                  validation_fraction=0.1, verbose=0, warm_start=False)



숫자 5 이미지 감지


```python
sgd_clf.predict([some_digit])
```




    array([ True])



분류기가 해당하는 이미지가 5를 나타낸다고 추측함.(True)


```python
from sklearn.model_selection import cross_val_score
cross_val_score(sgd_clf, X_train, y_train_5, cv=3, scoring="accuracy")
```




    array([0.95035, 0.96035, 0.9604 ])



# 성능 측정

## 교차 검증을 사용한 정확도 측정

- 교차 검증 구현

다음 코드는 사이킷런의 cross_val_score()함수와 거의 같은 작업을 수행하는 코드입니다.


```python
from sklearn.model_selection import StratifiedKFold
from sklearn.base import clone

# shuffle=False가 기본값이기 때문에 random_state를 삭제하던지 shuffle=True로 지정하라는 경고가 발생합니다.
# 0.24버전부터는 에러가 발생하니 shuffle=True을 지정
skfolds = StratifiedKFold(n_splits=3, random_state=42, shuffle=True)

for train_index, test_index in skfolds.split(X_train, y_train_5):
    clone_clf = clone(sgd_clf)
    X_train_folds = X_train[train_index]
    y_train_folds = y_train_5[train_index]
    X_test_folds = X_train[train_index]
    y_test_folds = y_train_5[train_index]

    clone_clf.fit(X_train_folds, y_train_folds)
    y_pred = clone_clf.predict(X_test_folds)
    n_correct = sum(y_pred == y_test_folds)
    print(n_correct / len(y_pred))
```

    0.9705
    0.91645
    0.9715
    

StractifiedFold
- 클래스별 비율이 유지되도록 폴드를 만들기 위해 계층적 샘플링 수행
- 매 반복에서 분류기 객체를 복제하여 훈련 폴드로 훈련시키고 테스트 폴드로 예측을 만듦
- 올바른 예측의 수를 세어 정확한 예측의 비율 출력


```python
from sklearn.base import BaseEstimator
class Never5Classifier(BaseEstimator):
    def fit(self, X, y=None):
        pass
    def predict(self, X):
        return np.zeros((len(X), 1), dtype=bool)
```


```python
never_5_clf = Never5Classifier()
cross_val_score(never_5_clf, X_train, y_train_5, cv=3, scoring="accuracy")
```




    array([0.91125, 0.90855, 0.90915])



- 정확도가 90% 이상으로 나옴
- 이미지의 10% 정도만 숫자 5이기 때문에 5가 아닌 경우로 예측하면 맞출 확률이 90%

따라서 이 예제는 정확도를 분류기의 성능 측정 지표로 사용하지 않는 이유를 보여줌. 특히 불균형한 데이터셋(어떤 클래스가 다른 것보다 많은 경우)을 다룰 때 더욱 그러함.

- 알고리즘이 조금씩 변경되어 결과값 변동이 있을 수 있음.
- 무작위성에 의존함.
- 연산이 실행되는 순서가 보장되지 않음.
- 딕셔너리나 셋은 완벽한 재현이 불가능.

## 오차 행렬
- 클래스 A의 샘플이 클래스 B로 분류된 횟수를 세는 것

ex) 분류기가 숫자 5의 이미지를 3으로 잘못 분류한 횟수를 알고 싶다면 오차 행렬의 5행 3열을 참고

오차 행렬을 만들려면 실제 타겟과 비교할 수 있도록 먼저 예측값을 만들어야 함
테스트 세트로 예측을 만들 수 있지만 테스트 세트는 분류기가 출시 준비를 마치고 나서 프로젝트의 맨 마지막에 사용
대신 cross_val_predict()함수 사용


```python
from sklearn.model_selection import cross_val_predict

y_train_pred = cross_val_predict(sgd_clf, X_train, y_train_5, cv=3)
```

cross_val_predict()함수는 k-겹 교차 검증을 수행하지만 평가 점수를 반환하지 않고 각 테스트 폴드에서 얻은 예측을 반환합니다. 모델이 훈련하는 동안 보지 못 했던 데이터에 대해 예측합니다.

confusion_matrix()함수를 사용해 오차 행렬 만들기
- 타깃 글래스(y_train_5)와 예측 클래스(y_train_pred)를 넣고 호출하기.


```python
from sklearn.metrics import confusion_matrix

confusion_matrix(y_train_5, y_train_pred)
```




    array([[53892,   687],
           [ 1891,  3530]])



- 행은 실제 클래스
- 열은 예측한 클래스

5가 아닌 이미지를 아닌 것으로 정확하게 분류한 것을 진짜 음성이라고 함.
5라고 잘 못 분류한 것을 거짓 양성이라고 함.
5 이미지를 5가 아닌 것으로 잘 못 분류한 것을 거짓음성이라고 함.
정확히 5라고 분류한 것을 진짜 양성이라고 함.



```python
y_train_perfect_predictions = y_train_5 # 완벽한 분류기일 경우
confusion_matrix(y_train_5, y_train_perfect_predictions)
```




    array([[54579,     0],
           [    0,  5421]])



- 분류기의 정밀도

더 요약된 지표가 필요한 경우 양성 예측의 정확도를 활용

확실한 양성 샘플 하나만 예측하면 간단히 완벽한 정밀도를 얻을 수 있지만, 이는 분류기가 다른 모든 양성 샘플을 무시하기 때문에 그리 유용하지 않음.
정밀도는 재현율이라는 또 다른 지표와 같이 사용하는 것이 일반적.
재현율은 분류기가 정확하게 감지한 양성 샘플의 비율로 민감도 또는 진짜 양성 비율이라고도 함.

## 정밀도와 재현율

사이킷런은 정밀도와 재현율을 포함하여 분류기의 지표를 계산하는 여러 함수 제공


```python
from sklearn.metrics import precision_score, recall_score

precision_score(y_train_5, y_train_pred)
```




    0.8370879772350012




```python
cm = confusion_matrix(y_train_5, y_train_pred)
cm[1, 1] / (cm[0, 1] + cm[1, 1])
```




    0.8370879772350012




```python
recall_score(y_train_5, y_train_pred)
```




    0.6511713705958311




```python
cm[1, 1] / (cm[1, 0] + cm[1, 1])
```




    0.6511713705958311



F1 점수 = 정밀도와 재현율의 조화 평균


```python
from sklearn.metrics import f1_score

f1_score(y_train_5, y_train_pred)
```




    0.7325171197343846




```python
cm[1, 1] / (cm[1, 1] + (cm[1, 0] + cm[0, 1]) / 2)
```




    0.7325171197343847



## 정밀도/재현율 트레이드오프

임곗값을 내리면 재현율이 높아지고 정밀도가 줄어듦

predict() 메소드 대신 dccision_function()메소드를 호출하면 각 샘플의 점수를 얻을 수 있음 이 점수를 기반으로 원하는 임곗값을 정해 예측을 만들 수 있음


```python
y_scores = sgd_clf.decision_function([some_digit])
y_scores
```




    array([2164.22030239])




```python
threshold = 0
y_some_digit_pred = (y_scores > threshold)
```


```python
y_some_digit_pred
```




    array([ True])




```python
threshold = 8000
y_some_digit_pred = (y_scores > threshold)
y_some_digit_pred
```




    array([False])



이미지가 실제로 숫자 5이고 임곗값이 0일 때는 분류기가 이를 감지했지만, 임곗값을 8천으로 높이면 놓치게 됨

cross_val_predict()함수를 사용해 훈련 세트에 있는 모든 샘플의 점수를 구해야 함.
결정 점수를 반환 받도록 지정


```python
y_scores = cross_val_predict(sgd_clf, X_train, y_train_5, cv=3,
                             method="decision_function")
```

precision_recall_curve()함수를 사용하여 가능한 모든 임곗값에 대해 정밀도와 재현율 계산 가능


```python
from sklearn.metrics import precision_recall_curve

precisions, recalls, thresholds = precision_recall_curve(y_train_5, y_scores)
```

matplotlib을 이용해 임곗값의 함수로 정밀도와 재현율 그리기


```python
def plot_precision_recall_vs_threshold(precisions, recalls, thresholds):
    plt.plot(thresholds, precisions[:-1], "b--", label="Precision", linewidth=2)
    plt.plot(thresholds, recalls[:-1], "g--", label="Recall", linewidth=2)
    plt.legend(loc="center right", fontsize=16) # Not shown in the book
    plt.xlabel("Threshold", fontsize=16)        # Not shown
    plt.grid(True)                              # Not shown
    plt.axis([-50000, 50000, 0, 1])             # Not shown

recall_90_precision = recalls[np.argmax(precisions >= 0.90)]
threshold_90_precision = thresholds[np.argmax(precisions >= 0.90)]

plt.figure(figsize=(8, 4))
plot_precision_recall_vs_threshold(precisions, recalls, thresholds)
plt.plot([-50000, threshold_90_precision], [0.9, 0.9], "r:")
plt.plot([-50000, threshold_90_precision], [recall_90_precision, recall_90_precision], "r:")
plt.plot([threshold_90_precision], [0.9], "ro")
plt.plot([threshold_90_precision], [recall_90_precision], "ro")
save_fig("precision_recall_vs_threshold_plot")
plt.show()
```

    그림 저장: precision_recall_vs_threshold_plot




![output_67_1](https://user-images.githubusercontent.com/59479116/121621174-9a79fd80-caa6-11eb-8b7d-3a3c8c3f3517.png)

    



```python
(y_train_pred == (y_scores > 0)).all()
```




    True




```python
def plot_precision_vs_recall(precisions, recalls):
    plt.plot(recalls, precisions, "b-", linewidth=2)
    plt.xlabel("Recall", fontsize=16)
    plt.ylabel("Precision", fontsize=16)
    plt.axis([0, 1, 0, 1])
    plt.grid(True)

plt.figure(figsize=(8, 6))
plot_precision_vs_recall(precisions, recalls)
plt.plot([recall_90_precision, recall_90_precision], [0., 0.9], "r:")
plt.plot([0.0, recall_90_precision], [0.9, 0.9], "r:")
plt.plot([recall_90_precision], [0.9], "ro")
save_fig("precision_vs_recall_plot")
plt.show()
```

    그림 저장: precision_vs_recall_plot




![output_69_1](https://user-images.githubusercontent.com/59479116/121621185-a36acf00-caa6-11eb-88ed-2ca7840dbf66.png)
    



```python
threshold_90_precision = thresholds[np.argmax(precisions >= 0.90)]
threshold_90_precision
```




    3370.0194991439557




```python
y_train_pred_90 = (y_scores >= threshold_90_precision)
```


```python
precision_score(y_train_5, y_train_pred_90)
```




    0.9000345901072293




```python
recall_score(y_train_5, y_train_pred_90)
```




    0.4799852425751706



충분히 큰 임곗값을 지정해 정밀도 90%를 달성한 분류기를 만들었으나 재현율이 너무 낮다면 높은 정밀도의 분류기는 의미없음

## ROC 곡선

거짓 양성 비율(FPR)에 대한 진짜 양성 비율(TPR)의 곡선

ROC곡선을 그리려면 먼저 roc_curve()함수를 사용해 여러 임곗값에서 TPR과 FPR을 계산 해야 함


```python
from sklearn.metrics import roc_curve

fpr, tpr, thresholds = roc_curve(y_train_5, y_scores)
```


```python
def plot_roc_curve(fpr, tpr, label=None):
    plt.plot(fpr, tpr, linewidth=2, label=label)
    plt.plot([0,1], [0,1], 'k--') # 대각 점선
    plt.axis([0,1,0,1])
    plt.xlabel('False Positive Rate (Fall-Out)', fontsize=16) # 축 이름, 그리드 추가
    plt.ylabel('True Positive Rate (Recall)', fontsize=16)
    plt.grid(True)

plt.figure(figsize=(8,6))
plot_roc_curve(fpr, tpr)
fpr_90 = fpr[np.argmax(tpr >= recall_90_precision)]
plt.plot([fpr_90, fpr_90], [0., recall_90_precision], "r:")
plt.plot([0.0, fpr_90], [recall_90_precision, recall_90_precision], "r:")
plt.plot([fpr_90], [recall_90_precision], "ro")
save_fig("roc_curve_plot")
plt.show()
```

    그림 저장: roc_curve_plot




![output_77_1](https://user-images.githubusercontent.com/59479116/121621206-abc30a00-caa6-11eb-92a2-28a9c0f18a95.png)
    


붉은 점이 선택한 비율의 지점(43.68% 재현율)

재현율이 높을수록 분류기가 만드는 거짓 양성이 늘어남. 점선은 완전한 랜덤 분류기의 ROC 곡선을 뜻함. 좋은 분류기는 이 점선에서 최대한 멀리 떨어져 있어야 함.

양성 클래스가 드물거나 거짓 음성보다 거짓 양성이 더 중요할 때 PR 곡선 사용 그렇지 않으면 ROC 곡선을 사용


```python
from sklearn.metrics import roc_auc_score

roc_auc_score(y_train_5, y_scores)
```




    0.9604938554008616



predict_proba()메소드는 샘플이 행, 클래스가 열이고 샘플이 주어진 클래스에 속할 확률을 담은 배열을 반환


```python
from sklearn.ensemble import RandomForestClassifier
forest_clf = RandomForestClassifier(n_estimators=100, random_state=42)
y_probas_forest = cross_val_predict(forest_clf, X_train, y_train_5, cv=3,
                                    method="predict_proba")
```

roc_curve()함수는 레이블과 점수를 기대함. 점수 대신에 클래스 확률을 전달할 수 있음.


```python
y_scores_forest = y_probas_forest[:, 1] # 점수 = 양성 클래스의 확률
fpr_forest, tpr_forest, theresholds_forest = roc_curve(y_train_5, y_scores_forest)
```


```python
recall_for_forest = tpr_forest[np.argmax(fpr_forest >= fpr_90)]

plt.figure(figsize=(8,6))
plt.plot(fpr, tpr, "b:", linewidth=2, label="SGD")
plot_roc_curve(fpr_forest, tpr_forest, "Random Forest")
plt.plot([fpr_90, fpr_90], [0., recall_90_precision], "r:")
plt.plot([0.0, fpr_90], [recall_90_precision, recall_90_precision], "r:")
plt.plot([fpr_90], [recall_90_precision], "ro")
plt.plot([fpr_90, fpr_90], [0., recall_for_forest], "r:")
plt.plot([fpr_90], [recall_for_forest], "ro")
plt.grid(True)
plt.legend(loc="lower right", fontsize=16)
save_fig("roc_curve_comparison_plot")
plt.show()
```

    그림 저장: roc_curve_comparison_plot




![output_86_1](https://user-images.githubusercontent.com/59479116/121621217-b5e50880-caa6-11eb-8b4a-5829dc4f2f84.png)



ROC 곡선 비교 : 랜덤 포레스트 분류기가 SGD 분류기보다 훨씬 좋습니다. 랜덤 포레스트의 ROC 곡선이 왼쪽 위 모서리에 더 가까워 AUC값이 크기 때문


```python
roc_auc_score(y_train_5, y_scores_forest)
```




    0.9983436731328145




```python
y_train_pred_forest = cross_val_predict(forest_clf, X_train, y_train_5, cv=3)
precision_score(y_train_5, y_train_pred_forest)
```




    0.9905083315756169




```python
recall_score(y_train_5, y_train_pred_forest)
```




    0.8662608374838591



99.0% 정밀도와 86.6% 재현율

# 다중 분류

- Ova, Ovr 전략 : 이미지를 분류할 때 각 분류의 결정 점수 중에서 가장 높은 것을 클래스로 선택

- OvO 전략 : 1,2 1,3과 같이 각각의 숫자의 조합마다 이진 분류기를 훈련

다중 클래스 분류 작업에 이진 분류 알고리즘을 선택하면 사이킷런이 알고리즘에 따라 자동으로 OvR 또는 OvO를 실행합니다. sklearn.svm.SVC 클래스를 사용해서 서포트 벡터 머신 분류기 테스트


```python
from sklearn.svm import SVC

svm_clf = SVC(gamma="auto", random_state=42)
svm_clf.fit(X_train[:1000], y_train[:1000])
svm_clf.predict([some_digit])
```




    array([5], dtype=uint8)



5를 구별한 타깃 클래스(y_train_5) 대신 0~9까지의 원래 타깃 클래스(y_train)을 사용해 SVC 훈련. 그 다음 예측 하나를 만들고 내부에서는 사이킷런이 OvO 전략을 사용해 45개의 이진 분류기를 훈련시키고 각각의 결정 점수를 얻어 점수가 가장 높은 클래스를 선택

decision_function()메소드를 호출하면 샘플당 10개의 점수를 반환함. 이 점수는 클래스마다 하나씩임


```python
some_digit_scores = svm_clf.decision_function([some_digit])
some_digit_scores
```




    array([[ 2.81585438,  7.09167958,  3.82972099,  0.79365551,  5.8885703 ,
             9.29718395,  1.79862509,  8.10392157, -0.228207  ,  4.83753243]])



가장 높은 점수가 클래스 5에 해당하는 값


```python
np.argmax(some_digit_scores)
```




    5




```python
svm_clf.classes_
```




    array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], dtype=uint8)



분류기가 훈련될 때 classes_ 속성에 타깃 클래스의 리스트를 값으로 정렬하여 저장. 위 예제에서는 classes_ 배열에 있는 각 클래스의 인덱스가 클래스 값 자체와 같음(인덱스 5에 해당하는 클래스의 값은 5) 그러나 이런경우는 드뭄

사이킷런에서 OvO나 OvR을 사용하도록 강제하려면 OneVsOneClassifier나 OneVsRestClassifier를 사용. 이진 분류기 인스턴스를 만들어 객체를 생성할 때 전달하면 됨. 다음 코드는 SVC기반으로 OvR 전략을 사용하는 다중 분류기를 만듦


```python
from sklearn.multiclass import OneVsRestClassifier
ovr_clf = OneVsRestClassifier(SVC(gamma="auto", random_state=42))
ovr_clf.fit(X_train[:1000], y_train[:1000])
ovr_clf.predict([some_digit])
```




    array([5], dtype=uint8)



SGDClassifier(or RandomForestClassifier)를 훈련시키는 것도 간단함


```python
len(ovr_clf.estimators_)
```




    10




```python
sgd_clf.fit(X_train, y_train)
sgd_clf.predict([some_digit])
```




    array([3], dtype=uint8)



이 경우 SGD분류기는 직접 샘플을 다중 클래스로 분류할 수 있기 때문에 별도로 사이킷런의 OvR이나 OvO를 적용할 필요가 없음. decision_function()메소드는 클래스마다 하나의 값을 반환함.


```python
sgd_clf.decision_function([some_digit])
```




    array([[-31893.03095419, -34419.69069632,  -9530.63950739,
              1823.73154031, -22320.14822878,  -1385.80478895,
            -26188.91070951, -16147.51323997,  -4604.35491274,
            -12050.767298  ]])



분류 평가에는 일반적으로 교차 검증을 사용. cross_val_score() 함수를 사용해 SGDClassifier의 정확도를 평가


```python
cross_val_score(sgd_clf, X_train, y_train, cv=3, scoring="accuracy")
```




    array([0.87365, 0.85835, 0.8689 ])



모든 테스트 폴드에서 84% 이상을 얻었음. 랜덤 분류기 사용시 10% 정확도를 얻었을 것이므로 성능을 더 높일 여지가 있음. 입력의 스케일을 조정


```python
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train.astype(np.float64))
cross_val_score(sgd_clf, X_train_scaled, y_train, cv=3, scoring="accuracy")
```




    array([0.8983, 0.891 , 0.9018])



# 에러 분석

오차 행렬 살펴보기. cross_val_predict() 함수를 사용해 예측을 만들고 confusion_matrix()함수 호출


```python
y_train_pred = cross_val_predict(sgd_clf, X_train_scaled, y_train, cv=3)
conf_mx = confusion_matrix(y_train, y_train_pred)
conf_mx
```




    array([[5577,    0,   22,    5,    8,   43,   36,    6,  225,    1],
           [   0, 6400,   37,   24,    4,   44,    4,    7,  212,   10],
           [  27,   27, 5220,   92,   73,   27,   67,   36,  378,   11],
           [  22,   17,  117, 5227,    2,  203,   27,   40,  403,   73],
           [  12,   14,   41,    9, 5182,   12,   34,   27,  347,  164],
           [  27,   15,   30,  168,   53, 4444,   75,   14,  535,   60],
           [  30,   15,   42,    3,   44,   97, 5552,    3,  131,    1],
           [  21,   10,   51,   30,   49,   12,    3, 5684,  195,  210],
           [  17,   63,   48,   86,    3,  126,   25,   10, 5429,   44],
           [  25,   18,   30,   64,  118,   36,    1,  179,  371, 5107]])



matplotlib의 matshow()함수를 사용해 이미지로 표현하면 보기 편리할 수 있음

사이킷런 0.22버전부터는 sklearn.metrics.plot_confusion_matrix()함수를 사용할 수 있습니다.


```python
def plot_confusion_matrix(matrix):

    fig = plt.figure(figsize=(8,8))
    ax = fig.add_subplot(111)
    cax = ax.matshow(matrix)
    fig.colorbar(cax)
```


```python
plt.matshow(conf_mx, cmap=plt.cm.gray)
save_fig("confusion_matrix_plot", tight_layout=False)
plt.show()
```

    그림 저장: confusion_matrix_plot




![output_118_1](https://user-images.githubusercontent.com/59479116/121621233-c2696100-caa6-11eb-9607-0f84ead326a5.png)
    


데이터셋에 숫자5의 이미지가 적거나 분류기가 숫자 5를 다른 숫자만큼 잘 분류하지 못 함.
두 경우의 대하여 확인

그래프의 에러 부분에 초점을 맞춰 오차 행렬의 각 값을 대응되는 클래스의 이미지 개수로 나누어 에러 비율 비교


```python
row_sums = conf_mx.sum(axis=1, keepdims=True)
norm_conf_mx = conf_mx / row_sums
```

다른 항목은 유지하고 주 대각선만 0으로 채워서 그래프 그리기


```python
np.fill_diagonal(norm_conf_mx, 0)
plt.matshow(norm_conf_mx, cmap=plt.cm.gray)
save_fig("confusion_matrix_errors_plot", tight_layout=False)
plt.show()
```

    그림 저장: confusion_matrix_errors_plot




![output_123_1](https://user-images.githubusercontent.com/59479116/121621251-ca290580-caa6-11eb-9a44-67eb8eff15ec.png)
    


행은 실제 클래스를 나타내고 열은 예측한 클래스를 나타냄. 클래스 8의 열이 상당히 밝으므로 많은 이미지가 8로 잘못 분류 되었음을 암시 하지만 클래스 8의 행은 그리 나쁘지 않은 걸로 보아 실제 8이 적절하게 8로 분류되었음 3과 5가 서로 많이 혼동되고 있음


```python
cl_a, cl_b = 3, 5
X_aa = X_train[(y_train == cl_a) & (y_train_pred == cl_a)]
X_ab = X_train[(y_train == cl_a) & (y_train_pred == cl_b)]
X_ba = X_train[(y_train == cl_b) & (y_train_pred == cl_a)]
X_bb = X_train[(y_train == cl_b) & (y_train_pred == cl_b)]

plt.figure(figsize=(8,8))
plt.subplot(221); plot_digits(X_aa[:25], images_per_row=5)
plt.subplot(222); plot_digits(X_ab[:25], images_per_row=5)
plt.subplot(223); plot_digits(X_ba[:25], images_per_row=5)
plt.subplot(224); plot_digits(X_bb[:25], images_per_row=5)
save_fig("error_analysis_digits_plot")
plt.show()
```

    그림 저장: error_analysis_digits_plot




![output_125_1](https://user-images.githubusercontent.com/59479116/121621279-d57c3100-caa6-11eb-9251-2484cfa893dd.png)
    


원인은 선형 모델인 SGDClassifier를 사용했기 때문이며 선형 분류기는 클래스마다 픽셀에 가중치를 할당하고 새로운 이미지에 대해 단순히 픽셀 강도의 가중치 합을 클래스의 점수로 계산함. 따라서 3과 5는 몇개의 픽셀만 다르기 때문에 모델이 쉽게 혼동함.

분류기는 이미지의 위치나 회전 방향에 매우 민감함.
이 경우에 대해서 에러를 줄이는 방법 중에 하나는 이미지를 중앙에 위치시키고 회전되어 있지 않도록 전처리 하는 것

# 다중 레이블 분류

여러 개의 이진 꼬리표를 출력하는 분류 시스템


```python
from sklearn.neighbors import KNeighborsClassifier

y_train_large = (y_train >= 7)
y_train_odd = (y_train % 2 == 1)
y_multilabel = np.c_[y_train_large, y_train_odd]

knn_clf = KNeighborsClassifier()
knn_clf.fit(X_train, y_multilabel)
```




    KNeighborsClassifier(algorithm='auto', leaf_size=30, metric='minkowski',
                         metric_params=None, n_jobs=None, n_neighbors=5, p=2,
                         weights='uniform')



각 숫자 이미지에 두 개의 타깃 레이블이 담긴 y_multilabel 배열을 만듦. 첫 번째는 숫자가 큰 값(7,8,9)인지 나타내고 두 번째는 홀수인지 나타냄. KNeighborsClassifier 인스턴스를 만들고 다중 타깃 배열을 사용하여 훈련. 이제 예측을 만들면 레이블이 두 개 출력


```python
knn_clf.predict([some_digit])
```




    array([[False,  True]])



숫자 5는 크지 않고 홀수.

모든 레이블에 대한 F1점수의 평균 계산


```python
y_train_knn_pred = cross_val_predict(knn_clf, X_train, y_multilabel, cv=3)
f1_score(y_multilabel, y_train_knn_pred, average="macro")
```




    0.976410265560605



레이블에 클래스의 지지도를 가중치로 주는 것. 이전 코드에서 average="weighted"로 설정

# 다중 출력 분류

다중 레이블 분류에서 한 레이블이 다중 클래스가 될 수 있도록 일반화 한 것(값을 두 개 이상 가질 수 있음)

분류기의 출력이 다중 레이블(픽셀당 한 레이블)이고 각 레이블은 값을 여러개 가짐(0~255)

분류와 회귀 사이의 경계는 때때로 모호함 픽셀 강도 예측은 분류보다 회귀와 비슷함 다중 출력 시스템이 분류 작업에 국한되지도 않음 그래서 샘플마다 클래스와 값을 모두 포함하는 다중 레이블이 출력되는 시스템도 가능

MNIST 이미지에서 추출한 훈련 세트와 테스트 세트에 numpy의 randint() 함수를 사용하여 픽셀 강도에 잡음을 추가함


```python
noise = np.random.randint(0, 100, (len(X_train), 784))
X_train_mod = X_train + noise
noise = np.random.randint(0, 100, (len(X_test), 784))
X_test_mod = X_test + noise
y_train_mod = X_train
y_test_mod = X_test
```


```python
some_index = 0
plt.subplot(121); plot_digit(X_test_mod[some_index])
plt.subplot(122); plot_digit(X_test_mod[some_index])
save_fig("noisy_digit_example_plot")
plt.show()
```

    그림 저장: noisy_digit_example_plot




![output_141_1](https://user-images.githubusercontent.com/59479116/121621295-df9e2f80-caa6-11eb-8c02-bb6fd1adda82.png)
    


테스트 세트에서 이미지를 하나 선택하고(테스트 데이터를 들여다 보는 것이 아님) 분류기를 훈련시켜 이 이미지를 깨끗하게 처리함


```python
knn_clf.fit(X_train_mod, y_train_mod)
clean_digit = knn_clf.predict([X_test_mod[some_index]])
plot_digit(clean_digit)
save_fig("cleaned_digit_example_plot")
```

    그림 저장: cleaned_digit_example_plot




![output_143_1](https://user-images.githubusercontent.com/59479116/121621316-e88f0100-caa6-11eb-8631-5cc95b9e3735.png)
    


분류 작업에서 좋은 측정 지표를 선택, 적절한 정밀도/재현율 트레이드오프를 고르고, 분류기를 비교해야 함.
