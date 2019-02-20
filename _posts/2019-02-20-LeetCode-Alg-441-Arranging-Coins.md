---
layout:     post
title:      LeetCode-Alg-441-Arranging-Coins
subtitle:   LeetCode-Alg-441-Arranging-Coins
date:       2019-02-20
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目
You have a total of n coins that you want to form in a staircase shape, where every k-th row must have exactly k coins.

Given n, find the total number of full staircase rows that can be formed.

n is a non-negative integer and fits within the range of a 32-bit signed integer.

Example 1:

    n = 5

    The coins can form the following rows:
    ¤
    ¤ ¤
    ¤ ¤

    Because the 3rd row is incomplete, we return 2.

Example 2:

    n = 8

    The coins can form the following rows:
    ¤
    ¤ ¤
    ¤ ¤ ¤
    ¤ ¤

    Because the 4th row is incomplete, we return 3.

### 2、思路

使用二分查找的思想，定义两个指针```low high```初始指向```1和num```,然后计算中间值```mid```, 前```mid```项为当差数列，对该数列求和，通过等差数列的和来判断```low high```指针走向。

### 3、实现
    public int arrangeCoins(int n) {
        if (n < 1)
        {
            return 0;
        }

        int low = 1;
        int high = n;

        while (low <= high)
        {
            int mid = low + (high - low) / 2;
            long sum = (long) mid*(1 + mid)/2;  // 使用等差数列求和公式可能造成溢出
            if (sum == n)
            {
                return mid;
            }else if (sum > n)
            {
                if (sum - mid < n)
                {
                    return mid - 1;
                }else
                {
                    high = mid;
                }
            }else if (sum < n)
            {
                if (sum + (mid + 1) > n)
                {
                    return mid;
                }else
                {
                    low = mid;
                }
            }
        }

        throw null;
    }

需要注意的是，当n比较大时，求前mid项等差数列和可能导致int类型无法表示，这个时候我们将```mid```和```sum```强制类型转换成```long```，时间复杂度为O(logn).

### 4、方法二

从第一行开始排列硬币，然后通过比较剩下的硬币和下一行所需硬币数，若剩下硬币小于下一行所需硬币数，则只能排列到当前行.

    public int arrangeCoins2(int n) {
        if (n < 1)
        {
            return 0;
        }

        int line = 1;
        n -= line;
        while (n >= line+1)
        {
            line++;
            n -= line;
        }

        return line;
    }

### 5、方法三
设n个硬币能排x行，则根据等差数列求和公式可得:
$$ x^{2}+x-2n=0  $$
根据求根公式得正根为:
$$ \dfrac {-1+\sqrt {1+8n}}{2} $$

    public int arrangeCoins3(int n) {
        if (n < 1)
        {
            return 0;
        }
        // 注意这里n*8该操作可能导致溢出
        return (int) (-1 + Math.sqrt(1+8*(long)n)) / 2;
    }
