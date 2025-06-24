---
title: "Basic Operation"
date: "2025-06-24"
thumbnail: "/assets/img/AI/ai_thumbnail.png"
---

# Basic Operation

---

## 1. **이미지 Read & Write**<br>

- 이미지 파일 준비

<img src="/assets/img/AI/image.jpg" style="width:75% !important;">

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
<img src="/assets/img/AI/image1.png" style="width:50% !important;">
<img src="/assets/img/AI/image2.png" style="width:50% !important;">

## 2. **색상 채널 분리와 색공간 변환**<br>
- 이미지 파일 준비
<img src="/assets/img/AI/image3.png" style="width:50% !important;">

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
<img src="/assets/img/AI/image4.png" style="width:75% !important;">
<img src="/assets/img/AI/image5.png" style="width:75% !important;">

## 3. **이미지 일부 영역 자르기, 크기 바꾸기, 회전하기**<br>
- 이미지 파일 준비
<img src="/assets/img/AI/image.jpg" style="width:75% !important;">

```
import numpy as np
import cv2

img = cv2.imread("image.jpg")
print(img.shape) # 0,100  
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

<img src="/assets/img/AI/image6.png" style="width:50% !important;">
<div style="text-align:center;">
  <strong>cropped</strong>
</div>

<img src="/assets/img/AI/image7.png" style="width:50% !important;">
<div style="text-align:center;">
  <strong>resized</strong>
</div>

<img src="/assets/img/AI/image8.png" style="width:50%; height:250px; !important;">
<div style="text-align:center;">
  <strong>rotated_90</strong>
</div>

## 4. **원본 색상 반전시키기**<br>
- 이미지 파일 준비
<img src="/assets/img/AI/image.jpg" style="width:75% !important;">

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
<img src="/assets/img/AI/image9.png" style="width:75% !important;">

## 5. **임계값 기준으로 이진화시키기**<br>
- 이미지 파일 준비
<img src="/assets/img/AI/image.jpg" style="width:75% !important;">

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
<img src="/assets/img/AI/image10.png" style="width:75% !important;">

## 6. **이미지 흐리게(블러) 처리**<br>
- 이미지 파일 준비
<img src="/assets/img/AI/image.jpg" style="width:75% !important;">

```
import cv2

src = cv2.imread("image.jpg", cv2.IMREAD_COLOR)
dst = cv2.blur(src, (9, 9), anchor=(-1, -1), borderType=cv2.BORDER_DEFAULT)

cv2.imshow("dst", dst)

cv2.waitKey()
cv2.destroyAllWindows()
```

- 출력 결과 <br>
<img src="/assets/img/AI/image11.png" style="width:75% !important;">

## 7. **세 가지 대표적인 엣지(경계) 검출 알고리즘**<br>
- 이미지 파일 준비
<img src="/assets/img/AI/image.jpg" style="width:75% !important;">

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

<img src="/assets/img/AI/image12.png" style="width:75% !important;">
<div style="text-align:center;">
  <strong>sobel</strong>
</div>

<img src="/assets/img/AI/image13.png" style="width:75% !important;">
<div style="text-align:center;">
  <strong>laplacian</strong>
</div>

<img src="/assets/img/AI/image14.png" style="width:75% !important;">
<div style="text-align:center;">
  <strong>canny</strong>
</div>

## 8. **컬러 이미지의 BGR(Blue, Green, Red) 채널을 분리 후 채널 순서를 바꿔서 이미지 합치기**<br>
- 이미지 파일 준비
<img src="/assets/img/AI/bgr.png" style="width:35% !important;">

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

**출력 결과** <br>

<img src="/assets/img/AI/imageb.png" style="width:35% !important;">
<div style="text-align:center;">
  <strong>B</strong>
</div>

<img src="/assets/img/AI/imageg.png" style="width:35% !important;">
<div style="text-align:center;">
  <strong>G</strong>
</div>

<img src="/assets/img/AI/imager.png" style="width:35% !important;">
<div style="text-align:center;">
  <strong>R</strong>
</div>

<img src="/assets/img/AI/inverse.png" style="width:35% !important;">
<div style="text-align:center;">
  <strong>inverse</strong>
</div>


## 9. **컬러 이미지의 BGR(Blue, Green, Red) 채널을 분리 후 Red 채널만 0(검정색)**<br>
- 이미지 파일 준비
<img src="/assets/img/AI/bgr.png" style="width:35% !important;">

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

**출력 결과** <br>

<img src="/assets/img/AI/imageb.png" style="width:35% !important;">
<div style="text-align:center;">
  <strong>B</strong>
</div>

<img src="/assets/img/AI/imageg.png" style="width:35% !important;">
<div style="text-align:center;">
  <strong>G</strong>
</div>

<img src="/assets/img/AI/imager.png" style="width:35% !important;">
<div style="text-align:center;">
  <strong>R</strong>
</div>

<img src="/assets/img/AI/bgz.png" style="width:35% !important;">
<div style="text-align:center;">
  <strong>bgz</strong>
</div>

## 10. **동영상에서 원하는 장면을 이미지로 캡처하기**<br>
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

**출력 결과**<br>

<img src="/assets/img/AI/image15.png" style="width:35% !important;">
<img src="/assets/img/AI/image16.png" style="width:50% !important;">

## 11. **다양한 OpenCV 그리기 함수 사용해보기**<br>

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

**출력 결과** <br>
<img src="/assets/img/AI/image18.png" style="width:75% !important;">