---
layout: post
title: leetcode:【动态规划】Interleaving String   
date: 2015-04-22 19:24
categories: 算法
tags: leetcode dp
---

# 问题描述 Interleaving String   

Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.

>For example,  
Given:  
s1 = "aabcc",  
s2 = "dbbca",    
When s3 = "aadbbcbcac", return true.  
When s3 = "aadbbbaccc", return false.

# code

```
/*
感觉题目的interleaving给弄糊涂了
s1=12345，s2=abcde，s3=123a45bcde===true
反正就是从s1和s2中拿数据往s3上填就行
Submission Result: Time Limit Exceeded
*/
public class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if(s1 == null || s2 == null || s3 == null) return false;
        if(s1.length() + s2.length() != s3.length()) return false;
        return isMatch(s1,0,s2,0,s3,0);
    }
    private boolean isMatch(String s1,int count1,String s2,int count2,String s3,int count3) {
        if(count1 == s1.length() && count2 == s2.length()) return true;
        if(count1 == s1.length()) return s2.substring(count2).equals(s3.substring(count3));
        if(count2 == s2.length()) return s1.substring(count1).equals(s3.substring(count3));
        if(s1.charAt(count1) == s3.charAt(count3) && s2.charAt(count2) == s3.charAt(count3))
            return isMatch(s1,count1+1,s2,count2,s3,count3+1) || isMatch(s1,count1,s2,count2+1,s3,count3+1);
        else if(s1.charAt(count1) == s3.charAt(count3))
            return isMatch(s1,count1+1,s2,count2,s3,count3+1);
        else if(s2.charAt(count2) == s3.charAt(count3))
            return isMatch(s1,count1,s2,count2+1,s3,count3+1);
        else return false;
     }
}


/*
interleaving的关键就是，对s1,s2中取字符的顺序有要求，
下一个字符必须是两个字串当前结尾的字符后面的第一个字符，不能跳过。
dp的感觉就是优化，选中择优。
从多个false、true中选中true也算，关键状态能转移就行。
多个变量，多个条件，多维dp。
二维[][]中>=1,>=1.
dp[i][j] 代表取s1[0…i-1]中i个字符和s2[0…j-1]前j个字符，是不是能构成s3[0…k-1] (k==i+j)
字符s3[k-1]只能来自s1[i-1]或者s2[j-1]。
两种可能都满足，或者只有一个满足，或者如果都不满足就说明这种i,j组合不可能是interleaving了。
*/
public class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if(s1 == null || s2 == null || s3 == null) return false;
        if(s1.length() + s2.length() != s3.length()) return false;
        //"", "a", "",所有的+1
        boolean dp[][] = new boolean[s1.length()+1][s2.length()+1];
        dp[0][0] = true; //其他为false
        for(int i = 0;i<s1.length()+1;i++)
            for(int j = 0;j<s2.length()+1;j++) {
                if(j>0 && s3.charAt(i+j-1) == s2.charAt(j-1) && dp[i][j-1])
                    dp[i][j] = true;
                else if(i>0 && s3.charAt(i-1+j) == s1.charAt(i-1))
                    dp[i][j] = dp[i-1][j];
            }
        return dp[s1.length()][s2.length()];
    }
}
```