# Reverse Linked List

---

### Rule of Thumb for Reversing a Linked List

1. **Start with three pointers**: Think of three helpers to guide you through the list:
   - `prev`: This starts as nothing (like an empty placeholder).
   - `current`: This starts at the very first node in the list.
   - `next`: This is a temporary marker to hold the next node, so you don’t lose track as you go.

2. **Go through the list and reverse the links**: As you move through the list, you need to reverse the direction of each node’s link. Normally, each node points to the next one, but you’ll make it point to the one before it instead. For this, you:
   - Save the `next` node (so you don’t lose it),
   - Reverse the `current` node’s link to point to `prev`,
   - Then, move the `prev` and `current` pointers forward by one step.

3. **Return `prev` as the new head**: After you've flipped all the links, `prev` will be pointing to what used to be the last node in the list. This node is now the first node in the reversed list, so `prev` becomes the new head, and that’s what you return.

In simple terms, you're "rewriting" the arrows in the list so that each node points backwards instead of forwards, and the last node becomes the first.

---

### **Problem:**
Given the head of a singly linked list, reverse the list, and return the reversed list.

![reverse_linked_list.jpg](../static/images/reverse_linked_list.jpg)

---

### **Code Explanation**:

```python
class ListNode:
    def __init__(self, val=None, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseList(self, head):
        prev = None
        current = head
        
        while current:
            next_node = current.next  # Store next node
            current.next = prev       # Reverse the current node's pointer
            prev = current            # Move prev and current one step forward
            current = next_node
        
        return prev  # prev will be the new head of the reversed list
```

---

# Reverse Linked List II

### **Problem:**
Given the head of a singly linked list and two integers left and right where left <= right, reverse the nodes of the list from position left to position right, and return the reversed list.

![remove_nth_node_from_end.jpg](../static/images/reverse_linked_list_II.jpg)

---

## Rule of Thumb for Deleting a Node in a Linked List

### **Approach:**

#### **Steps to Solve:**

---

### **Code Explanation**:

```python
class ListNode:
    def __init__(self, val=None, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverse_between(self, head, left, right):
        # Edge case: if the list is empty or no need to reverse (left == right)
        # If the list is empty or if the sublist to reverse has no length (left == right), 
        # we don't need to do any processing, so we simply return the head as it is.
        if not head or left == right:
            return head
        
        # Initialize a dummy node to simplify edge cases where left == 1
        # The dummy node acts as a placeholder to easily handle the head of the list
        # when reversing a segment that starts at the very beginning of the list.
        dummy = ListNode(None)
        dummy.next = head  # The dummy node's next points to the original head.
        
        # Move left_node to the node just before the `left`-th node
        # left_node is the node that will point to the first node of the reversed sublist.
        left_node = dummy  # Start with the dummy node (which is before the head).
        for _ in range(left - 1):  # Move left_node to the (left-1)-th node.
            left_node = left_node.next  # This loop stops when left_node points to the node before the left-th node.

        # After this loop, left_node is the node just before the left-th node in the original list.
        # `sublist_tail` is the node at the left-th position, which will eventually point to the node after the reversed sublist.
        sublist_tail = left_node.next
        
        # `current` is the first node to be reversed.
        current = left_node.next
        
        # Reverse the sublist between left and right
        prev = None  # This variable will track the previous node as we reverse the sublist.
        # Iterate over the sublist to reverse the nodes in place
        for _ in range(right - left + 1):  # We need to reverse (right - left + 1) nodes.
            next_node = current.next  # Hold the next node because we're going to change current.next.
            current.next = prev  # Reverse the direction of the current node by pointing it to the previous node.
            prev = current  # Move prev to the current node to track the reversed portion.
            current = next_node  # Move to the next node in the sublist.
        
        # After the loop, `prev` points to the new head of the reversed sublist.
        # `current` now points to the node right after the reversed sublist, which might be None if we reversed till the end.
        
        # Connect the reversed sublist back into the list.
        # `left_node.next` should now point to the new head of the reversed sublist, which is `prev`.
        left_node.next = prev
        
        # `sublist_tail` now points to the node right after the reversed sublist.
        # We connect it to `current`, which is the node after the reversed sublist.
        sublist_tail.next = current
        
        # Return the new head of the list. 
        # Since we created a dummy node, the new head is at dummy.next.
        return dummy.next
```

### External Links 

- ### [Reverse Linked List 2 (LeetCode 92) | Full simplified solution](https://www.youtube.com/watch?v=oDL8vuu2Q0E&t=1021s)

---

#  Reverse Nodes in k-Group

### **Problem:**
Given the head of a linked list, reverse the nodes of the list k at a time, and return the modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes, in the end, should remain as it is.

![remove_nth_node_from_end.jpg](../static/images/reverse_k_group1.jpg)

Given the list: `1 -> 2 -> 3 -> 4 -> 5`, and `k = 2`, after removing the second node from the end, the list becomes: `2 -> 1 -> 4 -> 3 -> 5`.

![remove_nth_node_from_end.jpg](../static/images/reverse_k_group2.jpg)

Given the list: `1 -> 2 -> 3 -> 4 -> 5`, and `k = 3`, after removing the second node from the end, the list becomes: `3 -> 2 -> 1 -> 4 -> 5`.

---

### **Code Explanation**:

```python
class ListNode:
    def __init__(self, val=None, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverse_group(self, head, k):
        # Create a dummy node to simplify edge case handling (e.g., when head is modified)
        dummy = ListNode(None)
        dummy.next = head
        prev_group_end = dummy  # This will point to the node before the start of the current group
        
        # Step 1: Count the total number of nodes in the list
        length = 0
        curr = head
        while curr:
            length += 1  # Traverse the list and count the nodes
            curr = curr.next

        # Step 2: Reverse the list in groups of k
        curr = head  # Reinitialize curr to the head of the list
        for _ in range(length // k):  # Iterate for the number of full groups of k nodes
            group_start = curr  # The first node in the current group
            prev = None  # This will eventually become the new head of the reversed group
            
            # Reverse the k nodes in the current group
            for _ in range(k):
                next_node = curr.next  # Save the next node before changing pointers
                curr.next = prev  # Reverse the current node's pointer
                prev = curr  # Move prev to the current node
                curr = next_node  # Move to the next node in the group
            
            # After reversing the group, connect the previous part of the list
            prev_group_end.next = prev  # The end of the previous group should point to the new head of the    reversed group
            group_start.next = curr  # The start of the reversed group should point to the next part of the list (or None)
            
            # Move prev_group_end to the end of the current reversed group
            prev_group_end = group_start
```