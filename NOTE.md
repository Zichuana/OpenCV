### 图片输入输出

```python
import cv2
img1 = cv2.imread('C:/Users/zichuana/Desktop/1.jpg')  # 路径
cv2.imshow('t', img1)  # 图片输出
cv2.waitKey(1000)  # 0表示任意键终止，cv2.waitKey(10000)为毫秒级，10000为10秒
cv2.destroyAllWindows()  # 关闭窗口，（）里不指定任何参数，则删除所有窗口，删除特定的窗口，往（）输入特定的窗口值。

img2 = cv2.imread('C:/Users/zichuana/Desktop/1.jpg', cv2.IMREAD_GRAYSCALE)  # 灰度图
cv2.imshow('t2', img2)
cv2.waitKey(1000)
cv2.destroyAllWindows()

# 保存 cv2.imwrite('',img) 路径，图像
# type(img) 格式
# img.size 像素点个数
# img.dtype 类型

```

### 视频输入输出

```python
vc = cv2.VideoCapture('C:/Users/zichuana/Desktop/1.mp4')
# 检查是否打开正确
if vc.isOpened():
    open, frame = vc.read()  # vc.read() 取帧
else:
    open = False

while open:
    ret, frame = vc.read()
    if frame is None:
        break
    if ret is True:  # 读帧没有问题
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)  # 灰度
        cv2.imshow('re', gray)  # 展示
        if cv2.waitKey(100) & 0xFF == 27:  # 64位与27比较
            break
# vc.release() ？？？
cv2.destroyAllWindows()
```

### 读取部分图像数据

```python
img = cv2.imread('C:/Users/zichuana/Desktop/1.jpg')
jiequ = img[0:50, 0:200]
cv2.imshow('jiequ', jiequ)
cv2.waitKey(1000)
cv2.destroyAllWindows()
```

### 颜色通道提取

```python
b, g, r = cv2.split(img1)  # b.shape
img1 = cv2.merge((b, g, r))
# 只保留R
cur_img1 = img1.copy()  # ':'通配符
# cur_img1[:, :, :] = 255 白
# cur_img1[:, :, :] = 0 黑
cur_img1[:, :, 0] = 0
cur_img1[:, :, 1] = 0
cv2.imshow('R', cur_img1)
cv2.waitKey(1000)
cv2.destroyAllWindows()
# B-0 G-1 R-2
```

### 边界填充

```python
top_size, bottom_size, left_size, right_size = (50, 50, 50, 50)

replicate = cv2.copyMakeBorder(img1, top_size, bottom_size, left_size, right_size, borderType=cv2.BORDER_REPLICATE)
constant = cv2.copyMakeBorder(img1, top_size, bottom_size, left_size, right_size, borderType=cv2.BORDER_CONSTANT, value=0)
# cv2.imshow('t', replicate)
# cv2.waitKey(1000)
# cv2.destroyAllWindows()
plt.subplot(212), plt.imshow(replicate), plt.title('ORIGINAL')  # subplot()内参数表位置
plt.subplot(222), plt.imshow(constant), plt.title('ORIGINAL')
plt.show()
# borderType参数
# BORDER_REPLICATE 复制 复制最边缘像素
# BORDER_REFLECT 反射 hgfedcba|abcdefgh|hgfedcba
# BORDER_REFLECT_101 反射 hgfedcb|abcdefgh|gfedcba
# BORDER_WRAP 外包装 abcdefgh|abcdefgh|abcdefgh
# BORDER_CONSTANT 填充
```

### 图像数值计算

```python
img12 = img1 + 10
print(img1[:5, :, 0])
print(img12[:5, :, 0])
print((img1 + img12)[:5, :, 0])  # 相当于%256
print(cv2.add(img1, img12)[:5, :, 0])  # >=255 -> 255 ; <255 -> x
'''
cv2.imshow('test', img1+img12)
cv2.waitKey(1000)
cv2.destroyAllWindows()

cv2.imshow('test', cv2.add(img1, img12))
cv2.waitKey(1000)
cv2.destroyAllWindows()
'''
```

