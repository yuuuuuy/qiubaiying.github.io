---
layout:     post
title:      LeetCode-698. Partition to K Equal Sum Subsets
subtitle:   LeetCode-698. Partition to K Equal Sum Subsets
date:       2019-07-22
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given an array of integers nums and a positive integer k, find whether it's possible to divide this array into k non-empty subsets whose sums are all equal.

Example 1:

    Input: nums = [4, 3, 2, 3, 5, 2, 1], k = 4
    Output: True
    Explanation: It's possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.

Note:

    1 <= k <= len(nums) <= 16.
    0 < nums[i] < 10000.

### 2、思路

这道题给我们一个数组和一个k，将数组中的元素分成k组，每组至少一个元素且各组元素的和相等，所以我们可以先计算数组中所有元素的和，如果能被k整除说明可能满足划分。

    1) 首先计算数组中所有元素和sum，如果sum不能被k整除，则结果为false，否则计算划分后每组的和 subSum = sum/k;

    2) 对数组进行非降序排序，从后往前遍历，如果最大的元素比subSum大，说明存在元素没法划分，结果为false，否则将所有等于sunSum的元素挑出来单独为一组，分成i组，此时剩下j个元素;

    3) 将剩下的j个元素(0,1,2..j-1)分成k-i组，可以使用回溯法不断的去尝试，对于元素j-1,如果可以放入第一组且剩下j-1个元素可以放入k-i组中，则存在这样的划分，否则，将元素j-1从第一组拿出来尝试放入第二组，知道所有的元素尝试完;

    4) 如果元素尝试完都没有找到可能的划分，则不存在这样的划分.

### 3、实现

    public boolean canPartitionKSubsets(int[] nums, int k) {
       int sum = sum(nums);
       if (sum % k != 0){
           return false;
       }

       int subSum = sum / k;

       Arrays.sort(nums);
       int i = nums.length - 1;
       if (nums[i] > subSum){
           return false;
       }
       while (i >= 0 && nums[i] == subSum){
           i--;
           k--;
       }

       return patition(new int[k], nums, i, subSum);
    }

    public int sum(int[] nums){
       int res = 0;
       for (int num : nums){
           res += num;
       }
       return res;
    }

    public boolean patition(int[] subSets, int[] nums, int start, int target){
       if (start < 0){
           return true;
       }

       for (int i = 0; i < subSets.length; i++){
          if (subSets[i] + nums[start] <= target){
              subSets[i] += nums[start];
              if (patition(subSets, nums, start-1, target)){
                  return true;
              }
              subSets[i] -= nums[start];
          }
       }
       return false;
    }
