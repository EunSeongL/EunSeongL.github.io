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

<img src="/assets/img/AI/image.jpg " width="200" height="200"/>

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
**출력 결과** <br>

![cropped](/assets/img/AI/image6.png "image6")
- **cropped**

![resized](/assets/img/AI/image7.png "image7")
- **resized**

![rotated_90](/assets/img/AI/image8.png "image8")
- **rotated_90**

## 4. Basic Operation
**원본 색상 반전시키기**<br>
- 이미지 파일 준비
![image](/assets/img/AI/image.jpg "image")

```
import numpy as np
import cv2

src = cv2.imread("output.png", cv2.IMREAD_COLOR)
dst = cv2.bitwise_not(src)

cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```
- 출력 결과 <br>
![image9](/assets/img/AI/image9.png "image9")

## 5. Basic Operation
**임계값을 기준으로 이진화시키기**<br>
- 이미지 파일 준비
![image](/assets/img/AI/image.jpg "image")

```
import cv2

src = cv2.imread("image.jpg", cv2.IMREAD_COLOR)
gray = cv2.cvtColor(src, cv2.COLOR_BGR2GRAY)

ret, dst = cv2.threshold(gray, 100, 255, cv2.THRESH_BINARY)

cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```
- 출력 결과 <br>
![image10](/assets/img/AI/image10.png "image10")

## 6. Basic Operation
**이미지 흐리게(블러) 처리**<br>
- 이미지 파일 준비
![image](/assets/img/AI/image.jpg "image")

```
import cv2

src = cv2.imread("image.jpg", cv2.IMREAD_COLOR)
dst = cv2.blur(src, (9, 9), anchor=(-1, -1), borderType=cv2.BORDER_DEFAULT)

cv2.imshow("dst", dst)

cv2.waitKey()
cv2.destroyAllWindows()
```
- 출력 결과 <br>
![image11](/assets/img/AI/image11.png "image11")

## 7. Basic Operation
**세 가지 대표적인 엣지(경계) 검출 알고리즘**<br>
- 이미지 파일 준비
![image](/assets/img/AI/image.jpg "image")

```
import cv2

src = cv2.imread("image.jpg", cv2.IMREAD_COLOR)
gray = cv2.cvtColor(src, cv2.COLOR_BGR2GRAY)

sobel = cv2.Sobel(gray, cv2.CV_8U, 1, 0, 3)
cv2.imshow("sobel", sobel)

laplacian = cv2.Laplacian(gray, cv2.CV_8U, ksize=3)
cv2.imshow("laplacian", laplacian)

canny = cv2.Canny(gray, 100, 200)
cv2.imshow("canny", canny)

cv2.waitKey()
cv2.destroyAllWindows()
```
**출력 결과** <br>

![image12](/assets/img/AI/image12.png "image12")
- **sobel**

![image13](/assets/img/AI/image13.png "image13")
- **laplacian**

![image14](/assets/img/AI/image14.png "image14")
- **canny**

## 8. Basic Operation
**컬러 이미지의 BGR(Blue, Green, Red) 채널을 분리 후 채널 순서를 바꿔서 이미지 합치기**<br>
- 이미지 파일 준비
![rgb](/assets/img/AI/bgr.png "rgb")

```
import numpy as np
import cv2

src = cv2.imread("image.jpg", cv2.IMREAD_COLOR)
#b,g,r = cv2.split(src)
b = src[:, :, 0]
g = src[:, :, 1]
r = src[:, :, 2]

inverse = cv2.merge((r,g,b))

cv2.imshow("b", b)
cv2.imshow("g", g)
cv2.imshow("r", r)
cv2.imshow("inverse", inverse)
cv2.waitKey()
cv2.destroyAllWindows()
```
- 출력 결과 <br>

![imageb](/assets/img/AI/imageb.png "imageb")
- **b**

![imageg](/assets/img/AI/imageg.png "imageg")
- **g**

![imager](/assets/img/AI/imager.png "imager")
- **r**

![inverse](/assets/img/AI/inverse.png "inverse")
- **inverse**

## 9. Basic Operation
**컬러 이미지의 BGR(Blue, Green, Red) 채널을 분리 후 Red 채널만 0(검정색)**<br>
- 이미지 파일 준비
![rgb](/assets/img/AI/bgr.png "rgb")

```
import numpy as np
import cv2

src = cv2.imread("bgr.png", cv2.IMREAD_COLOR)

b = src[:, :, 0]
g = src[:, :, 1]
r = src[:, :, 2]

height, width, channel = src.shape
zero = np.zeros((height, width, 1), dtype=np.uint8)

bgz = cv2.merge((b, g, zero))

cv2.imshow("b", b)
cv2.imshow("g", g)
cv2.imshow("r", r)
cv2.imshow("bgz", bgz)
cv2.waitKey()
cv2.destroyAllWindows()

```
- 출력 결과 <br>

![imageb](/assets/img/AI/imageb.png "imageb")
- **b**

![imageg](/assets/img/AI/imageg.png "imageg")
- **g**

![imager](/assets/img/AI/imager.png "imager")
- **r**

![bgz](/assets/img/AI/bgz.png "bgz")
- **bgz**

## 10. Basic Operation
**동영상에서 원하는 장면을 이미지로 캡처하기**<br>
- 동영상 파일 준비
![son](/assets/img/AI/son.mp4 "son")

```
import numpy as np
import cv2
import os

save_dir = "SON"
os.makedirs(save_dir, exist_ok=True)

cap = cv2.VideoCapture("output.mp4")
img_idx = 1

while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        cap.set(cv2.CAP_PROP_POS_FRAMES, 0)
        continue

    height, width = frame.shape[:2]
    small_frame = cv2.resize(frame, (width // 2, height // 2), interpolation=cv2.INTER_AREA)

    cv2.imshow("Frame", small_frame)
    key = cv2.waitKey(80)

    if key & 0xFF == ord('q'):
        break

    if key & 0xFF == ord('c'):
        while True:
            filename = f"{img_idx:03d}.jpg"
            filepath = os.path.join(save_dir, filename)
            if not os.path.exists(filepath):
                cv2.imwrite(filepath, small_frame)
                print(f"Saved {filepath}")
                img_idx += 1
                break
            else:
                img_idx += 1

cap.release()
cv2.destroyAllWindows()
```
- 출력 결과 <br>
![image15](/assets/img/AI/image15.png "image15")
![image16](/assets/img/AI/image16.png "image16")

## 12. Basic Operation
**다양한 OpenCV 그리기 함수 사용해보기**<br>
```
import numpy as np
import cv2

cap = cv2.VideoCapture(5)

circle_centers = []

def draw_circle(event, x, y, flags, param):
    if event == cv2.EVENT_LBUTTONDOWN:
        circle_centers.append((x, y))

cv2.namedWindow("Camera")
cv2.setMouseCallback("Camera", draw_circle)

topLeft = (50, 50)
bottomRight = (300, 300)

while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        break

    cv2.line(frame, topLeft, bottomRight, (0, 255, 0), 3)

    cv2.rectangle(frame,
                  [pt+30 for pt in topLeft], [pt-30 for pt in bottomRight], (255, 0, 255), 3)

    font = cv2.FONT_ITALIC
    cv2.putText(frame, 'me',
                [pt+40 for pt in bottomRight], font, 2, (255, 0, 255), 5)

    for center in circle_centers:
        cv2.circle(frame, center, 30, (255, 255, 0), 3)

    cv2.imshow("Camera", frame)
    key = cv2.waitKey(1)
    if key & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```
- 출력 결과 <br>
![image18](/assets/img/AI/image18.png "image18")