### 图像融合

```python
img3 = cv2.imread('C:/Users/zichuana/Desktop/2.jpg')
print(img1.shape)
print(img3.shape)
img3 = cv2.resize(img3, (835, 501))  # img3 = cv2.resize(img3, (0, 0), fx=?, fy=?) 比例放缩
print(img3.shape)
res = cv2.addWeighted(img1, 0.4, img3, 0.6, 0)
cv2.imshow('test', res)
cv2.waitKey(1000)
cv2.destroyAllWindows()
```

### 图像阈值

```python
'''
ret, dst = cv2.threshold(src, thresh, maxval, type)
src:输入图，只能输入单通道图像，通常来说是灰度图
dst:输出图
thresh:阈值
maxval:当像素值超过了阈值（或者小于阈值，根据type来决定），所赋予的值
type:二值化操作的类型，包括以下5种类型
1. cv2.THRESH_BINARY 超过阈值部分取最大值，否则取0
2. cv2.THRESH_BINARY_INV 1的反转
3. cv2.THRESH_TRUNC 大于阈值部分设为阈值，否则不变
4. cv2.THRESH_TOZERO 大于阈值部分不改变，否则设为0
5. cv2.THRESH_TOZERO_INV 4的反转
'''
ret, thresh1 = cv2.threshold(img2, 127, 255, cv2.THRESH_BINARY)
ret, thresh2 = cv2.threshold(img2, 127, 255, cv2.THRESH_BINARY_INV)
ret, thresh3 = cv2.threshold(img2, 127, 255, cv2.THRESH_TRUNC)
ret, thresh4 = cv2.threshold(img2, 127, 255, cv2.THRESH_TOZERO)
ret, thresh5 = cv2.threshold(img2, 127, 255, cv2.THRESH_TOZERO_INV)

titles = ['1', '2', '3', '4', '5']
images = [thresh1, thresh2, thresh3, thresh4, thresh5]

for i in range(5):
    plt.subplot(3, 2, i + 1), plt.imshow(images[i], 'gray')
    plt.title(titles[i])
    plt.xticks([]), plt.yticks([])  # 警用刻度以及标签
plt.show()
```

### 图像平滑处理

```python
import cv2
import matplotlib.pyplot as plt

# 图像平滑处理（通俗就是给图像去除’噪音‘）
img1 = cv2.imread('C:/Users/zichuana/Desktop/3.jpg')  # 原始图像输入输出
cv2.imshow('img1', img1)
cv2.waitKey(1000)
cv2.destroyAllWindows()
# 均值滤波，简单的平均卷积操作
bl1 = cv2.blur(img1, (3, 3))  # 矩阵参数最好为奇数
cv2.imshow('bl1', bl1)
cv2.waitKey(1000)
cv2.destroyAllWindows()
# 方框滤波
# 该情况下和均值滤波一样
box1 = cv2.boxFilter(img1, -1, (3, 3), normalize=True)  # 参数-1表示颜色通道一致，通常情况下是-1
cv2.imshow('box1', box1)
cv2.waitKey(1000)
cv2.destroyAllWindows()
# normalize=False情况
box2 = cv2.boxFilter(img1, -1, (3, 3), normalize=False)  # >=255 白色
cv2.imshow('box2', box2)
cv2.waitKey(1000)
cv2.destroyAllWindows()
# 高斯与中值滤波
# 高斯（根据权重矩阵，距离越近的作用越大, 同样也是去除噪音点）
Gauss = cv2.GaussianBlur(img1, (5, 5), 1)
cv2.imshow('Gauss', Gauss)
cv2.waitKey(1000)
cv2.destroyAllWindows()
# 中值滤波 （去排序取中位数为中心）除滤波操作的话 这个是最好的
median = cv2.medianBlur(img1, 5)
cv2.imshow("median", median)
cv2.waitKey(1000)
cv2.destroyAllWindows()
# 一次性拼接展示所有图像
res = np.hstack((bl1, box1, Gauss, median))
cv2.imshow("res", res)
cv2.waitKey(0)
cv2.destroyAllWindows()


```

