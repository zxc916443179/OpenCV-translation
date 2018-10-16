# Canny Edge Dectection
## Goal
    在本节中，我们将学习关于：
- canny边缘检测算法的思路
- OpenCV提供的函数：cv2.Canny()
## Theory
    Canny边缘检测算法是一个常用的边缘检测算法，它由John F.Canny在1986年提出。它是一个多步算法，我们会详细地介绍每一个步骤。
### 1. Noise Reduction
    由于边界在图片中是易受噪声影响的，因此第一步我们使用5x5的高斯核移除噪声，这在之前已经做过相关的工作。
### 2. Finding Intensity Gradient of the Image
    随后我们将平滑处理后的图片用Sobel卷积核在水平和竖直方向上处理分别得到水平方向和竖直方向的导数Gx和Gy。我们可以通过以下公式得到每个像素的边界梯度和方向：
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/math/fc9752466c9c38d07985d62e86946489e23c61e2.png)

    梯度的方向始终与边的方向垂直，它有四个取值分别代表水平、竖直、和两个对角线方向。
### 3. Non-maximum Suppression
    在得到梯度和方向之后，接下来需要做的是扫描整张图片移除那些不构成边界的像素点。对每个像素点我们检查它是否为局部的最大值。观察以下图片：
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/nms.jpg)

    点A在边界上（竖直方向）。梯度方向与边界正交。点B与点C在梯度方向上。点A与B、C比较是否为最大值，如果是，则进入下一阶段，否则将这个点抑制（令其为零）。
    简单地说，你将会得到只有边界的二进制图。
### 4. Hysteresis Thresholding
    这一步是检查是否所有边界都是真的边界。对此，我们需要两个阈值，minVal和maxVal。那些强度梯度大于maxVal的边一定是边界，而那些小于minVal的则必不是边界，因此我们舍弃它们。那些位于阈值之间的边将根据它们的连通性来判断它们是否为边界。如果它们可以与“确定边”的像素点连通的话，则它们被认为是边的一部分。否则，它们同样被舍弃。观察下图：
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/hysteresis.jpg)

    边A在maxVal之上，因此是“确定边”。尽管边C在maxVal之下，但它与边A相连，因此被认为是边的一部分。对于边B，尽管它与边C一样在minVal之上，但它并没有与确定边相连，因此它被舍弃。因此，对我们来说选择minVal和maxVal的值是至关重要的。
    这一步同样可以移除小像素噪声。
    因此最后我们得到的是“强边”图。
## Canny Edge Detection in OpenCV
    OpenCV将上述所有操作都包含在了cv2.Canny()中，我们将学习如何使用它。第一个参数是我们的输入图像，第二和第三个参数是我们的minVal和maxVal。第三个参数是aperture_size。它是我们用来计算图像梯度的Sobel卷积核大小，默认为3.最后一个参数是L2梯度，它用来指定计算梯度幅值的等式。如果它为True，它将使用我们上面提到的计算更为精确地公式，否则将使用以下函数：
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/math/559f1d19fb3ffb98feccf9e5931edc0f73e1f26e.png)
    其默认值为False。
```
    import cv2
    import numpy as np
    from matplotlib import pyplot as plt

    img = cv2.imread('messi5.jpg',0)
    edges = cv2.Canny(img,100,200)

    plt.subplot(121),plt.imshow(img,cmap = 'gray')
    plt.title('Original Image'), plt.xticks([]), plt.yticks([])
    plt.subplot(122),plt.imshow(edges,cmap = 'gray')
    plt.title('Edge Image'), plt.xticks([]), plt.yticks([])

    plt.show()
```
    处理结果如下：
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/canny1.jpg)