---
title: "Day2 : OpenCV"
tags:
    - Study
    - Language
date: "2025-06-24"
thumbnail: "/assets/img/thumbnail/Linux_logo.png"
bookmark: true
---

### - 핵심/중요 표시
📌 (핀)
🔑 (열쇠)
⭐ (별)
🚨 (비상)
💡 (아이디어)
### - 상태/진행
✅ (체크)
❌ (엑스)
🚧 (공사 중)
🚀 (성공/시작)
🔄 (반복/리로드)
### - 감정/피드백
😊 (웃는 얼굴)
👍 (좋아요)
👎 (싫어요)
🙏 (부탁/감사)
🎉 (축하)
### - 알림/문의
📢 (알림)
❓ (물음표)
❔ (투명 물음표)
❗ (느낌표)
❕ (투명 느낌표)
### - 기타
🔍 (돋보기)
📝 (메모)
📅 (달력)
📂 (폴더)
📎 (클립)

# 📌 OpenCV란?
---
OpenCV(Open Source Computer Vision Library)는 **실시간 컴퓨터 비전** 및 **머신러닝**을 위한 오픈소스 라이브러리입니다.  
다양한 이미지/비디오 처리 기능을 제공하며, Python, C++, Java 등 다양한 언어에서 사용 가능합니다.

# 🚀 CUDA 모듈의 역할
---
- GPU 가속을 활용한 **고속 이미지 처리** 수행
- OpenCV의 일부 함수들은 CUDA를 통해 **병렬 처리**되어 성능을 향상시킴
- 사용 예: `cv2.cuda.GpuMat`, `cv2.cuda.filter2D()`, `cv2.cuda.resize()` 등

# 🛠️ 작업할 디렉토리 생성 및 환경 설정
---

```bash
# 1. 작업 디렉토리 생성
mkdir opencv                 # 디렉토리 이름: opencv
cd opencv                   # 해당 디렉토리로 이동

# 2. 가상 환경 생성 및 활성화
python3 -m venv .env        # 가상 환경 생성 (폴더 이름: .env)
source .env/bin/activate    # 가상 환경 활성화

# 3. 패키지 설치
pip install opencv-python          # OpenCV 기본 기능(core, imgproc 등)
pip install opencv-contrib-python # 추가 모듈(contrib 포함)
pip install -U pip                 # pip 최신 버전으로 업그레이드
```

# ✅ 설치 확인 (Python 인터프리터 실행)
---

```py
>>> import numpy as np
>>> import cv2

>>> np.__version__
'2.2.6'          # 설치된 NumPy 버전 출력

>>> cv2.__version__
'4.11.0'         # 설치된 OpenCV 버전 출력

>>> exit()       # Python 인터프리터 종료
```

# 🎨 색상 정보
---