### 型态学 腐蚀操作

```python
# 型态学 腐蚀操作 （假设是一个手臂的近照片，上面有毛毛，可以除去毛毛）
img2 = cv2.imread('C:/Users/zichuana/Desktop/4.jpg', cv2.IMREAD_GRAYSCALE)
cv2.imshow('img2', img2)
cv2.waitKey(1000)
cv2.destroyAllWindows()

kn1 = np.ones((5, 5), np.uint8)
es1 = cv2.erode(img2, kn1, iterations=1)  # iterations 迭代次数
cv2.imshow("es1", es1)
cv2.waitKey(1000)
cv2.destroyAllWindows()

# 膨胀操作，腐蚀操作逆运算，可以弥补腐蚀操作带来的损害
# 0 -> 255 腐蚀，255 -> 0 膨胀
kn1 = np.ones((5, 5), np.uint8)
dl1 = cv2.dilate(es1, kn1, iterations=1)  # iterations 迭代次数
cv2.imshow("es1", es1)
cv2.waitKey(1000)
cv2.destroyAllWindows()

kn1 = np.ones((5, 5), np.uint8)
es1 = cv2.erode(img2, kn1, iterations=3)  # iterations 迭代次数
cv2.imshow("es1", es1)
cv2.waitKey(1000)
cv2.destroyAllWindows()

kn1 = np.ones((5, 5), np.uint8)
dl1 = cv2.dilate(es1, kn1, iterations=3)  # iterations 迭代次数
cv2.imshow("es1", es1)
cv2.waitKey(1000)
cv2.destroyAllWindows()

ret, img3 = cv2.threshold(img2, 177, 255, cv2.THRESH_BINARY)
cv2.imshow('img3', img3)
cv2.waitKey(1000)
cv2.destroyAllWindows()

kn2 = np.ones((5, 5), np.uint8)
es2 = cv2.erode(img3, kn2, iterations=1)  # iterations 迭代次数
cv2.imshow("es2", es2)
cv2.waitKey(1000)
cv2.destroyAllWindows()

img4 = cv2.imread('C:/Users/zichuana/Desktop/4.jpg')
cv2.imshow("img4", img4)
cv2.waitKey(1000)
cv2.destroyAllWindows()

kn3 = np.ones((5, 5), np.uint8)
es3 = cv2.erode(img3, kn3, iterations=1)  # iterations 迭代次数 色彩腐蚀次数
# 类似与水的浸润 越迭代次数越多，越浸蚀
cv2.imshow("es2", es3)
cv2.waitKey(1000)
cv2.destroyAllWindows()  # 非灰度图 出现的图片接近0

```

### 开闭运算

```python
import cv2
import matplotlib.pyplot as plt
import numpy as np

img1 = cv2.imread("C:/Users/zichuana/Desktop/1.jpg", cv2.IMREAD_GRAYSCALE)
ret, thresh1 = cv2.threshold(img1, 127, 255, cv2.THRESH_BINARY_INV)
ret, thresh2 = cv2.threshold(img1, 127, 255, cv2.THRESH_BINARY)
cv2.imshow("thresh1", thresh1)
cv2.waitKey(1000)
cv2.destroyAllWindows()
cv2.imshow("thresh2", thresh2)
cv2.waitKey(1000)
cv2.destroyAllWindows()
# 开运算 先腐蚀再膨胀 255 -> 0 -> 255 (由上述假设 这里可以想象成毛毛没有，并且胳膊肘粗细不变)
mid1 = np.ones((5, 5), np.uint8)  # unit8 图片矩阵适使用的数据类型
opening = cv2.morphologyEx(thresh1, cv2.MORPH_OPEN, mid1)
cv2.imshow("opening", opening)
cv2.waitKey(1000)
cv2.destroyAllWindows()
# 闭运算 先膨胀再腐蚀
closing = cv2.morphologyEx(thresh1, cv2.MORPH_CLOSE, mid1)
cv2.imshow("closing", closing)
cv2.waitKey(1000)
cv2.destroyAllWindows()

opening2 = cv2.morphologyEx(thresh2, cv2.MORPH_OPEN, mid1)
cv2.imshow("opening", opening2)
cv2.waitKey(1000)
cv2.destroyAllWindows()
# 闭运算 先膨胀再腐蚀 0 -> 255 -> 0
closing2 = cv2.morphologyEx(thresh2, cv2.MORPH_CLOSE, mid1)
cv2.imshow("closing", closing2)
cv2.waitKey(1000)
cv2.destroyAllWindows()

res1 = np.hstack((thresh1, opening, closing))
cv2.imshow("res1", res1)
cv2.waitKey(1000)
cv2.destroyAllWindows()
res2 = np.hstack((thresh2, opening2, closing2))
cv2.imshow("res2", res2)
cv2.waitKey(1000)
cv2.destroyAllWindows()
```

