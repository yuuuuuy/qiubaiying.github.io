---
layout:     post
title:      LeetCode-Bomb Enemy
subtitle:   LeetCode-Bomb Enemy
date:       2019-06-30
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

有一个```M*N```的网格，每一个格子可能为空，可能有一个敌人，可能有一堵墙,空格子使用```'0'```表示，敌人用```'E'```表示，墙使用```'W'```表示.
只能在某个空格子里放一个炸弹，炸弹会炸死网格所在同行同列的敌人，但是不能穿透墙，
最多能炸死多人敌人？

### 2、思路

这是一道坐标型动态规划的题，我们可以按照下面四步来分析：

#### 2.1、确定状态

首先分析一个方向(向上)

假设在网格的每一个格子都可以放置炸弹:

如果放到有敌人的格子里，格子里的敌人会被炸死并继续往四个方向炸，

如果放到有墙的格子里，炸弹不会炸死任何敌人

在(i, j)放炸弹

        当(i, j)为空地，炸死的敌人数为(i-1, j)格向上炸死的敌人数；
        当(i, j)有敌人，炸死的敌人数为(i-1, j)格向上炸死的敌人数+1；
        当(i, j)为墙, 炸死的敌人数为0；

原问题：

    (i, j)格子放炸弹向上能炸死的敌人数

子问题：

    (i-1, j)格子放炸弹向上能炸死的敌人数

状态：

    Up[i][j]表示(i, j)格放炸弹向上能炸死的敌人数

#### 2.2、转移方程

                 Up[i-1][j]    格子里没有敌人
    Up[i][j] =   Up[i-1][j]+1  格子里有敌人
                 0             格子里是墙

#### 2.3、 初始条件和边界情况

第0行的Up值与格子相关：

    如果格子里是敌人 Up[0][j]=1
    如果格子里不是敌人 Up[0][j]=0

#### 2.4、 计算顺序

向上炸的计算顺序从：```f[0][0], f[0][1],,, f[m-1][n-1]```

分别计算四个方向上能炸死的敌人数，结果 Up[i][j]+Down[i][j]+Left[i][j]+Right[i][j] (i, j)是空地

时间复杂度为```O(MN)```

空间复杂度为```O(MN)```

### 3、实现

    public int bombEnemy(char[][] A){
        if (A == null || A.length == 0 || A[0].length == 0){
            return 0;
        }
        int m = A.length;
        int n = A[0].length;

        // Up Down Left Right
        int[][] f = new int[m][n];
        int[][] res = new int[m][n];

        for (int i = 0; i < m; i++){
            for (int j = 0; j < n; j++){
                // 代表有多少敌人被炸死
                res[i][j] = 0;
            }
        }

        // UP
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (A[i][j] == 'W'){
                    f[i][j] = 0;
                }else{
                    f[i][j] = 0;
                    // 如果是敌人
                    if (A[i][j] == 'E'){
                        f[i][j] = 1;
                    }
                    // 空地
                    if (i > 0){
                        f[i][j] += f[i-1][j];
                    }
                }
                res[i][j] += f[i][j];
            }
        }

        // DOWN
        for(int i = m-1; i>=0; i--){
            for(int j= 0; j < n; j++){
                if (A[i][j] == 'W'){
                    f[i][j] = 0;
                }else{
                    f[i][j] = 0;
                    if (A[i][j] == 'E'){
                        f[i][j] = 1;
                    }
                    if (i + 1 < m){
                        f[i][j] += f[i+1][j];
                    }
                }
                res[i][j] += f[i][j];
            }
        }

        // LEFT
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (A[i][j] == 'W'){
                    f[i][j] = 0;
                }else{
                    f[i][j] = 0;
                    // 如果是敌人
                    if (A[i][j] == 'E'){
                        f[i][j] = 1;
                    }
                    // 空地
                    if (j > 0){
                        f[i][j] += f[i][j-1];
                    }
                }
                res[i][j] += f[i][j];
            }
        }

        // RIGHT
        for (int i = 0; i < m; i++) {
            for (int j = n-1; j >= 0; j--) {
                if (A[i][j] == 'W'){
                    f[i][j] = 0;
                }else{
                    f[i][j] = 0;
                    // 如果是敌人
                    if (A[i][j] == 'E'){
                        f[i][j] = 1;
                    }
                    // 空地
                    if (j + 1 < n){
                        f[i][j] += f[i][j+1];
                    }
                }
                res[i][j] += f[i][j];
            }
        }

        int ans = 0;
        for (int i = 0; i < m; i++){
            for (int j = 0; j < n; j++){
                if (A[i][j] == '0'){
                    ans = Math.max(ans, res[i][j]);
                }
            }
        }

        return ans;
    }
