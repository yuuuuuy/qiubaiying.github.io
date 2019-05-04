---
layout:     post
title:      LeetCode-Alg-108-Convert Sorted Array to Binary Search Tree
subtitle:   LeetCode-Alg-108-Convert Sorted Array to Binary Search Tree
date:       2019-05-04
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

Example:

Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5

### 2、思路

从题中可以得知，让我们将一个非降序的数组中的元素构建一个高度平衡的二叉搜索树，于是想到了直接构造一个平衡二叉树。
所谓平衡二叉树可以是一棵空树或者他的左右两个子树的高度差的绝对值不能超过1，且左右两个子树都是一棵平衡二叉树
对于平衡二叉树在往树中添加元素过程中，可能会造成树的失衡，这时候就需要我们去旋转来让树恢复平衡，失衡的情况不过四种情况：
左左失衡-->往右旋转
右右失衡-->往左旋转
左右失衡-->先左旋再右旋
右左失衡-->先右旋再左旋

### 4、实现(构造平衡二叉树)

    class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;
        TreeNode(int x) { val = x; }
    }

    public class Solution {
        public static void main(String[] args){
            int[] nums = new int[] {-10,-3,0,5,9};
            new Solution().sortedArrayToBST(nums);
        }

        public TreeNode sortedArrayToBST(int[] nums) {
            TreeNode root = null;

            for (int num : nums){
                root = add(num, root);
            }

            return root;
        }

        private TreeNode add(int val, TreeNode node){
            // 当node为null时，新建一个节点
            if (node == null){
                node = new TreeNode(val);
            }

            // 插入到左子树
            if (val < node.val){
                node.left = add(val, node.left);
                // 判断树是否失衡（左左 左右）
                if (treeHigh(node.left) - treeHigh(node.right) == 2){
                    // 左左旋转
                    if (val < node.left.val){
                        node = rotateLeftLeft(node);
                    }else{
                        // 左右旋转
                        node = rotateLeftRight(node);
                    }
                }
            }

            // 插入到右子树
            if (val > node.val){
                node.right = add(val, node.right);
                // 判断树是否失衡（右右 右左）
                if (treeHigh(node.right) - treeHigh(node.left) == 2){
                    // 右右
                    if (val > node.right.val){
                        node = rotateRightRight(node);
                    }else{
                        // 右左
                        node = rotateRightLeft(node);
                    }
                }
            }

            return node;
        }

        // 左左
        private TreeNode rotateLeftLeft(TreeNode node){
            TreeNode top = node.left;
            node.left = top.right;
            top.right = node;

            return top;
        }

        // 右右
        public TreeNode rotateRightRight(TreeNode node){
            TreeNode top = node.right;
            node.right = top.left;
            top.left = node;

            return top;
        }

        // 左右
        public TreeNode rotateLeftRight(TreeNode node){
            node.left = rotateRightRight(node.left);

            return rotateLeftLeft(node);
        }

        // 右左
        public TreeNode rotateRightLeft(TreeNode node){
            node.right = rotateLeftRight(node.right);

            return rotateRightRight(node);
        }

        // 获取树高
        private int treeHigh(TreeNode root){
            if (root == null){
                return 0;
            }

            return Math.max(treeHigh(root.left), treeHigh(root.right)) + 1;
        }
    }

### 5、他山之石

看了别人的思路后发现，题中已经给了数组是排序后的，对于二叉搜索树的中序遍历会得到有序数列，所以通过中序遍历结果来恢复一棵二叉搜索树，
数组的中间元素一定是树的根节点，根节点左右两边的子序列分别来恢复左子树和右子树。

### 6、实现

    public class Solution{
        public TreeNode sortedArrayToBST(int[] nums) {
            return bst(nums, 0, nums.length-1);
        }

        private TreeNode bst(int[] nums, int start, int end){
            if (start > end){
                return null;
            }

            int mid = start + (end - start)/2;
            TreeNode node = new TreeNode(nums[mid]);
            node.left = bst(nums, start, mid-1);
            node.right = bst(nums, mid+1, end);

            return node;
        }
    }
