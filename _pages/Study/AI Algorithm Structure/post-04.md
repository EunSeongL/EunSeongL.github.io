---
title: "Day4: Deep Learning"
date: "2025-06-26"
thumbnail: "/assets/img/thumbnail/deeplr.png"
---

# Compare Perceptron, Multi Layer perceptron XOR Gate

---

## 1. 네트워크 구조의 차이
### Perceptron
#### - Single layer : 입력층과 출력층만 존재하며, 입력값에 가중치와 바이어스를 더한 뒤 바로 출력
```
self.weights = np.zeros(input_size)
self.bias = 0
```
#### - 활성화 함수: 계단함수와 같은 단순한 함수를 사용한다.

### Multi Layer Perceptron
#### - Multi layer : 입력층, 은닉층, 출력층으로 구성된다.
```
self.w1 = np.random.randn(input_size, hidden_size)
self.b1 = np.zeros(hidden_size)
self.w2 = np.random.randn(hidden_size, output_size)
self.b2 = np.zeros(output_size)
```
#### - 활성화 함수: sigmoid, tanh 등 연속적이고 미분 가능한 함수 사용. 각 층마다 적용한다.

## 2. 순전파 연산의 차이
### Perceptron
#### - 입력층 -> 출력층
```
linear_output = np.dot(x, self.weights) + self.bias
return self.activation(linear_output)
```

### Multi Layer Perceptron
#### - 입력층 -> 은닉층 -> 출력층
```
self.z1 = np.dot(x, self.w1) + self.b1
self.a1 = sigmoid(self.z1)
self.z2 = np.dot(self.a1, self.w2) + self.b2
self.a2 = sigmoid(self.z2)
return self.a2
```

## 3. 가중치 업데이트 방식의 차이
### Perceptron
#### - 단순 업데이트 
```
update = self.lr * (target - prediction)
self.weights += update * x1
self.bias += update
```

### Multi Layer Perceptron
#### - 역전파(Backpropagation) 사용
#### 출력층 오차를 은닉층까지 전파하여 모든 가중치/바이어스를 미분값(gradient)으로 업데이트한다
```
# 출력층 오차
error_output = self.a2 - y
delta2 = error_output * sigmoid_diff(self.z2)
# 은닉층 오차
error_hidden = np.dot(self.w2, delta2)
delta1 = error_hidden * sigmoid_diff(self.z1)
# 가중치 업데이트
self.w2 -= self.lr * np.outer(self.a1, delta2)
self.b2 -= self.lr * delta2
self.w1 -= self.lr * np.outer(x, delta1)
self.b1 -= self.lr * delta1
```

## 4. 문제 해결 능력의 차이
### Perceptron
#### - 퍼셉트론: XOR처럼 선형적으로 구분 불가능한 문제는 절대 풀 수 없음. 단일 직선(혹은 평면)만으로 데이터를 나눈다.

### Multi Layer Perceptron
#### - MLP: 은닉층의 비선형 변환 덕분에 XOR 등 복잡한 패턴도 학습 가능. 실제로 MLP 코드는 XOR 문제를 성공적으로 해결할 수 있음


## 5. 코드 예시
### Perceptron
- #### 출력층 오차만 사용
```
class Perceptron:
    def __init__(self, input_size, ...):
        self.weights = np.zeros(input_size)
        self.bias = 0
    def predict(self, x):
        return self.activation(np.dot(x, self.weights) + self.bias)
    def train(self, X, y):
```
### Multi Layer Perceptron
- #### 여러 층 순전파
- #### 역전파로 모든 층 가중치 업데이트
```
class MLP:
    def __init__(self, input_size, hidden_size, output_size, ...):
        self.w1 = np.random.randn(input_size, hidden_size)
        self.b1 = np.zeros(hidden_size)
        self.w2 = np.random.randn(hidden_size, output_size)
        self.b2 = np.zeros(output_size)
    def forward(self, x):
    def backward(self, x, y):
```

## 6. 요약
| 구분           | 퍼셉트론 코드                        | MLP 코드                                         |
|----------------|--------------------------------------|--------------------------------------------------|
| **층 구조**        | 입력-출력(단일층)                       | 입력-은닉-출력(다층)                                 |
| **가중치/바이어스** | 1개(입력→출력)                         | 2세트(입력→은닉, 은닉→출력)                            |
| **순전파**         | 한 번의 선형 연산+활성화                   | 여러 층의 선형 연산+비선형 활성화 반복                     |
| **학습 방식**      | 출력층 오차로 단순 업데이트                  | 역전파로 모든 층의 가중치/바이어스 업데이트                 |
| **활성화 함수**    | 계단함수/시그모이드(단순)                   | 시그모이드/탄젠트 등(비선형, 미분 가능)                     |
| **문제 해결력**    | 선형 문제만 해결(XOR 불가)                  | 비선형 문제(XOR 등) 해결 가능                              |

