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

一般解动态规划问题时需要开一个数组，数组的每一个元素f[i]或f[i][j]表示一种状态，类似数学中的变量XYZ，确定状态一般从最后一步开始，
假设最优策略中有k枚硬币[a1,a2,,,ak]面值和为amount，我们拿出最后一枚硬币ak，剩下的amount-ak就需要k-1枚硬币，于是我们将问题化成更小的问题：
原问题： 最少使用多少枚硬币拼出amount
子问题： 最少使用多少枚硬币拼出amount-ak
于是我们定义状态：
设状态f(X)=最少使用多少枚硬币拼出X
但是ak是多少？

在Example中ak可能是1/2/5

如果ak=1 f(amount) = f(amount-1) + 1

如果ak=2 f(amount) = f(amount-2) + 1

如果ak=5 f(amount) = f(amount-5) + 1

所以 f(amount) = min{f(amount-1) + 1, f(amount-2) + 1, f(aount-5) + 1}

#### 2.2 转移方程
    对于任意的X
    转移方程 f[X] = min{f[X-1]+1, f[X-2]+1, f[X-5]+1}

#### 2.3 初始条件和边界情况
    初始条件为 f[0]=0 0元需要0枚硬币

#### 2.4 计算顺序
    从1元开始从小往大计算

### 3、实现
