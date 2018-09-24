---
layout:     post
title:      LeetCode-ALg-344-ReverseString
subtitle:   Algorithm-Easy
date:       2018-09-24
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---
### 1、题目

###### Write a function that takes a string as input and returns the string reversed.

Example 1:

    Input: "hello"
    Output: "olleh"

Example 2:

    Input: "A man, a plan, a canal: Panama"
    Output: "amanaP :lanac a ,nalp a ,nam A"

### 2、思路

根据题意可知，需要将string反转，可以直接定义两个指针i、j，分别指向string的首尾位置，同时移动ij指针并交换所指位置的值。

### 3、实现(c++)

    class Solution {
        public:
        string reverseString(string s) {
            int i = 0;
            int j = s.size() - 1;
            char c;

            while (i < j)
            {
                //使用swap函数交换
                swap(s[i], s[j]);
                // c = s[i];
                // s[i] = s[j];
                // s[j] = c;
                ++i;
                --j;
            }

            return s;
        }
    };

最开始的实现是自己定义一个临时变量用来辅助交换两个元素，但是string提供swap函数来交换两个值，所以还是直接使用swap函数来帮助我们完成这个操作。

### 5、END
