---
layout:     post
title:      LeetCode-1027. Longest Arithmetic Sequence
subtitle:   LeetCode-1027. Longest Arithmetic Sequence
date:       2019-07-02
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given an array A of integers, return the length of the longest arithmetic subsequence in A.

Recall that a subsequence of A is a list A[i_1], A[i_2], ..., A[i_k] with 0 <= i_1 < i_2 < ... < i_k <= A.length - 1, and that a sequence B is arithmetic if B[i+1] - B[i] are all the same value (for 0 <= i < B.length - 1).



    Example 1:

    Input: [3,6,9,12]
    Output: 4
    Explanation:
    The whole array is an arithmetic sequence with steps of length = 3.
    Example 2:

    Input: [9,4,7,2,10]
    Output: 3
    Explanation:
    The longest arithmetic subsequence is [4,7,10].
    Example 3:

    Input: [20,1,15,3,10,5,8]
    Output: 4
    Explanation:
    The longest arithmetic subsequence is [20,15,10,5].


Note:

    2 <= A.length <= 2000
    0 <= A[i] <= 10000

### 2、思路

这是一道坐标型动态规划的题,让求最长算术子序列，所谓算术子序列就是序列中相邻元素之间的差值都相等，我们可以按照下面四步来分析：

#### 2.1、确定状态

我们从最后一个元素入手，假设它是一个最长算术子序列中最后一个元素```a[k]```,则有：

    a[k]-a[k-1] = a[k-1]-a[k-2]

但是```diff=a[k]-a[k-1]```差值是多少我们不知道，对于所给列表中的最后一位元素，我们需要知道它前面哪个元素和他的差值是最长算术子序列的差值```diff```.

例如：```[1,3,5,4,7]```
对于```7```，和前面四个元素的差值分别是 ```3、2、4、6```，哪个差值才是最长子序列的差值```diff```？显然我们需要枚举某个元素```a[k]```与前面```k-1```个元素的差值，并找到以该差值为标准的最长算术子序列长。

这里我们需要枚举```a[k]```与其前```k-1```个元素之间的差值，题中给出```0=<A[i]<=10000```,所以```a[k]-a[k-1]```的范围就是```[-10000, 10000]```，于是我们可以开一个```n*20001```的数组来存放差值对应的最长算术子序列长度

状态：

设```f[i][j]=```以元素```a[i]```结尾且以差值j为标准的的最长算术子序列长度

#### 2.2、转移方程

    f[i][j] = 2
    f[i][j] = max{f[i-1][j], f[i-2][j],...f[0][j]}

#### 2.3、 初始条件和边界情况

无初始条件和边界情况

#### 2.4、 计算顺序

从```f[1][diff], f[2][diff], f[3][diff]...f[n-1][diff]```

结果就是找到最大的f[i][diff]

时间复杂度```O(n^2)```

时间复杂度```O(n*20001)```

### 3、实现

    public int longestArithSeqLength(int[] A) {
        int n = A.length;
        int[][] f = new int[n][20000+1];
        int max = 2;
        for (int i = 1; i < n; i++){
            for (int j = 0; j < i; j++){
                int diff = A[i]-A[j]+10000;
                // 说明前面没有差值相同的元素
                if (f[j][diff] == 0){
                    f[i][diff] = 2;
                    continue;
                }

                f[i][diff] = Math.max(f[i][diff], f[j][diff]+1);
                max = Math.max(max, f[i][diff]);
            }
        }
        return max;
    }

### 4、优化

我们直接开了一个n*20001的二维数组，导致空间浪费,我们可以找到所给数组中的最大值和最小值，再决定开多大的数组。

    public int longestArithSeqLength(int[] A) {
        int n = A.length;
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;

        for (int a : A){
            if (a > max) max = a;
            if (a < min) min = a;
        }

        int maxDiff = max - min;
        int[][] f = new int[n][maxDiff*2+1];
        int ans = 2;
        for (int i = 1; i < n; i++){
            for (int j = 0; j < i; j++){
                int diff = A[i] - A[j] + maxDiff;
                if (f[j][diff] == 0){
                    f[i][diff] = 2;
                }
                f[i][diff] = Math.max(f[j][diff] + 1, f[i][diff]);
                if (f[i][diff] > ans){
                    ans = f[i][diff];
                }
            }
        }
        return ans;
    }
