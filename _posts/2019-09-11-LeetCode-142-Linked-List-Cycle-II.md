---
layout:     post
title:      LeetCode-142. Linked List Cycle II
subtitle:   LeetCode-142. Linked List Cycle II
date:       2019-09-11
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

Note: Do not modify the linked list.



Example 1:

    Input: head = [3,2,0,-4], pos = 1
    Output: tail connects to node index 1
    Explanation: There is a cycle in the linked list, where tail connects to the second node.


Example 2:

    Input: head = [1,2], pos = 0
    Output: tail connects to node index 0
    Explanation: There is a cycle in the linked list, where tail connects to the first node.


Example 3:

    Input: head = [1], pos = -1
    Output: no cycle
    Explanation: There is no cycle in the linked list.




Follow-up:
Can you solve it without using extra space?

### 2、思路

该题让我们判断链表是否有环且找到环的入口节点位置，对于判断链表是否有环可以使用快慢指针，快指针每次走两步，慢指针每次走一步，如果有环存在，快慢指针一定会在环的某个节点上相遇，但是如果链表中有环，那么环的入口位置怎么找呢？

我们通过下面的图来分析：

![example](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/LeetCode-142/example.png)

假设链表头距离环入口点距离为```a```， 快慢指针相遇点距离环入口点距离为```b```，环的周长为```r```，则慢指针走过的路程为```(a+b)```,因为快指针速度是其两倍，所以快指针走过的路程为```2(a+b)```,还可以得知，当快慢指针相遇时，慢指针还没有绕环一圈，而快指针已经绕环```n```圈了。

于是有下面关系：

    2(a + b) = a + b + nr

化简得

    a = nr-b=(n-1)r + (r-b)

其中```(r-b)```就是从相遇点继续前进到环入口点的距离，所以我们可以让一个指针从链表头开始每步前进一个节点，另一个指针从相遇点每步前进一个节点，一定能在环入口点相遇。

### 3 实现

    class ListNode {
        int val;
        ListNode next;
        ListNode(int x) {
            val = x;
            next = null;
        }
     }

     public class Solution {
        public ListNode detectCycle(ListNode head) {
            ListNode fast = head;
            ListNode slow = head;

            while (fast != null && fast.next != null){
                fast = fast.next.next;
                slow = slow.next;
                if (fast == slow){
                    break;
                }
            }
            if (fast == null || fast.next == null) return null;

            slow = head;
            while (fast != slow){
                fast = fast.next;
                slow = slow.next;
            }

            return fast;
        }
    }
