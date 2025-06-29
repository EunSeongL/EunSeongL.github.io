---
title: "Day5: Neural Network Model"
tags:
    - Activate function
    - Learning Analysis
    - Optimization Techniques
date: "2025-06-27"
thumbnail: "/assets/img/AI/neural.png"
---

# Activate function

---

# Learning Analysis

---

# Optimization Techniques

---

# LAB

---
## Mnist 실습

### 공통점
### 모두 MNIST 손글씨 숫자 이미지 데이터셋을 사용합니다.
### 데이터 전처리(reshape, 정규화), 모델 학습, 성능 시각화, 평가 등 전반적인 워크플로우가 유사합니다.
### Keras의 Sequential 모델을 사용하며, 손실 함수로 'sparse_categorical_crossentropy', 평가지표로 'acc'(정확도)를 사용합니다.

### 차이점

| 항목         | op_mnist.py                                   | mnist.py                           |
|--------------|-----------------------------------------------|------------------------------------|
| 모델 구조    | 4개 Dense 계층(512→256→128→10), Dropout 3회   | 1개 Dense 계층(10)                 |
| 활성화 함수  | relu(은닉층), softmax(출력층)                 | softmax(출력층)                    |
| Dropout 사용 | 있음(0.3 비율, 3회)                           | 없음                               |
| Optimizer    | Adam(learning_rate=0.0003)                    | SGD(기본값)                        |
| Epoch 수     | 35                                            | 30                                 |
| Batch Size   | 128                                           | 64                                 |
| 모델 복잡도  | 높음                                          | 매우 단순(로지스틱 회귀와 유사)    |

#### 1. 모델 구조 정의

| mnist.py 코드 | op_mnist.py 코드 |
|---------------|------------------|
| ```py
md = Sequential()
md.add(Dense(10, activation = 'softmax', input_shape = (28*28,)))``` 
| ```py
md = Sequential()
md.add(Dense(512, activation='relu', input_shape=(28*28,)))
md.add(Dropout(0.3))
md.add(Dense(256, activation='relu'))
md.add(Dropout(0.3))
md.add(Dense(128, activation='relu'))
md.add(Dropout(0.3))
md.add(Dense(10, activation='softmax'))
``` |

#### 2. Optimizer 및 하이퍼파라미터 설정
##### mnist.py 

```py
md.compile(loss='sparse_categorical_crossentropy', optimizer = 'sgd', metrics=['acc'])
hist = md.fit(train_x2, train_y, epochs = 30, batch_size = 64, validation_split = 0.2)
```

##### op_mnist.py

```py
md.compile(loss='sparse_categorical_crossentropy', optimizer=Adam(learning_rate=0.0003), metrics=['acc'])
hist = md.fit(train_x2, train_y, epochs = 35, batch_size = 128, validation_split = 0.2)
```
