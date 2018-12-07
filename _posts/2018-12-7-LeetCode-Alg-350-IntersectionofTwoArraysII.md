---
layout:     post
title:      LeetCode-ALg-350-IntersectionofTwoArraysII
subtitle:   Algorithm-Easy
date:       2018-12-07
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
    Output: [2,2]

Example 2:

    Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
    Output: [4,9]

Note:

    1. Each element in the result should appear as many times as it shows in both arrays.
    2. The result can be in any order.

### 2、思路

- 首先想到定义一个字典，将一个长度较短的数组中的元素作为字典的key，出现的次数作为value存储到字典中，然后遍历另一个数组，如果元素在字典中，且字典对应的value大于0，就将该元素存放到结果集中，同时将该值对应的value值减小1。

### 3、实现(Java)

    public int[] intersect(int[] nums1, int[] nums2) {
        if (nums1.length == 0 || nums2.length == 0)
        {
            return new int[0];
        }
        HashMap<Integer, Integer> map = new HashMap<>();
        int k = 0;

        if (nums1.length >= nums2.length)
        {
            for (int elem : nums2)
            {
                // 这里的containsKey() get() put()操作比较耗时
                int count = map.containsKey(elem) ? map.get(elem) : 0;
                map.put(elem, count + 1);
            }

            int[] result = new int[nums1.length];
            for (int i : nums1)
            {
                if (map.containsKey(i) && map.get(i) > 0)
                {
                    result[k] = i;
                    map.put(i, map.get(i) - 1);
                    k++;
                }
            }
            return Arrays.copyOfRange(result, 0, k);
        }else
        {
            return intersect(nums2, nums1);
        }
    }

### 4、思考
#### 4.1 如果所给的两个数组已经排好序
#### 4.2 如果所给的数组中nums1中元素相比nums2较少
#### 4.3 如果nums2数组已经在磁盘上排好序，因为内存限制无法将所有的元素加载到内存中
