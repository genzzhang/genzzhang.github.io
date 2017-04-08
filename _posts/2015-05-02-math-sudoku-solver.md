---
layout: post
title: leetcode:【数独】Sudoku Solver
date: 2015-05-02 23:11
categories: 算法
tags: leetcode math
---

# 问题描述 

Write a program to solve a Sudoku puzzle by filling the empty cells.

Empty cells are indicated by the character '.'.

You may assume that there will be only one unique solution.

![](/assets/2015-05-02-math-sudoku-solver图1.png)

A sudoku puzzle...

![](/assets/2015-05-02-math-sudoku-solver图2.png)

...and its solution numbers marked in red.


# code

```
/*
http://www.csie.ntnu.edu.tw/~u91029/Backtracking.html#1
回溯法&&DFS==【退回，已经操作的状态复原】
所有解void
void sovle(char[][] board, int x, int y) {
    if(y == 9) {y = 0; x++;}
    if(x == 9) {solution(); return;}
    //do thing
}
有一解boolean
boolean sovle(char[][] board)
boolean sovle(cahr[][] board,int x,int y)
*/
public class Solution {
    public void solveSudoku(char[][] board) {
        sovle(board);
    }
    
 private boolean sovle(char[][] board) {  
        for(int i = 0;i<9;i++)  
            for(int j = 0;j<9;j++) {  
                if(board[i][j] == '.') {  
                    for(char k = '1';k<='9';k++) {  
                        board[i][j] = k;  
                        if(isVaild(board,i,j) && sovle(board)) {  
                            return true;  
                        }  
                    }  
                    board[i][j] = '.';  
                    return false;  
                }  
            }  
        return true;  
    }  
    
    private boolean isVaild(char[][] board,int i,int j) {
        int[] row = new int[9];
        int[] column = new int[9];
        int[] block = new int[9];
        int a = i/3*3;
        int b = j/3*3;
        for(int k = 0;k<9;k++) {
            if(board[i][k] != '.') {
                if(row[board[i][k]-'1']>0) return false;
                else {
                    row[board[i][k]-'1']++;
                }
            }
            if(board[k][j] != '.') {
                if(column[board[k][j]-'1']>0) return false;
                else {
                    column[board[k][j]-'1']++;
                }
            }
        }
        for(int k1 = 0;k1<3;k1++)
            for(int k2 = 0;k2<3;k2++) {
                if(board[a+k1][b+k2] != '.') {
                    if(block[board[a+k1][b+k2]-'1']>0) return false;
                    else {
                        block[board[a+k1][b+k2]-'1']++;
                    }
                }
            }
        return true;
    }
}
```