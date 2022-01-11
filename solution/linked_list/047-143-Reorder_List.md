# 143. Reorder List

## Description
You are given the head of a singly linked-list. The list can be represented as:
```
L0 → L1 → … → Ln - 1 → Ln
```
Reorder the list to be on the following form:
```
L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
```
You may not modify the values in the list's nodes. Only nodes themselves may be changed.

**Example:**
```
Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]
```

## My Solution
I keep all the values in a vector and generate new nodes according to the request, linking them one by one.

```C++
void reorderList(ListNode* head) {
    vector<int> value;
    ListNode *ptr = head;
    while(ptr) {
        value.push_back(ptr->val);
        ptr = ptr->next;
    }
    
    int n = value.size();
    if(n < 2)
        return;
    
    ListNode *tail = head;
    head->next = new ListNode(value[n - 1]);
    tail = tail->next;
    for(int i = 1; i < (n + 1) / 2; i++) {
        ListNode *new_node_front = new ListNode(value[i]);
        ListNode *new_node_end = new ListNode(value[n - i - 1]);
        tail->next = new_node_front;
        tail = tail->next;
        if(i == n - i - 1)
            break;
        tail->next = new_node_end;
        tail = tail->next;
    }
}
```

## Classic Solution
Another solution uses two pointers and solves this questions in 3 steps:

1. Find the middle of the list
2. Reverse the second part of the list

    (e.g. 1->2->3->4 becomes 1->2->4->3) 
3. Merge one by one

```C++
void reorderList(ListNode* head) {
    if(head == nullptr || head->next == nullptr)
        return;
    
    ListNode *ptr1 = head, *ptr2 = head;
    while(ptr2->next && ptr2->next->next) {
        ptr1 = ptr1->next;
        ptr2 = ptr2->next->next;
    }
    
    ListNode *mid = ptr1;
    ListNode *cur = mid->next;
    ListNode *prev = nullptr;
    while(cur) {
        ListNode *temp = cur->next;
        cur->next = prev;
        prev = cur;
        cur = temp;
    }
    mid->next = nullptr;

    ptr1 = head;
    ptr2 = prev;
    while(ptr1 && ptr2) {
        ListNode *temp1 = ptr1->next;
        ListNode *temp2 = ptr2->next;
        ptr1->next = ptr2;
        ptr2->next = temp1;
        ptr1 = temp1;
        ptr2 = temp2;
    }
}
```