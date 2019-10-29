---
layout:     post
title:      LeetCode-1035. Uncrossed Lines
subtitle:   LeetCode-1035. Uncrossed Lines
date:       2019-10-29
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

We write the integers of A and B (in the order they are given) on two separate horizontal lines.

Now, we may draw connecting lines: a straight line connecting two numbers A[i] and B[j] such that:

A[i] == B[j];
The line we draw does not intersect any other connecting (non-horizontal) line.
Note that a connecting lines cannot intersect even at the endpoints: each number can only belong to one connecting line.

Return the maximum number of connecting lines we can draw in this way.



Example 1:


    Input: A = [1,4,2], B = [1,2,4]
    Output: 2
    Explanation: We can draw 2 uncrossed lines as in the diagram.
    We cannot draw 3 uncrossed lines, because the line from A[1]=4 to B[2]=4 will intersect the line from A[2]=2 to B[1]=2.

Example 2:

    Input: A = [2,5,1,2,5], B = [10,5,2,1,5,2]
    Output: 3
    Example 3:

    Input: A = [1,3,7,1,7,5], B = [1,9,2,5,1]
    Output: 2


Note:

    1 <= A.length <= 500
    1 <= B.length <= 500
    1 <= A[i], B[i] <= 2000

### 2、思路

该题意为将上下两行数组中相等元素使用不交叉的直线相连，问最多有多少条这样的直线？这道题我们可使用动态规划的思想求解：

**确定状态:**

题目要求数组A[a1,a2...ai]和数组B[b1,b2..bj]最多有多少条不交叉的连线，我们假设最优策略中ai==bj,也就是ai和bj相连,此时转化成子问题: 求数组A[a1,a2...ai-1]和数组B[b1,b2...bj-1]最多有多少条不交叉的连线，所以我们定义dp[i][j]表示数组A[a1,a2...ai]和数组B[b1,b2...bj]最多有多少条不交叉的连线。

**转移方程:**

设状态dp[i][j]=数组A[a1..ai]和数组B[b1...bj]最多有多少条不交叉的连线，对于任意的i,j都有：

    dp[i][j] = dp[i-1][j-1]+1 当A[i]=B[j]时
    dp[i][j] = max{dp[i-1][j], dp[i][j-1]} 当A[i] != B[j]时

**初始条件和边界情况**

    dp[0][0] = 0
    dp[i][j]就是最后结果
    算法时间复杂度为O(mn)m n分别为数组A和B的长度
    空间复杂度为O(mn)

### 3 实现

    public class Solution {
        public int maxUncrossedLines(int[] A, int[] B) {
            int m = A.length;
            int n = B.length;
            int[][] dp = new int[m+1][n+1];

            // 初始化
            dp[0][0] = 0;
            for (int i = 1; i <= m; i++){
                for (int j = 1; j <= n; j++){
                    if (A[i-1] == B[j-1]){
                        dp[i][j] = dp[i-1][j-1] + 1;
                    }else{
                        dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
                    }
                }
            }
            return dp[m][n];
        }
    }
