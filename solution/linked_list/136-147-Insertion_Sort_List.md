# 147. Insertion Sort List

## Description
Given the `head` of a singly linked list, sort the list using **insertion sort**, and return the sorted list's head.

The steps of the insertion sort algorithm:

- Insertion sort iterates, consuming one input element each repetition and growing a sorted output list.
- At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list and inserts it there.
- It repeats until no input elements remain.

**Example:**
```
Input: head = [4,2,1,3]
Output: [1,2,3,4]
```

## My Solution
An ordinary insertion sort for linked list.

Note that we should preserve the `curr->next` before each  insertion and set that value to `curr` before next iteration.

```C++
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        if(!head)
            return nullptr;
        
        ListNode dummy(INT_MIN);
        
        ListNode *curr = head;
        while(curr) {
            ListNode *temp = curr->next;
            ListNode *p = &dummy;
            while(p) {
                if(p->val <= curr->val && (!p->next || p->next->val >= curr->val))
                    break;
                p = p->next;
            }
            insertNode(p, curr);
            curr = temp;
        }
        return dummy.next;
    }
private:
    void insertNode(ListNode* beforeNode, ListNode* newNode) {
        newNode->next = beforeNode->next;
        beforeNode->next = newNode;
    }
};
```