---
layout:     post
title:      LeetCode-ALg-349-IntersectionofTwoArrays
subtitle:   Algorithm-Easy
date:       2018-09-26
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---
### 1、题目

###### Given two arrays, write a function to compute their intersection.

Example 1:

    Input: nums1 = [1,2,2,1], nums2 = [2,2]
    Output: [2]

Example 2:

    Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
    Output: [9,4]

### 2、要求

- 结果中每一个元素只能出现一次
- 结果可以是任意顺序

### 2、思路

- 思路一：首先想到的方法就是将其中一个数组以元素作为map的key存储,
定义一个set用来存放结果,然后遍历另一个数组,如果元素在map中就添加到set中,
这样做的结果就是需要开辟一个额外内存空间。
- 思路二:先对两个数组进行排序(升序),然后定义两个指针ij和保存结果的集合set,ij分别指向两个数组的首元素<br>
    如果nums1[i] < nums2[j] ++i;<br>
    如果nums1[i] > nums2[j] ++j;<br>
    如果nums1[i] == nums2[j] 存放到结果集中.

### 3、实现(c++)

    class Solution {
    public:
        vector<int> intersection1(vector<int>& nums1, vector<int>& nums2) {
            map<int, int> amap;
            for (int i : nums1)
            {
                amap[i] += 1;
            }
            set<int> aset;
            for (int i : nums2)
            {
                if (amap.find(i) != amap.end())
                {
                    aset.insert(i);
                }
            }
            return vector<int>(aset.begin(), aset.end());
        }

        vector<int> intersection2(vector<int>& nums1, vector<int>& nums2) {
            sort(nums1.begin(), nums1.end());
            sort(nums2.begin(), nums2.end());

            int i = 0;
            int j = 0;
            set<int> res;

            while (i < nums1.size() && j < nums2.size())
            {
                if (nums1[i] < nums2[j]) {++i;}
                else if (nums1[i] > nums2[j]) {++j;}
                else {res.insert(nums1[i]); ++i; ++j;}
            }
            return vector<int>(res.begin(), res.end());
        }
    };
