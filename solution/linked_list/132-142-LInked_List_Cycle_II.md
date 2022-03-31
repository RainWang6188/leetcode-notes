# 142. Linked List Cycle II

## Description
Given the `head` of a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, `pos` is used to denote the index of the node that tail's next pointer is connected to (0-indexed). It is `-1` if there is no cycle. Note that pos is not passed as a parameter.

Do not modify the linked list.

**Example:**
```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

## My Solution

I used the Robert W. Floyd's [tortoise and hare algorithm](https://en.wikipedia.org/wiki/Cycle_detection) to solve this question.

Note that we should add `hare != turtle => return nullptr` after the first part, to make sure a loop exists.

```C++
ListNode *detectCycle(ListNode *head) {
    ListNode *turtle = head;
    ListNode *hare = head;
    
    while(hare) {
        turtle = turtle->next;
        
        if(hare->next)
            hare = hare->next->next;
        else
            return nullptr;
        
        if(turtle == hare)
            break;
    }
    if(hare != turtle)
        return nullptr;
    
    hare = head;
    while(turtle != hare) {
        turtle = turtle->next;
        hare = hare->next;
    }
    
    return turtle;
}
```