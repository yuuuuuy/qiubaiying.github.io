---
layout:     post
title:      LeetCode-969. Pancake Sorting
subtitle:   LeetCode-969. Pancake Sorting
date:       2019-10-31
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Given an array A, we can perform a pancake flip: We choose some positive integer k <= A.length, then reverse the order of the first k elements of A.  We want to perform zero or more pancake flips (doing them one after another in succession) to sort the array A.

Return the k-values corresponding to a sequence of pancake flips that sort A.  Any valid answer that sorts the array within 10 * A.length flips will be judged as correct.



Example 1:

    Input: [3,2,4,1]
    Output: [4,2,4,3]
    Explanation:
    We perform 4 pancake flips, with k values 4, 2, 4, and 3.
    Starting state: A = [3, 2, 4, 1]
    After 1st flip (k=4): A = [1, 4, 2, 3]
    After 2nd flip (k=2): A = [4, 1, 2, 3]
    After 3rd flip (k=4): A = [3, 2, 1, 4]
    After 4th flip (k=3): A = [1, 2, 3, 4], which is sorted.
    Example 2:

    Input: [1,2,3]
    Output: []
    Explanation: The input is already sorted, so there is no need to flip anything.
    Note that other answers, such as [3, 3], would also be accepted.


Note:

    1 <= A.length <= 100
    A[i] is a permutation of [1, 2, ..., A.length]

### 2、思路

这道题要我们通过n次反转数组前K个元素直到数组变的有序，我们通过观察数组可以发现，任意一个元素想回到有序的位置翻转两次就能够做到，例如
数组[3,2,4,1]中的元素4，首先翻转前3(3即是元素4的下标+1)个元素变为[4,2,3,1]然后再翻转前4(未排序数组长度)个元素就能回到属于自己的位置,但是我们应该以一种什么样的顺序来反转每一个元素呢？
如果以原数组的顺序来依次进行这样的翻转，比如我们将1翻转到属于自己的位置，但是在翻转比1大的元素时，1的位置会被改变，所以我们按照原数组中从大到小的元素进行翻转，最大元素翻转到指定位置后就认为
从该元素往后的元素都是有序的，且不再翻转。
这里我们首先要解决的一个问题就是对数组的下标按照元素从大到小排序，我们定义一个数组来存放元素对应的下标+1,然后对存储下标的数组按照元素从大到小排序，然后依次对每一个元素进行两次翻转，直到数组有序。

### 3 实现

    class Solution {
        public List<Integer> pancakeSort(int[] A) {
            int N = A.length;
            List<Integer> ans = new ArrayList<>();

            Integer[] B = new Integer[N];
            for (int i = 0; i < N; i++){
                B[i] = i+1;
            }
            Arrays.sort(B, (i, j) -> A[j-1]-A[i-1]);
            // System.out.println(Arrays.toString(B));
            for (int i : B){
                for (int move : ans){
                    if (i <= move){
                        i = move + 1 - i;
                    }
                }
                ans.add(i);
                ans.add(N--);
            }
            return ans;
        }
    }

#### 4 收获
对数组元素的下标按照元素大小排序，可以通过lambda来实现,关于java中是使用Arrays.sort()对数组进行排序，默认Arrays.sort()对数组进行升序排序，该函数可以又第二个参数Comparator，
java8中可以使用lambda表达式来实现数组的排序，但是这个参数只支持引用类型，不支持基本类型，所以我们不能对int[]使用lambda表达式
进行逆序，这里我们使用了Integer[]来代替。
