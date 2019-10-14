---
layout:     post
title:      LeetCode-974. Subarray Sums Divisible by K
subtitle:   LeetCode-974. Subarray Sums Divisible by K
date:       2019-10-14
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

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

### 2、思路

这道题要求原数组所构成的连续子数组元素和能被```K```整除的所有子数组个数，我们定义```sum(i, j)```表示下标从```i```到```j-1```的子数组元素的和，则```sum(i, j) = sum(0, j) - sum(0, i)```,
所以我们知道了```sum(0, a)(1<=a<=N N为数组元素个数)```，任何一个子数组的和我们都可以利用```sum(i, j)=sum(0, j)-sum(0, i)```求出来.于是有以下推导

任意子数组和```sum(i, j)```能被```K```整除，即```sum(i, j) % K = 0 ==> [sum(0, j)-sum(0, i)] % K = 0 ==> sum(0, j) % K = sum(0, i) % K```,

于是我们只需要知道```sum(0, a)```中哪些和存在同余的关系，即```sum(0, x)```与```sum(0, y)```同余，关于同余的概念可以去数论中了解。

举例：```A = [4, 5, 0, -2, -3, 1]```

1). 我们首先计算```sum(0, a)```存到数组中得 ```P=[0, 4, 9, 9, 7, 4, 5]```

2). 然后按照同余关系进行计数得:

    与0同余的有(0, 5) => (P[0], P[6])

    与2同余的有(7) =>(P[4])

    与4同余的有(4, 9, 9, 4) =>(P[1], P[2], P[3], P[5])

3).对于同余的P[i]中，两两组合所得到的组合数相加就是要求的结果


### 3 实现

    public class Solution {
        public int subarraysDivByK(int[] A, int K) {
            int n = A.length;
            int[] P = new int[n+1];
            for (int i = 0; i < n; i++){
                P[i+1] = P[i] + A[i];
            }

            int[] C = new int[K];
            for (int x : P){
                C[(x % K + K) % K]++;
            }

            int ans = 0;
            for (int c : C){
                ans += c * (c-1) / 2;
            }
            return ans;
        }
    }
