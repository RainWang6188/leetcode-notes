# 82. Remove Duplicates from Sorted List II

## Description
Given the head of a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list. Return the linked list **sorted** as well.


## My Solution

This solution is called **Sentinel Head + Predecessor**, as described [here](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/solution/).

We use a variable `target` to record the current duplicate value by comparing the value of current node and the one after current node. If the value of current node is `target`, we delete this node, otherwise.

Note that we need to set `tail->next = nullptr` in the end.

The whole point of asking any candidates a linked list problem is to test if the candidates think about edge cases, including:

- Dereferencing Null Pointer, usually targeting tail pointer
- When given Head is None
- When there are duplications in the list

```C++
ListNode* deleteDuplicates(ListNode* head) {
    ListNode dummy(INT_MIN);
    dummy.next = head;
    ListNode* curr = head;
    ListNode* tail = &dummy;
    int target = INT_MIN;
    while(curr) {
        if(curr->next && curr->val == curr->next->val) {
            target = curr->val;
        }
        
        if(target >= 0 && curr->val == target)
            tail->next = curr->next;
        else if(curr->val != target) {
            tail->next = curr;
            tail = tail->next;
            target = INT_MIN;
        }
        
        curr = curr->next;
    }
    tail->next = nullptr;
    return dummy.next;
}
```
