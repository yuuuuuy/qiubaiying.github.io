---
layout:     post
title:      LeetCode-Alg-285-Add Digits
subtitle:   LeetCode-Alg-285-Add Digits
date:       2019-06-03
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

Example:

    Input: 38
    Output: 2
    Explanation: The process is like: 3 + 8 = 11, 1 + 1 = 2.
          Since 2 has only one digit, return it.

Follow up:

    Could you do it without any loop/recursion in O(1) runtime?


### 2、思路

这道题要求数字的每一个的和，如果计算的和在```1-9```之间则结束计算，否则继续对每一位求和，知道计算结果在```1-9```之间，首先想到的就是首先对给定的数字所有位进行求和，然后对和进行判断，
如果和在```1-9```之间就输出，否则递归求解。

### 3、实现

    public class Solution{
        public int addDigits(int num) {
            if (num >= 0 && num < 10){
                return num;
            }
            int sum = 0;
            while (num != 0){
                sum += num % 10;
                num /= 10;
            }
            return addDigits(sum);
        }
    }

#### 4、他山之石

题中要求不使用循环，且计算处结果的时间复杂度为常数，于是看了别人的思路发现，求这道题是有规律的，
对于给定的一个数，如果该数能被9整除，则所有位的和为9，否则所有位的和为除9的余数。

#### 5、实现

    public class Solution{
        public int addDigits(int num) {
            if (num < 10){
                return num;
            }else if (num % 9 == 0){
                return 9;
            }else{
                return num % 9;
            }
        }
    }
