# 234. Palindrome Linked List

## Description
Given the `head` of a singly linked list, return `true` if it is a palindrome.

**Example:**
```
Input: head = [1,2,2,1]
Output: true
```

## My Solution
My solution traverse the linked list and store the value of each node in a vector.

And then compare if the values in the vector is palindromic.

```C++
bool isPalindrome(ListNode* head) {
    vector<int> val;
    ListNode* curr = head;
    while(curr) {
        val.push_back(curr->val);
        curr = curr->next;
    }
    int low = 0;
    int high = val.size() - 1;
    while(low < high && val[low] == val[high]) {
        low++;
        high--;
    }
    
    return low >= high;
}
```

## Classic Solution
Use fast and slow pointers to find the mid point of the linked list, and reverse the first half during the traversal.

Then compare the two halves node by node.

```C++
bool isPalindrome(ListNode* head) {
    ListNode *fast = head;
    ListNode *slow = head;
    ListNode *prev = nullptr;
    
    while(fast && fast->next) {
        fast = fast->next->next;
        ListNode* temp = slow;
        slow = slow->next;
        temp->next = prev;
        prev = temp;
    }
    
    if(fast)
        slow = slow->next;
    
    while(slow && prev) {
        if(slow->val != prev->val)
            return false;
        else {
            slow = slow->next;
            prev = prev->next;
        }
    }
    return true;
}
```