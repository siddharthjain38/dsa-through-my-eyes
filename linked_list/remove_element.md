# Remove Elements from Linked List

This repository contains two approaches for solving the problem of removing all nodes from a singly linked list that have a specific value. The problem is as follows:

**Problem**:  
Given the head of a singly linked list and an integer `val`, remove all nodes that have `Node.val == val`, and return the new head of the list.

![Description of Image](../static/images/removelinked-list.jpg)
---

## Table of Contents

- [Approach 1: Explicitly Handling the Head Node](#approach-1-explicitly-handling-the-head-node)
- [Approach 2: Using a Dummy Node to Simplify Handling](#approach-2-using-a-dummy-node-to-simplify-handling)

---

## Approach 1: Explicitly Handling the Head Node

We start by checking if the head node has the target value (`val`). If it does, we adjust the `head` pointer to skip over the node, effectively removing it. Once that's done, we go through the rest of the list, skipping any nodes with the target `value` and making sure the links between the remaining `nodes` are updated properly. This approach treats the `head` node differently from the rest of the list, ensuring everything stays connected as it should.

### Code:

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # Initialize previous pointer as None (it will be used for linking previous nodes)
        prev = None
        # Start with the current node as the head of the list
        curr = head

        # Traverse through the linked list until we reach the end
        while curr:
            # Check if the current node's value is the value to be removed
            if curr.val == val:
                # If previous node exists, link it to the next node (skipping current node)
                if prev:
                    prev.next = curr.next
                else:
                    # If the current node is the head, update head to the next node
                    head = curr.next
            else:
                # If the value doesn't match, move the prev pointer to the current node
                prev = curr

            # Move the current pointer to the next node in the list
            curr = curr.next
        
        # Return the potentially updated head of the list
        return head
```

### Explanation of the Approach

#### 1. **Handle Head Special Case**

The first `while` loop checks if the head node itself needs to be removed (i.e., if `head.val == val`). If this condition is true, we keep updating the `head` pointer to skip over nodes that match the target value (`val`). We continue this process until the head node no longer matches the target value.

- **Why Handle the Head Special Case?**
  The head node needs special handling because it’s the first node of the list. If it matches `val`, it needs to be removed, and we need to update the `head` pointer to the next node. Without this special handling, we would not be able to correctly remove the head node.

#### 2. **Traverse the List**

After handling the head node, the second `while` loop traverses through the remaining nodes in the list. If a node's value matches `val`, we skip it by updating the `prev.next` pointer to point to `curr.next`. If the node is not to be removed, we move the `prev` pointer to `curr` and continue the traversal.

- **Pointer Update for Removal**:
  If a node’s value matches `val`, we perform the following update to skip it:
  ```python
  prev.next = curr.next

---

## Approach 2: Using a Dummy Node to Simplify Handling

in the Dummy Node approach, we add a fake “dummy” node right before the head of the list. The purpose of this dummy node is to make things easier when we need to remove nodes, even the very first one (the head). Normally, you'd have to treat the head node separately, but with the dummy node in place, everything is handled the same way. We don't have to worry about extra checks for removing the head, which simplifies the whole process. It just gives us a consistent and clean way to loop through and update the list, especially when we want to get rid of nodes that have a specific value (like val).

1. A dummy node is created and placed before the head of the list.
2. All operations like removing nodes, including the head node, are handled in the same way, without special treatment for the head.
3. The list can be updated more easily, ensuring that edge cases are handled without extra complexity.

### Code:

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # Step 1: Create a dummy node to handle edge cases uniformly
        # (e.g., when the head of the list itself needs to be removed)
        dummy = ListNode(0)
        dummy.next = head  # Set the next of dummy to the actual head of the list
        prev = dummy  # Initialize 'prev' to the dummy node to keep track of the node before current
        
        # Step 2: Traverse the linked list
        while prev.next:  # Continue as long as there's a next node
            curr = prev.next  # 'curr' points to the node after 'prev'
            
            # Step 3: If current node's value matches 'val', remove it
            if curr.val == val:
                prev.next = curr.next  # Bypass 'curr' by linking 'prev' to 'curr.next'
            else:
                prev = curr  # If current node's value doesn't match 'val', move 'prev' forward
            
            # Move 'curr' forward to the next node (not necessary since 'prev.next' changes, but kept for clarity)
            curr = curr.next

        # Step 4: Return the modified list starting from the node after dummy
        return dummy.next  # 'dummy.next' will point to the new head of the list

```

### Explanation of the Approach

#### 1. **Dummy Node**

A dummy node is created at the beginning of the list. The dummy node’s `next` pointer is set to the original `head` of the linked list. This dummy node acts as a placeholder and helps us avoid the need to explicitly check and update the head of the list, making the code cleaner and easier to follow.

- **Why Use a Dummy Node?**  
  The dummy node makes it easier to handle cases where the first node (head) needs to be removed. Without it, we would need additional logic to handle the head node separately. By using the dummy node, we can treat the head node just like any other node in the list.

#### 2. **Uniform Traversal**

Once the dummy node is added, we begin the traversal of the list starting from the dummy node (not the original head). This ensures that we can uniformly treat every node in the list, including the head node.

- **Why is it Uniform?**  
  Using the dummy node, we don't need to treat the head node as a special case anymore. We can safely skip any node, including the head, when its value matches `val`. If the current node doesn't match `val`, we move to the next node, just as we would with any other node.

#### 3. **Pointer Updates**

During the traversal, if a node’s value matches the target value (`val`), we update the previous node’s `next` pointer to point to the current node’s `next` pointer. This effectively removes the current node from the list.

- **Pointer Update for Removal**:  
  If a node's value equals `val`, we perform the following update:
  ```python
  prev.next = curr.next
