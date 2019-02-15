---
layout:     post
title:      Numpy中shape为(m,)的是行向量还是列向量
subtitle:   Numpy中shape为(m,)的是行向量还是列向量
date:       2019-02-15
author:     Jae
header-img: img/numpy.jpg
catalog: true
tags:
    - Python
    - Numpy
---
### 1、shape为(m,)的向量是行向量还是列向量

定义一个一维数组

    v = np.array([1, 2])
    v
    array([1, 2])

该数组的```shape```为

    v.shape
    (2,)

再定义一个数组

    z = np.array([[1], [2]])

该数组的```shape```为

    z.shape
    (2, 1)
到这里我们可能认为```shape```为```(2,)```的数组是个行向量，先不过早下结论，往下验证.
### 2、向量和矩阵相乘

定义一个```2x2```的矩阵```A```

    A = np.arange(4).reshape((2, 2))
    A
    array([[0, 1],
       [2, 3]])

将矩阵和前面定义的向量```v```相乘，按照我们前面的猜想，认为```v```是个行向量```(1x2)```，于是可以使用```vxA```

    v.dot(A)
    array([4, 7])

下面我们使用```A(2x2)xv(1x2)```，如果```v```是个行向量是无法计算的.

    A.dot(v)
    array([2, 8])
可以计算，从结果我们可以看出，此时的向量```v```被作为```(2x1)```的列向量。

### 3、原因

因为```Numpy```在做向量和矩阵相乘时，会自动为我们判断该向量取行向量还是列向量。
