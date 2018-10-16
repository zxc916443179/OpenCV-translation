# SURF介绍

## Goal
在本章节中，
    - 我们将介绍SURF的基础
    - 我们将介绍OpenCV中SURF的函数库

## Theory
    在上一节中，我们介绍了SIFT的特征点检测和描述，但相对来说它还是比较慢的，因此人们需要速度更快的版本。2006年，Bay H.，Tuytelaars T.,和Van Gool L.，三人共同发表了一篇名为“SURF：Speeded up Robust Features”，介绍了一个名为SURF的算法。顾名思义，它是SIFT的加速版本。
    在SIFT中，Lowe使用近似的高斯差分的高斯拉普拉斯来寻找尺度空间