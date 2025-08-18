---
title: "Add Two Numbers"
description: "Step-by-step explanation and solution to the classic linked list problem 'Add Two Numbers' from LeetCode, including code, examples, and algorithm design."
lang: "en"
publish: true
draft: false
enableToc: true
tags:
  - leetcode
  - leetcode/medium
  - linkedList
  - typescript
---
## The Problem
You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order,** and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.
You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**
- **Input:** `l1 = [2,4,3], l2 = [5,6,4]`
- **Output:** `[7,0,8]`
- **Explanation:** 342 + 465 = 807

**Example 2:** 
- **Input:** `l1 = [0], l2 = [0]`
- **Output:** `[0]`

**Example 3:**
- **Input:** `l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]`
- **Output:** `[8,9,9,9,0,0,0,1]`

**Constraints:**
- The number of nodes in each linked list is in the range [1, 100]
- `0 <= ` Node.val `<= 9`
- It is guaranteed that the list represents a number that does not have leading zeros

## Entry Code
```typescript
/**
  * class ListNode {
  *   val: number
  *   next: ListNode | null
  *   constructor(val?: number, next?: ListNode | null) {
  *     this.val = (val === undefined ? 0 : val)
  *     this.next = (next === undefined ? null : next)
  *   }
  * }
**/

function addTwoNumbers(l1: ListNode | null, l2: ListNode | null): ListNode | null {}
```

## Understanding the Approach
We can solve this problem efficiently with a single loop throught both linked lists. The key insight is that since the digits are already in reverse order, we can process them from left to right while handling carries naturally. 

## What is a LinkedList?
A [LinkedList](https://en.wikipedia.org/wiki/Linked_list) is a linear collection of data elements with O(n) time complexity for accessing data, but it's more flexible for insertions and updates compared to arrays. 
```
A -> B -> C -> D
```
In this problem, we work with a basic ListNode that only has `val` and `next` properties. This can be done as follows:
```typescript
while (current) {
	current = current.next;
}
```

## Step-by-Step Solution
Let's build the solution incrementally:

### **Step 1: Basic Loop Structure**
```typescript
function addTwoNumbers(l1: ListNode | null, l2: ListNode | null): ListNode | null {
	let pl1 = l1, pl2 = l2;

	while (pl1 || pl2) {
	    pl1 = pl1?.next || null;
	    pl2 = pl2?.next || null;
	}

}
```

### **Step 2: Adding Values**
```typescript
function addTwoNumbers(l1: ListNode | null, l2: ListNode | null): ListNode | null {
	let pl1 = l1, pl2 = l2;
	let dummy = null;
	
	while (pl1 || pl2) {
	    const val1 = pl1?.val || 0;
	    const val2 = pl2?.val || 0;
	    const sum = val1 + val2;
	
	    dummy = new ListNode(sum, dummy);
	
	    pl1 = pl1?.next || null;
	    pl2 = pl2?.next || null;
	}
	
	return dummy;
}
```

### **Step 3: Handling Carry**
The constraint `0 <= ` Node.val `<= 9` means we need to handle cases where the sum exceeds 9. We use the carry method from elementary arithmetic:
```typescript
function addTwoNumbers(l1: ListNode | null, l2: ListNode | null): ListNode | null {
	let pl1 = l1, pl2 = l2;
	let dummy = null, carry = 0;
	
	while (pl1 || pl2 || carry) {
	    const val1 = pl1?.val || 0;
	    const val2 = pl2?.val || 0;
	    const sum = val1 + val2 + carry;
	    carry = Math.floor(sum / 10);
	
	    const value = sum % 10;
	
	    dummy = new ListNode(value, dummy);
	    
	    pl1 = pl1?.next || null;
	    pl2 = pl2?.next || null;
	}
	
	return dummy;
}
```

### **Final Solution**
The final step is to build the result list in the correct order:
```typescript
function addTwoNumbers(l1: ListNode | null, l2: ListNode | null): ListNode | null {
	let pl1 = l1, pl2 = l2;
	let current = null, head = null, carry = 0;
	
	while (pl1 || pl2 || carry) {
	    const val1 = pl1?.val || 0;
	    const val2 = pl2?.val || 0;
	    const sum = val1 + val2 + carry;
	    carry = Math.floor(sum / 10);
	
	    const value = sum % 10;
	    const node = new ListNode(value, null);
	
	    if (!head) {
	        head = current = node;
	    } else {
	        current!.next = node;
	        current = node;
	    }
	
	    pl1 = pl1?.next || null;
	    pl2 = pl2?.next || null;
	}
	
	return head;
}
```

## Key Concepts
- **current:** Points to the last node in out result list, allowing us to append new nodes.
- **head:** Points to the first node of our result list, which we return at the end
- **carry:** Stores the overflow when the sum of two digits exceeds 9

This solution runs in `O(max(m,n))` time complexity where `m` and `n` are the lengths of the input lists, and uses `O(max(m,n))` space for the result list.
