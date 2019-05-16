---
layout:     post
title:      LeetCode-Alg-669-Trim a Binary Search Tree
subtitle:   LeetCode-Alg-669-Trim a Binary Search Tree
date:       2019-05-16
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a binary search tree and the lowest and highest boundaries as L and R, trim the tree so that all its elements lies in [L, R] (R >= L). You might need to change the root of the tree, so the result should return the new root of the trimmed binary search tree.

    Example 1:
    Input:
        1
       / \
      0   2

      L = 1
      R = 2

    Output:
        1
          \
           2
    Example 2:
    Input:
        3
       / \
      0   4
       \
        2
       /
      1

      L = 1
      R = 3

    Output:
          3
         /
       2   
      /
     1

### 2、思路

这道题的题意是对二叉搜索树进行裁剪，保证每一个节点的值落在去年[L, R]中，由于二叉搜索树的性质可以知道，左子树的值小于根节点，右子树的值都大于根节点，所以如果对当前节点进行判断，
如果当前节点的值满足区间的要求，则递归的对左子树和右子树进行裁剪，如果当前节点的值落在了L的左边，说明该节点需要裁剪掉，那么只有其右子树的节点可能会落到去间中，则去右子树裁剪；
如果当前节点的值麻落在了R的右边，说明该节点需要裁剪掉，那么只有其左子树上的节点可能会落到去间中，则去右子树裁剪。

### 3、实现

    public class Solution {

        public TreeNode trimBST(TreeNode root, int L, int R) {
            if (root == null){
                return null;
            }

            if (root.val >= L && root.val <= R){
                root.left = trimBST(root.left, L, R);
                root.right = trimBST(root.right, L, R);
            }
            if (root.val < L){
                root = trimBST(root.right, L, R);
            }else if (root.val > R){
                root = trimBST(root.left, L, R);
            }

            return root;
        }
    }
    
