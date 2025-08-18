---
title: "Two Sum"
description: "Solve the Two Sum problem starting with a simple O(n²) approach, then optimize to O(n) using a hash table."
comments: true
lang: "en"
publish: true
draft: false
enableToc: true
tags:
  - leetcode
  - leetcode/easy
  - typescript
socialDescription: "Learn how to solve Two Sum efficiently: start with the brute-force baseline and then use a hash table to reach O(n). Clear intuition, examples, and a practical JS implementation."
---
## The Problem
Given an array of integers `nums` and an integer `target`, we need to return the indices of two numbers that add up to the target. The constraints guarantee exactly one solution exists, and we can't use the same element twice. 

## Basic Approach: Nested Loops
The straightforward  solution uses two nested loops to check every possible pair. We iterate through each element and compare it with every other element to find the target sum.
```typescript 
function twoSum(nums: number[], target: number): number[] {
    let result: number[] = [];
    for (let i = 0; i < nums.length; i++) {
        for (let j = 0; j < nums.length; j++) {
            if (i === j) continue;
            if (nums[i] + nums[j] !== target) continue;
            
            result = [i, j];
        }
        if (result.length) break;
    }
    
    return result;
}
```
This approach has O(n^2) time complexity, which work but isn't optimal for large datasets. 

## The Hash Table Solution
The follow-up challenge asks for better than O(n^2) complexity. This is where hash table shine, offering O(n) time complexity through efficient key-value lookups.

### Understanding Hash Tables
A hash table is a data structure that implements an associative array, mapping keys to values. It's designed for fast data retrieval - instead of searching through every element like a basic array, it uses a hash function to quickly locate items. 

#### Hash Function
A hash function maps data of arbitrary size to fixed-size values. Here's an example using the [[FNV-1a]] hash function:

```typescript
function fnv1a(str: string, seed: 0x811c9dc5): number {
    let h = seed >>> 0;
    for (let i = 0; i < str.length; i++) {
        h ^= str.charCodeAt(i);
        h = Math.imul(h, 0x01000193);
    }
    return h >>> 0;
}
```

**Key components explained:**
- `seed` : initial hash value ([[FNV-1a]] offset basis)
- `Math.imul`: Preserves 32-bit integer overflow semantics in Javascript
- `0x811c9dc5 and 0x01000193`: Standard [[FNV-1a]] 32-bit parameters
- `^=` (XOR Assignment): Flips bits where the input has 1s
- `>>>` (zero-fill right shift): Ensures unsigned 32-bit result

**Hash Table Components:**
- **Key-Value Pairs**: Unique keys mapped to values
- **Hash Function**: Converts keys to array indices
- **Buckets/Slots**: Array positions storing the data
- **Collision Resolution**: Handles when different keys produce the same hash (chaining or open addressing)

## Optimized Two Sum Solution
Using Javascript's built-in [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) (which implements a hash table), we can solve this in O(n) time:

```typescript
function twoSum(nums: number[], target: number): number[] {
    const hashTable = new Map();

    for (let i = 0; i < nums.length; i++) {
        hashTable.set(nums[i], i);
    }

    for (let j = 0; j < nums.length; j++) {
        const value = target - nums[j];

        if (hashTable.has(value) && hashTable.get(value) !== j) {
            return [j, hashTable.get(value)];
        }
    }

    return [];
}
```

This solution: 
1. First pass: Store each number and it's index in the hash table
2. Second pass: For each number, check if it's complement (target - current number) exists in the hash table 
3. Return the indices when found, ensuring we don't use the same element twice

The hash table approach reduces time complexity from O(n^2) to O(n), making it significantly more efficient for large arrays.
