---
layout:     post
title:      LeetCode-Alg-1005-Maximize Sum Of Array After K Negations
subtitle:   LeetCode-Alg-1005-Maximize Sum Of Array After K Negations
date:       2019-06-20
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given an array A of integers, we must modify the array in the following way: we choose an i and replace A[i] with -A[i], and we repeat this process K times in total.  (We may choose the same index i multiple times.)

Return the largest possible sum of the array after modifying it in this way.



    Example 1:

    Input: A = [4,2,3], K = 1
    Output: 5
    Explanation: Choose indices (1,) and A becomes [4,-2,3].
    Example 2:

    Input: A = [3,-1,0,2], K = 3
    Output: 6
    Explanation: Choose indices (1, 2, 2) and A becomes [3,1,0,2].
    Example 3:

    Input: A = [2,-3,-1,5,-4], K = 2
    Output: 13
    Explanation: Choose indices (1, 4) and A becomes [2,3,-1,5,4].


Note:

    1 <= A.length <= 10000
    1 <= K <= 10000
    -100 <= A[i] <= 100

### 2、思路

该题要我们对数组中的元素取相反数，让数组中元素和最大，对每一个数字去取相反数的次数可以多于一次。

如果想让数组和最大，就需要将那些负数变成正数，正数最好保持不变，遇到0所有的取反操作对数组和没有影响。

1）我们可以先对数组进行非降序排序，然后从前往后扫描，遇到负数消耗一次操作将其变为正数，遇到```0```将全部操作用在其上，此时数组取得最大值；

2）当遇到正数，如果```K```是```2```的倍数，则两次操作后原数不变，如果K是奇数则可能需要将正数变为负数，但是这里有个问题，如果排序后的数组第一个元素就是正数，
取决于```K%2```的值，如果在其它位置遇到了正数，说明前面一个数一定是负数，此时不仅需要判断```K%2```的值，还要判断前一个数与当前数的大小，如果小于当前数则撤销前一个数的取反操作让当前```K```为偶数，这样就保证最终数组和是增加的。

举个例子：

对于数组```[4,2,3] K= 1```来说，排序后为```[2,3,4]``` 第一个遇到的是个正数，```K```为奇数，只能对其取反，此时损失最小

对于数组```[-3,-2,3] K = 3```来说，排序后为```[-3,-1,3]``` 第一个数为负数，将其取反获得最大增长，第二个数为负数，同样取反，第三个数是正数，此时```K=1```，需要对其取反但是如果对其取反对于最终求和结果就是 ```2-3``` 减少了```-1```，如果对前一个数不操作，此时```K=2```，于是```3```保持不变，对于最终求和结果影响是 ```-2+3``` 增加了```1```，所以后一种操作划算.

### 3、实现

    public class Solution {
        public int largestSumAfterKNegations(int[] A, int K) {
            Arrays.sort(A);
            for (int i = 0; K > 0 && i < A.length; i++){
                if (A[i] < 0){
                    A[i] = -A[i];
                    K--;
                }else if (A[i] == 0){
                    break;
                }else{
                    // 如果遇到正数且i!=0说明前面一定是个负数
                    if (i != 0 && K % 2== 1){
                        if (A[i-1] < A[i]){
                            A[i-1] = -A[i-1];
                        }else{
                            A[i] = -A[i];
                        }
                    }else{
                        A[i] = K % 2 == 0 ? A[i] : -A[i];
                    }
                    break;
                }
            }
            int ans = 0;
            for (int a : A){
                ans += a;
            }
            return ans;
        }
    }
