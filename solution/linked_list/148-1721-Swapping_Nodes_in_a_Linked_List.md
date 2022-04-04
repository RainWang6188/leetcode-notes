# 1721 Swapping Ndoes in a Linked List

## Description
You are given the `head` of a linked list, and an integer `k`.

Return the `head` of the linked list after **swapping** the values of the `kth` node from the beginning and the `kth` node from the end (the list is **1-indexed**).

## My Solution

A strict nodes swapping procedure.

There're two tips we need to be careful:
- `k` may be larger than the mid (i.e. swapping the $4^{th}$ node in a linked list with length `5` is equal to swapping the $2^{th}$ node)
- The swapped nodes may be identical or adjacent neighbors (i.e. `node1 == node2` or `node1->next == node2`) or they're not directly linked.

```C++
ListNode* swapNodes(ListNode* head, int k) {
    ListNode dummy(INT_MIN);
    dummy.next = head;
    
    int len = 0;
    ListNode *curr = head;
    while(curr) {
        len++;
        curr = curr->next;
    }
    int mid = (len + 1) / 2;
    if(k > mid)
        k = len - k + 1;
    
    
    int start = k - 1;
    int end = k + 1;
    ListNode *front = &dummy;
    ListNode *fast = &dummy;
    ListNode *slow = &dummy;
    while(start--)
        front = front->next;
    ListNode *node1 = front->next;
    
    while(end--)
        fast = fast->next;
    while(fast) {
        fast = fast->next;
        slow = slow->next;
    }
    ListNode *back = slow;
    ListNode *node2 = back->next;
    
    ListNode *frontNext = node1->next;
    ListNode *backNext = node2->next;
    
    if(node1->next == node2) {
        front->next = node2;
        node2->next = node1;
        node1->next = backNext;
    }
    else {
        front->next = node2;
        node2->next = frontNext;
        back->next = node1;
        node1->next = backNext;   
    }
    
    return dummy.next;
}
```

## Classic Solution

Here's a much more elegant solution.

We store those nodes in a vector, which is easy to perform the swap. Then we link them all together.

```C++
ListNode* swapNodes(ListNode* head, int k) {
    ListNode dummy(INT_MIN);
    
    vector<ListNode*> list;
    ListNode* curr = head;
    while(curr) {
        list.push_back(curr);
        curr = curr->next;
    }
    
    swap(list[k-1], list[list.size()-k]);
    ListNode* tail = &dummy;
    for(auto& node : list) {
        tail->next = node;
        tail = tail->next;
    }
    tail->next = nullptr;
    return dummy.next;
}
```