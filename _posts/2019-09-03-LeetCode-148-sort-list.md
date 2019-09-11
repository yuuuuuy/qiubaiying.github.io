---
layout:     post
title:      LeetCode-148. Sort List
subtitle:   LeetCode-148. Sort List
date:       2019-09-03
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Sort a linked list in O(n log n) time using constant space complexity.

Example 1:

  Input: 4->2->1->3
  Output: 1->2->3->4
  Example 2:

  Input: -1->5->3->4->0
  Output: -1->0->3->4->5

### 2、思路

这道题让我们在使用常量空间和```nlgn```的时间复杂度对单链表进行排序，在排序算法中时间复杂度能达到```nlgn```的有快速排序、归并排序和堆排序，，这里我们使用快速排序算法进行实现，快速排序的思想为：首先选一个枢轴，对于一趟排序过后，枢轴左边的元素都小于枢轴，枢轴右边的元素都大于枢轴，然后递归的对枢轴左半部分，右半部分进行排序，知道数组有序。

但是题目给的是单链表，只能从头节点往后遍历，所以我们把第一个节点作为枢轴，从前往后遍历，遇到比枢轴小的元素进行元素交换，遇到比枢轴大的元素跳过，例如
```4->3->2->5->7->null```
枢轴为4，定义指针p指向枢轴，q指向枢轴下一个元素，q指针往前遍历，遇到节点值小于枢轴4，则p=p.next,交换p和q节点的值，直到q节点指向链表尾null时，第一趟结束，此时交换p节点和枢轴的值，则p就是一次划分的分割点。

上面链表第一趟排序结果为：```2->3->4->5->7->null```

### 3 实现

    public class Solution{
      public ListNode sortList(ListNode head) {
          return sortListHelper(head, null);
      }
      public ListNode sortListHelper(ListNode pHead, ListNode pTail){
          if (pHead != pTail){
              ListNode p = patition(pHead, pTail);
              sortListHelper(pHead, p);
              sortListHelper(p.next, pTail);
          }
          return pHead;
      }

      public ListNode patition(ListNode begin, ListNode end){
          int key = begin.val;
          ListNode p = begin;
          ListNode q = begin.next;
          while (q != end){
              if (q.val < key){
                // SWAP
                p = p.next;
                int t = p.val;
                p.val = q.val;
                q.val = t;
              }
              q = q.next;
          }
          begin.val = p.val;
          p.val = key;
          return p;
      }
    }
