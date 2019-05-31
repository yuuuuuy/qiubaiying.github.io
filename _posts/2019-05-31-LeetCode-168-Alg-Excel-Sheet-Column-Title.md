---
layout:     post
title:      LeetCode-Alg-168-Excel Sheet Column Title
subtitle:   LeetCode-Alg-168-Excel Sheet Column Title
date:       2019-05-31
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:

        1 -> A
        2 -> B
        3 -> C
        ...
        26 -> Z
        27 -> AA
        28 -> AB
        ...

Example 1:

    Input: 1
    Output: "A"
    Example 2:

    Input: 28
    Output: "AB"
    Example 3:

    Input: 701
    Output: "ZY"

### 2、思路

这道题让我们求给定一个正整数，求对应的Excel的列名，Excel使用A-Z的组合来表示列，数字1-26分别对应A-Z，27对应AA等;我们将数多对应的列名写下来找下规律。

    1 2 3 4 5 6 ... 25 26
    A B C D E F ... Y  Z
    27 28 29 30 ... 51 52
    AA AB AC AD ... AY AZ
    53 53 55 56 ... 77 78
    BA BB BC BD ... BY BZ
    ...

从上面的对应关系可以看出，
1）计算n/26得到的商和余数，余数可能为0或者不为0，当为0时说明n是26的倍数，这时候列名的最后一位一定是'Z',即需要将此时的余数变成26，商减小1,然后判断商与26的大小;

2）如果商的值大于26则需要再次对商进行(1)的操作,否则找到商对应A-Z的那个字母;

3) 直到商的范围在[1, 26]之间(余数范围一定在[1, 26])时输出结果。

### 3、实现

    public class Solution {

        public String convertToTitle(int n) {
            StringBuilder sb = new StringBuilder();
            return convert(n/26, n%26, new StringBuilder());
        }

        public String convert(int n, int rem, StringBuilder sb){
            if (rem == 0){
                n -= 1;
                rem = 26;
            }
            sb.append((char)(rem + 64));
            if (n <= 26){
                if (n > 0){
                    sb.append((char)(n + 64));
                }
                return sb.reverse().toString();
            }

            return convert(n/26, n%26, sb);
        }
    }
