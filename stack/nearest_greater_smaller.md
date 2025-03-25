# Nearest Greater Smaller Element

### Nearest Greater Element (NGE) :

- Pop from the stack while the current element is greater than the element at the stack's top.

- Push the current index after processing.

### Nearest Smaller Element (NSE) :

- Pop from the stack while the current element is smaller than the element at the stack's top.

- Push the current index after processing.

### Approach:

- Use a stack to store indices of elements as you traverse the array.

- For each element:

  - Pop from the stack while the element at the index stored at the top of the stack is `greater or equal / smaller or equal` to the current element. 
  - If the stack is not empty, the top element of the stack is the nearest `greater / smaller` element. 
  - If the stack is empty, it means there is no `greater or smaller` element, so mark it as -1 or a similar value. 
  - Push the current element's index onto the stack.

---

## Nearest Smaller Element

### **Problem:**
Given an array A, find the nearest smaller element G[i] for every element A[i] in the array such that the element has an index smaller than i.

Given the list: `heights = [4, 5, 2, 10, 8]`, The nearest smaller elements are `[-1, 4, -1, 2, 2]`.

---

### **Code Explanation**:

```python
heights = [4, 5, 2, 10, 8]
n = len(heights)
```
#### **Nearest smaller to the left (NSE Left)**

```python
nse_left = [-1] * n  # Initialize with -1, indicating no smaller element to the left
stack = []  # Stack to store indices of the bars
for i in range(n):
    # Pop bars from the stack until we find a bar with a height smaller than the current one
    while stack and heights[stack[-1]] >= heights[i]:
        stack.pop()
    
    # If the stack is not empty, the element at the top of the stack is the nearest smaller element
    if stack:
        nse_left[i] = stack[-1]
    
    # Push the current index onto the stack
    stack.append(i)
```

#### **Nearest smaller to the right (NSE Right)**

```python
nse_right = [n] * n  # Initialize with n, indicating no smaller element to the right
stack = []  # Stack to store indices of the bars
for i in range(n - 1, -1, -1):
    # Pop bars from the stack until we find a bar with a height smaller than the current one
    while stack and heights[stack[-1]] >= heights[i]:
        stack.pop()
    
    # If the stack is not empty, the element at the top of the stack is the nearest smaller element
    if stack:
        nse_right[i] = stack[-1]
    
    # Push the current index onto the stack
    stack.append(i) 
```

---
