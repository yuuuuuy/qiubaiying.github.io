---
layout:     post
title:      LeetCode-Alg-112-Path-Sum
subtitle:   LeetCode-Alg-112-Path-Sum
date:       2019-02-27
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

 Note: A leaf is a node with no children.

  Example:

  Given the below binary tree and sum = 22,

            5
           / \
          4   8
         /   / \
        11  13  4
       /  \      \
      7    2      1
  return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

### 2、思路

    题目中让我们判断从根节点到叶子节点是否存在一条路径的和等于```sum```，我们可以认为一棵树是又一个树根和若干子树构成，我们用```sum-root.val```的数值，
    再递归判断该根节点的左子树和右子树是否有满足```sum=sum-root.val```的路径.

### 3、递归实现

    public boolean hasPathSum(TreeNode root, int sum) {
             if (root == null)
             {
                 return false;
             }
             // 如果是叶子节点直接返回
             if (root.left==null && root.right==null)
             {
                 return root.val == sum;
             }

             boolean res = true;
             // 判断左子树是否满足
            if (root.left != null)
            {
                // 如果是左叶子节点
                if (root.left.left == null && root.left.right == null )
                {
                    if ((sum - root.val) == root.left.val)
                    {
                        return true;
                    }
                    res = false;
                }
                // 非叶子节点
                else{
                    res = hasPathSum(root.left, sum-root.val);
                }

                if (res)
                {
                    return res;
                }
            }
            // 如果左子树不满足再判断右子树是否满足
            if (root.right != null)
            {
                // 如果是右叶子节点
                if (root.right.left == null && root.right.right == null)
                {
                    if ((sum - root.val) == root.right.val)
                    {
                        return true;
                    }
                    res = false;
                }else
                {
                    res =  hasPathSum(root.right, sum-root.val);
                }

                if (res)
                {
                    return res;
                }
            }
            return res;
        }
