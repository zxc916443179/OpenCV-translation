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

    