---
layout:     post
title:      LeetCode-Alg-475-heaters
subtitle:   LeetCode-Alg-475-heaters
date:       2019-02-22
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目
Winter is coming! Your first job during the contest is to design a standard heater with fixed warm radius to warm all the houses.

Now, you are given positions of houses and heaters on a horizontal line, find out minimum radius of heaters so that all houses could be covered by those heaters.

So, your input will be the positions of houses and heaters seperately, and your expected output will be the minimum radius standard of heaters.

Note:

Numbers of houses and heaters you are given are non-negative and will not exceed 25000.
Positions of houses and heaters you are given are non-negative and will not exceed 10^9.
As long as a house is in the heaters' warm radius range, it can be warmed.
All the heaters follow your radius standard and the warm radius will the same.


Example 1:

    Input: [1,2,3],[2]
    Output: 1
    Explanation: The only heater was placed in the position 2, and if we use the radius 1 standard, then all the houses can be warmed.


Example 2:

    Input: [1,2,3,4],[1,4]
    Output: 1
    Explanation: The two heater was placed in the position 1 and 4. We need to use radius 1 standard, then all the houses can be warmed.

### 2、思路

首先对```houses```和```heaters```数组进行非降序排序，然后分别求出每一所房屋距离(```distance```)最近的```heater```，然后取这些```distances```中最大值就是要求的最小的半径

### 3、使用二分查找求房屋最近的heater

定义两个指针```left```和```high```，并计算```mid=left+(righ-left)/2```,循环终止条件为```left+1>=righ```,循环终止说明目标值落在```left```和```righ```指向的两个值之间即```heaters[left]<=target<=heaters[righ]```.即找到了距离目标值最近的两个值

    private int binarySearch(int[] heaters, int house)
        {
            int left = 0;
            int righ = heaters.length - 1;

            while (left + 1 < righ)
            {
                int mid = left + (righ - left) / 2;

                if (heaters[mid] == house)
                {
                    return 0;
                }else if (heaters[mid] < house)
                {
                    left = mid;
                }else
                {
                    righ = mid;
                }
            }

            return Math.min(Math.abs(house - heaters[left]), Math.abs(house - heaters[righ]));
        }

### 4、实现
    public int findRadius(int[] houses, int[] heaters) {
            Arrays.sort(houses);
            Arrays.sort(heaters);

            int radius = 0;
            for (int house : houses)
            {
                int local = binarySearch(heaters, house);
                radius = Math.max(radius, local);
            }

            return radius;
        }
