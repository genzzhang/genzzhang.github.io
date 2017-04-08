---
layout: post
title: leetcode:【集合】Remove Linked List Elements
date: 2015-04-27 13:02
categories: 算法
tags: leetcode collection
---

# 问题描述 
Remove all elements from a linked list of integers that have value val.

Example  
Given: 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, val = 6  
Return: 1 --> 2 --> 3 --> 4 --> 5

# code

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode newHead = new ListNode(0);
        newHead.next = head;
        ListNode p = head,q = newHead;
        while(p != null) {
            if(p.val == val) {
                q.next = p.next;
                p = q.next;
            } else {
                q = p;
                p = p.next;
            }
        }
        return newHead.next;
    }
}
```