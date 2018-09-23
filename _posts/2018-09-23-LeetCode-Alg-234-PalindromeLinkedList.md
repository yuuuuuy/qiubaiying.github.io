---
layout:     post
title:      LeetCode-ALg-234-PalindromeLinkedList
subtitle:   Algorithm-Easy
date:       2018-09-23
author:     Jae
header-img: img/post-bg-e2e-ux.jpg
catalog: true
tags:
    - Algorithm
---
### 1、题目
##### Given a singly linked list, determine if it is a palindrome.
Example 1:

     Input: 1->2
     Output: false

Example 2:

    Input: 1->2->2->1
    Output: true

### 2、要求
##### 使用```O(n)```的时间复杂度和```O(1)```的空间复杂度解决此问题。

### 3、思路
首先根据题意是一个单链表，所以指针只能从链表的头移动到链表的尾，节点没有保存其前驱的信息。

##### 第一想法

将链表中的元素依次取出存放到如双端队列中，然后通过队首队尾的比较来判断是否是个回文。但是这样做好像会将问题变的复杂，而且会增加空间复杂度。

##### 第二想法

是否可以根据链表中元素的数量将元素从中间分开，对前一部分元素取负值，再与后一部分相加，如果和为0，则是回文。但是这样如果后半部分的元素交换位置同样和也是0，所以这种思路是错误的。

##### 第三想法

既然想到将链表的元素分为前后部分，就可以对后半部分进行反转，反转后再对前半部分和后半部分进行逐一比较，判断回文。

### 4、分析

最后采用第三种思路，定义两个指针lift、right，lift指向首元素，right移动到中间节点位置，然后对后半部分进行反转。

    1、首先遍历链表计算节点数量;
    2、计算中间节点的位置(len/2);
    3、将right指针移动到中间节点所在位置;
    4、反转后半部分链表;
    5、以right!=nullptr为条件，lift和right同时后移并逐一比较。

### 5、实现

    class Solution {
    public:
        //反转单链表
        ListNode* reverseLinkList(ListNode* head)
        {
            if (head==nullptr) {return head;}

            ListNode* tail = head;
            ListNode* forward = head->next;

            while (forward != nullptr)
            {
                tail -> next = forward -> next;
                forward -> next = head;
                head = forward;
                forward = tail -> next;
            }
            return head;
        }

        bool isPalindrome(ListNode* head) {
            int count = 0;
            ListNode* p = head;
            while (p != nullptr)
            {
                ++count;
                p = p->next;
            }
            if (count == 0 || count == 1) {return true;}

            int mid = (count % 2 == 0) ? count/2 : count/2+1;
            ListNode* i = head;
            ListNode* j = head;
            while (mid>0)
            {
                i = i->next;
                --mid;
            }
            ListNode* new_start = this->reverseLinkList(i);
            while (new_start!=nullptr)
            {
                if (j->val != new_start->val) {return false;}
                j = j->next;
                new_start = new_start->next;
            }
            return true;
        }
    };

### 6、他山之石

关于寻找单链表的中间节点，我首先遍历链表获取节点数量，然后计算中间位置数值并将指针移动到相应的节点上，搜索之后发现有更好的方法来获取中间节点的方法。

定义两个指针分别指向首元素，一个指针每次走一步，另一个指针每次走两步；

当快指针走到链表的尾部时，慢指针刚好在链表的中间;

原理：

    拿物体的运动来说明：
    有一个物体a的速度为v，另一个物体b的速度为2v；
    在t时间内： Sa=vt, Sb=2vt,当b物体跑完全部的路程时，a经过的路程就是b的一半，所以就是中点。

### 7、实现

    bool isPalindrome2(ListNode* head) {
            if (!head) {return true;}

            ListNode* slow = head;
            ListNode* fast = head;

            while (fast->next && fast->next->next)
            {
                slow = slow->next;
                fast = fast->next->next;
            }
            ListNode* right = slow->next;
            ListNode* left = head;

            right = this->reverseLinkList(right);

            while (right)
            {
                if (right->val != left->val) {return false;}
                left = left->next;
                right = right->next;
            }

            return true;
        }

### 8、收获
通过这道题我学会了如何进行单链表的反转，和快速寻找单链表中间节点的方法，感觉很多小技巧在解题上真的很爽！
