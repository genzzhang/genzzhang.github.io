---
layout: post
title: leetcode:【集合】Reverse Linked List
date: 2015-05-07 10:18
categories: 算法
tags: leetcode collection
---

# 问题描述   
```
翻转
Reverse Linked List  
```

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
    public ListNode reverseList(ListNode head) {
        if(head == null) return null;
        ListNode q = head;
        ListNode p = head.next;
        while(p != null) {
            ListNode temp = p.next;
            p.next = q;
            q = p;
            p = temp;
        }
        head.next = null;
        head = q;
        return head;
    }
}
```