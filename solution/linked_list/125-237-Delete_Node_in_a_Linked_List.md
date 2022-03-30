# 237. Delete Node in a Linked List

## Description
Write a function to delete a `node` in a singly-linked list. You will **not** be given access to the head of the list, instead you will be given access to **the node to be deleted directly**.

It is guaranteed that the node to be deleted is **not a tail node** in the list.

**Example:**
```
Input: head = [4,5,1,9], node = 5
Output: [4,1,9]
Explanation: You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function.
```

## My Solution
My soluiton is sure not optimal. I change the value of each node by passing the value of next node to the current node.

```C++
void deleteNode(ListNode* node) {
    ListNode* curr = node;
    while(curr->next->next) {
        curr->val = curr->next->val;
        curr = curr->next;
    }
    curr->val = curr->next->val;
    curr->next = nullptr;
}
```


## Classic Solution
Here's a more direct way of changing the content of `node` structure.

Here, the deference of pointers is necessary.

```C++
void deleteNode(ListNode* node) {
    ListNode* temp = node->next;
    *node = *temp;
    delete temp;
}
```