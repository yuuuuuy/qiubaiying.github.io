---
layout:     post
title:      LeetCode-931. Minimum Falling Path Sum
subtitle:   LeetCode-931. Minimum Falling Path Sum
date:       2019-06-30
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a square array of integers A, we want the minimum sum of a falling path through A.

A falling path starts at any element in the first row, and chooses one element from each row.  The next row's choice must be in a column that is different from the previous row's column by at most one.



Example 1:

    Input: [[1,2,3],[4,5,6],[7,8,9]]
    Output: 12
    Explanation:
    The possible falling paths are:
    [1,4,7], [1,4,8], [1,5,7], [1,5,8], [1,5,9]
    [2,4,7], [2,4,8], [2,5,7], [2,5,8], [2,5,9], [2,6,8], [2,6,9]
    [3,5,7], [3,5,8], [3,5,9], [3,6,8], [3,6,9]
    The falling path with the smallest sum is [1,4,7], so the answer is 12.



Note:

    1 <= A.length == A[0].length <= 100
    -100 <= A[i][j] <= 100


### 2、思路

这道题是一道坐标型动态规划，我们可以按照下面四步来分析：

#### 2.1、确定状态

我们枚举所有的下降路径中，对于```mxn```的网格，每一种可能的结果```[a1, a2, a3... ak]```中有```m```个元素，因为需要分别从每一行中选取一个元素,

设最优策略中最后一位数字是```a[i][j]```(位于```m```行)，这个```a[i][j]```可能在行首、行尾或行中间;

**前提是列数大于1**

    如果a[i][j]位于行首，其前一行的元素只能选m-1行的 min{a[i-1][j], a[i-1][j+1]}
    如果a[i][j]位于行中间，其前一行的元素只能选m-1行的 min{a[i-1][j], a[i-1][j-1], [i-1][j+1]}
    如果a[i][j]位于行尾，其前一行的元素只能选m-1行的 min{a[i-1][j],[i-1][j-1]}

设状态```f[i][j]=```以元素```a[i][j]```结尾的最小下降路径和

##### 2.2、 状态方程

**列数大于1**

                        min{a[i-1][j], a[i-1][j+1]}     元素a[i][j]位于行首
    状态方程为 f[i][j] = min{a[i-1][j], a[i-1][j-1], [i-1][j+1]} 元素a[i][j]位于行中间
                        min{a[i-1][j],[i-1][j-1]}       元素a[i][j]位于行尾

如果列数为```1```，则只能从上往下走，最短下降路径的和就是累加结果.

#### 2.3、 初始条件和边界情况

对于第一行，最小下降路径和就是其自身

    f[0][0] = A[0][0]
    f[0][1] = A[0][1]
    ...
    f[0][n-1] = A[0][n-1]

#### 2.4、 计算顺序

计算顺序从：```f[0][0], f[0][1],,, f[m-1][n-1]```

最后的最小路径和就是最后一行所有元素中取最小值

时间复杂度为```O(MN)```

空间复杂度为```O(MN)```

### 3、实现

    public int minFallingPathSum(int[][] A) {
        int n = A.length;
        int m = A[0].length;

        int[][] f = new int[n][m];
        for (int i = 0; i < n; i++){
            for (int j = 0; j < m; j++){
                // 初始化
                if (i == 0){
                    f[i][j] = A[i][j];
                }else{
                    // 只有一列的情况
                    if (m == 1){
                        f[i][j] = A[i][j] + f[i-1][j];
                        continue;
                    }
                    // 第一列
                    if (j == 0){
                        f[i][j] = A[i][j] + Math.min(f[i-1][j], f[i-1][j+1]);
                    }else if (j == m-1){ // 最后一列
                        f[i][j] = A[i][j] + Math.min(f[i-1][j], f[i-1][j-1]);
                    }else{ // 非边界
                        f[i][j] = A[i][j] + Math.min(Math.min(f[i-1][j], f[i-1][j-1]), f[i-1][j+1]);
                    }
                }
            }
        }
        // 最优解在最后一行产生
        int ans = Integer.MAX_VALUE;
        for (int i = 0; i < m; i++){
            ans = Math.min(ans, f[n-1][i]);
        }
        return ans;
    }

### 4 、优化

从转移方程可以看出，在计算```f[i][j]```时只用到前一行的元素，所以我们在开数组时不需要开```mxn```的数组，只需要开```2xn```的数组，对于不再需要的数据直接
擦除掉，则数组变成了滚动数组，这样就把空间复杂度变成```O(n)```

### 5、实现

    public int minFallingPathSum(int[][] A) {
            int n = A.length;
            int m = A[0].length;

            // 开一个两行m列的数组
            int[][] f = new int[2][m];
            // 变量old表示前一行 now表示当前行
            int old = 1;
            int now = 0;

            for (int i = 0; i < n; i++){
                // 每次换行需要改变当前行和前一行下标
                old = now;
                now = 1 - now;
                for (int j = 0; j < m; j++){
                    if (i == 0){
                        f[now][j] = A[i][j];
                        continue;
                    }
                    if (m == 1){
                        f[now][j] = A[i][j] + f[old][j];
                    }else{
                        if (j == 0){
                            f[now][j] = A[i][j] + Math.min(f[old][j], f[old][j+1]);
                        }else if (j == m-1){
                            f[now][j] = A[i][j] + Math.min(f[old][j], f[old][j-1]);
                        }else{
                            f[now][j] = A[i][j] + Math.min(Math.min(f[old][j], f[old][j-1]), f[old][j+1]);
                        }
                    }
                }
            }
            int ans = Integer.MAX_VALUE;
            for (int i = 0; i < m; i++){
                ans = Math.min(ans, f[now][i]);
            }
            return ans;
        }
