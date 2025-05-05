# Remove Nth Node From End of List

### **Problem:**
You are given a linked list and an integer `B`. Your task is to remove the B-th node from the end of the linked list and return the modified list.

![remove_nth_node_from_end.jpg](../static/images/remove_nth_node_from_end.jpg)

#### **Example:**
Given the list: `1 -> 2 -> 3 -> 4 -> 5`, and `B = 2`, after removing the second node from the end, the list becomes: `1 -> 2 -> 3 -> 5`.

---

## Rule of Thumb for Deleting a Node in a Linked List

**The two-pointer technique, also known as the fast and slow pointer approach (or tortoise and hare algorithm), is a useful method for problems where you need to remove an element from the x-th position from the end of a linked list.**

Here’s how it works:

- You use two pointers: one (the fast pointer) moves ahead by x steps, while the other (the slow pointer) starts at the head of the list.
- Once the fast pointer is x steps ahead, both pointers start moving one step at a time.
- When the fast pointer reaches the end of the list, the slow pointer will be at the x-th element from the end, which is the one you want to remove.
This approach is efficient because it allows you to find the target element in just one pass through the list.

### **Approach:**

**The problem can be solved efficiently in **one pass** using the **two-pointer technique** (also known as the **fast and slow pointer approach**), without using extra space (constant space).**

#### **Steps to Solve:**

1. **Use two pointers** (`fast` and `slow`), both starting at the head of the list.
   
   - The `fast` pointer will be used to move **B steps ahead** from the head to create a gap between `fast` and `slow`.
   - The `slow` pointer will eventually point to the node just before the one that needs to be removed.

2. **Move the `fast` pointer**:
   - First, move the `fast` pointer **B steps ahead** so that the gap between `fast` and `slow` becomes B nodes.

3. **Move both pointers**:
   - After the initial step, move both `fast` and `slow` pointers one step at a time until the `fast` pointer reaches the end of the list. This ensures that the `slow` pointer is positioned just before the node to be removed.

4. **Remove the B-th node from the end**:
   - Once the `slow` pointer is in position, adjust its `next` pointer to skip the node after it (i.e., the node to be deleted).

5. **Handle Edge Cases**:
   - If `B` is greater than the size of the list, this means we need to remove the head node. The dummy node technique will make sure this is handled smoothly by allowing you to always work with a non-null node before the head.

6. **Return the result**:
   - Return `dummy.next`, which is the new head of the modified list (it may or may not be the same as the original head).

---

### **Code Explanation**:

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def remove_nth_from_end(self, head: ListNode, B: int) -> ListNode:
        # Create a dummy node to handle edge cases like removing the head
        dummy = ListNode(0)
        dummy.next = head
        slow = dummy  # Slow pointer will track the node before the one to remove
        fast = dummy  # Fast pointer will create the gap with slow

        # Move the fast pointer B steps ahead
        for _ in range(B):
            if fast.next:
                fast = fast.next

        # Move both pointers until fast reaches the end
        while fast.next:
            slow = slow.next
            fast = fast.next

        # Remove the node after the slow pointer (the B-th node from the end)
        slow.next = slow.next.next
        
        # Return the head of the modified list
        return dummy.next
```

---

### The fast and slow pointer technique is most commonly used with linked lists for problems like:

- **Detecting cycles:** If there's a cycle in the list, the fast pointer will eventually meet the slow pointer.
- **Finding the middle element:** The slow pointer will be at the middle when the fast pointer reaches the end.
- **Finding the N-th node from the end:** The fast pointer is moved N steps ahead, and then both pointers move together to find the node from the end.