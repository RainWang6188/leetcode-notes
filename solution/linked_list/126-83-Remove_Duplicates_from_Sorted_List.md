# 83. Remove Duplicates from Sorted List

## Description
Given the head of a **sorted** linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.


# My Solution
Since the linked list is sorted, it is easy to find the duplicates by simply comparing the value of the current node with the value of the previous node.

```C++
ListNode* deleteDuplicates(ListNode* head) {
    ListNode dummy(INT_MIN);
    dummy.next = head;
    ListNode* prev = &dummy;
    ListNode* curr = head;
    
    while(curr) {
        if(curr->val == prev->val)
            prev->next = curr->next;
        else 
            prev = curr;
        curr = curr->next;
    }
    return dummy.next;
}
```

Note about **freeing memory**. We need to free memory when we delete a node. But don't use `delete node` construct on an interview without discussing it with the interviewer. A list node can be allocated in many different ways and we can use `delete node` only if we are sure that the nodes were allocated with `new TreeNode(...)`.

