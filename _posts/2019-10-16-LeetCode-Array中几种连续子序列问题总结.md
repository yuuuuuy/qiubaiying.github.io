---
layout:     post
title:      LeetCode-Array中几种连续子序列问题总结
subtitle:   LeetCode-Array中几种连续子序列问题总结
date:       2019-10-16
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1 和能被K整除的子数组个数

#### 1.1 题目
Given an array A of integers, return the number of (contiguous, non-empty) subarrays that have a sum divisible by K.

Example 1:

    Input: A = [4,5,0,-2,-3,1], K = 5
    Output: 7
    Explanation: There are 7 subarrays with a sum divisible by K = 5:
    [4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]


Note:

    1 <= A.length <= 30000
    -10000 <= A[i] <= 10000
    2 <= K <= 10000

#### 1.2 思路

这道题让我们求所有连续子数组中和能被K整除的个数，暴力的方法就是求出所有的连续子数组并计算它们的和并判断是否能被```K```整除，
但是枚举所有可能的子数组时间复杂度为```O(n^2)```,有没有更好的方法不用枚举所有的情况？我们知道可以使用前缀和来计算连续子数组的和，

我们定义```sum(i, j)```表示下标从```i```到```j-1```的元素的和，所以下标从```i```到```j-1```的连续子数组的和```sum(i, j) = sum(0, j) - sum(0, i)```,于是我们只要求出
数组元素的前缀和就能求出任意连续子数组的和。

这里设```P[i]```表示下标```0```到```i```的子数组的前缀和，假设任意一个连续子数组的和```sum(i, j) % K = 0```,则```(sum(0, j) - sum(0, i)) % K = 0```,其中```sum(0, j)```就是
前缀和```P[j]```,```sum(0, i)```为前缀和```P[i]```,所以有```P[j]```和```P[i]```同余，假设```a```和```b```关于```K```同余，则有```a = b + n*K```,所以```a-b=n*K```,所以这样的前缀和是满足我们条件的。
在满足条件的前缀和```(P[n], P[m]...)```中两两排列，所有的组合数相加即为最后结果。

举例: 输入数组```[4,5,0,-2,-3,1] K = 5```
计算得```P=[0, 4, 9, 9, 7, 4, 5]```
于是同余的有：
余数为```0```的有```P[0] P[6]```  排列数为```C(2,2)=1```
余数为```4```的有```P[1] P[2] P[3] P[5]``` 排列数为```C(4, 2)=6```
余数为```2```的有````P[4]C(1,2) = 0```
所以结果为```1+6=7```

#### 1.3 实现

    public class Solution {
        public int subarraysDivByK(int[] A, int K) {
            int n = A.length;
            int[] P = new int[n+1];
            for (int i = 0; i < n; i++){
                P[i+1] = P[i] + A[i];
            }

            int[] C = new int[K];
            for (int x : P){
                // 这里两次取模原因是java的取模可能返回负数
                C[(x % K + K) % K]++;
            }

            int ans = 0;
            for (int c : C){
                ans += c * (c-1) / 2;
            }
            return ans;
        }
    }

### 2 连续子数组最大乘积

#### 2.1 题目

Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

Example 1:

    Input: [2,3,-2,4]
    Output: 6
    Explanation: [2,3] has the largest product 6.
    Example 2:

    Input: [-2,0,-1]
    Output: 0
    Explanation: The result cannot be 2, because [-2,-1] is not a subarray.

#### 2.2 思路


#### 2.3 实现

### 3 子数组累加和为K的个数

#### 3.1 题目

Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.

Example 1:
    Input:nums = [1,1,1], k = 2
    Output: 2
    Note:
    The length of the array is in range [1, 20,000].
    The range of numbers in the array is [-1000, 1000] and the range of the integer k is [-1e7, 1e7].

#### 3.2 思路

#### 3.3 实现

### 4 最小子数组和大于给定值

#### 4.1 题目

Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.

Example:

    Input: s = 7, nums = [2,3,1,2,4,3]
    Output: 2
    Explanation: the subarray [4,3] has the minimal length under the problem constraint.
    Follow up:
    If you have figured out the O(n) solution, try coding another solution of which the time complexity is O(n log n).

#### 4.2 思路

#### 4.3 实现

### 5 乘积小于K的连续子数组个数

#### 5.1 题目

Your are given an array of positive integers nums.

Count and print the number of (contiguous) subarrays where the product of all the elements in the subarray is less than k.

Example 1:

    Input: nums = [10, 5, 2, 6], k = 100
    Output: 8
    Explanation: The 8 subarrays that have product less than 100 are: [10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6].
    Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.

Note:

    0 < nums.length <= 50000.
    0 < nums[i] < 1000.
    0 <= k < 10^6.

#### 5.2 思路

#### 5.3 实现
