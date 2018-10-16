# Image Gradients
## Goal
    在这节中，我们将学习：
- 寻找图片的梯度、边界等；
- 我们将会学习以下函数：cv2.Sobel(),cv2.Scharr(),cv2.Laplacian()等
## Theory
    OpenCV中提供了三种梯度滤波器或高通滤波器：Sobel，Scharr，Laplacian。我们将逐一学习他们。
### 1. Sobel and Scharr Derivatives
    Sobel算子结合了高斯平滑和微分操作的算子，因此它可以有效的阻止噪声。你可以指定导数的方向即水平或者垂直（分别通过yorder和xorder参数）。同样你可以通过ksize参数来指定卷积核的大小，如果ksize=-1，则默认使用一个3x3的scharr滤波器，3x3的Scharr滤波器得到的结果将会比3x3的Sobel滤波器更好。卷积核的使用请参见文档。
### 2. Laplacian Derivatives
    