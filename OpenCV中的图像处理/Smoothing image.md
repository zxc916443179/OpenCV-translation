# Smoothing Images 
## Goals
    我们将学习：
- 使用不同的低通滤波器模糊图片
- 使用自定义的滤波器处理图像（2D 卷积）

## 2D Convolution（Image Filtering）
    对于一位的信号，图像可以被不同的滤波器处理，如低通滤波器（LPF），高通滤波器（HPF）等。低通滤波器可以用来使图像模糊或者去除噪声。高通滤波器可以用来寻找图像中的边界。
    OpenCV中提供cv2.filer2D（）函数来将卷积核应用于图像。举个例子，我们将尝试使用一个均值卷积核来处理图像。一个5x5的均值卷积核的定义如下：
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/math/220e403e44b16ea8e05d350c4ce69e9aedff5bd1.png)

    使用上述卷积核的过程如下：对每个像素，以该像素为中心，将大小为5x5的窗口内的像素值得总和除以25.实际上就是求窗口内的像素的平均值。对所有像素应用这个操作就可以得到滤波后的图片。尝试以下代码并观察结果：
```
    import cv2
    import numpy as np
    from matplotlib import pyplot as plt

    img = cv2.imread('opencv_logo.png')

    kernel = np.ones((5,5),np.float32)/25
    dst = cv2.filter2D(img,-1,kernel)

    plt.subplot(121),plt.imshow(img),plt.title('Original')
    plt.xticks([]), plt.yticks([])
    plt.subplot(122),plt.imshow(dst),plt.title('Averaging')
    plt.xticks([]), plt.yticks([])
    plt.show()
```    
    
    结果如下：
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/filter.jpg)

## Image Blurring(Image Smoothing)
    图像模糊是通过低通滤波卷积核得到的，它对于去除噪声很有帮助。应用卷积核后，实际上是去除图片中边界上的高频内容。（当然也有保留边界的模糊技术）。OpenCV主要提供四种模糊技术。
### 1.Averaging
    即上述例子
### 2.Gaussian Filtering
    在该方法中，我们使用的是高斯内核而不是相同的卷积系数。它有cv2.GaussianBlur（）函数完成。我们需要为高斯核指定非负奇数的宽度和长度。我们还需要指定X和Y方向上的标准差，sigmaX和sigmaY。如果仅指定了sigmaX，则sigmaY将取与它相等的值。如果两个都指定为0，则将会根据高斯核的大小来计算它们的值。高斯滤波器可以有效的去除图片中的高斯噪声。
    如果你愿意，你可以使用cv2.getGaussianKernel（）函数来自行构造一个高斯核。
    可以对上面的代码做如下修改：
```
    blur = cv2.GaussianBlur(img, (5, 5), 0)
```
    结果如下：
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/gaussian.jpg)
### 3. Median Filtering
    这里，我们使用cv2.medianBlur（）来计算窗口下所有像素的中位数并取代中心像素。该方法可以有效的去除图片中的椒盐噪声。值得注意的是，在高斯滤波器和盒式滤波器中，处理后的值可能在原图中并不存在，而在中值滤波器并不这样，因为中心的元素总是被图片中的一些像素所代替。这样可以有效的减少噪声。卷积核的大小必须是正奇数。
    在这个demo中，我们添加50%的噪声并使用中值滤波器。观察以下结果：
```
    median = cv2.medianBlur(img, 5)
```
    结果：
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/median.jpg)

### 4. Bilateral Filtering
    我们提到过，先前使用的滤波器会让边界模糊，但在双边滤波器中不会发生这样的事。cv2.bilateralFilter（），该函数可以有效的去除噪声同时保留边界，但相比于其他几个滤波器，双边滤波器的计算时间更久。我们已经知道，高斯滤波器取某个像素周围的像素并计算它们的平均高斯权重。这种高斯滤波器是一种空间无视型函数，即卷积时没有将周围的像素考虑进去。它没有考虑是否在像素周围存在相同密度值或者像素是否位于一条边上。这样的结果就是高斯滤波器会将边界模糊，这不是我们想要的。
    双边滤波器在空间上同样使用高斯卷积核，但是它同时由多个高斯矩阵点乘得到，用来计算像素的密度差。高斯函数可以保证相邻的点都被计算进去，而高斯模块可以确保只有那些与中心像素密度值相近的像素点进行模糊操作。所以这样的结果就是该方法可以保留边界，因为对于边界周围的像素点，那些分布在边界另一点的像素点与它有较大的密度差，因此不会被加入到模糊计算中去。
    下面的例子描述了双边滤波器的用法（详细声明请参见OpenCV文档）
```
    blur = cv2.bilateralFilter(img, 9, 75, 75)
```
    结果：
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/bilateral.jpg)
    
    可以看到图像上的纹理已经消失了，但边界依然保留。
