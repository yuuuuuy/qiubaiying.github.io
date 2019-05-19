---
layout:     post
title:      LeetCode-Alg-687-Longest Univalue Path
subtitle:   LeetCode-Alg-687-Longest Univalue Path
date:       2019-05-17
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a binary tree, find the length of the longest path where each node in the path has the same value. This path may or may not pass through the root.

The length of path between two nodes is represented by the number of edges between them.



    Example 1:

    Input:

                  5
                 / \
                4   5
               / \   \
              1   1   5
    Output: 2



    Example 2:

    Input:

                  1
                 / \
                4   5
               / \   \
              4   4   5
    Output: 2



Note: The given binary tree has not more than 10000 nodes. The height of the tree is not more than 1000.

### 2、思路

#### 2.1 题意

这道题的题目要求翻译过来为：给你一个二叉树，找到最长的路径长度，路径中的每一个节点拥有相同的值，这个路径可能经过根节点，也可以不经过，两个节点之间的长度使用两个节点之间的边来表示。

#### 2.2 分析

对于这样的二叉树：

                  4
                 / \
                4   5
               / \   \
              4   4   5


我们可以找到如下图的三条路径：

![路径](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/Leetcode-687/demo.png)

从图中可以看出，如果选取一个节点，例如选区根节点(4)的左孩子(4),对于该节点来说，其左孩子和右孩子的值都和自己相同，未加入其父节点到路径中时，这个时候路径1的长度为2；

如果再将其根节点计算进去时，当前节点对其根节点上报的路径长度只能从左子树和右子树中选择最长的路径，当前该节点的左右子树的长度都是1，所以往其父节点(4)汇报时也是1。

这个时候我们定义一个全局变量ans，初始为0，用来保存当前树中最长的路径，对于节点既有左孩子又有右孩子时使用左右子树的长度和来更新ans，但是往其父节点汇报时只能返回最长的一棵子树。
还是以这棵树为例，还选取根节点(4)的左孩子(也是4)，该节点既有左子树又有右子树，且左子树和右子树的值都等于其本身，于是ans应该使用左右子树上的路径长度相加并加上当前节点1来更新ans，但是返回只能返回左右子树中最长的路径。

### 3、实现

    public class Solution {
       private int ans = 0;

       public int longestUnivaluePath(TreeNode root) {
           univaluePath(root);
           return ans;
       }

       public int univaluePath(TreeNode root){
           if (root == null){
               return 0;
           }

           int left = univaluePath(root.left);
           int right = univaluePath(root.right);

           int leftLen = 0;
           int rightLen = 0;
           if (root.left != null && root.left.val == root.val){
               leftLen += left + 1;
           }
           if (root.right != null && root.right.val == root.val){
               rightLen += right + 1;
           }

           ans = Math.max(ans, leftLen + rightLen);
           return Math.max(leftLen, rightLen);
       }
    }
