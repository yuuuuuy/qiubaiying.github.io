---
layout:     post
title:      LeetCode-Alg-536-Binary-Tree-Tilt
subtitle:   计算二叉树的坡度
date:       2019-03-08
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a binary tree, return the tilt of the whole tree.

The tilt of a tree node is defined as the absolute difference between the sum of all left subtree node values and the sum of all right subtree node values. Null node has tilt 0.

The tilt of the whole tree is defined as the sum of all nodes' tilt.

    Example:
    Input:
             1
           /   \
          2     3
    Output: 1
    Explanation:
    Tilt of node 2 : 0
    Tilt of node 3 : 0
    Tilt of node 1 : |2-3| = 1
    Tilt of binary tree : 0 + 0 + 1 = 1
Note:

The sum of node values in any subtree won't exceed the range of 32-bit integer.
All the tilt values won't exceed the range of 32-bit integer.

### 2、题意

这题的意思是每一个节点的坡度等于其左子树与右子树和的绝对值，树的坡度等于每一个节点的坡度之和，所以这里需要计算每一个节点
的坡度并求和，但是在计算每一个节点的坡度时需要计算其左子树值与右子树值。

### 3、思路

    由于没计算一个节点的坡度时需要计算其左子树和右子树的值，所以可以从下往上计算，后序遍历是先左孩子、右孩子再根，所以使用后序遍历来
    计算根节点的坡度，然后把该节点值+左子树值+右子树值就等于该节点双亲节点的左子树值。

### 4、递归实现

    class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;
        TreeNode(int x) { val = x; }
    }

    public class Solution{
        // 定义一个私有成员变量来存放每个节点的坡度
        private int res = 0;

        public int findTilt(TreeNode root) {
            postOrderTravels(root);
            return this.res;
         }

        // 获取遍历计算子树值
        public int postOrderTravels(TreeNode root)
        {
            if (root == null)
            {
                return 0;
            }

            int leftSum = postOrderTravels(root.left);
            int rightSum = postOrderTravels(root.right);

            this.res += Math.abs(leftSum - rightSum);

            return leftSum + rightSum + root.val;
        }
    }
