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

```py
# 기본 라이브러리 불러오기
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import tensorflow as tf

# 데이터 셋 불러오기
from tensorflow.keras.datasets.mnist import load_data

# 모델 설정용 라이브러리 불러오기
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Dropout
from tensorflow.keras.optimizers import Adam

(train_x, train_y), (test_x, test_y) = load_data()

# 데이터 크기 확인하기
train_x.shape, train_y.shape
test_x.shape, test_y.shape

# 이미지 확인하기
from PIL import Image

img = train_x[0]

img1 = Image.fromarray(img, mode = 'L')
plt.imshow(img1)

train_y[0]

# 데이터 전처리
train_x1 = train_x.reshape(60000, -1)   # 1차원 배열
test_x1 = test_x.reshape(10000, -1)     # 1차원 배열 

train_x2 = train_x1/255
test_x2 = test_x1/255

print(train_x2.shape)   # (60000, 784)
print(test_x2.shape)    # (10000, 784)


md = Sequential()

md.add(Dense(128, activation='relu', input_shape=(28*28,)))
md.add(Dropout(0.2))
md.add(Dense(64, activation='relu'))
md.add(Dropout(0.2))
md.add(Dense(10, activation='softmax'))

md.compile(loss='sparse_categorical_crossentropy', optimizer=Adam(learning_rate=0.0001), metrics=['acc'])
hist = md.fit(train_x2, train_y, epochs = 30, batch_size = 128, validation_split = 0.2)

acc = hist.history['acc']
val_acc = hist.history['val_acc']
epoch = np.arange(1, len(acc) +1)

plt.figure(figsize=(10,8))
plt.plot(epoch, acc, 'b', label='Training accuracy')
plt.plot(epoch, val_acc, 'r', label='Validation accuracy')
plt.title("Training and validation accuracy")
plt.xlabel('Epochs')
plt.ylabel('Accuracy')
plt.legend()
plt.show()

md.evaluate(test_x2, test_y)
weight = md.get_weights()

plt.plot(hist.history['loss'], label='loss')
plt.plot(hist.history['val_loss'], label='val_loss')
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.legend(['train', 'test'], loc='upper left')
plt.show()
```

```py
# 기본 라이브러리 불러오기
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import tensorflow as tf

# 데이터 셋 불러오기
from tensorflow.keras.datasets import cifar10

# 모델 설정용 라이브러리 불러오기
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Dropout
from tensorflow.keras.optimizers import Adam

(train_x, train_y), (test_x, test_y) = cifar10.load_data()

# 데이터 크기 확인하기
print(train_x.shape)  # (50000, 32, 32, 3)
print(train_y.shape)  # (50000, 1)
print(test_x.shape)   # (10000, 32, 32, 3)
print(test_y.shape)   # (10000, 1)

# 이미지 확인하기
from PIL import Image

img = train_x[0]

img1 = Image.fromarray(img, mode = 'RGB')
plt.imshow(img1)

train_y[0]

# 데이터 전처리
train_x1 = train_x.reshape(50000, -1)   # 1차원 배열
test_x1 = test_x.reshape(10000, -1)     # 1차원 배열 

train_x2 = train_x1/255
test_x2 = test_x1/255

print(train_x2.shape)   
print(test_x2.shape)    


md = Sequential()
md.add(Dense(512, activation='relu', input_shape=(3072,)))
md.add(Dropout(0.3))
md.add(Dense(256, activation='relu'))
md.add(Dropout(0.3))
md.add(Dense(128, activation='relu'))
md.add(Dropout(0.3))
md.add(Dense(10, activation='softmax'))

md.compile(loss='sparse_categorical_crossentropy', optimizer=Adam(learning_rate=0.0003), metrics=['acc'])
hist = md.fit(train_x2, train_y, epochs = 35, batch_size = 128, validation_split = 0.2)

acc = hist.history['acc']
val_acc = hist.history['val_acc']
epoch = np.arange(1, len(acc) +1)

plt.figure(figsize=(10,8))
plt.plot(epoch, acc, 'b', label='Training accuracy')
plt.plot(epoch, val_acc, 'r', label='Validation accuracy')
plt.title("Training and validation accuracy")
plt.xlabel('Epochs')
plt.ylabel('Accuracy')
plt.legend()
plt.show()

md.evaluate(test_x2, test_y)
weight = md.get_weights()
#print(weight)

plt.plot(hist.history['loss'], label='loss')
plt.plot(hist.history['val_loss'], label='val_loss')
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.legend(['train', 'test'], loc='upper left')
plt.show()
```