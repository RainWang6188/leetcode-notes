# 61. Rotate List

## Description
Given the `head` of a linked list, rotate the list to the right by `k` places.

**Example:**
```
Input: head = [1,2,3,4,5], k = 2
Output: [4,5,1,2,3]
```
## My Solution
- Traverse the linked list and find the `length` of the list
- Then use fast and slow pointers to find the cut point, connect the latter part to the front of the list.

Here's a quite error prone part of this solution
```C++
k = (k + 1) % length == 0 ? length : (k + 1) % length;
```

Since this add one operation may cause k set to 0 so that `fast` will not move while we want it points to the `nullptr` at the end of the list.


```C++
ListNode* rotateRight(ListNode* head, int k) {
    if(!head)
        return head;
    
    ListNode *curr = head;
    int length = 0;
    while(curr) {
        length++;
        curr = curr->next;
    }
    
    if(k % length == 0)
        return head;
    
    k = (k + 1) % length == 0 ? length : (k + 1) % length;
    
    ListNode *fast = head;
    ListNode *slow = head;
    while(k--) {
        fast = fast->next;
    }
    
    while(fast) {
        fast = fast->next;
        slow = slow->next;
    }
    
    ListNode *newHead = slow->next;
    slow->next = nullptr;
    curr = newHead;
    while(curr->next) {
        curr = curr->next;
    }
    
    curr->next = head;
    return newHead;
}
```

## Classic Solution
Here's a more concise version of code.
```C++
ListNode* rotateRight(ListNode* head, int k) {
    if(!head)
        return nullptr;
    
    ListNode *curr = head;
    int length = 1;
    while(curr->next) {
        length++;
        curr = curr->next;
    }
    
    curr->next = head;
    
    k %= length;
    int move = length - k;
    while(move--) {
        curr = curr->next;
    }
    
    ListNode *newHead = curr->next;
    curr->next = nullptr;
    return newHead;
}
```