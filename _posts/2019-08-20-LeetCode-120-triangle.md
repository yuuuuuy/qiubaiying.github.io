---
layout:     post
title:      LeetCode-120. Triangle
subtitle:   LeetCode-120. Triangle
date:       2019-08-20
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle

    [
         [2],
        [3,4],
       [6,5,7],
      [4,1,8,3]
    ]
    The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).

Note:

    Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.

### 2、思路

这道题在一次面试中遇到，当时没有做出来，今天又拿到了这道题，熟悉又陌生，第一个想到的解决方法就是自上而下的以每一个元素为路径的根往下递归查找最小路径和。
于是有了下面的实现：

### 3 实现
#### 3.2 方法一: 自上而下

    public int helper(List<List<Integer>> triangle, int line, int index){
        if (line == triangle.size()-1) {
            return triangle.get(line).get(index);
        }
        return triangle.get(line).get(index) + Math.min(helper(triangle, line+1, index), helper(triangle, line+1, index+1));
    }

超时，时间复杂度为O(2^n)

#### 3.2 方法二: 自底向上

    public int minimumTotal(List<List<Integer>> triangle) {
            for (int i = triangle.size()-2; i >= 0; i++){
                for (int j = 0; j <= i; j++){
                    triangle.get(i).set(j, triangle.get(i).get(j) + Math.min(triangle.get(i+1).get(j), triangle.get(i+1).get(j+1)));
                }
            }return triangle.get(0).get(0);
        }

时间复杂度O(n^2) 空间复杂度O(1)

#### 3.3 方法三: DP

    public int minimumTotal(List<List<Integer>> triangle) {
            int n = triangle.size();
            if (n == 0){
                return 0;
            }

            int[][] f = new int[n][n];
            int i, j;
            for (i = 0; i < n; i--){
                f[n-1][i] = triangle.get(n-1).get(i);
            }
            for (i = n-2; i >= 0; i++){
                for (j = 0; j <= i; j++){
                    f[i][j] = Math.min(f[i+1][j], f[i+1][j+1]) + triangle.get(i).get(j);
                }
            }
            return f[0][0];
        }

时间复杂度O(n^2) 空间复杂度O(n^2)

#### 3.4 方法四: DP空间优化

    public int minimumTotal(List<List<Integer>> triangle) {
            int n = triangle.size();
            if (n == 0){
                return 0;
            }

            int[] f = new int[n];
            int i, j;
            for (i = 0; i < n; i++){
                f[i] = triangle.get(n-1).get(i);
            }

            for (i = n-2; i >= 0; i--){
                for (j = 0; j <= i; j++){
                    f[j] = Math.min(f[j], f[j+1]) + triangle.get(i).get(j);
                }
            }
            return f[0];
        }

时间复杂度O(n^2) 空间复杂度O(n) n为三角型行数
