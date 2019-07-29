---
layout:     post
title:      LeetCode-516. Longest Palindromic Subsequence
subtitle:   LeetCode-516. Longest Palindromic Subsequence
date:       2019-07-29
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is 1000.

    Example 1:
    Input:

    "bbbab"
    Output:
    4
    One possible longest palindromic subsequence is "bbbb".
    Example 2:
    Input:

    "cbbd"
    Output:
    2
    One possible longest palindromic subsequence is "bb".

### 2、思路

这道题我们使用动态规划的思想来求解

#### 2.1 确定状态

我们首先假设所给的字符串长度为```n```，由```[a0 a1 a2 ... an-1]``` ```n```个字符组成，在最优策略中，假设最长的回文子串的包含最后一个字符```an-1```,则最长回文子串从最短在子串```[ai.....an-1]```中产生
且字符```ai==an-1```，这里的子串```[ai ... an-1]```中字符顺序和原字符串相同，则子串```[ai .... an-1]```去掉首尾```ai```和```an-1```字符剩下的子串中的回文串在类似去掉回文串首尾元素情况下也是最长或等长的，
于是我们定义```f[i][j]```表示以元素```[ai ... aj]```为首尾的字符串中最长的回文串长度

#### 2.2 转移方程

如果```a[i]==a[j]```则```f[i][j] = 2 + f[i+1][j-1]```; 否则 ```f[i][j] = max{ f[i+1][j], f[i][j-1] }```

#### 2.3 初始条件和边界情况

```f[i][i] = 1```

答案为```f[i][j]```中最大值

时间复杂度为```O(n^2)```

空间负责度为```O(n^2)```

### 3、实现

    public int longestPalindromeSubseq(String s) {
        if (s == null || "".equals(s) || s.length() == 0){
            return 0;
        }
        char[] ss = s.toCharArray();
        int n = ss.length;

        int[][] f = new int[n][n];
        int i, j, len;
        int ans = 0;
        // 定义f[i][j]表示下标i-j的最长回文串长度
        for (len = 0; len < n; len++){
            for (i = 0; i+len < n; i++){
                j = i+len;
                if (len == 0){
                    f[i][j] = 1;
                }else{
                    if (ss[i] == ss[j]){
                        f[i][j] = 2 + f[i+1][j-1];
                    }else{
                        f[i][j] = Math.max(f[i][j-1], f[i+1][j]);
                    }
                }
                ans = Math.max(ans, f[i][j]);
            }
        }
        return ans;
    }
