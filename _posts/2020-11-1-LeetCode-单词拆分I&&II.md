---
layout:     post
title:      LeetCode-单词拆分
subtitle:   记忆化递归
date:       2020-11-01
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、单词拆分(139)

给定一个非空字符串```s``` 和一个包含非空单词的列表 ```wordDict```，判定 ```s```是否可以被空格拆分为一个或多个在字典中出现的单词。

说明：

- 拆分时可以重复使用字典中的单词。
- 你可以假设字典中没有重复的单词。

示例 1：

    输入: s = "leetcode", wordDict = ["leet", "code"]
    输出: true
    解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。

示例 2：

    输入: s = "applepenapple", wordDict = ["apple", "pen"]
    输出: true
    解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
         注意你可以重复使用字典中的单词。
示例 3：

    输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
    输出: false


#### 1.1、思路

暴力枚举

这个题让我们使用任意多个空格插入到所给的非空字符串中，将字符串```s```分成若干个单词，且分成的单词要在所给的字典中，那么最容易被想到的方法就是直接枚举所有的位置，插入空格，我们可以用递归的方式去求解。
例如字符串```leetcode```，我们从后往前枚举可插入空格的位置，初始时```end=s.length```，枚举下标```i```，```i```初始为```end-1```，如果```[i, end)```所代表的字符串在字典中，则只需要递归判定```[0, i)```是否满足条件，如果满足直接返回结果，否则继续枚举```i```的位置，直到枚举完所有的下标。

实现：

    public boolean wordBreak(String s, List<String> wordDict) {
            int n = s.length();
            Set<String> set = new HashSet<>();

            wordDict.forEach(word -> set.add(word));
            return helper(s, n, set);
    }

    private boolean helper(String s, int end, Set<String> set){
          if (end == 0){
              return true;
          }
          int i = end - 1;
          while (i >= 0){
              // 递归判断
              if (set.contains(s.substring(i, end)) && helper(s, i, set)){
                  return true;
              }
              i--;
          }
          return false;
      }

**动态规划**

上面的解法的思路是没有问题的，但是时间复杂度较高，在递归的过程中重复判断了很多子串，我们可以通过动态规划的思想对上面的算法进行优化。
上面的算法中，我们判定子串```[0, i)```是否满足条件的结果并没有存储，假设```f[i]```表示前```i```个字符串满足条件的状态，如果子串```[i, end)```在字典中，那么问题的结果就是```ans=f[i]&[i, end)在字典中```。
于是有

**状态转移方程**

    设f[i] 表示前i个字符组成的子串是否满足题意
    则s是否满足条件=f[i] & [i, end)是字典中的单词

**初始化**

    f[0] = true

**实现**

    public boolean wordBreak(String s, List<String> wordDict) {
            int n = s.length();
            int[] f = new int[n+1];
            f[0] = 1;

            Set<String> set = new HashSet<>();
            wordDict.forEach(word -> set.add(word));

            return helper(s, n, f, set);
    }

    private boolean helper(String s, int end, int[] f, Set<String> set){
            if (f[end] == 1){
                return true;
            }

            if (f[end] == -1){
                return false;
            }

            int i = end - 1;
            while (i >= 0){
                if (set.contains(s.substring(i, end)) && helper(s, i, f, set)){
                    f[end] = 1;
                    return true;
                }
                i--;
            }
            f[end] = -1;
            return false;
        }

优化之后是直接AC的。

### 2. 单词拆分II

给定一个非空字符串 ```s``` 和一个包含非空单词列表的字典 ```wordDict```，在字符串中增加空格来构建一个句子，使得句子中所有的单词都在词典中。返回所有这些可能的句子。

说明：

 - 分隔时可以重复使用字典中的单词。
 - 你可以假设字典中没有重复的单词。

示例 1：

    输入:
    s = "catsanddog"
    wordDict = ["cat", "cats", "and", "sand", "dog"]
    输出:
    [
      "cats and dog",
      "cat sand dog"
    ]
示例 2：

    输入:
    s = "pineapplepenapple"
    wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
    输出:
    [
      "pine apple pen apple",
      "pineapple pen apple",
      "pine applepen apple"
    ]
    解释: 注意你可以重复使用字典中的单词。
