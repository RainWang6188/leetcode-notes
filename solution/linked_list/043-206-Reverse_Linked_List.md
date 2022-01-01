# 206. Reverse Linked List
## Description
Given the head of a singly linked list, reverse the list, and return the reversed list.

**Example:**
```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```
## My Solution
It has been a long time since I worked on linked list problems, so this solution is quite redundant. 

I use the pointer `new_head` to flag the end of the original linked list (i.e. the head of reversed list). Then I insert each node after the `new_head` iteratively.

```C++
ListNode* reverseList(ListNode* head) {
    if(!head || !(head->next))
        return head;
    
    int len = 1;
    ListNode *ptr = head;
    while(ptr->next) {
        ptr = ptr->next;
        len++;
    } 
    ListNode *new_head = ptr;
    
    for(int i = 1; i < len; i++) {
        int step = len - i - 1;
        ptr = head;
        for(int j = 0; j < step; j++)
            ptr = ptr->next;
        ListNode *ptr_remove = ptr;
        
        ptr = new_head;
        while(ptr->next)
            ptr = ptr->next;
        ListNode *ptr_insert = ptr;
        
        ptr_insert->next = ptr_remove;
        ptr_remove->next = nullptr;
    }

    return new_head;
}
```
## Classic Solution
Actually this can be done in-place.

We traverse the original linked list from start to end, and insert the current node into the front of the result list.

In this solution, three pointers are used:
- `cur`: points to the current node in the original list, which is also the node to be inserted.
- `prev`: points to the head of the result list, which is also the previous inserted node.
- `temp`: points to the next node to be inserted in the original list, since we change the `cur->next`, so it is necessary to record the nodes in the original list.

```C++
ListNode* reverseList(ListNode* head) {
    ListNode *cur = head, *prev = nullptr;
    while(cur) {
        ListNode *temp = cur->next;
        cur->next = prev;
        prev = cur;
        cur = temp;
    }
    return prev;
}
```