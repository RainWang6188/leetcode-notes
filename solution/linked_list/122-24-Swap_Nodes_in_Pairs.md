# 24. Swap Nodes in Pairs

## Description
Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

**Example:**
```
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

## My Solution
The swap process itself is not difficult, but we need to keep record of the previous tail node, since we have to link the current latter one after that tail node.

```C++
ListNode* swapPairs(ListNode* head) {
    ListNode dummy(INT_MIN);
    dummy.next = head;
    ListNode* curr = head;
    ListNode* prev = &dummy;

    while(curr && curr->next) {
        ListNode* follow = curr->next;
        ListNode* temp = follow->next;

        prev->next = follow;            
        follow->next = curr;
        curr->next = temp;
        prev = curr;
        curr = temp;
    }
    return dummy.next;
}
```

## Classic Solution

Here's an recursive version of the solution

```C++
ListNode* swapPairs(ListNode* head) {
    if(!head || !head->next)
        return head;
    
    ListNode* follow = head->next;
    head->next = swapPairs(head->next->next);
    follow->next = head;
    return follow;
}
```