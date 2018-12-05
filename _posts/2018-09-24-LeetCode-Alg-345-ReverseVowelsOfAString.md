---
layout:     post
title:      LeetCode-ALg-344-ReverseVowelsOfAString
subtitle:   Algorithm-Easy
date:       2018-12-5
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---
### 1、题目

###### Write a function that takes a string as input and reverse only the vowels of a string.

    Example 1:

     Input: "hello"
     Output: "holle"
     Example 2:

     Input: "leetcode"
     Output: "leotcede"

     Note:
     The vowels does not include the letter "y".

### 2、思路

根据题意可知，需要将string反转，java中的String是不可变对象，首先想到将String转成StringBuilder，然后定义两个指针i、j，分别指向stringBuilder的首尾位置，分别移动i和j，当i、j同时指向元音字母时，交换所指位置的值。

### 3、实现(Java)

    public String reverseVowels2(String s) {

        int i = 0;
        int j = s.length() - 1;
        StringBuffer sb = new StringBuffer(s);
        HashSet vowelSet = new HashSet();
        vowelSet.add('a');
        vowelSet.add('e');
        vowelSet.add('i');
        vowelSet.add('o');
        vowelSet.add('u');
        vowelSet.add('A');
        vowelSet.add('E');
        vowelSet.add('I');
        vowelSet.add('O');
        vowelSet.add('U');

        while (i < j)
        {
            if (!vowelSet.contains(ch[i]))
            {
                i++;
                continue;
            }
            if (!vowelSet.contains(ch[j]))
            {
                j--;
                continue;
            }
            char c = sb.charAt(i);
            sb.setCharAt(i, sb.charAt(j));
            sb.setCharAt(j, c);
            i++;
            j--;

        }
        return sb.toString();
    }

发现这样做耗时较长为```10ms```,于是直接使用```String```的
```toCharArray()```方法将字符串转为字符数组。发现耗时减少为```6ms```,使用一个条件判断将```HashSet```替换掉。
### 4、修改后(Java)

    private boolean isVowel(char c) {
         return c == 'a' || c == 'e' || c == 'i' || c =='o' || c =='u' || c =='A' || c=='E' || c=='I' || c=='O' || c=='U';
     }

    public String reverseVowels2(String s) {

         int i = 0;
         int j = s.length() - 1;
         char[] ch = s.toCharArray();

         while (i < j)
         {
             if (!isVowel(ch[i]))
             {
                 i++;
                 continue;
             }
             if (!isVowel(ch[j]))
             {
                 j--;
                 continue;
             }

             char c = ch[i];
             ch[i] = ch[j];
             ch[j] = c;
             i++;
             j--;

         }
         return String.valueOf(ch);
     }


### 5、END