### 膨胀腐蚀差运算（梯度）

```python
mid2 = np.ones((7, 7), np.uint8)
result1 = cv2.dilate(thresh2, mid2, iterations=3)
result2 = cv2.erode(thresh2, mid2, iterations=3)
res3 = np.hstack((result1, result2))
cv2.imshow("res3", res3)
cv2.waitKey(1000)
cv2.destroyAllWindows()

gra = cv2.morphologyEx(thresh2, cv2.MORPH_GRADIENT, mid2)
cv2.imshow("gra", gra)
cv2.waitKey(1000)
cv2.destroyAllWindows()
```

### 礼貌与黑帽

```python
# 礼貌 = 原始图像 - 开运算结果
# 黑帽 = 闭运算结果 - 原始输入
# 礼貌 = 毛毛(瑕疵)
# 黑帽 = 轮廓

# 礼貌
tophat = cv2.morphologyEx(thresh2, cv2.MORPH_TOPHAT, mid1)
rus3 = np.hstack((thresh2, opening2, tophat))
cv2.imshow("rus3", rus3)
cv2.waitKey(1000)
cv2.destroyAllWindows()

# 黑帽
blackhat = cv2.morphologyEx(thresh2, cv2.MORPH_BLACKHAT, mid1)
rus4 = np.hstack((thresh2, closing2, blackhat))
cv2.imshow("rus4", rus4)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

### 梯度计算

```python
import cv2
import matplotlib.pyplot as plt
import numpy as np

def cvshow1(img,name):
    cv2.imshow(name, img)
    cv2.waitKey(1000)
    cv2.destroyAllWindows()

