---
layout:     post
title:      LeetCode-1053. Previous Permutation With One Swap
subtitle:   LeetCode-1053. Previous Permutation With One Swap
date:       2019-11-16
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given an array A of positive integers (not necessarily distinct), return the lexicographically largest permutation that is smaller than A, that can be made with one swap (A swap exchanges the positions of two numbers A[i] and A[j]).  If it cannot be done, then return the same array.



Example 1:

  Input: [3,2,1]
  Output: [3,1,2]
  Explanation: Swapping 2 and 1.
  Example 2:

  Input: [1,1,5]
  Output: [1,1,5]
  Explanation: This is already the smallest permutation.
  Example 3:

  Input: [1,9,4,6,7]
  Output: [1,7,4,6,9]
  Explanation: Swapping 9 and 7.
  Example 4:

  Input: [3,1,1,3]
  Output: [1,3,1,3]
  Explanation: Swapping 1 and 3.


Note:

  1 <= A.length <= 10000
  1 <= A[i] <= 10000

### 2、思路

拿到这道题，首先分析题意，题目给了我们一个包含正整数的数组，让我们在数组中进行一次数字交换，交换后的排列要比原排列字典序小，但是交换后的排序又是所有满足条件的序列中字典序最大的，于是我们分析：
对于一个数来说，如果让我们交换数字中的两个数让交换的数比原数小，最直接的就是把最高位上的数减小，但是还要满足交换后的数是比原数小的最大的数，那么就不能从最高位开始交换，而是从最低位改变，这样对于原数的影响才最小。

比如```6534```，我们从后往前判断，找一个数（满足该数后存在比其小的数），发现在```5```上改变对原数影响最小，只需要从```5```后面的数中选一个比```5```小的数把```5```换掉即可，但是```5```后面可能有很多比```5```小的数字，选取哪一个数字来和```5```交换呢？从贪心来看我们肯定想找一个比5小一点点的数字，这样对原数减小的程度才最小，也就是小于```5```的所数中最大的那个数，这里是```4```。

于是可以把算法分成三个步骤：
- 步骤1： 首先从后往前找到第一个满足比其后数字大的数，如果不存在，说明该数无法通过交换来减小，算法结束，否则转步骤2；
- 步骤2: 从步骤1中找到的数往后找比该数小的最大数位置；
- 步骤3: 交换步骤1和步骤2找的两个索引上对应的数；

### 3 实现

    public class Solution {
        public int[] prevPermOpt1(int[] A) {
            int N = A.length;
            int s1 = -1;
            for (int i = N-2; i >= 0; i--){
                if (A[i] > A[i+1]){
                    s1 = i;
                    break;
                }
            }
            if (s1 == -1) return A;
            int s2 = s1 + 1;
            // 这里使用的是查找最大值次大值方法
            for (int j = s1 + 2; j < N; j++){
                if (A[j] < A[s1] && A[j] > A[s2]){
                    s2 = j;
                }
            }
            int t = A[s1];
            A[s1] = A[s2];
            A[s2] = t;

            return A;
        }
    }
