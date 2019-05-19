---
layout:     post
title:      LeetCode-Alg-897-Increasing Order Search Tree
subtitle:   LeetCode-Alg-897-Increasing Order Search Tree
date:       2019-05-19
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a tree, rearrange the tree in in-order so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only 1 right child.

    Example 1:
    Input: [5,3,6,2,4,null,8,1,null,null,null,7,9]

           5
          / \
        3    6
       / \    \
      2   4    8
     /        / \
    1        7   9

    Output: [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]

     1
      \
       2
        \
         3
          \
           4
            \
             5
              \
               6
                \
                 7
                  \
                   8
                    \
                     9  
Note:

    The number of nodes in the given tree will be between 1 and 100.
    Each node will have a unique integer value from 0 to 1000.

### 2、思路

这道题意思是对一棵树进行重新排列，将最左边的节点作为树的根节点，每一个非叶子节点只有右孩子，没有左孩子，节点的值不需要按照大小排序，于是想到直接在树上做连接，将每个节点的左孩子全部断开置为null。

### 3、实现

#### 3.1 栈实现中序非递归遍历

    public class Solution {
        public TreeNode increasingBST(TreeNode root) {
            Stack<TreeNode> stack = new Stack<>();
            // 根节点
            TreeNode rootNode = null;
            // 保存前一个节点
            TreeNode preNode = null;
            boolean isRoot = true;

            while (root != null || !stack.isEmpty()){
                while (root != null){
                    stack.push(root);
                    if (root.left == null && isRoot){
                        rootNode = root;
                        preNode = root;
                    }
                    root = root.left;
                }

                root = stack.pop();
                if (isRoot){
                    root = root.right;
                    isRoot = false;
                    continue;
                }
                // 左子树全部置为null
                root.left = null;
                preNode.right = root;
                preNode = root;
                root = root.right;
            }

            return rootNode;
        }
    }

#### 3.2 递归中序遍历

    public class Solution {
        // 保存前一个节点
        private TreeNode current;

        public TreeNode increasingBST3(TreeNode root){
            TreeNode ans = new TreeNode(0);
            current = ans;
            inOrder(root);
            return ans.right;
        }
        public void inOrder(TreeNode root){
            if (root == null){
                return;
            }
            inOrder(root.left);
            root.left = null;
            current.right = root;
            current = root;
            inOrder(root.right);
        }
    }