### 🔗 참고 사이트
- [W3Schools - RGB Colors](https://www.w3schools.com/colors/colors_rgb.asp)

---

### 🌈 RGB (Red, Green, Blue)
- 각 색상 채널: **0~255 (8bit)**
  - R (Red): 8bit
  - G (Green): 8bit
  - B (Blue): 8bit
- 픽셀 1개 = **24bit (8bit × 3)**

---

### 🎨 HSL (Hue, Saturation, Lightness)
- **H**: 색상 (Hue) → 0 ~ 360°
- **S**: 채도 (Saturation) → 0 ~ 100%
- **L**: 밝기 (Lightness) → 0 ~ 100%

---

### 🔄 RGB vs HSL 차이점

| 항목 | RGB | HSL |
| :--: | :--: | :--: |
| 구성 | Red, Green, Blue (각 0~255) | Hue (0~360), Saturation & Lightness (0~100%) |
| 직관성 | 컴퓨터에서 사용하기 적합 | 사람이 색을 이해하기 쉬움 |
| 색 조절 | 색상 조정이 복잡함 | 채도/밝기 조절이 용이함 |
| 용도 | 디스플레이, 이미지 처리 등 | 디자인, 색상 선택 도구 등에 유용 |

---

### ✅ **요약**:  
- RGB는 화면 출력/처리에 적합한 **디지털 색 표현 방식**  
- HSL은 색상 구성요소를 분리해 **사람이 이해하거나 조절하기 쉬운 방식**

---

### 📝 메모
- vi ex1.py : python 스크립트 생성
- python ex1.py : 생성된 스크립트 실행
- jpg : 파일이 작고 속도가 빠르며, 주로 사진이나 웹 배경 이미지에 사용
- png : 화질 보존, 투명 배경이 필요한 경우 사용

# 👨‍💻 실습
---

### 💡 이미지 Read / Write / Display

```py
# ex1.py
import numpy as np
import cv2

# 이미지 파일을 Read
img = cv2.imread("Rengoku.jpg")

# Image 란 이름의 Display 창 생성
cv2.namedWindow("image", cv2.WINDOW_NORMAL)

# Numpy ndarray H/W/C order
print(img.shape)

# Read 한 이미지 파일을 Display
cv2.imshow("image", img)

# 별도 키 입력이 있을때 까지 대기
cv2.waitKey(0)

# ex1_output.jpg 로 읽은 이미지 파일을 저장
cv2.imwrite("ex1_output.jpg", img)

# Destory all windows
cv2.destroyAllWindows()
```

![alt text](../../../assets/img/Linux/ex1_output.jpg "ex1_output")

### ❓ Quiz: 이미지 Read / Write / Display

```
1. print(img.shape)의 출력 결과는 무슨 의미일까?

2. 본인이 좋아하는 사진을 web 에서 다운받아서 OpenCV API를 사용해서 Display 및 파일로 저장해보자.

3. 현재는 별도의 키 입력이 있을 때까지 cv2.waitKey(0) 함수에서 대기하게 된다. 코드를 추가해서 소문자 “s” 키를 입력받을 때만 이미지 파일을 저장하고 다른 키가 입력되면 이미지 파일을 저장하지 않게 수정해보자.
```

---

### 💡 RGB/HSV Color Space (색 공간)

```py
# ex2.py
import numpy as np
import cv2

# 이미지 파일을 Read 하고 Color space 정보 출력
color = cv2.imread("Rengoku.jpg", cv2.IMREAD_COLOR)
print(color.shape)

height,width,channels = color.shape
cv2.imshow("Original Image", color)

# Color channel 을 B,G,R 로 분할하여 출력
b,g,r = cv2.split(color)
rgb_split = np.concatenate((b,g,r),axis=1)
cv2.imshow("BGR Channels",rgb_split)

# 색공간을 BGR 에서 HSV 로 변환
hsv = cv2.cvtColor(color, cv2.COLOR_BGR2HSV)

# Channel 을 H,S,V 로 분할하여 출력
h,s,v = cv2.split(hsv)
hsv_split = np.concatenate((h,s,v),axis=1)
cv2.imshow("Split HSV", hsv_split)
```

### ❓ Quiz : RGB/HSV Color Space (색 공간)

```
1. 위 색공간 이미지의 링크로 이동해서 각 색 공간의 표현 방법을 이해해 보자.

2. HSV color space가 어떤 경우에 효과적으로 사용될까?

3. HSV로 변환된 이미지를 BGR이 아닌 RGB로 다시 변환해서 출력해 보자.

4. COLOR_RGB2GRAY를 사용해서 흑백으로 변환해 출력해 보자.
```

---

### 💡 Crop / Resize (자르기 / 크기 조정)

```py
# ex3.py
import numpy as np
import cv2

# 이미지 파일을 Read
img = cv2.imread("Rengoku.jpg")

# Crop 300x400 from original image from (100, 50)=(x, y)
# 세로(y): 100:500 →  500 - 100 = 400픽셀
# 가로(x): 500:1200 → 1200 - 500 = 700픽셀
cropped = img[100:500, 500:1200]

# Resize cropped image from 300x400 to 400x200
resized = cv2.resize(cropped, (800,200))

# Display all
cv2.imshow("Original", img)
cv2.imshow("Cropped image", cropped)
cv2.imshow("Resized image", resized)
cv2.imwrite("ex3_cropped.jpg", cropped)
cv2.imwrite("ex3_resized.jpg", resized)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

![alt text](../../../assets/img/Linux/ex3_cropped.jpg "ex3_cropped")
![alt text](../../../assets/img/Linux/ex3_resized.jpg "ex3_resized")

### ❓ Quiz : Crop / Resize (자르기 / 크기 조정)

```
1. Input image 를 본인이 좋아하는 인물 사진으로 변경해서 적용하자. 그리고 본인이 사용한 input image 의 size 를 확인해 보자.

2. 본인이 사용한 이미지의 얼굴 영역만 crop 해서 display 해 보자.

3. 원본 이미지의 정확히 1.5배만큼 이미지를 확대해서 파일로 저장해 보자.

4. openCV 의 rotate API 를 사용해서 우측으로 90도만큼 회전된 이미지를 출력해 보자.
```

---

### 💡 역상 (Reverse Image)

```py
# ex4.py
import numpy as np
import cv2

src = cv2.imread("Rengoku.jpg", cv2.IMREAD_COLOR)
dst = cv2.bitwise_not(src)

cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.imwrite("ex4_reverse.jpg", dst)

cv2.waitKey()
cv2.destroyAllWindows()
```

![alt text](../../../assets/img/Linux/ex4_reverse.jpg "ex4_reverse")

### ❓ Quiz : 역상 (Reverse Image)

```
1. AND, OR, XOR 연산에 대해서 확인해 보자.
```

---

### 💡 이진화 (Binary)

```py
# ex5.py
import numpy as np
import cv2

src = cv2.imread("Rengoku.jpg", cv2.IMREAD_COLOR)
gray = cv2.cvtColor(src, cv2.COLOR_BGR2GRAY)

ret, dst = cv2.threshold(gray, 150, 255, cv2.THRESH_BINARY)

cv2.imshow("dst", dst)
cv2.imwrite("ex5_binary.jpg", dst)

cv2.waitKey()
cv2.destroyAllWindows()
```

![alt text](../../../assets/img/Linux/ex5_binary.jpg "ex5_binary")

### ❓ Quiz : 이진화 (Binary)

```
1. 임계값을 변화시켜 보자.
```

---

### 💡 흐림효과 (Blur)

```py
# ex6.py
import numpy as np
import cv2

src = cv2.imread("Rengoku.jpg", cv2.IMREAD_COLOR)
dst = cv2.blur(src, (9, 9), anchor=(-1,- 1), borderType=cv2.BORDER_DEFAULT)

cv2.imshow("dst", dst)
cv2.imwrite("ex6_blur.jpg", dst)

cv2.waitKey()
cv2.destroyAllWindows()
```

![alt text](../../../assets/img/Linux/ex6_blur.jpg "ex6_blur")

### ❓ Quiz : 흐림효과 (Blur)

```
1. Kernel Size를 변경하여 보자.

2. borderType을 변경하여 보자.(cv2.BORDER_REFLECT)
```

---

### 💡 가장자리 검출 (Edge)

```py
# ex7.py
import numpy as np
import cv2

src = cv2.imread("Rengoku.jpg", cv2.IMREAD_COLOR)
gray = cv2.cvtColor(src, cv2.COLOR_BGR2GRAY)

sobel = cv2.Sobel(gray, cv2.CV_8U, 1, 0, 3)

cv2.imshow("sobel", sobel)
cv2.imwrite("ex7_edge.jpg", sobel)

cv2.waitKey()
cv2.destroyAllWindows()
```

![alt text](../../../assets/img/Linux/ex7_edge.jpg "ex7_edge")

### ❓ Quiz : 가장자리 검출 (Edge)

```
1. Laplacian 변환을 적용해 보자.

2. Canny Edge Detection을 적용해 보자.
```

---

### 💡 배열 병합 (add Weighted)

```py
# ex8.py
import numpy as np
import cv2

src = cv2.imread("RGB.png", cv2.IMREAD_COLOR)
hsv = cv2.cvtColor(src, cv2.COLOR_BGR2HSV)

# 1. Red 마스크 생성
lower_red = cv2.inRange(hsv, (0, 100, 100), (5, 255, 255))
upper_red = cv2.inRange(hsv, (170, 100, 100), (180, 255, 255))
mask_red = cv2.addWeighted(lower_red, 1.0, upper_red, 1.0, 0.0)

# 2. Green 마스크 생성
mask_green = cv2.inRange(hsv, (40, 100, 100), (85, 255, 255))

# 3. Blue 마스크 생성
mask_blue = cv2.inRange(hsv, (100, 100, 100), (130, 255, 255))

# 4. 각 색상 추출 (HSV → BGR 변환 포함)
red = cv2.bitwise_and(hsv, hsv, mask=mask_red)
green = cv2.bitwise_and(hsv, hsv, mask=mask_green)
blue = cv2.bitwise_and(hsv, hsv, mask=mask_blue)

red = cv2.cvtColor(red, cv2.COLOR_HSV2BGR)
green = cv2.cvtColor(green, cv2.COLOR_HSV2BGR)
blue = cv2.cvtColor(blue, cv2.COLOR_HSV2BGR)

# 5. 화면 출력
cv2.imshow("Original", src)
cv2.imshow("Red", red)
cv2.imshow("Green", green)
cv2.imshow("Blue", blue)

cv2.imwrite("ex8_original.png", src)
cv2.imwrite("ex8_red.png", red)
cv2.imwrite("ex8_green.png", green)
cv2.imwrite("ex8_blue.png", blue)


cv2.waitKey()
cv2.destroyAllWindows()
```

![alt text](../../../assets/img/Linux/ex8_original.png "ex8_original")
![alt text](../../../assets/img/Linux/ex8_red.png "ex8_red")
![alt text](../../../assets/img/Linux/ex8_green.png "ex8_green")
![alt text](../../../assets/img/Linux/ex9_blue_gray.png "ex8_blue")

### - sudo apt install v4l-utils : 카메라 지원 해상도 확인용 도구 설치
- v4l2-ctl -d /dev/video0 --list-formats-ext : 해당 카메라의 해상도 및 포맷 목록 출력
