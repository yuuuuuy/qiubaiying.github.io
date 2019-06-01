---
layout:     post
title:      LeetCode-Alg-171-Excel Sheet Column Number
subtitle:   LeetCode-Alg-171-Excel Sheet Column Number
date:       2019-06-1
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a column title as appear in an Excel sheet, return its corresponding column number.

For example:

    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28
    ...
Example 1:

    Input: "A"
    Output: 1
    Example 2:

    Input: "AB"
    Output: 28
    Example 3:

    Input: "ZY"
    Output: 701

### 2、思路

该题和题168是相反的，根据所给的Excel列名反推代表的数字，我们首先那几个数据计算一下找到对应的关系：

    A 0*26+1 = 1
    AA 1*26+1 = 27
    ZY 26*26+25 = 701
    XYZ (24*26+25)*26+26 = 16900

从上面的推导可以发现，列名对应的数字计算方法就是最后一位字母表示的数```+```剩下字母表示数```*```26，
对于剩下的字母有多少位我们不用关心，还按照这样的思路递归的去计算即可

### 3、实现

    public class Solution {
        public int titleToNumber(String s) {
            if (s.length() == 1){
                return s.charAt(0) - 64;
            }
            return (s.charAt(s.length()-1) - 64) + (titleToNumber(s.substring(0, s.length()-1)))*26;
        }
    }
