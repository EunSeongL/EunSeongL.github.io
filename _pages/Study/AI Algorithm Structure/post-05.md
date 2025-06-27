---
title: "Day5: Linear Model"
date: "2025-06-27"
thumbnail: "/assets/img/AI/linear_model.jpg"
---

# Mnist 손글씨, Cifar_10 Dataset

---

# 데이터셋 및 코드 개요

## ❓ Mnist 손글씨 데이터셋이란?
 - ## 손으로 쓴 숫자(0~9)를 인식하는 머신러닝 및 딥러닝 모델의 성능 평가를 위해 널리 사용되는 대표적인 이미지 데이터셋이다.

 - ## 총 70,000개의 흑백(그레이스케일) 이미지로 이루어져 있으며, 60,000개는 학습용, 10,000개는 테스트용이다. 각 이미지는 28x28 픽셀 크기의 정사각형 형태이다.

 <img src="/assets/img/AI/mnist_data.png" style="width:50%; height:50%; object-fit:contain;">

- ### 데이터: (60000, 28, 28) 크기의 흑백 이미지, 라벨(0~9)
- ### 전처리: 이미지를 1차원(784)으로 변환 후 0~1 정규화

- ### 모델:
    입력: Dense(3072)
    출력층: Dense(10, softmax) (은닉층 없음)
    손실함수: sparse_categorical_crossentropy
    최적화: SGD
    평가: 정확도(acc)

```py
# Mnist 데이터 전처리
train_x1 = train_x.reshape(60000, -1)   # 1차원 배열
test_x1 = test_x.reshape(10000, -1)     # 1차원 배열 

train_x2 = train_x1/255
test_x2 = test_x1/255

print(train_x2.shape)   # (60000, 784)
print(test_x2.shape)    # (10000, 784)

md = Sequential()
md.add(Dense(10, activation = 'softmax', input_shape = (28*28,)))
```

## 코드 실행 및 결과 시각화

 <img src="/assets/img/AI/mnist_acc.png" style="width:50%; height:50%; object-fit:contain;">

 <img src="/assets/img/AI/mnist_loss.png" style="width:50%; height:50%; object-fit:contain;">

---

## ❓ Cifar_10 데이터셋이란?
 - ## 컴퓨터 비전 분야에서 이미지 분류 모델의 성능을 평가하기 위해 널리 사용되는 컬러 이미지 데이터셋이다.

 - ## 총 60,000개의 컬러(RGB, 3채널) 이미지로 이루어져 있으며, 50,000개는 학습용, 10,000개는 테스트용이다. 각 이미지는 32x32 픽셀 크기이다.

<img src="/assets/img/AI/cifar10_data.png" style="width:50%; height:50%; object-fit:contain;">

- ### 데이터: (50000, 32, 32, 3) 크기의 컬러 이미지, 라벨(0~9)
- ### 전처리: 이미지를 1차원(3072)으로 변환 후 0~1 정규화

- ### 모델:
    입력: Dense(3072)
    출력층: Dense(10, softmax) (은닉층 없음)
    손실함수: sparse_categorical_crossentropy
    최적화: SGD
    평가: 정확도(acc)

```py
# Cifar_10 데이터 전처리
train_x1 = train_x.reshape(50000, -1)   # 1차원 배열
test_x1 = test_x.reshape(10000, -1)     # 1차원 배열 

train_x2 = train_x1/255
test_x2 = test_x1/255

print(train_x2.shape)   #(50000, 3072)
print(test_x2.shape)    #(10000, 3072)

md = Sequential()
md.add(Dense(10, activation = 'softmax', input_shape = (3072,)))
```

## 코드 실행 및 결과 시각화

 <img src="/assets/img/AI/cifar10_acc.png" style="width:50%; height:50%; object-fit:contain;">

 <img src="/assets/img/AI/cifar10_loss.png" style="width:50%; height:50%; object-fit:contain;">
 
 ## 두 코드의 주요 차이점

 | 항목         | MNIST 코드                      | CIFAR-10 코드                  |
|--------------|--------------------------------|-------------------------------|
| 데이터 타입  | 흑백(1채널), 28x28              | 컬러(3채널), 32x32            |
| 입력 차원    | 784                             | 3072                          |
| 모델 구조    | 은닉층(128) 포함                | 은닉층 없음                   |
| 난이도       | 상대적으로 쉬움                 | 상대적으로 어려움             |
| 분류 대상    | 손글씨 숫자(0~9)                | 10가지 사물                   |
| 성능 기대치  | 높은 정확도 달성 용이           | 단순 모델로는 낮은 정확도     |

