# 25. Reverse Nodes in k-Group

## Description
Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return the modified list.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

**Example:**
```
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
```

## My Solution
Use a variable `prevTail` to store the pointer of the previous tail node. Perform reverse operations for `k` nodes, and connect the `prevTail` to the current head node.

If the number of reverse operations is less than `k` (has left-out nodes), then we shoud reverse the left-out nodes back.

```C++
ListNode* reverseKGroup(ListNode* head, int k) {
    if(!head)
        return nullptr;
    
    ListNode dummy(INT_MIN);

    ListNode *prevTail = &dummy;
    ListNode *curr = nullptr;
    ListNode *prev = nullptr;
    ListNode *temp = head;
    
    while(1) {
        prev = nullptr;
        curr = temp;
        ListNode *currTail = curr;
        int count = 0;
        while(curr && count < k) {
            temp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = temp;
            count++;
        }
        if(count == k) {
            prevTail->next = prev;
            prevTail = currTail;
        }
        else {
            curr = prev;
            prev = nullptr;
            while(count--) {
                temp = curr->next;
                curr->next = prev;
                prev = curr;
                curr = temp;
            }
            prevTail->next = prev;
            break;
        }
    }
    return dummy.next;
}
```