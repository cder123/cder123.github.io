---
title: Python-OpenCV
tag: Python3
categories:
  - 后端
  - Python3
---

# Python-OpenCV

[toc]



 

## 0、资料

-   [openCV-唐宇迪](https://www.bilibili.com/video/BV1tb4y1C7j7?p=1)：https://www.bilibili.com/video/BV1tb4y1C7j7?p=1



## 1、OpenCV 环境的搭建



>   环境：

-   Python 3.6
-   （Anaconda）Jupyter NoteBook
-   opencv-python（`pip install opencv-python==3.4.1.15`）【最新版有部分算法不开源】
    -   进入python的交互模式，检查是否安装opencv的库：
        -   `import cv2`
        -   `cv2.__version__`
        -   `exit()`
-   opencv-contrib–python（`pip install opencv-contrib-python==3.4.1.15`）【版本需一致】





>   启动 Jypyter NotaBook

-   前提：安装完 Anaconda

-   启动软件：cmd中输入 `jupyter notebook`

-   打开界面：浏览器输入 `http://localhost:8888/`

    

![image-20210906093522233](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20210906093522233.png)









>   新建 1个Python文件，运行（ <kbd>shift + Enter</kbd> ）

![image-20210906093821634](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20210906093821634.png)







![image-20210906093935198](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20210906093935198.png)







>   上传文件

![image-20210906115334774](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20210906115334774.png)



![image-20210906115411850](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20210906115411850.png)





## 2、计算机眼中的图像



计算机中的**图像**是由**像素**构成的。

-   像素点：取值（表示亮度）为（0~255 ，其中 0 为黑色，255 为白色 ）。

-   RGB：图像的颜色通道（3 通道）。

-   黑白图：只有1个通道。





### 2.1 读取图像



>   图像色彩：

-   `cv2.IMREAD_COLOR`：彩色图像
-   `cv2.IMREAD_GRAYSCALE`：灰色图像



>   图像显示：

-   `%matplotlib inline`：在 notebook 中直接展示图片，无需调用 ` plt.show()`



>   图像 读、写

-   读取：`cv2.imread("图片名")`
-   写入：`cv2.imwrite("图片名",已读取的图片)`



>   图片的常用属性

-   底层格式：`type(img1)`
-   像素点的个数：`img.size`
-   数据类型：`img.dtype`





>   模板：

```python
import cv2 # 默认读取的是BGR（而不是 RGB）
import matplotlib.pyplot as plt
import numpy as np

%matplotlib inline

img1 = cv2.imread("cat.jpg")

# 得到图像的矩阵（三维数组）
## 三维数组一般用：[h,w,c] 来表示
print(img1)


cv2.imshow("窗口标题",img1)

# （等待的毫秒数），0表示按任意键停止等待
cv2.waitKey(0)

# 关闭窗口
cv2.destroyAllWindows()
```





>   读取图片-案例：

```python
import cv2
import matplotlib.pyplot as plt
import numpy as np
%matplotlib inline

img1 = cv2.imread("1.jpg")

cv2.imshow("pic-1",img1)
cv2.waitKey(0)
cv2.destroyAllWindows()
```





>   将读取图片的代码封装为函数

```python
import cv2
import matplotlib.pyplot as plt
import numpy as np
%matplotlib inline

# 定义函数
def myCV_show(win_name,imgName):
    img = cv2.imread(imgName)
    cv2.imshow(win_name,img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
    
# 调用
myCV_show("pic-2","1.jpg")
```





>   读取灰度图

```python
import cv2
import matplotlib.pyplot as plt
import numpy as np
%matplotlib inline

# 定义函数
def myCV_show(win_name,imgName):
    # 指定参数：灰度图（只有1个通道，打印形状也只有[h,w]）
    img = cv2.imread(imgName,cv2.IMREAD_GRAYSCALE)    
    # 打印形状
    print(img.shape)
    cv2.imshow(win_name,img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
```





>   写入灰度图

```python
import cv2
import matplotlib.pyplot  as plt
import numpy as np
%matplotlib inline

img = cv2.imread("1.jpg",cv2.IMREAD_GRAYSCALE)
cv2.imshow("pic-4",img)
cv2.waitKey(0)
cv2.destroyAllWindows()
cv2.imwrite("2.jpg",img)
```







### 2.2 读取视频



-   `cv2.videoCapture("视频名")`



>   模板

```python
import cv2
import matplotlib.pyplot  as plt
import numpy as np
%matplotlib inline



vc = cv2.VideoCapture("1.mp4")

# open：是否 打开成功
# frame: 当前这一帧图像
if vc.isOpened:
    open,frame = vc.read()
else:
    open = False
 
#打开成功，则不断遍历每一帧,处理每一帧
while open:
    isopen,frame = vc.read()
    
    if frame is None:
        break;
    if isopen == True:
        gray = cv2.cvtColor(frame,cv2.COLOR_BGR2GRAY)
        cv2.imshow("res-Pic",gray)	# 展示灰度视频
        cv2.imshow("res-Pic",frame) # 展示正常有颜色的视频
        # 10：代表 每一帧处理完后等待10秒
        # 27：代表 键盘上的 esc 键，退出
        if cv2.waitKey(10) & 0xFF==27:
            break;

vc.release()
cv2.destroyAllWindows()
```





### 2.3 ROI 截取图像



```python
import cv2
import matplotlib.pyplot  as plt
import numpy as np
%matplotlib inline


img = cv2.imread("1.jpg")

res_img = img[100:200,100:200]

cv2.imshow("pic-1",res_img)

cv2.waitKey(0)

cv2.destroyAllWindows()

```





### 2.4 提取、合并颜色通道 BGR

```python
import cv2
import matplotlib.pyplot  as plt
import numpy as np
%matplotlib inline

img1 = cv2.imread("1.jpg")

# 分离通道
b,g,r = cv2.split(img1)

# 合并通道
img2 = cv2.merge((b,g,r))
```







>   只保留一个通道

```python
import cv2
import matplotlib.pyplot  as plt
import numpy as np
%matplotlib inline


img  = cv2.imread("1.jpg")

# BGR 三个通道中，只保留 B通道（下标0~2）
img_1 = img.copy()
img_1[:,:,1] = 0
img_1[:,:,2] = 0
cv2.imshow("pic-B",img_1)
cv2.waitKey(0)
cv2.destroyAllWindows()


# BGR 三个通道中，只保留 G通道（下标 0 ~ 2 代表3个通道）
img_2 = img.copy()
img_2[:,:,0] = 0
img_2[:,:,2] = 0
cv2.imshow("pic-G",img_2)
cv2.waitKey(0)
cv2.destroyAllWindows()


# BGR 三个通道中，只保留 R通道（下标0~2）
img_3 = img.copy()
img_3[:,:,0] = 0
img_3[:,:,1] = 0
cv2.imshow("pic-R",img_3)
cv2.waitKey(0)
cv2.destroyAllWindows()

```







### 2.5 边界填充



-   `cv2.copyMakeBorder(图片,上填充,下填充,左填充,右填充,borderType=cv2.BORDER_REPLICATE)`
    -   `borderType`属性的取值：
        -   `cv2.BORDER_REPLICATE `：replicate，复制边缘像素
        -   `cv2.BORDER_REPFLECT`：reflect，反射（可能有重复）
        -   `cv2.BORDER_REPFLECT_101`：reflect，反射（不重复）
        -   `cv2.BORDER_WRAP`：wrap，包裹边缘
        -   `BORDER_CONSTANT`：constant，配合value属性使用

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

img = cv2.imread("1.jpg")

# 边界的填充大小都设为50px
top_px,bottom_px,left_px,right_px =(50,50,50,50)

# 复制
replicate = cv2.copyMakeBorder(img,top_px,bottom_px,left_px,right_px,borderType=cv2.BORDER_REPLICATE)

# 反射
reflect = cv2.copyMakeBorder(img,top_px,bottom_px,left_px,right_px,borderType=cv2.BORDER_REFLECT)

# 反射
reflect_101 = cv2.copyMakeBorder(img,top_px,bottom_px,left_px,right_px,borderType=cv2.BORDER_REFLECT_101)

# 包裹
warp = cv2.copyMakeBorder(img,top_px,bottom_px,left_px,right_px,borderType=cv2.BORDER_WRAP)

# 常量
constant = 
cv2.copyMakeBorder(img,top_px,bottom_px,left_px,right_px,borderType=cv2.BORDER_CONSTANT,value=0)

plt.subplot(231),plt.imshow(img,"gray"),plt.title("orignal")
plt.subplot(232),plt.imshow(replicate,"gray"),plt.title("replicate")
plt.subplot(233),plt.imshow(reflect,"gray"),plt.title("reflect")
plt.subplot(234),plt.imshow(reflect_101,"gray"),plt.title("reflect_101")
plt.subplot(235),plt.imshow(warp,"gray"),plt.title("warp")
plt.subplot(236),plt.imshow(constant,"gray"),plt.title("constant")

```







### 2.6 图像融合

将2张图片合为一张图片



```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

img1 = cv2.imread("1.jpg")
img2 = cv2.imread("2.jpg")

# shape 的大小(1200,1000,3)
print(img1.shape)
# shape 的大小(1000,1000,3)
print(img2.shape)

# 将所需融合的2张图片的大小更改为一致的大小【按固定大小更改】（高h，宽w）
img3 = cv2.resize(img1,(1000,1000))
# 将所需融合的2张图片的大小更改为一致的大小【按高、宽的倍数更改】（高h，宽w）
# img4 = cv2.resize(img1,(0，0)，fx=3,fy=1)

# 融合图片（参数说明：0.4为img1的权重，0.6为img2的权重，0为提高亮度的偏移）
res_img = cv2.addWeighted(img1,0.4,img2,0.6,0)

# 展示
plt.imshow(res_img )

```





### 2.7 图像阈值



`res,dst = cv2.threshold( 原单通道灰度图，阈值127，最大阈值255，类型)`



参数-“类型”的取值（用于与==参数-最大阈值==结合起来使用）：

-   `cv2.THRESH_BINARY`：超过部分取才最大阈值，否则取0【亮的像素全白，暗的像素全黑】
-   `cv2.THRESH_BINARY_INV`：反转【亮的像素全黑，暗的像素全白】
-   `cv2.THRESH_TRUNC`：截断，亮度大于阈值的部分才设置，亮度小于的部分不变
-   `cv2.THRESH_TOZERO`：大于阈值的部分不变，小于阈值的部分取0（小于阈值的部分，其像素为黑）
-   `cv2.THRESH_TOZERO_INV`：反转（大于阈值的部分，其像素为黑）



```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

img2 = cv2.imread("2.jpg",cv2.IMREAD_GRAYSCALE)

threshold,img = cv2.threshold(img2,127,255,cv2.THRESH_BINARY)

print(threshold)

plt.imshow(img,"gray")
```







## 3、图像平滑处理

图像平滑处理 也就是对图像进行滤波操作。