示例 3：

    输入:
    s = "catsandog"
    wordDict = ["cats", "dog", "sand", "and", "cat"]
    输出:
    []
#### 2.2 思路

这个题相比于第一个题的变化就是需要返回所有满足拆分后的单词组成的句子，我们还是先使用最简单最暴力的方式从后往前回溯。
定义```end=s.length```，然后```end-1```到```0```枚举下标```i```，如果```[i, end)```所表示的子串在字典中，则递归枚举子串```[0, i)```,然后进行回溯，直到枚举出所有的可能，即是本题的答案。

实现：

    public List<String> wordBreak(String s, List<String> wordDict) {
            int n = s.length();

            Set<String> set = new HashSet<>();
            wordDict.forEach(word -> set.add(word));
            List<String> ans = new ArrayList<>();
            helper(s, n, set, new ArrayList<>(), set);
            return ans;
        }
    private void helper(String s, int end, Set<String> set, List<String> list, List<String> ans){
            if (end == 0){
                // 将单词拼接成句子
                StringBuilder sb = new StringBuilder();
                for (int i = list.size()-1; i >= 0; i--){
                    sb.append(list.get(i));
                    if (i > 0){
                        sb.append(" ");
                    }
                }
                ans.add(sb.toString());
                return;
            }

            int i = end - 1;
            while (i >= 0){
                if (set.contains(s.substring(i, end))){
                    list.add(s.substring(i, end));
                    helper(s, i, set, list, ans);
                    // 回溯
                    list.remove(list.size()-1);
                }
                i--;
            }
            return;
        }

该答案会超时，枚举的时间复杂度为指数级别，于是我们可以借鉴第一题的思路看看是否能够通过记忆化递归来将一些中间结果存储下来，以便后面的计算过程直接使用。
我们可以分析下第一题的思路，我们定义状态```f[i]```表示前```i```个字符是否满足题意，```F[i]```存储前```i```个字符能拆分的单词集合，如字符串```catsanddog```, ```f[7]=true```,```F[7]={[cat, sand], [cats, and]}```,当我们枚举到子串```dog下标为[7, 10)```时，判断```f[7]```为```true```，则直接可以知道当前可以组成两个句子，分别是```[cat, sand]+dog=cat sand dog```和```[cats, and]+dog=cats and dog```。

**实现**

    public List<String> wordBreak(String s, List<String> wordDict) {
            int n = s.length();
            int[] f = new int[n+1];
            f[0] = 1;
            List<List<String>>[] F = new List[n+1];
            for (int i = 0; i < F.length; i++){
                F[i] = new ArrayList<>();
            }

            Set<String> set = new HashSet<>();
            wordDict.forEach(word -> set.add(word));
            List<String> ans = new ArrayList<>();
            helper(s, n, f, F, set);
            // F[end]中存储的单词即是最终的结果
            for (List<String> l : F[n]){
                StringBuilder sb = new StringBuilder();
                for (int i = l.size()-1; i >= 0; i--){
                    sb.append(l.get(i));
                    if (i > 0){
                        sb.append(" ");
                    }
                }
                ans.add(sb.toString());
            }
            System.out.println(F);
            return ans;
        }

    private boolean helper(String s, int end, int[] f, List<List<String>>[] F, Set<String> set){
            if (f[end] == 1){
                return true;
            }
            if (f[end] == -1) {
                return false;
            }

            int i = end - 1;
            while (i >= 0){
                if (set.contains(s.substring(i, end)) && helper(s, i, f, F, set)){
                    f[end] = 1;
                    if (F[i].size() == 0){
                        F[end].add(Arrays.asList(s.substring(i, end)));
                    }else{
                        for (List<String> l : F[i]) {
                            List<String> t = new ArrayList<>();
                            t.add(s.substring(i, end));
                            t.addAll(l);
                            F[end].add(t);
                        }
                    }
                }
                i--;
            }
            if (f[end] == 0){
                f[end] = -1;
                return false;
            }
            return true;
        }
