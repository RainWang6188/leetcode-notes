# 86. Partition List

## Description
Given the `head` of a linked list and a value `x`, partition it such that all nodes **less** than `x` come before nodes **greater than or equal** to `x`.

You should preserve the original relative order of the nodes in each of the two partitions.

**Example:**
```
Input: head = [1,4,3,2,5,2], x = 3
Output: [1,2,2,4,3,5]
```
## My Solution
We store the nodes whose values are less than `x` and whose values are greater than or equal to `x` respectively starting from sentinal head `dummyLess` and `dummyMore`.

Connect them after all the nodes have been traversed.

**Note** that the list of `dummyLess` may be empty, so we should take that into consideration when performing the concatenation.

```C++
ListNode* partition(ListNode* head, int x) {
    if(!head)
        return nullptr;
    
    ListNode dummyLess(0);
    ListNode dummyMore(0);
    
    ListNode *tailLess = &dummyLess;
    ListNode *tailMore = &dummyMore;
    
    ListNode *curr = head;
    while(curr) {
        ListNode *temp = curr->next;
        curr->next = nullptr;
        if(curr->val < x) {
            tailLess->next = curr;
            tailLess = tailLess->next;
        }
        else {
            tailMore->next = curr;
            tailMore = tailMore->next;
        }
        curr = temp;
    }
    if(tailLess == &dummyLess)
        return dummyMore.next;
    
    ListNode dummy(0);
    dummy.next = dummyLess.next;
    tailLess->next = dummyMore.next;
    
    return dummy.next;
}
```