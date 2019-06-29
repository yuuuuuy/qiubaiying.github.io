---
layout:     post
title:      动态规划-Coin Change
subtitle:   Coin Change
date:       2019-06-27
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

Example 1:

    Input: coins = [1, 2, 5], amount = 11
    Output: 3
    Explanation: 11 = 5 + 5 + 1
    Example 2:

    Input: coins = [2], amount = 3
    Output: -1
    Note:
    You may assume that you have an infinite number of each kind of coin.

### 2、题解

#### 2.1、确定状态

一般解动态规划问题时需要开一个数组，数组的每一个元素```f[i]```或```f[i][j]```表示一种状态，类似数学中的变量```XYZ```，确定状态一般从最后一步开始，
假设最优策略中有```k```枚硬币```[a1,a2,,,ak]```面值和为```amount```，我们拿出最后一枚硬币```ak```，剩下的```amount-ak```就需要```k-1```枚硬币，于是我们将问题化成更小的问题：

原问题： 最少使用多少枚硬币拼出```amount```

子问题： 最少使用多少枚硬币拼出```amount-ak```

于是我们定义状态：

设状态```f(X)=```最少使用多少枚硬币拼出```X```
但是```ak```是多少？

在Example中```ak```可能是1/2/5

如果```ak=1``` ```f(amount) = f(amount-1) + 1```

如果```ak=2``` ```f(amount) = f(amount-2) + 1```

如果```ak=5``` ```f(amount) = f(amount-5) + 1```

所以 ```f(amount) = min{f(amount-1) + 1, f(amount-2) + 1, f(aount-5) + 1}```

#### 2.2 转移方程
对于任意的```X```， 转移方程 ```f[X] = min{f[X-1]+1, f[X-2]+1, f[X-5]+1}```

#### 2.3 初始条件和边界情况

初始条件为 ```f[0]=0``` 0元需要0枚硬币

#### 2.4 计算顺序

从1元开始从小往大计算

### 3、实现

    public int coinChange(int[] coins, int amount){
      int[]f = new int[amount+1];
      f[0] = 0;
      for (int i = 1; i <= amount; i++){
        f[i] = Integer.MAC_VALUE;
        for (int coin : coins){
          if (i >= coin && f[i - coin] !+ Integer.MAX_VALUE && f[i - coin] + 1 < f[i]){
            f[i] = f[i - coin] + 1;
          }
        }
        return f[amount] == Integer.MAX_VALUE ? -1 : f[amount];
      }
    }
