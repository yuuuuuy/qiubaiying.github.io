---
layout:     post
title:      LeetCode-Alg-367-Valid-Perfect-Square
subtitle:   LeetCode-Alg-367-Valid-Perfect-Square
date:       2019-02-13
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目
Given a positive integer num, write a function which returns True if num is a perfect square else False.

Note: Do not use any built-in library function such as sqrt.

Example 1:

    Input: 16
    Output: true
Example 2:

    Input: 14
    Output: false
### 2、思路

使用二分查找的思想，定义两个指针```low high```初始指向```1和num```,然后计算中间值```mid```,再对mid进行```平方```和```num```进行比较来决定```low high```的走向。

### 3、实现
    public boolean isPerfectSquare(int num) {
        if (num < 0)
        {
            return false;
        }
        if (num == 1)
        {
            return true;
        }
        int low = 1;
        int high = num;

        while (low <= high)
        {
            int mid = low + (high - low) / 2;
            int product = (int) Math.pow(mid, 2);
            if (product == mid)
            {
                return true;
            }else if(product > mid)
            {
                high = mid - 1;
            }else if(product < mid)
            {
                low = mid + 1;
            }
        }

        return false;
    }
    
当测试的输入较大时，其数值的一半再平方将会超出```int```所能表示的范围。于是我们不对```mid```进行```平方```，而是使用```num/mid```得到```商```来和```mid```进行比较；
但是有个问题，使用```/```取```商```可能会造成精度的丢失，导致错误。于是引入```余数```，若```商```和```mid```相同且```余数```为```0```则说明该数是平方数。

### 4、修改
    public boolean isPerfectSquare(int num) {
        if (num < 0)
        {
            return false;
        }
        if (num == 1)
        {
            return true;
        }
        int low = 1;
        int high = num;

        while (low <= high)
        {
            int mid = low + (high - low) / 2;
            int div = num / mid;
            int remainder = num % mid;
            if (div == mid && remainder == 0)
            {
                return true;
            }else if(div > mid)
            {
                low = mid + 1;
            }else if(div < mid)
            {
                high = mid - 1;
            }else
            {
                return false;
            }
        }

        return false;
    }