def cvshow2(img,name):
    cv2.imshow(name, img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
# 梯度计算 （可以用于描述轮廓-灰度图效果好看）
# sobel 算子
img1 = cv2.imread("C:/Users/zichuana/Desktop/1.jpg")
cvshow1(img1, "img1")
sobel1 = cv2.Sobel(img1, cv2.CV_64F, 0, 1, ksize=3)
# cv2.Sobel(图像,图像深度（0-255，当计算结果小于0时需要使得等于0也就是截断成0）,x轴（含权重，矩阵的相乘得到）,y轴,算子的大小（矩阵的大小）)
cvshow1(sobel1, "sobel")
sobel1 = cv2.convertScaleAbs(sobel1)  # 使其不截断，进行取绝对值操作
cvshow1(sobel1, "sobel")
sobel2 = cv2.Sobel(img1, cv2.CV_64F, 1, 0, ksize=3)
# cv2.Sobel(图像,图像深度（0-255，当计算结果小于0时需要使得等于0也就是截断成0）,x轴（含权重，矩阵的相乘得到）,y轴,算子的大小（矩阵的大小）)
cvshow1(sobel2, "sobel2")
sobel2 = cv2.convertScaleAbs(sobel2)  # 使其不截断，进行取绝对值操作
cvshow1(sobel2, "sobel2")

sobel = cv2.addWeighted(sobel1, 0.5, sobel2, 0.5, 0)  # 权重 权重 偏置项
cvshow1(sobel, "sobel")
res = np.hstack((sobel1, sobel2, sobel))
cvshow1(res, "res")

sobel3 = cv2.Sobel(img1, cv2.CV_64F, 1, 1, ksize=3)
sobel3 = cv2.convertScaleAbs(sobel3)
cvshow1(sobel3, "sobel3")  # 不建议直接计算，先计算再求和

# Scharr算子
# 原理与sobel 算子差不多 但是数值大一些，对结果的差异更敏感
# laplacian算子
# 存在二阶导 对变化更加敏感 对噪音点也会很敏感 矩阵是四个边缘点与中间点进行比较 不适合单独使用

scharrx = cv2.Scharr(img1, cv2.CV_64F, 1, 0)
scharry = cv2.Scharr(img1, cv2.CV_64F, 0, 1)
scharrx = cv2.convertScaleAbs(scharrx)
scharry = cv2.convertScaleAbs(scharry)
scharr = cv2.addWeighted(scharrx, 0.5, scharry, 0.5, 0)

lap = cv2.Laplacian(img1, cv2.CV_64F)
lap = cv2.convertScaleAbs(lap)

res1 = np.hstack((sobel, scharr, lap))
cvshow2(res1, "res1")
```

### Canny边缘检测

```python
import cv2
import matplotlib.pyplot as plt
import numpy as np

# Canny边缘检测
# 高斯滤波器，以平滑图像，过滤噪音点
# 计算图像中每个像素点的梯度强度和方向
# 应用非极大值（non-maximum suppression）抑制，以消除边缘检测带来的杂散影响（体现大的梯度值，展现最明显的差异边界）*视频讲解P21 目前不是很懂
# 应用双阈值（Double-Threshold）检测来确定真实和潜在的边缘（1 梯度值>maxval:处理为边界 2 minval<梯度值<maxval:连有边界则保留，否则舍弃 3 梯度值<minval:则舍弃）
# 通过抑制孤立的弱边缘最终完成边缘检测
def cvshow1(img,name):
    cv2.imshow(name, img)
    cv2.waitKey(1000)
    cv2.destroyAllWindows()

def cvshow2(img,name):
    cv2.imshow(name, img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()

img1 = cv2.imread('C:/Users/zichuana/Desktop/1.jpg', cv2.IMREAD_GRAYSCALE)
cvshow1(img1, "img1")
v1 = cv2.Canny(img1, 50, 150)
v2 = cv2.Canny(img1, 200, 250)
res = np.hstack((v1, v2))
cvshow2(res, "res")

```

### 图像金字塔

```python
# 图像金字塔定义
# 金字塔尖到底 向上采样 放大 1 将Gi与高斯内核卷积 2 将所有偶数行和列去除
# 金字塔底到尖 向下采用 缩小
# 1 将图像再每个方向扩大为原来的两倍，新增的行和列以0填充
# 2 使用先前同样的内核（乘以4）与放大后的图像卷积，获得近似值

# 金字塔制作
up = cv2.pyrUp(img1)  # 向上
down = cv2.pyrDown(img1)  # 向下
print("img1:", img1.shape)
print("up:", up.shape)
print("down:", down.shape)
cvshow1(img1, "img1")
cvshow1(up, "up")
cvshow1(down, "down")
# 先向上再向下 图像损失
# 拉普拉斯金字塔 1低通滤波 2缩小尺寸 3放大尺寸 4图像相减
down_up = cv2.pyrUp(down)
print("down_up", down_up.shape)
# res1 = img1-down_up
# cvshow1(res1, "res1")  # 不能成功，需要shape相同
# img1 = cv2.copyMakeBorder(img1, 0.5, 0.5, 0.5, 0.5, borderType=cv2.BORDER_CONSTANT) 参数不能指定float型无法进行边界填充
# print("img1", img1.shape)
# res1 = img1-down_up
# cvshow1(res1, "res1")
img1 = img1.resize((502, 836))  # AttributeError: 'NoneType' object has no attribute 'shape'
print("img1", img1.shape)
res1 = img1-down_up
cvshow1(res1, "res1")
```

### 轮廓检测与特征

```python
import cv2
import matplotlib.pyplot as plt
import numpy as np

def cvshow1(img,name):
    cv2.imshow(name, img)
    cv2.waitKey(1500)
    cv2.destroyAllWindows()

def cvshow2(img,name):
    cv2.imshow(name, img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()

img1 = cv2.imread('C:/Users/zichuana/Desktop/1.jpg')
# 轮廓检测
# cv2.findContours(img, mode, method)
# mode:轮廓检索模式
# RETR_EXTERNAL 只检索最外面的轮廓
# RETR_LIST 检索所有的轮廓，并将其保存到一条链表当中
# RETR_CCOMP 检索所有轮廓，并将他们组织为两层：顶层是各部分的外部边界，第二层是空洞的边界
# RETR_TREE 检索所有的轮廓，并重构嵌套轮廓的整个层次（最常用）
# method:轮廓逼近方法
# CHAIN_APPROX_NOME:以Freeman链码的方式输出轮廓，所有其他方法输出多边形（顶点的序列）
# CHAIN_APPROX_SIMPLE:压缩水平的、垂直的和斜的部分，也就是说，函数只保留他们的终点部分
gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)  # 转灰度
ret, thresh1 = cv2.threshold(gray1, 127, 255, cv2.THRESH_BINARY)  # 阈值
cvshow1(thresh1, "thresh1")

binary, contours, hierarchy = cv2.findContours(thresh1, cv2.RETR_TREE, cv2.CHAIN_APPROX_NONE)
# binary 也就是thresh1, contours 图像轮廓信息（list格式）, hierarchy 层级（结果保存）
# 需要copy原始img1图像 操作轮廓操作会后修改原图

# 绘制轮廓
img2 = img1.copy()
res = cv2.drawContours(img2, contours, -1, (0, 0, 255), 2)
# 第三个参数默认为-1 （画第几个轮廓？，-1全部）
# 第四个参数（B，G，R）(0,0,255)红色
# 第五参数轮廓线条的宽带
cvshow1(img2, "img2")

img3 = img1.copy()
res2 = cv2.drawContours(img3, contours, 77, (0, 0, 255), 2)
cvshow1(img3, "img3")

# 轮廓特征
cnt = contours[77]  # 取第77个轮廓
# 面积
print(cv2.contourArea(cnt))
# 周长，True表示闭合
print(cv2.arcLength(cnt, True))

# 轮廓近似
# 取点到直线的最长距离是否大于某一个阈值，如果小于，说明这条直线可以，大于由该点开始在做直线重复操作（不断二分）
epsilon = 0.1*cv2.arcLength(cnt, True)
approx = cv2.approxPolyDP(cnt, epsilon, True)
img4 = img1.copy()
res = cv2.drawContours(img4, [approx], -1, (0, 0, 255), 2)
cvshow1(res, "res")

# 外接矩形
x, y, w, h = cv2.boundingRect(cnt)  # 必须指定是哪一个轮廓
img4 = cv2.rectangle(img4, (x, y), (x+w, y+h), (0, 255, 0), 2)
cvshow1(img4, "img5")

area1 = cv2.contourArea(cnt)
x, y, w, h = cv2.boundingRect(cnt)
area2 = w*h
extent = area1/area2
print("轮廓面积与边界面积比", extent)
extent = float(area1)/area2
print("轮廓面积与边界面积比", extent)

# 外接圆
(x, y), radius = cv2.minEnclosingCircle(cnt)
center = (int(x), int(y))
radius = int(radius)
img5 = cv2.circle(img4, center, radius, (0, 255, 0), 2)
cvshow1(img4, "img4")
```

