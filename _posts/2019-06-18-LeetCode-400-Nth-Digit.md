---
layout:     post
title:      LeetCode-Alg-400-Nth-Digit
subtitle:   LeetCode-Alg-400-Nth-Digit
date:       2019-06-18
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Find the nth digit of the infinite integer sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...

Note:
n is positive and will fit within the range of a 32-bit signed integer (n < 231).

Example 1:

    Input:
    3

    Output:
    3
    Example 2:

    Input:
    11

    Output:
    0

Explanation:

    The 11th digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... is a 0, which is part of the number 10.

### 2、思路

对于这题要找第i位数是多少，我们按照位数列举部分情况找找规律：

    1 2 3 4 5 6 7 8 9 总共 9*1位
    10 11 ...      99 总共 90*2位
    100 101 ...   999 总共 900*3位
    ...
所以我们只需要找到n会落在那个区间就能找到第```n```位对应的数字， 对于第一行范围为```[1, 9]```，第二行范围```[10, 189](90*2+9*1)```,第三行范围```[190, 2889](900*3+90*2+9*1)```...

假设```n=12```，```n```落在了第二行，计算```n```与该行首元素```10```的差值```diff```为```2```，第二行全是两位数，使用```diff/2```得到在首数```10```基础上需要跳过几个数，当找到具体某一个数后再通过```diff%2```就能知道是该数的第几个数字

### 3、实现

    public class Solution {
        public int findNthDigit(int n) {
            int left = 0;
            int right = left;
            for (int i = 0; ; i++){
                left = right + 1;
                right += Math.pow(10, i) * 9 * (i + 1);
                if (n >= left && n <= right){
                    // 位数是i+1
                    int head = (int)Math.pow(10, i);
                    // 差值
                    int diff = n - left;
                    // 已经找到
                    int target = head + diff / (i + 1);
                    // 取特定的一位
                    return String.valueOf(target).charAt(diff % (i + 1)) - '0';
                }
            }
        }
    }
