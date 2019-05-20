---
layout:     post
title:      LeetCode-Alg-1022-Sum of Root To Leaf Binary Numbers
subtitle:   LeetCode-Alg-1022-Sum of Root To Leaf Binary Numbers
date:       2019-05-20
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a binary tree, each node has value 0 or 1.  Each root-to-leaf path represents a binary number starting with the most significant bit.  For example, if the path is 0 -> 1 -> 1 -> 0 -> 1, then this could represent 01101 in binary, which is 13.

For all leaves in the tree, consider the numbers represented by the path from the root to that leaf.

Return the sum of these numbers.



    Example 1:



    Input: [1,0,1,0,1,0,1]
    Output: 22
    Explanation: (100) + (101) + (110) + (111) = 4 + 5 + 6 + 7 = 22


Note:

    The number of nodes in the tree is between 1 and 1000.
    node.val is 0 or 1.
    The answer will not exceed 2^31 - 1.

### 2、思路

这道题目是要求计算从根节点到每一个叶子节点路径组成的二进制数的和，路径可能存在部分重复，所以根节点的值在二进制中表示的值也需要随着变化，可以从根开始计算，每下一层对当前计算的值x2，再和当前节点值(0/1)相加，这里使用```<<```运算符向右移一位对原值x2，最低为是0.然后```|```当前节点的值来完成加法运算。

### 3、实现

public class Solution {
    public int sumRootToLeaf(TreeNode root){
        if (root == null){
            return 0;
        }

        return sumRootToLeaf(root, 0);
    }
    public int sumRootToLeaf(TreeNode root, int val){
        int num = val << 1;
        num = num | root.val;
        // 如果是叶子节点，直接返回此时计算的路径和
        if (root.left == null && root.right == null){
            return num;
        }
        int res = 0;
        if (root.left != null){
            res += sumRootToLeaf(root.left, num);
        }
        if (root.right != null){
            res += sumRootToLeaf(root.right, num);
        }
        return res;
    }
}
