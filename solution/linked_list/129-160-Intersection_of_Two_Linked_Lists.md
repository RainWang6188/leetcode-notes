# 160. Intersection of Two Linked Lists

## Description
Given the heads of two singly linked-lists `headA` and `headB`, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return `null`.

## My Solution
I used the `std::unordered_set` to store all the nodes in one list. Then during the traversal of the other list, we always check if current node has already been stored in the set. If so, we find the intersection.


```C++
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
    unordered_set<ListNode*> pointerSet;
    ListNode* curr = headA;
    while(curr) {
        pointerSet.insert(curr);
        curr = curr->next;
    }
    
    curr = headB;
    while(curr) {
        if(pointerSet.find(curr) != pointerSet.end()) {
            return curr;
        }
        curr = curr->next;
    }
    return nullptr;
}
```

## Classic Solution
Here's a much more elegant solution.

We can use two iterations to to make sure two pointers reach the intersection node at the same time. 

In the first iteration, we will reset the pointer of one linkedlist to the head of another linkedlist after it reaches the tail node.

In the second iteration, we will move two pointers until they points to the same node. 

Our operations in first iteration will help us counteract the difference. 

So if two linkedlist intersects, the meeting point in second iteration must be the intersection point. If the two linked lists have no intersection at all, then the meeting pointer in second iteration must be the tail node of both lists, which is null
```C++
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
    ListNode *a = headA;
    ListNode *b = headB;
    while(a != b) {
        a = !a ? headB : a->next;
        b = !b ? headA : b->next;
    }
    return a;
}
```