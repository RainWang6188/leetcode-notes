# 92. Reverse Linked List

## Description
Given the head of a singly linked `list` and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return the reversed list.

**Example:**
```
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]
```

## My Solution
We first traverse the original linked list to find the node before the `left` position (marked as `beforeRev`) and the node after the `right` position (marked as `afterRev`). And then, we perform an ordinary reverse operation.

```C++
ListNode* reverseBetween(ListNode* head, int left, int right) {
    ListNode dummy(INT_MIN);
    dummy.next = head;
    ListNode* curr = &dummy;
    int count = 1;
    
    ListNode* beforeRev = nullptr;
    ListNode* afterRev = nullptr;
    
    while(count <= right + 1) {
        if(count == left)
            beforeRev = curr;
        curr = curr->next;
        count++;
    }
    afterRev = curr;
    
    ListNode* prev = afterRev;
    curr = beforeRev->next;
    while(curr != afterRev) {
        ListNode* temp = curr->next;
        curr->next = prev;
        prev = curr;
        curr = temp;
    }
    
    beforeRev->next = prev;
    
    return dummy.next;
}
```