---
title: "Day3: Perceptron_MLP"
date: "2025-06-25"
thumbnail: "/assets/img/thumbnail/perceptron.jpg"
---

# 

---

# 📌 Perceptron이란?
---
- 1957년 프랭크 로젠브라트가 고안한 최초의 인공신경망 모델 중 하나. 
- 생물학적 뉴런을 수학적으로 모델링한 **인공 뉴런**으로, 여러 입력 신호를 받아 각각의 가중치를 곱한 후 이를 합산하여, 활성화 함수를 통해 단일 신호를 출력한다.
- 퍼셉트론의 출력은 신호 유무 (1 또는 0)로 표현된고, 이진 분류 문제 해결에 효과적이다.

# 📌 Perceptron AND 실습
---
```
import numpy as np
import matplotlib.pyplot as plt

class Perceptron:
    def __init__(self, input_size, lr=0.1, epochs=10):
        self.weights = np.zeros(input_size)
        self.bias = 0
        self.lr = lr
        self.epochs = epochs
        self.errors = []

    def activation(self, x):
        return np.where(x >= 0, 1, 0)

    def predict(self, x):
        linear_output = np.dot(x, self.weights) + self.bias
        return self.activation(linear_output)

    def train(self, X, y):
        for epoch in range(self.epochs):
            total_error = 0
            for x1, target in zip(X, y):
                prediction = self.predict(x1)
                update = self.lr * (target - prediction)
                self.weights += update * x1
                self.bias += update
                total_error += int(update != 0)
            self.errors.append(total_error)
            print(f"Epoch {epoch+1}/{self.epochs}, Errors: {total_error}")

# AND 게이트 데이터
X_and = np.array([[0,0],[0,1],[1,0],[1,1]])
y_and = np.array([0,0,0,1])

# 퍼셉트론 모델 훈련
ppn_and = Perceptron(input_size=2)
ppn_and.train(X_and, y_and)

# 예측 결과 확인
print("\nAND Gate Test:")
for x in X_and:
    print(f"Input: {x}, Predicted Output: {ppn_and.predict(x)}")
```
