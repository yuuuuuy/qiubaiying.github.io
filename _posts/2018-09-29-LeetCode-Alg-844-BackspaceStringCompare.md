---
layout:     post
title:      LeetCode-ALg-844-BackspaceStringCompare
subtitle:   Algorithm-Easy
date:       2018-09-29
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---
### 1、题目

###### Given two strings ```S``` and ```T```, return if they are equal when both are typed into empty text editors. # means a backspace character.

Example 1:

    Input: S = "ab#c", T = "ad#c"
    Output: true
    Explanation: Both S and T become "ac".

Example 2:

    Input: S = "ab##", T = "c#d#"
    Output: true
    Explanation: Both S and T become "".

Example 3:

    Input: S = "a##c", T = "#a#c"
    Output: true
    Explanation: Both S and T become "c".

Example 4:

    Input: S = "a#c", T = "b"
    Output: false
    Explanation: S becomes "c" while T becomes "b".

    1 <= S.length <= 200
    1 <= T.length <= 200
    S and T only contain lowercase letters and '#' characters.

### 2、要求

- 时间复杂度为O(n)
- 空间复杂度为O(1)

### 2、思路

- 思路一：定义两个空栈，将非```#```字符入栈，如果遇到```#```就将栈定元素弹出，
直到遍历完该字符串。

### 3、实现(c++)

    class Solution {
    public:
        bool backspaceCompare(string S, string T) {
            //定义两个空栈
            stack<char, vector<char>> a_stk;  
            stack<char, vector<char>> b_stk;
            for (char c : S)
            {
                if (c != '#')
                {
                    a_stk.push(c);
                }
                else if(a_stk.empty())
                {
                    continue;
                }
                else
                {
                    a_stk.pop(); //弹出栈顶元素
                }
            }
            for (char c : T)
            {
                if (c != '#')
                {
                    b_stk.push(c);
                }
                else if(b_stk.empty())
                {
                    continue;
                }
                else
                {
                    b_stk.pop(); //弹出栈顶元素
                }
            }

            return a_stk == b_stk; //相当于比较容器适配器底层的容器
        }
    };

### 4、他山之石

在查看别人的所提交的代码中发现一种解法：
将字符串从后往前，将```#```与所对应字符呼唤位置，然后返回新子串的开始下标。

    如 string S = "abc##c";
    经过转换后变为 S变为abc#ac，并返回新子串开始下标4；
    最后比较两个子串是否相等。

### 5、实现(C++)

    int squeeze(string& S)
        {
            int i = S.size()-1;
            int j = S.size()-1;
            int count = 0;

            while (i>=0)
            {
                while (count>0 || i>=0 && S[i]=='#')
                {
                    if (S[i]=='#')
                        ++count;
                    else
                        --count;
                    --i;
                }
                if (i>=0)
                {
                    S[j] = S[i];
                    --j;
                    --i;
                }
            }
            return j + 1;
        }

    bool backspaceCompare2(string S, string T)
    {
        int i = squeeze(S);
        int j = squeeze(T);

        return S.substr(i) == T.substr(j);
    }
