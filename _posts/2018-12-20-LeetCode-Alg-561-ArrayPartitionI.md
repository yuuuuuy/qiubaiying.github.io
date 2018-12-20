---
layout:     post
title:      LeetCode-ALg-561-ArrayPartitionI
subtitle:   Algorithm-Easy
date:       2018-12-20
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---
### 1、题目

###### Given an array of ```2n``` integers, your task is to group these integers into n pairs of integer, say ```(a1, b1)```, ```(a2, b2)```, ```...```, ```(an, bn)``` which makes sum of ```min(ai, bi)``` for all ```i``` from ```1``` to ```n``` as large as possible.

    Example 1:
    Input: [1,4,3,2]

    Output: 4
    Explanation: n is 2, and the maximum sum of pairs is 4 = min(1, 2) + min(3, 4).

Note:

- n is a positive integer, which is in the range of [1, 10000].

- All the integers in the array will be in the range of [-10000, 10000].


### 2、思路

又题可以知道将数组中这2n个数字分成两两一组，然后将每组中的最小值相加得到和要最大，最先能
想到的方法就是将数组升序，然后从最大的一端开始挑选，每两个数字中选择次大的相加，于是得到最大结果。
如排序后的数组为[1,2,3,4],4和3选择次大3， 2和1选择次大1，于是结果为1+3=4。

时间复杂度为```O(nlogn)```

### 3、实现(Java)

    public int arrayPairSum(int[] nums) {
        Arrays.sort(nums);
        int sum = 0;
        for (int i = nums.length - 1 ; i >= 0; i-=2)
        {
            sum += nums[i-1];
        }
        return sum;
    }
### 4、他山之石
参考了其他人提供的方法，算法描述如下：

由于题中给出```n```的范围为```[1, 10000]```，数组中每一个元素取值范围为```[-10000, 10000]```

- 首先开辟一个长度为```20000+1```的数组；

- 遍历原始数组中的每一个元素```elem```，使用```10000+elem```得到的结果作为新数组的下标(也就将数组中的元素全部转换为正整数和```0```)，
并将该位置的值自增```1```；

- 遍历新数组，内层```while```循环判断该下标位置的值是否为```0```，不为```0```说明该位置对应有原数组中的元素```k```个，并使用一个```boolean```变量```flag```(初始值为```true```)控制取是否取元素；

### 5、实现(Java)
    public int arrayPairSum(int[] nums) {
            byte[] mark = new byte[200001];
            for (int elem : nums)
            {
                mark[10000 + elem]++;
            }
            int sum = 0;
            boolean flag = true;

            for (int i = 0; i <= 20000; i++)
            {
                while (mark[i] > 0)
                {
                    if (flag)
                    {
                        sum += (i - 10000);
                    }
                    flag = !flag;
                    mark[i]--;
                }
            }
            System.out.println(sum);
            return sum;
        }
