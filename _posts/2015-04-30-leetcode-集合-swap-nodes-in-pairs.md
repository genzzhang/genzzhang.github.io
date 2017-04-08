---
layout: post
title: leetcode:【集合】Swap Nodes in Pairs 
date: 2015-04-30 16:30
categories: 算法
tags: leetcode collection
---

# 问题描述 

Given a linked list, swap every two adjacent nodes and return its head.

For example,  
Given 1->2->3->4, you should return the list as 2->1->4->3.

Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.

# code

```
/*
比较简单
脑子清醒就行
*/
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode newHead = new ListNode(0);
        newHead.next = head;
        ListNode p = newHead;
        while(p.next != null && p.next.next != null) {
            ListNode temp = p.next;
            p.next = temp.next;
            temp.next = p.next.next;
            p.next.next = temp;
            p = temp;
        }
        return newHead.next;
    }
}
```