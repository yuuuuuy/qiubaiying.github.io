---
layout:     post
title:      LeetCode-Alg-235-Lowest Common Ancestor of a Binary Search Tree
subtitle:   LeetCode-Alg-235-Lowest Common Ancestor of a Binary Search Tree
date:       2019-05-05
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as
the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

Given binary search tree:  root = [6,2,8,0,4,7,9,null,null,3,5]




    Example 1:

    Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
    Output: 6
    Explanation: The LCA of nodes 2 and 8 is 6.
    Example 2:

    Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
    Output: 2
    Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.


 Note:

All of the nodes' values will be unique.
p and q are different and both values will exist in the BST.

### 2、思路

这道题是求二叉搜索树中给定两个节点，寻找其最小公共祖先，由于是二叉搜索树，根节点的左孩子小于右孩子，如果待寻找的两个节点分别位于某个节点的两侧，那么该节点一定是两节点的最小公共祖先，
否则根据最小节点与当前节点值比较决定去左子树还是去右子树递归寻找。

### 4、实现

    public class Solution {

        public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
            if (p.val > q.val){
                return lowestCommonAncestor(root, q, p);
            }

            if (root == null){
                return null;
            }

            if ((p.val <= root.val && q.val > root.val) || (p.val < root.val && q.val >= root.val)){
                return root;
            }else if(p.val < root.val){
                return lowestCommonAncestor(root.left, p, q);
            }else{
                return lowestCommonAncestor(root.right, p, q);
            }
        }
    }
