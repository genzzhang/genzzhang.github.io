---
layout: post
title: leetcode:Reverse Words in a String
date: 2015-04-25 18:38
categories: 算法
tags: leetcode 
---

# 问题描述   
Given an input string, reverse the string word by word.

For example,  
Given s = "the sky is blue",  
return "blue is sky the".

# code

```
//java好像没啥好讲的，用个正则表达式很好split
/*
input="   a      b "
output="b a"
貌似是这个要求就ac了
*/
public class Solution {
    public String reverseWords(String s) {
        String[] str = s.trim().split("[ ]+");
        StringBuilder sb = new StringBuilder();
        for(int i = str.length-1;i>0;i--) {
        	sb.append(str[i]+" ");
        }
        if(str.length > 0)
        	sb.append(str[0]);
        return sb.toString();
    }
}



//Integer--int--char转化
/*
没觉得这个有啥好
写上去的感觉，提供另一个很好地想法：词和句子。
并且用的是栈
如果题目要求多点，比如符号啥点，可以这么来考虑
当然，如果像前面的正表来split，啥都不说了
*/
public class Solution {
    public String reverseWords(String s) {
        String ss = s.trim();
        Stack<Character> word = new Stack<Character>();
        Stack<Character> sentence = new Stack<Character>();
        StringBuilder sb = new StringBuilder();
        for(int i = 0;i<ss.length();i++) {
            char a = ss.charAt(i);
            if(a != ' ') word.push(a);
            else {
                //两个while去掉多个空格
            	while(!word.isEmpty()) {
                    while(!word.isEmpty()) {
                        sentence.push(word.pop());
                    }
                    sentence.push(' ');
            	}
            }
        }
        //"hil"没有遇到空格，第一次
        while(!word.isEmpty()) {
            sentence.push(word.pop());
        }
        while(!sentence.isEmpty()) {
            sb.append(sentence.pop());
        }
        return sb.toString();
    }
}
```