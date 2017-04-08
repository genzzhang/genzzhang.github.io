---
layout: post
title: leetcode:【集合】Reorder List  
date: 2015-04-23 23:50
categories: 算法
tags: leetcode collection
---

# 问题描述 Reorder List

Given a singly linked list L: L0→L1→…→Ln-1→Ln,
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…

You must do this in-place without altering the nodes' values.

For example,
Given {1,2,3,4}, reorder it to {1,4,2,3}.

# code

```
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
 
 /*
 我的思路比较简单，牺牲内存来处理，明天想想直接用指针来
 建立一个headnew，方便处理
 最后的next为null
 */
public class Solution {
    public void reorderList(ListNode head) {
        if(head == null) return;
        List<ListNode> list = new ArrayList<ListNode>();
        ListNode headNew = new ListNode(0);
        headNew.next = head;
        ListNode p = head;
        while(p!=null) {
            list.add(p);
            p = p.next;
        }
        int len = list.size();
        p = headNew;
        for(int i = 0;i<len/2;i++) {
            p.next = list.get(i);
            p.next.next = list.get(len-1-i);
            p = p.next.next;
        }
        p.next = list.get(len/2);
        p.next.next = null;
            
    }
}


/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
 
 /*
 接着来
 将list分成前后两个部分
 后一个部分反转
 两个部分插入
 注意将最后的node的next赋值为null
 */
public class Solution {
    public void reorderList(ListNode head) {
        if(head == null || head.next == null) return;
        twistList(head,reverseList(midList(head)));
    }
    
    public ListNode midList(ListNode head) {
        if(head == null || head.next == null) return null;
        ListNode p = head,q = head.next;
        while(q != null && q.next != null) {
            p = p.next;
            q = q.next.next;
        }
        return p.next;
    }
    
    public ListNode reverseList(ListNode head) {
        ListNode p=head,q=head.next;
        head.next = null;
        while(q != null) {
            ListNode temp = q.next;
            q.next = p;
            p = q;
            q = temp;
        }
        return p;
    }
    
    public ListNode twistList(ListNode head1,ListNode head2) {
        ListNode q = head1,p = head2;
        ListNode newHead = new ListNode(0);
        ListNode qp = newHead;
        while(q != null) {
            if(p != null) {
                ListNode qTemp = q.next;
                ListNode pTemp = p.next;
                qp.next = q;
                qp.next.next = p;
                qp = p;
                q = qTemp;
                p = pTemp;   
            } else {
                qp.next = q;
                qp.next.next = null;
                break;
            }
            
        }
        return newHead.next;
    }
}
```