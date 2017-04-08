---
layout: post
title: leetcode:Set Matrix Zeroes
date: 2015-04-16 13:41
categories: 算法
tags: leetcode
---

# 问题描述

Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in place.

Follow up:  
Did you use extra space?  
A straight forward solution using O(mn) space is probably a bad idea.  
A simple improvement uses O(m + n) space, but still not the best solution.  
Could you devise a constant space solution?  

# code

```
/*
Contant Space，考虑定义几个变量或者加以题目本身的空间
本体利用矩阵的第一行和第一列来作为辅助空间使用，记录该行和该列有必要set to 0
同时注意第一行和第一列本身是否要set to 0
1.确定第一行和第一列是否需要清零
 isFirstRowZero、isFirstColumnZero
2.扫描剩下的矩阵元素，如果遇到了0，就将对应的第一行和第一列上的元素赋值为0
 matrix[i][0] = 0;
 matrix[0][j] = 0;
3.根据第一行和第一列的信息，将该清0的清0
4.处理第一行和第一列
*/
public class Solution {
    public void setZeroes(int[][] matrix) {
        boolean isFirstRowZero = false;
        boolean isFirstColumnZero = false;
        for(int i = 0;i<matrix.length;i++) {
            if(matrix[i][0] == 0) {
                isFirstRowZero = true;
                break;
            }
        }
        for(int i = 0;i<matrix[0].length;i++) {
            if(matrix[0][i] == 0) {
                isFirstColumnZero = true;
                break;
            }
        }
        for(int i = 1;i<matrix.length;i++)
            for(int j = 1;j<matrix[0].length;j++) {
                if(matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        for(int i = 1;i<matrix.length;i++) {
            if(matrix[i][0] == 0) {
                for(int j = 1;j<matrix[0].length;j++)
                    matrix[i][j] = 0;
            }
        }
        for(int i = 1;i<matrix[0].length;i++) {
            if(matrix[0][i] == 0) {
                for(int j = 1;j<matrix.length;j++) 
                    matrix[j][i] = 0;
            }
        }
        if(isFirstRowZero)
            for(int i = 0;i<matrix.length;i++)
                matrix[i][0] = 0;
        if(isFirstColumnZero)
            for(int i = 0;i<matrix[0].length;i++)
                matrix[0][i] = 0;
    }
}
```