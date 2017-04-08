---
layout: post
title: leetcode:Reverse Linked List II
date: 2015-04-07 19:31
categories: 算法
tags: leetcode
---

#问题描述
Reverse a linked list from position m to n. Do it in-place and in one-pass.

For example:  
Given 1->2->3->4->5->NULL, m = 2 and n = 4,

return 1->4->3->2->5->NULL.

Note:
Given m, n satisfy the following condition:  
1 ≤ m ≤ n ≤ length of list.

#code
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
 
/*
单链表反转，一是导入List中，而是插入。插入，将链表分为两块，一边是已经处理的，一边是未处理的。
处理好的首尾，未处理的首。
本题中，新建一个head，便于处理。
*/
public class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        ListNode newHead = new ListNode(0);
        newHead.next = head;
        ListNode NodeBeforeM = newHead;
        //使用插入法，找出newHead
        for(int i = 1;i<m;i++) {
        	NodeBeforeM = NodeBeforeM.next;
        }
        //开发反转
        int item = n-m;
        ListNode pCur=null,pTail=null;
        //newHead.next != null
        pTail = NodeBeforeM.next;
        pCur = pTail.next;
        for(int i = 0;i<item;i++) {
            pTail.next = pCur.next;
            pCur.next = NodeBeforeM.next;
            NodeBeforeM.next = pCur;
            pCur = pTail.next;
        }
        return newHead.next;
    }
}
```