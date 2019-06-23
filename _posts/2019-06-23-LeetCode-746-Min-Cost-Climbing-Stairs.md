---
layout:     post
title:      LeetCode-746-Min Cost Climbing Stairs
subtitle:   LeetCode-746-Min Cost Climbing Stairs
date:       2019-06-23
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

 On a staircase, the i-th step has some non-negative cost cost[i] assigned (0 indexed).
 Once you pay the cost, you can either climb one or two steps. You need to find minimum cost to reach the top of the floor,

 and you can either start from the step with index 0, or the step with index 1.

 Example 1:

     Input: cost = [10, 15, 20]
     Output: 15
     Explanation: Cheapest is start on cost[1], pay that cost and go to the top.
     Example 2:
     Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
     Output: 6
     Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].

 Note:

     cost will have a length in the range [2, 1000].
     Every cost[i] will be an integer in the range [0, 999].



### 2、思路

对于这道题，当我们已经上到了顶层i,对于前一步，我们可能从i-1层直接上来，或者从i-2层上来，

我们定义函数```F[x]```表示上到x层时的最小代价，于是此时的代价最小为```F[i] = min{F[i-1] + F[i-2]} + cost[i]```,这就是我们的转移方程;

初始条件为```F[0] = cost[0] F[1] = cost[1]```，因为题中给出可以从地面开始或者从第一层开始；
我们从第二层开始计算上到每一层的代价：

### 3、实现

    public class Solution {
        public static void main(String[] args){
            int[] cost = {10, 15, 20};
            System.out.println(new Solution().minCostClimbingStairs(cost));
        }

        // 1、确定状态，一般选区最后一步，当走到顶层i时，可能又i-1层直接上来也可能由i-2层上来
        // 2、转移方程 f[i] = min(f[i-1], f[i-2]) + cost[i]
        // 3、初始条件 f[0] = cost[0] f[1] = cost[1]
        // 4、计算顺序 从2层开始，因为一层和地面的代价都是0
        public int minCostClimbingStairs(int[] cost) {
            int len = cost.length;
            int[] dp = new int[len];
            dp[0] = cost[0];
            dp[1] = cost[1];
            for (int i = 2; i < len; i++) {
                dp[i] = Math.min(dp[i - 1]+ cost[i], dp[i - 2] + cost[i]);
            }
            return Math.min(dp[len - 1], dp[len - 2]);
        }
    }
