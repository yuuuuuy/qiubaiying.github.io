---
layout:     post
title:      LeetCode-Alg-530-Minimum Absolute Difference in BST
subtitle:   LeetCode-Alg-530-Minimum Absolute Difference in BST
date:       2019-05-08
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a binary search tree with non-negative values, find the minimum absolute difference between values of any two nodes.

    Example:

    Input:

       1
        \
         3
        /
       2

    Output:
    1

    Explanation:
    The minimum absolute difference is 1, which is the difference between 2 and 1 (or between 2 and 3).


Note: There are at least two nodes in this BST.

### 2、思路

对于这道题，所给的条件是一棵二叉搜索树，二叉搜索树满足左孩子小于根节点，右孩子大于根节点，其子树也满足该条件，所以对于一个二叉搜索树与根节点最接近的只可能在左子树的最靠右的叶子节点或
右子树的最靠左的叶子节点，于是以每一个节点作为根节点查找最小差值。

### 4、实现

    public  class Solution {
        private int min = Integer.MAX_VALUE;

        public int getMinimumDifference(TreeNode root) {
            if (root == null || (root.left == null && root.right == null)){
                return -1;
            }

            int minDiff = Integer.MAX_VALUE;

            min = Math.min(getMinDiff(root), min);
            getMinimumDifference(root.left);
            getMinimumDifference(root.right);

            return min;
        }

        public int getMinDiff(TreeNode root){
            if (root == null){
                return Integer.MAX_VALUE;
            }
            int leftDiff = Integer.MAX_VALUE;
            int rightDiff = Integer.MAX_VALUE;
            int minDiff = Integer.MAX_VALUE;

            if (root.left != null){
                leftDiff = root.val - getNode(root.left).val;
            }
            if (root.right != null) {
                rightDiff = getNode2(root.right).val - root.val;
            }

            minDiff = Math.min(minDiff, leftDiff);
            minDiff = Math.min(minDiff, rightDiff);

            return minDiff;
        }


        // 给一个根节点，去左子树找最接近根的节点
        private TreeNode getNode(TreeNode root){
            if (root.right == null){
                return root;
            }
            return getNode(root.right);
        }

        private TreeNode getNode2(TreeNode root){
            if (root.left == null){
                return root;
            }
            return getNode2(root.left);
        }
    }

这样做部分节点被重复访问，带来了不必要的耗时，于是想到二叉搜索树的中序遍历是有序的，那么对于有序的数列彼此之间的距离也是小的，这样就很容易计算树中任意两个节点之间的最小绝对值了。

### 5、优化

    public  class Solution {
        private int min = Integer.MAX_VALUE;
        // 保存中序遍历中上一个节点的值
        private Integer preValue;

        public int getMinimumDifference(TreeNode root){
            inOrder(root);
            return min;
        }

        private void inOrder(TreeNode root){
            if (root == null){
                return;
            }
            inOrder(root.left);
            if (preValue != null){
                min = Math.min(min, root.val - preValue);
            }
            preValue = root.val;
            inOrder(root.right);
        }
    }
