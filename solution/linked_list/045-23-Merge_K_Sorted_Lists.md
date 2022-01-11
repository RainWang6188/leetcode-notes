# 23. Merge K Sorted Lists
## Description
You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

**Example:**
```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```
## My Solution
This is my naive solution, which keeps a record of ptrs of all the lists in an vector, and advances the one with minimum value. It uses no extra space since it strings all the nodes in one needle.
```C++
ListNode* mergeKLists(vector<ListNode*>& lists) {
    int k = lists.size();
    ListNode dummy(INT_MIN);
    ListNode* new_list = &dummy;
    int prev_index = -1;
    int min_index = -1;
    
    vector<ListNode*> ptrs(k, nullptr);
    for(int i = 0; i < k; i++)
        ptrs[i] = lists[i];
    
    while(1) {
        bool all_null = true;
        for(auto ptr : ptrs)
            if(ptr) {
                all_null = false;
                break;
            }
        if(all_null)
            break;
        
        int min_value = 10000;
        for(int i = 0; i < k; i++) {
            if(!ptrs[i])
                continue;
            else {
                if(ptrs[i]->val <= min_value) {
                    min_value = ptrs[i]->val;
                    min_index = i;
                }
            }
        }
        
        if(prev_index == min_index) 
            new_list = new_list->next;
        else {
            new_list->next = ptrs[min_index];
            new_list = new_list->next;
        }
        ptrs[min_index] = ptrs[min_index]->next;
        prev_index = min_index;
    }
    return dummy.next;
}
```
## Classic Solution
