# Copy List with Random Pointer

### **Problem:**
- You are given a linked list A
- Each node in the linked list contains two pointers: a next pointer and a random pointer 
- The next pointer points to the next node in the list 
- The random pointer can point to any node in the list, or it can be NULL 
- Your task is to create a deep copy of the linked list A 
- The copied list should be a completely separate linked list from the original list, but with the same node values and random pointer connections as the original list 
- You should create a new linked list B, where each node in B has the same value as the corresponding node in A 
- The next and random pointers of each node in B should point to the corresponding nodes in B (rather than A)

![remove_nth_node_from_end.jpg](../static/images/copy_list_with_random_pointer.png)

#### **Example:**
Given the list: `1 -> 2 -> 3 -> 4 -> 5`, and `B = 2`, after removing the second node from the end, the list becomes: `1 -> 2 -> 3 -> 5`.

---

### **Step-by-Step Approach:**

### Step 1: Create a copy of each node and interweave the new nodes with the original list.

- We go through each node of the original list.
- For each node, we create a **new node** that has the same value as the original node.
- We **insert** this new node right after the original node in the linked list.
  
#### After this step, the list will look like this:

```
Original node -> New node (copy) -> Original node -> New node (copy) -> ...
```

#### We interweave the new nodes with the original ones so that both the original and copied nodes are adjacent.

```python
curr = head
while curr:
    new_node = Node(curr.val, curr.next)  # Create new node with same value as current node
    curr.next = new_node  # Insert new node after the original node
    curr = new_node.next  # Move to the next original node
```

### Step 2: Set the random pointers for the new nodes.

- Now, we need to set the random pointers of the newly created nodes.
- The random pointer of a new node should point to the copy of the node that the original node’s random pointer was pointing to.


#### We do this by checking the random pointer of the original node. If it exists, we set the random pointer of the new node to be the node after the original node’s random node (since we inserted the copied node right after the original node).

```python
curr = head
while curr:
    if curr.random != None:
        curr.next.random = curr.random.next  # Set the random pointer of the new node
    curr = curr.next.next  # Move to the next original node (skip over the new node)
```

### Step 3: Separate the interwoven list into two separate lists (original and copied).

- Now that the new nodes have been created and their random pointers are set, we need to separate the interwoven list back into two individual lists:
  - The original list (which should contain only the original nodes).
  - The new copied list (which should contain only the copied nodes).

**To do this, we:**

- Keep track of the original list's head and the new copied list's head.
- Restore the next pointers for the original list.  
- Separate the two lists by adjusting the next pointers accordingly.

```python
new_list = head.next  # The head of the copied list starts right after the original head
prev = head  # Pointer to the current original node
curr = head.next  # Pointer to the first copied node
while curr:
    prev.next = curr.next  # Restore the original list's next pointer
    prev = curr  # Move prev to the copied node
    curr = curr.next  # Move curr to the next copied node
```

---

### **Code Explanation**:

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def copyRandomList(self, head):
        # If the input head is None (empty list), simply return None
        if not head:
            return None

        # Step 1: Create a copy of each node and interweave the new nodes with the original list.
        curr = head
        while curr:
            # Create a new node with the same value as the current node
            new_node = Node(curr.val, curr.next)  # new_node's next pointer initially points to the original next
            curr.next = new_node  # Insert the new node immediately after the current node
            curr = new_node.next  # Move to the next original node (skipping the new node)

        # Step 2: Set the random pointers for the new nodes.
        curr = head
        while curr:
            # If the original node has a random pointer, set the new node's random pointer
            if curr.random != None:
                curr.next.random = curr.random.next  # The new node's random pointer points to the copied random node
            curr = curr.next.next  # Move to the next original node (skipping the newly inserted node)

        # Step 3: Separate the interwoven list into two separate lists (original and copied).
        new_list = head.next  # The head of the new copied list (which starts right after the original head)
        prev = head  # Pointer to the current original node
        curr = head.next  # Pointer to the first copied node
        while curr:
            # Restore the original list's next pointer by skipping over the copied nodes
            prev.next = curr.next
            prev = curr  # Move prev to the copied node
            curr = curr.next  # Move curr to the next copied node
        
        # Return the head of the new copied list
        return new_list
```

---