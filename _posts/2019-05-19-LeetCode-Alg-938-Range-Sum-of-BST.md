---
layout:     post
title:      LeetCode-Alg-938-Range Sum of BST
subtitle:   LeetCode-Alg-938-Range Sum of BST
date:       2019-05-19
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given the root node of a binary search tree, return the sum of values of all nodes with value between L and R (inclusive).

The binary search tree is guaranteed to have unique values.



    Example 1:

    Input: root = [10,5,15,3,7,null,18], L = 7, R = 15
    Output: 32
    Example 2:

    Input: root = [10,5,15,3,7,13,18,1,null,6], L = 6, R = 10
    Output: 23


Note:

    The number of nodes in the tree is at most 10000.
    The final answer is guaranteed to be less than 2^31.

### 2、思路

对于一个根节点，可能落在区间内、区间左边和区间右边，所以可以递归计算，满足区间范围的节点和。

### 3、实现

public class Solution {

    public int rangeSumBST(TreeNode root, int L, int R) {
        if (root == null){
            return 0;
        }

        int sum = 0;
        if (root.val >= L && root.val <= R){
            // 递归计算左子树+右子树和
            sum = root.val + rangeSumBST(root.left, L, R) + rangeSumBST(root.right, L, R);
        }else if (root.val < L){
            // 左子树不可能存在满足区间的节点
            sum = rangeSumBST(root.right, L, R);
        }else if (root.val > R){
            // 右子树上不可能存在满足区间的节点
            sum = rangeSumBST(root.left, L, R);
        }
        return sum;
    }
}
