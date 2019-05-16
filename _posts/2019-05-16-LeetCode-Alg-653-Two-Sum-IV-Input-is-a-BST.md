---
layout:     post
title:      LeetCode-Alg-653-Two Sum IV Input is a BST
subtitle:   LeetCode-Alg-653-Two Sum IV Input is a BST
date:       2019-05-16
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that
their sum is equal to the given target.

    Example 1:

    Input:
        5
       / \
      3   6
     / \   \
    2   4   7

    Target = 9

    Output: True


    Example 2:

    Input:
        5
       / \
      3   6
     / \   \
    2   4   7

    Target = 28

    Output: False

### 2、思路

拿到这个题第一感觉和给一个数组找是是否存在两个元素的和满足给定的值类型相同，只是这道题的数据结构采用二叉搜索树，首先想到二叉搜索树的中序遍历结果是非降序排列，然后遍历每一个元素，并在该元素后面使用二分查找来判断是否存在该元素与目标值的差值相等的元素。

### 3、实现

    public class Solution {
        public boolean findTarget(TreeNode root, int k) {
            List<Integer> res = new ArrayList<>();
            inOrderTraversal(root, res);

            for (int i = 0; i < res.size(); i++){
                if (res.get(i) < k){
                    int diff = k - res.get(i);
                    if (diff < res.get(i)){
                        return false;
                    }
                    if (true == binarySearch(k-res.get(i), res, i+1, res.size()-1)){
                        return true;
                    }
                }
            }
            return false;
        }

        // 中序遍历，生成非降序的数组
        private void inOrderTraversal(TreeNode root, List<Integer> res){
            if (root == null){
                return;
            }
            inOrderTraversal(root.left, res);
            res.add(root.val);
            inOrderTraversal(root.right, res);
        }

        // 二分查找
        private boolean binarySearch(int k, List<Integer> res, int start, int end){
            int low = start;
            int high = end;
            while (low <= high){
                int mid = low + (high - low)/2;
                if (res.get(mid) == k){
                    return true;
                }
                if (res.get(mid) > k){
                    high = mid - 1;
                }
                if (res.get(mid) < k){
                    low = mid + 1;
                }
            }
            return false;
        }
    }

### 4、他山之石

#### 4.1、使用HashSet

使用集合来存放已经遍历过的节点的值，当遍历新节点时，判断目标值与当前节点的差值是否在集合中，如果在就说明满足条件，否则就将当前节点值放入集合中，再递归遍历其左右节点。

    public class Solution {

        // 使用HashSet
        public boolean findTarget(TreeNode root, int k) {
            Set<Integer> set = new HashSet<>();
            return find(root, k, set);
        }

        public boolean find(TreeNode root, int k, Set<Integer> set){
            if (root == null){
                return false;
            }
            if (set.contains(k - root.val)){
                return true;
            }
            set.add(root.val);
            return find(root.left, k, set) || find(root.right, k, set);
        }
    }

#### 4.2、使用BFS

对二叉搜素树进行中序遍历获得非降序数组，然后使用首尾指针所指向元素的和与目标值比较来决定两个指针的走向。

public class Solution {
    public boolean findTarget(TreeNode root, int k) {
        List<Integer> res = new ArrayList<>();
        inOrderTraversal(root, res);

        int low = 0;
        int high = res.size() - 1;

        while (low < high){
            int sum = res.get(low) + res.get(high);
            if (sum == k){
                return true;
            }
            if (sum < k){
                low++;
            }
            if (sum > k){
                high--;
            }
        }
        return false;
    }

    private void inOrderTraversal(TreeNode root, List<Integer> res){
        if (root == null){
            return;
        }
        inOrderTraversal(root.left, res);
        res.add(root.val);
        inOrderTraversal(root.right, res);
    }
}
