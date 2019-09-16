---
layout:     post
title:      LeetCode-109. Convert Sorted List to Binary Search Tree
subtitle:   LeetCode-109. Convert Sorted List to Binary Search Tree
date:       2019-09-16
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

Example:

    Given the sorted linked list: [-10,-3,0,5,9],

    One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

          0
         / \
       -3   9
       /   /
     -10  5

### 2、思路

这道题让我们将一个有序的单链表转成一个二叉平衡树，二叉平衡树的定义是一颗空树或者左右子树的高度差不大于1的二叉树，这里已经给了单链表是有序，所以只需要每次找到
链表的中点作为二叉平衡树的树根，然后递归的将中点左边的链表转成根节点的左子树，中点右边的链表转成根节点的右子树即可，
如何找到单链表的中心节点，可以使用快慢指针。

### 3 实现

    class ListNode {
        int val;
        ListNode next;
        ListNode(int x) { val = x; }
    }


    class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;
        TreeNode(int x) { val = x; }
    }

    class Solution {
        public TreeNode sortedListToBST(ListNode head) {
            if (head == null) return null;
            if (head.next == null) return new TreeNode(head.val);

            ListNode fast = head, slow = head;
            ListNode prev = new ListNode(0);
            prev.next = head;
            while (fast != null && fast.next != null){
                fast = fast.next.next;
                slow = slow.next;
                prev = prev.next;
            }
            prev.next = null;
            TreeNode root = new TreeNode(slow.val);
            root.left = sortedListToBST(head);
            root.right = sortedListToBST(slow.next);

            return root;
        }
    }
