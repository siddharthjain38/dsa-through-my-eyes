# Design a Stack 

### **Problem:**

**Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.**

Implement the MinStack class:

- MinStack() initializes the stack object.  
- void push(int val) pushes the element val onto the stack. 
- void pop() removes the element on the top of the stack. 
- int top() gets the top element of the stack. 
- int getMin() retrieves the minimum element in the stack. 
- You must implement a solution with O(1) time complexity for each function.

```commandline
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]
```

---

### Code:

```python
class MinStack:

    def __init__(self):
        # Initialize an empty list to act as the main stack
        self.stack = list()
        # Initialize an empty list to store the minimum values at each step
        self.min_stack = list()
        # Initialize the top element index for the main stack
        self.top_element = -1
        
    def push(self, val: int) -> None:
        """
        Pushes a new value onto the stack. Also updates the minimum stack if necessary.
        """
        # Add the new value to the stack
        self.stack.append(val)
        # Increment the top element index
        self.top_element += 1

        # If the min_stack is empty or the value is less than or equal to the current minimum
        if not self.min_stack or self.min_stack[-1] >= val:
            # Push the new value onto the min_stack (it maintains the minimum values)
            self.min_stack.append(val)

    def pop(self) -> None:
        """
        Pops the top value off the stack and updates the minimum stack if necessary.
        """
        if self.top_element >= 0:
            # Pop the value from the main stack
            val = self.stack.pop(self.top_element)
            # Decrement the top element index
            self.top_element -= 1

            # If the popped value is equal to the current minimum value, pop from min_stack as well
            if self.min_stack and self.min_stack[-1] == val:
                self.min_stack.pop()

    def top(self) -> int:
        """
        Returns the top value of the stack without removing it.
        """
        if self.top_element >= 0:
            return self.stack[self.top_element]
        
        return 0  # Return 0 if the stack is empty

    def getMin(self) -> int:
        """
        Returns the minimum value from the stack.
        """
        # The minimum value is always at the top of the min_stack
        if self.min_stack:
            return self.min_stack[-1]

# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(val)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```

---

### **Problem:**

**Design a stack that supports increment operations on its elements.**

Implement the CustomStack class:

- CustomStack(int maxSize) Initializes the object with maxSize which is the maximum number of elements in the stack. 
- void push(int x) Adds x to the top of the stack if the stack has not reached the maxSize. 
- int pop() Pops and returns the top of the stack or -1 if the stack is empty. 
- void inc(int k, int val) Increments the bottom k elements of the stack by val. If there are less than k elements in the stack, increment all the elements in the stack.

```commandline
Input
["CustomStack","push","push","pop","push","push","push","increment","increment","pop","pop","pop","pop"]
[[3],[1],[2],[],[2],[3],[4],[5,100],[2,100],[],[],[],[]]

Output
[null,null,null,2,null,null,null,null,null,103,202,201,-1]
```

---

### Code

```python
class CustomStack:

    def __init__(self, maxSize: int):             
        """
        Initializes the custom stack with a given maximum size.
        `maxSize`: Maximum capacity of the stack.
        """
        self.maxSize = maxSize        # Set the maximum size of the stack
        self.top = -1                  # Index of the top element, starts at -1 (empty stack)
        
        # Initialize the stack with -1 to represent empty slots
        self.stack = [-1] * self.maxSize
        # Initialize an array to keep track of incremental values for each element
        self.increment_by = [0] * self.maxSize

    def push(self, x: int) -> None:
        """
        Pushes an element onto the stack if there is space available.
        `x`: Value to push onto the stack.
        """
        # Check if there's room in the stack before pushing
        if self.top + 1 < self.maxSize:
            self.top += 1                  # Increment the top index
            self.stack[self.top] = x       # Store the value in the stack at the new top position

    def pop(self) -> int:
        """
        Pops the top element from the stack and returns it.
        Applies any increment that was stored for that element.
        """
        if self.top >= 0:
            # Calculate the final value by adding the increment to the current value
            ans = self.stack[self.top] + self.increment_by[self.top]
            
            # Reset the increment and the stack value at the top position
            self.increment_by[self.top] = 0
            self.stack[self.top] = -1
            self.top -= 1                   # Decrease the top index to remove the top element
            
            return ans                       # Return the value that was popped
        return -1                            # Return -1 if the stack is empty (no elements to pop)
        
    def increment(self, k: int, val: int) -> None:
        """
        Increments the bottom `k` elements of the stack by the given value `val`.
        `k`: Number of elements to increment (starting from the bottom of the stack).
        `val`: The value to increment each of the `k` elements by.
        """
        # Loop through the first `k` elements or up to the current top of the stack
        for i in range(k):
            # Stop if `i` exceeds the current stack size (top of the stack) or max size
            if i == self.maxSize or i > self.top:
                break
            # Increment the corresponding value in the increment_by array
            self.increment_by[i] += val

# Your CustomStack object will be instantiated and called as such:
# obj = CustomStack(maxSize)
# obj.push(x)
# param_2 = obj.pop()
# obj.increment(k,val)
```

---