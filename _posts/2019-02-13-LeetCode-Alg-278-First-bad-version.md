---
layout:     post
title:      LeetCode-Alg-278-First-bad-version
subtitle:   LeetCode-Alg-278-First-bad-version
date:       2019-02-13
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目
You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which will return whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

Example:

    Given n = 5, and version = 4 is the first bad version.

    call isBadVersion(3) -> false
    call isBadVersion(5) -> true
    call isBadVersion(4) -> true

Then 4 is the first bad version.
### 2、思路
最容易想到的方法就是类似二分查找，因为题中给出一个坏版本后面所有的版本都是坏版本，所以只要判断一个版本是否是坏版本就能决定第一个坏版本
应该在那一部分里找。
### 3、实现
    public int firstBadVersion(int n) {
        int low = 1;
        int high = n;

        while (low < high)
        {
            int mid = (low + high) / 2;
            if (isBadVersion(mid))
            {
                high = mid;
            }else
            {
                low = mid + 1;
            }
        }

        return low;
    }

但是部分测试用例失败(测试用例为2126753390,1702766719),这是因为在计算mid时，采用mid = (low + high) / 2，这个时候mid的值已经超出了int
所能表示的范围，导致溢出，这时候mid是负值。
### 4、修改
    public int firstBadVersion(int n) {
        int low = 1;
        int high = n;

        while (low < high)
        {
            int mid = low + (high - low) / 2;
            if (isBadVersion(mid))
            {
                high = mid;
            }else
            {
                low = mid + 1;
            }
        }

        return low;
    }

在计算mid时，使用mid = low + (high - low) / 2的方法就能避免溢出。
