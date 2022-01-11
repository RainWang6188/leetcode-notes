# 19. Remove Nth Node From End of List
## Description
Given the head of a linked list, remove the nth node from the end of the list and return its head.

**Example:**
```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```
## My Solution
Suppose the total number of nodes is $k$, then the $n^{th}$ node from end of list is the $(k-n+1)^{th}$ node from front of list.

We need to take care of the corner cases: less than 2 nodes and remove the original head node.
```C++
ListNode* removeNthFromEnd(ListNode* head, int n) {
    int total = 0;
    ListNode *ptr = head;
    while(ptr) {
        total++;
        ptr = ptr->next;
    }
    
    if(total < 2)
        return nullptr;
    
    ptr = head;
    if(total - n - 1 < 0)
        return head->next;
    
    for(int i = 0; i < total - n - 1; i++)
        ptr = ptr->next;
    
    ptr->next = ptr->next->next;
    
    return head;
}
```
## Classic Solution - One Pass
A one pass solution can be implemented using two pointers: `slow` has **n+1** steps delay with `fast`, so than when `fast` reaches the end of the list (`nullptr`), `slow` points to the node prior to the node to be deleted.

A trick here is start the traversal from a dummy head node, so than we don't need to take care of the corner cases.

```C++
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode dummy(INT_MIN);
    dummy.next = head;
    
    ListNode *fast = &dummy, *slow = &dummy;
    for(int i = 0; i < n + 1; i++)
        fast = fast->next;
    
    while(fast) {
        fast = fast->next;
        slow = slow->next;
    }
    
    slow->next = slow->next->next;
    
    return dummy.next;
}
```
