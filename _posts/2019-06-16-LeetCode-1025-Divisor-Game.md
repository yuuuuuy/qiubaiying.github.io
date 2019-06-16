---
layout:     post
title:      LeetCode-Alg-1025-Divisor Game
subtitle:   LeetCode-Alg-1025-Divisor Game
date:       2019-06-16
author:     Jae
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Algorithm
---

### 1、题目

Alice and Bob take turns playing a game, with Alice starting first.

Initially, there is a number N on the chalkboard.  On each player's turn, that player makes a move consisting of:

Choosing any x with 0 < x < N and N % x == 0.
Replacing the number N on the chalkboard with N - x.
Also, if a player cannot make a move, they lose the game.

Return True if and only if Alice wins the game, assuming both players play optimally.



Example 1:

    Input: 2
    Output: true
    Explanation: Alice chooses 1, and Bob has no more moves.
    Example 2:

    Input: 3
    Output: false
    Explanation: Alice chooses 1, Bob chooses 1, and Alice has no more moves.


Note:

    1 <= N <= 1000


### 2、思路

对于第一个开始的用户来说偶数会赢，计数会输，采用数学归纳法证明：
假设```N<=n```时，偶数会赢，奇数会输是成立的，证明当```N = n + 1```时，结论仍然成立
证明：当```n```是偶数，```N```为奇数，我们要想赢只能第一个数选偶数，但是N是一个奇数且要保证```N % x == 0```,所以第一个数只能选奇数，最后就会输
当```n```是奇数时，```N```为偶数，我们想赢只能让对方在选择时是个奇数，于是我们只要第一个数选1就能保证对方选的时候是奇数，对方就会输
综上所示，偶数会赢，奇数会输

### 3、实现

    public class{
        public boolean divisorGame(int N) {
               return N % 2 == 0;
           }
    }
