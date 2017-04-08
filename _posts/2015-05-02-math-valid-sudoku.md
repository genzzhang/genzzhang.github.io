---
layout: post
title: leetcode:【数独】Valid Sudoku
date: 2015-05-02 15:22
categories: 算法
tags: leetcode math
---

# 问题描述 
```
Determine if a Sudoku is valid, according to: Sudoku Puzzles - The Rules.

The Sudoku board could be partially filled, where empty cells are filled with the character '.'.
![](/assets/2015-05-02-数独-valid-sudoku图.png)
A partially filled sudoku which is valid.

Note:
A valid Sudoku board (partially filled) is not necessarily solvable. Only the filled cells need to be validated.
```

# code

```
/*
这道题目就是让你看看当前状态的是否符合Sudoku
就是每一行，每一列，每一个方块的看
而不是确认是否有解
*/
public class Solution {
    public boolean isValidSudoku(char[][] board) {
        int[] row = new int[9];
        int[] column = new int[9];
        boolean flag = false;
        //row && cloumn
        for(int i = 0;i<9;i++) {
            for(int k = 0;k<9;k++) {
                row[k] = 0;
                column[k] = 0;
            }
            for(int j = 0;j<9;j++) {
                if(board[i][j] != '.') {
                   if(row[board[i][j]-'1']>0) return false;
                   else {
                       row[board[i][j]-'1']++;
                   }
                }
                if(board[j][i] != '.') {
                   if(column[board[j][i]-'1']>0) return false;
                   else {
                       column[board[j][i]-'1']++;
                   }
                }
            }          
        }
        //block
        for(int i = 0;i<9;i = i+3)
            for(int j = 0;j<9;j = j + 3) {
                for(int k = 0;k<9;k++)
                    row[k] = 0;
                for(int m = 0;m<3;m++)
                    for(int n = 0;n<3;n++) {
                        if(board[i+m][j+n] != '.') {
                           if(row[board[i+m][j+n]-'1']>0) return false;
                           else {
                               row[board[i+m][j+n]-'1']++;
                            }
                         }
                    }
            }
        //
        return true;
    }
}

/*
将block放在一个行列循环中
block[i][j]表示第i个(从左至右从上至下)方块中的元素j
*/
public class Solution {
    public boolean isValidSudoku(char[][] board) {
        int[] row = new int[9];
        int[] column = new int[9];
        int[][] block = new int[9][9];
        boolean flag = false;
        for(int i = 0;i<9;i++) {
            for(int k = 0;k<9;k++) {
                row[k] = 0;
                column[k] = 0;
            }
            for(int j = 0;j<9;j++) {
                //cloumn
                if(board[i][j] != '.') {
                   if(row[board[i][j]-'1']>0) return false;
                   else {
                       row[board[i][j]-'1']++;
                   }
                }
                //cloumn
                if(board[j][i] != '.') {
                   if(column[board[j][i]-'1']>0) return false;
                   else {
                       column[board[j][i]-'1']++;
                   }
                }
                //block[i][j]表示第i个(从左至右从上至下)方块中的元素j
                if(board[i][j] != '.') {
                   if(block[i/3*3+j/3][board[i][j]-'1']>0) return false;
                   else {
                       block[i/3*3+j/3][board[i][j]-'1']++;
                   }
                }
            }          
        }
        //
        return true;
    }
}
```