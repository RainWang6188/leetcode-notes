# 328. Odd Even Linked List

## Description
Given the head of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return the reordered list.

The **first** node is considered **odd**, and the **second** node is **even**, and so on.

Note that the relative order inside both the even and odd groups should remain as it was in the input.

You must solve the problem in `O(1)` extra space complexity and `O(n)` time complexity.

**Example:**
```
Input: head = [2,1,3,5,6,4,7]
Output: [2,3,6,7,1,5,4]
```

## My Solution
We store the nodes with odd indices and even indices respectively. And then connect them together.

Note that we need to set the tail of the even list as `nullptr`, otherwise, dead loop will occur.

```C++
ListNode* oddEvenList(ListNode* head) {
    ListNode dummyOdd(INT_MIN);
    ListNode dummyEven(INT_MIN);
    
    ListNode* tailOdd = &dummyOdd;
    ListNode* tailEven = &dummyEven;
    ListNode* curr = head;
    int count = 1;
    
    while(curr) {
        if(count % 2) {
            tailOdd->next = curr;
            tailOdd = tailOdd->next;
        }
        else {
            tailEven->next = curr;
            tailEven = tailEven->next;
        }
        curr = curr->next;
        count++;
    }
    tailEven->next = nullptr;
    tailOdd->next = dummyEven.next;
    return dummyOdd.next;
}
```