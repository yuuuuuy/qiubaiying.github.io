---
layout:     post
title:      LeetCode-673. Number of Longest Increasing Subsequence
subtitle:   LeetCode-673. Number of Longest Increasing Subsequence
date:       2019-07-01
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given an unsorted array of integers, find the number of longest increasing subsequence.

    Example 1:
    Input: [1,3,5,4,7]
    Output: 2
    Explanation: The two longest increasing subsequence are [1, 3, 4, 7] and [1, 3, 5, 7].
    Example 2:
    Input: [2,2,2,2,2]
    Output: 5
    Explanation: The length of longest continuous increasing subsequence is 1, and there are 5 subsequences' length is 1, so output 5.

Note: Length of the given array will be not exceed 2000 and the answer is guaranteed to be fit in 32-bit signed int.

### 2、思路

这是一道坐标型动态规划的题，是最长增长子序列的变形，只不过这道题可能存在多个最优解，于是我们需要记录所有可能的解，我们可以按照下面四步来分析：

#### 2.1、确定状态

我们从最后一个元素入手，假设它是一个最长递增子序列中最后一个元素，对于前面所有的元素有下面两种情况：

    1、前面所有的元素都没有比它小的，则最长子序列长就是1，且该最长子序列只有一个;
    2、前面存在比他小的元素，则最长子序列长度就是比他小的元素中子序列最长的+1,且该最长子序列个数是所有最长子序列的个数相加;

例如：```[1,3,5,4,7]```

假设```7```是一个最长增长子序列的最后一个元素，对于```7```，假设前面的```4```和```5```的具有最长增长子序列且长度相等，于是不论是```[...5,7]```还是```[...4,7]```都是最优策略中的一个，所有以```7```结尾的最长子序列个数就取决于以```5```结尾的最长子序列个数```+```以```4```结尾的最长子序列个数，于是我们不仅要记录以每一个元素结尾的最长增长子序列的长度还要其个数.

状态：

设```f[i][0]=```以元素```a[i]```结尾的最长增长子序列长度,```f[i][1]=```以元素```a[i]```结尾的最长增长子序列的个数

#### 2.2、转移方程

    f[i][0]=max{f[i-1][0], f[i-2][0]... f[0][0]}
    f[i][1]=f[i-1][1]+f[i-2][1]....+f[0][0]当f[i-1][0] f[i-2][0]...f[0][0]最长增长子序列最大且都长度相同

#### 2.3、 初始条件和边界情况

第一行 ```f[0][0] = 1  f[0][1] = 1```

因为第一元素增长子序列数是1，前面没有元素比他小，所以子序列数为1


#### 2.4、 计算顺序

从```f[1][0]f[1][1],f[2][0]f[2][1]...f[n-1][0]f[n-1][1]```

结果就是找到最大的子序列并对f[i][1]求和

时间复杂度```O(n^2)```

时间复杂度```O(n)```

### 3、实现

    public int findNumberOfLIS(int[] nums) {
            if (nums == null || nums.length == 0){
                return 0;
            }
            int n = nums.length;
            // 既然有多个状态，那就保存下来
            int[][] f = new int[n][2];
            f[0][0] = 1;
            f[0][1] = 1;

            for (int i = 1; i < n; i++){
                int t = 1;
                int max = 0;
                for (int j = i-1; j >= 0; j--){
                    if (nums[j] < nums[i] && f[j][0] >= max){
                        if (f[j][0] == max){
                            // 存在和当前max相同的子序列
                            t += f[j][1];
                        }else{
                            // 当前子序列为最长
                            max = f[j][0];
                            t = f[j][1];
                        }
                    }
                }
                // 记录最长子序列和数量
                f[i][0] = max+1;
                f[i][1] = t;
            }

            int ans = 0;
            int m = 0;
            for (int i = 0; i < n; i++){
                if (f[i][0] >= m){
                    if (f[i][0] == m){
                        ans += f[i][1];
                    }else{
                        m = f[i][0];
                        ans = f[i][1];
                    }
                }
            }
            return ans;
    }

### 4、他山之石
