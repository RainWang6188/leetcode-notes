# 2. Add Two Numbers

## Description
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**
```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

## My Solution
An ordinary add operation in linked list.

Note that we don't create new nodes for each digits, which wastes a lot of time.

Another thing we need to be careful is that we have to create a tail node whose value is `1` when there's still a carry at last.

```C++
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    int carry = 0;
    
    ListNode *curr1 = l1;
    ListNode *curr2 = l2;
    ListNode dummy(INT_MIN);
    ListNode *curr = &dummy;
    
    while(curr1 && curr2) {
        int sum = carry + curr1->val + curr2->val;
        carry = sum / 10;
        curr1->val = sum % 10;
        curr->next = curr1;
        
        curr1 = curr1->next;
        curr2 = curr2->next;
        curr = curr->next;
    }
    
    curr->next = curr1 ? curr1 : curr2;
    
    ListNode *prev = curr;
    curr = curr->next;
    while(curr) {
        int sum = curr->val + carry;
        curr->val = sum % 10;
        carry = sum / 10;
        prev = curr;
        curr = curr->next;
    }
    
    if(carry) {
        prev->next = new ListNode(1);
    }
    
    return dummy.next;
}
```

## Classic Solution

A more clear way is as follows:
- Use `||` instead of `&&` in the loop condition.
- Create new node for each digits makes the code looks clearer.


```C++
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    ListNode *curr1 = l1;
    ListNode *curr2 = l2;
    ListNode dummy(INT_MIN);
    ListNode *curr = &dummy;
    
    int carry = 0;
    
    while(curr1 || curr2) {
        int sum = carry;
        if(curr1) {
            sum += curr1->val;
            curr1 = curr1->next;
        }
        
        if(curr2) {
            sum += curr2->val;
            curr2 = curr2->next;
        }
        
        carry = sum / 10;
        curr->next = new ListNode(sum % 10);
        curr = curr->next;
    }
    
    if(carry) {
        curr->next = new ListNode(1);
    }
    return dummy.next;
}
```