# 369. Plus One Linked List

## Description
Given a non-negative integer represented as non-empty a singly linked list of digits, plus one to the integer.

You may assume the integer do not contain any leading zero, except the number 0 itself.

The digits are stored such that the most significant digit is at the head of the list.

**Example:**
```
Input: 1 -> 2 -> 3 -> null
Output: 1 -> 2 -> 4 -> null
Explanation: 123 + 1 = 124
```

## My Solution

Here's my procedure of solving this question
1. Reverse the linked list, so that the lowest significant bit is the header of the linked list.
2. Perform plus one operation and add use `carry` variable to record if there's a carry currently.
3. Reverse back the linked list. Add a new node whose value is `1` if `carry` is set.

```C++
class Solution {
public:
    /**
     * @param head: the first Node
     * @return: the answer after plus one
     */
    ListNode* plusOne(ListNode *head) {
        if (!head)
            return nullptr;
        
        ListNode* reverseHead = reverseList(head);
        bool carry = true;
        ListNode* curr = reverseHead;
        while(carry && curr) {
            curr->val = (curr->val + 1) % 10;
            if(curr->val)
                carry = false;
            curr = curr->next;
        }

        head = reverseList(reverseHead);
        if(carry) {
            ListNode* newHead = new ListNode(1);
            newHead->next = head;
            head = newHead;
        }
        return head; 
    }

private:
    ListNode* reverseList(ListNode* head) {
        ListNode* curr = head;
        ListNode* prev = nullptr;
        while(curr) {
            ListNode* temp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = temp;
        }
        return prev;
    }
};
```

## Classic Solution
A more efficient way to implement plus one operation is as follows:

1. Find the rightmost node whose value is not `9`, marked its pointer as `notNine`.
2. If all nodes in the linked list are `9`s (i.e. `notNine == nullptr`), then we create a new header node whose value is `1`. Otherwise, we increment the `notNine->val`.
3. Set the value of all nodes after `notNine` to `0`.

```C++
ListNode* plusOne(ListNode *head) {
    if (!head)
        return nullptr;
    ListNode* curr = head;
    ListNode* notNine = nullptr;
    while(curr) {
        if(curr->val != 9)
            notNine = curr;
        curr = curr->next;
    }

    if(!notNine) {
        ListNode* newHead = new ListNode(1);
        newHead->next = head;
        head = newHead;
        notNine = head;
    } 
    else {
        notNine->val++;
    }

    curr = notNine->next;
    while(curr) {
        curr->val = 0;
        curr = curr->next;
    }
    return head;
}
```