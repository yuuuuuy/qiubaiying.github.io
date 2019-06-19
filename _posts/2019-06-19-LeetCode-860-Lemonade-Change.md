---
layout:     post
title:      LeetCode-Alg-860-Lemonade-Change
subtitle:   LeetCode-Alg-860-Lemonade-Change
date:       2019-06-19
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

At a lemonade stand, each lemonade costs $5.

Customers are standing in a queue to buy from you, and order one at a time (in the order specified by bills).

Each customer will only buy one lemonade and pay with either a $5, $10, or $20 bill.  You must provide the correct change to each customer, so that the net transaction is that the customer pays $5.

Note that you don't have any change in hand at first.

Return true if and only if you can provide every customer with correct change.



    Example 1:

    Input: [5,5,5,10,20]
    Output: true
    Explanation:
    From the first 3 customers, we collect three $5 bills in order.
    From the fourth customer, we collect a $10 bill and give back a $5.
    From the fifth customer, we give a $10 bill and a $5 bill.
    Since all customers got correct change, we output true.
    Example 2:

    Input: [5,5,10]
    Output: true
    Example 3:

    Input: [10,10]
    Output: false
    Example 4:

    Input: [5,5,10,10,20]
    Output: false
    Explanation:
    From the first two customers in order, we collect two $5 bills.
    For the next two customers in order, we collect a $10 bill and give back a $5 bill.
    For the last customer, we can't give change of $15 back because we only have two $10 bills.
    Since not every customer received correct change, the answer is false.


Note:

    0 <= bills.length <= 10000
    bills[i] will be either 5, 10, or 20.

### 2、思路

该题是找零问题，可以使用贪心来求解，每次找零时都先从大面值的开始找，这样用掉的钞票数量最少，能更多的满足顾客。首先对于这道题需要记录收到的钞票数量
然后在每次处理顾客找零时，都从手里已有的大面值开始找。

### 3、实现

    public class Solution {
        public boolean lemonadeChange(int[] bills) {
            // 存放已收到钞票数量
            int[] changeArr = new int[21];
            // 钞票面值
            int[] faceValue = {20, 10, 5};

            for (int bill : bills){
                int change = bill - 5;
                changeArr[bill]++;
                if (change > 0){
                    for (int v : faceValue){
                        if (change >= v && changeArr[v] > 0){
                            int count = change / v;
                            if (changeArr[v] < count){
                                return false;
                            }else{
                                changeArr[v]-=count;
                                change = change - v * count;
                            }
                        }
                    }
                    if (change != 0){
                        return false;
                    }
                }
            }
            return true;
        }
    }
