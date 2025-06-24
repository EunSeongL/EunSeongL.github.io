---
title: "Basic Operation"
date: "2025-06-24"
thumbnail: "/assets/img/AI/ai_thumbnail.png"
---

# Basic Operation

---

## 1. Basic Operation
**이미지 Read & Write**<br>

- 이미지 파일 준비
![image](/assets/img/AI/image.jpg "image")

```
import numpy as np
import cv2

img = cv2.imread("image.jpg")

cv2.namedWindow("image", cv2.WINDOW_NORMAL)

print(img.shape)

cv2.imshow("image", img)

cv2.waitKey(0)

cv2.imwrite("output.png", img)

cv2.destroyAllWindows()
```
- 출력 결과 <br>
![image1](/assets/img/AI/image1.png "image1")
![image2](/assets/img/AI/image2.png "image2")

## 2. Basic Operation
**색상 채널 분리와 색공간 변환**<br>
- 이미지 파일 준비
![image3](/assets/img/AI/image3.png "image3")

```
import numpy as np
import cv2

color = cv2.imread("strawberry.jpg", cv2.IMREAD_COLOR)

print(color.shape)

height, width, channels = color.shape
cv2.imshow("Original Image", color)

r,g,b = cv2.split(color)
rgb_split = np.concatenate((r,g,b), axis=1)
cv2.imshow("RGB Channels", rgb_split)

hsv = cv2.cvtColor(color, cv2.COLOR_RGB2HSV)

h,s,v = cv2.split(hsv)
hsv_split = np.concatenate((h,s,v),axis=1)
cv2.imshow("Split HSV", hsv_split)

cv2.waitKey(0)

cv2.imwrite("hsv2rgb_split.png", hsv_split)
```
- 출력 결과 <br>
![image4](/assets/img/AI/image4.png "image4")
![image5](/assets/img/AI/image5.png "image5")

## 3. Basic Operation
**이미지 일부 영역을 자르기, 크기를 바꾸기, 회전하기**<br>
- 이미지 파일 준비
![image](/assets/img/AI/image.jpg "image")

```
import numpy as np
import cv2

img = cv2.imread("image.jpg")
print(img.shape)

# 0,100  
cropped = img[0:220, 185:385]       #220, 200, 3
print(cropped.shape)

resized = cv2.resize(cropped, (400,200))
print(resized.shape)

rotated_90 = cv2.rotate(resized, cv2.ROTATE_90_CLOCKWISE)
#rotated_180 = cv2.rotate(image, cv2.ROTATE_180)
#rotated_270 = cv2.rotate(image, cv2.ROTATE_90_COUNTERCLOCKWISE)

cv2.imshow("Origin", img)
cv2.imshow("Cropped image", cropped)
cv2.imshow("Resized image", resized)
cv2.imshow("Rotated 90 image", rotated_90)

cv2.waitKey(0)
cv2.destroyAllWindows()
```
- 출력 결과 <br>
| 이미지         | 설명         |
|:-------------:|:------------|
| ![cropped](/assets/img/AI/image6.png "image6") | cropped      |
| ![resized](/assets/img/AI/image7.png "image7") | resized      |
| ![rotated_90](/assets/img/AI/image8.png "image8") | rotated_90   |


