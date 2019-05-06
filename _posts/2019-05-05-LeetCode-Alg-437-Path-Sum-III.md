---
layout:     post
title:      LeetCode-Alg-437-Path Sum III
subtitle:   LeetCode-Alg-437-Path Sum III
date:       2019-05-06
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

    Example:

    root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

          10
         /  \
        5   -3
       / \    \
      3   2   11
     / \   \
    3  -2   1

    Return 3. The paths that sum to 8 are:

    1.  5 -> 3
    2.  5 -> 2 -> 1
    3. -3 -> 11

### 2、思路

这道题，最可能想到的就是分别以每一个节点为根节点往下遍历到叶子节点并计算路径和，只要路径和满足目标值就将计数器加一，对于遍历所有的路径
很明显使用深度优先遍历，但是这样做会有些节点会被重复访问多次。

### 4、实现

public class Solution{
    private int count=0;
    public int pathSum(TreeNode root, int sum) {

        findPath(root, sum);
        return count;
    }

    // sum为路径和
    private void findPath(TreeNode root, int sum){
        if (root == null){
            return;
        }
        // 以当前节点为根节点往下寻找
        bfs(root, 0, sum);
        // 去左子树继续寻找
        findPath(root.left, sum);
        // 去右子树继续寻找
        findPath(root.right, sum);
    }

    // 深度优先遍历，并计算每次经过的路径的和
    public void bfs(TreeNode root, int sum, int target){
        if (root == null){
            return;
        }        
        sum += root.val;
        if (sum == target){
            count++;
        }
        bfs(root.left, sum, target);
        bfs(root.right, sum, target);
        sum -= root.val;
    }
}

### 5、优化
