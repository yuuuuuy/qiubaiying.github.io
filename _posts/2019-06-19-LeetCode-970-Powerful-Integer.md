---
layout:     post
title:      LeetCode-Alg-970-Powerful-Integers
subtitle:   LeetCode-Alg-970-Powerful-Integers
date:       2019-06-19
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given two positive integers x and y, an integer is powerful if it is equal to x^i + y^j for some integers i >= 0 and j >= 0.

Return a list of all powerful integers that have value less than or equal to bound.

You may return the answer in any order.  In your answer, each value should occur at most once.



    Example 1:

    Input: x = 2, y = 3, bound = 10
    Output: [2,3,4,5,7,9,10]
    Explanation:
    2 = 2^0 + 3^0
    3 = 2^1 + 3^0
    4 = 2^0 + 3^1
    5 = 2^1 + 3^1
    7 = 2^2 + 3^1
    9 = 2^3 + 3^0
    10 = 2^0 + 3^2
    Example 2:

    Input: x = 3, y = 5, bound = 15
    Output: [2,4,6,8,10,14]


Note:

    1 <= x <= 100
    1 <= y <= 100
    0 <= bound <= 10^6

### 2、思路

最先想到的方法就是暴力枚举所有的情况，使用双重```for```循环，内层循环遇到```x^i+y^j>bound```时跳出，外层循环遇到```x^i+1>bound```时也跳出，但是需要注意的一种情况就是
```x```和```y```可能为```1```，这时候无论```i```或```j```为多少值都为```1```，可能会造成无限循环。

### 3、实现

    public class Solution {
        // 暴力
        public List<Integer> powerfulIntegers(int x, int y, int bound) {
            int temp;
            Set<Integer> ans = new HashSet<>();
            for (int i = 0; x == 1 ? i < 1 : Math.pow(x, i) + 1 <= bound; i++){
                for (int j = 0; y == 1 ? j < 1 : Math.pow(x, i) + Math.pow(y, j) <= bound; j++){
                    temp = (int)(Math.pow(x, i) + Math.pow(y, j));
                    if (temp <= bound){
                        ans.add(temp);
                    }else{
                        break;
                    }
                }
            }
            return new ArrayList<>(ans);
        }
    }
