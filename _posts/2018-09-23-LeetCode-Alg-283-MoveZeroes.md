---
layout:     post
title:      LeetCode-ALg-283-MoveZeroes
subtitle:   Algorithm-Easy
date:       2018-09-23
author:     Jae
header-img: img/post-bg-e2e-ux.jpg
catalog: true
tags:
    - Algorithm
---
### 1、题目

###### Given an array ```nums```, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

### 2、要求
##### (1) 不允许拷贝数组来执行此操作
##### (2) 最小化操作的次数

### 3、思路

根据题意可知，需要将数组中所有的0元素放到数组的尾部，其他的非0元素不能改变原有顺序，
首先能想到的是使用c++对vector的成员函数erase来删除所有的0元素，然后在数组的末尾添加和
删除数量相同数量的0。
但是在数组的中间或者头删除元素的时间复杂度为```O(n)```,这样做会比较耗时。
### 4、分析

由于删除的复杂度较大，我们就考虑不删除元素，而将非0元素复制到相应的位置，最后留出末尾的位置
来赋值为0。

    (1) 定义两个指针```i```和```j```,i指向需要修改的位置，j向后移动寻找非0元素;
    (2) 当j移动到非0元素上时，将j所指单元的数值拷贝给i所指的单元,i和j均往后移动一步；
    (3) 当j移动到数组尾元素的下一个位置时，判断i的位置，如果i的位置还没有到数组尾元素的
    下一位置，就将i继续往后移，并将i在移动过程中所指元素均赋值为0。

### 5、实现(c++)

    class Solution {
    public:
        void moveZeroes(vector<int>& nums) {
            int i = 0;
            int j = 0;
            int len = nums.size();

            for (; j < len; ++j)
            {
                if (nums[j] == 0)
                {
                    continue;
                }
                nums[i] = nums[j];
                ++i;
            }
            if (i < len)
            {
                for (; i < len; ++i)
                {
                    nums[i] = 0;
                }
            }
        }
    };


### 6、他山之石

[LeetCode](https://leetcode.com/problems/move-zeroes/solution/)上提供了几种思路！

### 7、END
