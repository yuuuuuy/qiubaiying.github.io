---
layout:     post
title:      LeetCode-Alg-226-Invert-Binary-tree
subtitle:   LeetCode-Alg-226-Invert-Binary-tree
date:       2019-02-27
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Invert a binary tree.

  Example:

  Input:

           4
         /   \
        2     7
       / \   / \
      1   3 6   9
  Output:

           4
         /   \
        7     2
       / \   / \
      9   6 3   1
  Trivia:
  This problem was inspired by this original tweet by Max Howell:

### 2、思路

    如果该节点不是叶子节点，就交换该节点的左右节点，然后递归反转左子树和右子树

### 3、递归实现

    public TreeNode invertTree(TreeNode root) {
       if (root == null)
       {
           return root;
       }
       if (root.left != null || root.right != null)
       {
           TreeNode node = new TreeNode(0);
           node = root.left;
           root.left = root.right;
           root.right = node;
           // 反转左子树
           if (root.left != null)
           {
               invertTree(root.left);
           }
           // 反转右子树
           if (root.right != null)
           {
               invertTree(root.right);
           }
       }
       
       return root;
    }
