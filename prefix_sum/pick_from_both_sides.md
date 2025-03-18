# Pick from both sides!

### **Problem:** Maximum Score from Cards

There are several cards arranged in a row, and each card has an associated number of points. The points are given in the integer array `cardPoints`.

In one step, you can take one card from the beginning or from the end of the row. You have to take exactly `k` cards.

Your score is the sum of the points of the cards you have taken.

### Problem Description

Given the integer array `cardPoints` and the integer `k`, return the maximum score you can obtain.

#### Example

```python
Input: cardPoints = [1,2,3,4,5,6,1], k = 3 
Output: 12

Explanation: After the first step, your score will always be 1. 
However, choosing the rightmost card first will maximize your total score. 
The optimal strategy is to take the three cards on the right, giving a final score of 1 + 6 + 5 = 12.
```

## Table of Contents

- [Approach 1: Sliding Window Technique](#approach-1-sliding-window-technique)
- [Approach 2: Using a Dummy Node to Simplify Handling](#approach-2-using-a-dummy-node-to-simplify-handling)

---

## Approach 1: Sliding Window Technique

1. **Sliding Window Technique:**
   - Since you can only pick k cards, the problem can be broken down into finding the optimal combination of taking cards from the beginning and the end of the array.

2. **Sum of Last k Cards:**
   - If you take all the cards from the end, you can get the sum of the last k elements of the array.

3. **Sliding Window of k Cards:**
   - You can also consider taking a combination of cards from both the beginning and the end. This is where the sliding window technique comes into play. The idea is to take a few cards from the beginning and the rest from the end.
   - The sum of the first i cards and the last k-i cards (where i ranges from 0 to k) will give you a set of possible combinations.
 
4. **Maximizing the Sum:**
   - We compute the sum for each possible combination and return the maximum sum.

```python
def maxScore(cardPoints, k):
    # Total number of cards
    n = len(cardPoints)
    
    # Compute the sum of the first k cards
    total_sum = sum(cardPoints[:k])
    
    # Initialize the result with the sum of the first k cards
    max_score = total_sum
    
    # Start sliding the window
    for i in range(1, k+1):
        # Slide the window: remove the first card of the current window and add the next card from the end
        total_sum -= cardPoints[k-i]
        total_sum += cardPoints[n-i]
        
        # Update the maximum score
        max_score = max(max_score, total_sum)
    
    return max_score
```

---

## Approach 2: Using Prefix & Suffix Sum Array 

1. **Prefix Sum:**
   - A prefix sum array helps you compute the sum of the first `i` elements of the `cardPoints` array. This will be useful for selecting cards from the beginning of the array.

2. **Suffix Sum:**
   - Similarly, a suffix sum array will store the sum of the last `i` elements of the array, which will help us pick cards from the end of the array.

3. **Combine Both Prefix and Suffix:**
   - The idea is to pick `i` cards from the start of the array and `k-i` cards from the end. The sum for this combination can be computed using the prefix and suffix arrays. You want to maximize the sum of cards you can get by trying every possible combination of cards from the start and end.

### Detailed Steps:

1. **Prefix Sum:** Compute the sum of the first `i` cards, where `i` ranges from `0` to `k`.

2. **Suffix Sum:** Compute the sum of the last `i` cards, where `i` ranges from `0` to `k`.

3. **For each `i` from `0` to `k`:**
   - Calculate the sum of:
     - `i` cards from the beginning of the array (using the prefix sum array).
     - `k-i` cards from the end of the array (using the suffix sum array).
   
4. **Track the maximum sum** obtained from these combinations.


```python
def maxScore(cardPoints, k):
    n = len(cardPoints)
    
    # Step 1: Compute prefix and suffix sums
    prefix_sum = [0] * (k + 1)
    suffix_sum = [0] * (k + 1)
    
    # Prefix sum for the first k cards
    for i in range(1, k + 1):
        prefix_sum[i] = prefix_sum[i - 1] + cardPoints[i - 1]
    
    # Suffix sum for the last k cards
    for i in range(1, k + 1):
        suffix_sum[i] = suffix_sum[i - 1] + cardPoints[n - i]
    
    # Step 2: Try every possible combination of picking i cards from the start and k-i cards from the end
    max_score = 0
    for i in range(k + 1):
        # i cards from the start and k-i cards from the end
        current_score = prefix_sum[i] + suffix_sum[k - i]
        max_score = max(max_score, current_score)
    
    return max_score
```
---

## Similar Problems:

- ### Problem: Maximum Possible Sum After B Operations / Pick from both sides 

    You are given an integer array `A` of size `N`. 

    You need to perform `B` operations where, in each operation, you can remove either the leftmost or the rightmost element of the array `A`. Your task is to find and return the maximum possible sum of the `B` elements that were removed after these `B` operations.

    #### **Problem Description**

    Given:
    - An array `A` of size `N`
    - An integer `B` representing the number of elements to be removed

    ### **Objective**
    Maximize the sum of the `B` elements that are removed. You can remove elements from either the left end or the right end of the array.

    ### **Example**

    Let’s consider the case where:
    - `B = 3`
      - Array `A` contains `10` elements: `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`

    You can perform the following operations:
    - Remove 3 elements from the front and 0 elements from the back: `[1, 2, 3]`
      - Remove 2 elements from the front and 1 element from the back: `[1, 2, 10]`
      - Remove 1 element from the front and 2 elements from the back: `[1, 9, 10]`
      - Remove 0 elements from the front and 3 elements from the back: `[8, 9, 10]`
 
    The goal is to find the maximum possible sum of the `B` elements that are removed.

---
