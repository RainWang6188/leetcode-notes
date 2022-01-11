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

Time Complexity: $O(NK)$; Space Complexity:$O(1)$
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
### 1. Optimize by Priority Queue
In my solution, the time complexity is $O(KN)$, since the comparison in each selection costs $O(K)$ ($K-1$ comparisons).

As a result, we can use the priority queue to speed up the comparison process, which costs $O(\log K)$.

The code below employs the features of C++11, such as LAMBDA expression.

Time Compelxity: $O(N\log K)$; Space Complexity: $O(1)$

```C++
ListNode* mergeKLists(vector<ListNode*>& lists) {
    auto comp = [&](ListNode *a, ListNode *b) {
        return a->val > b->val;
    };
    priority_queue<ListNode*, vector<ListNode*>, decltype(comp)> pq(comp);

    for(auto list : lists)
        if(list)
            pq.push(list);

    ListNode dummy(INT_MIN);
    ListNode *tail = &dummy;

    while(!pq.empty()) {
        auto temp = pq.top();
        pq.pop();

        tail->next = temp;
        tail = tail->next;

        if(temp->next)
            pq.push(temp->next);
    }
    tail->next = nullptr;
    return dummy.next;
}
```
### 2. Using Map
Anther implementation is to use the map structure. We can store all the nodes in the **ordered_map** structure, with the second value as the frequency. Then simply iterate over that map and keep creating new nodes for every value.
```C++
ListNode* mergeKLists(vector<ListNode*>& lists) {
    map<int, int> dict;
    for(auto list : lists) 
        while(list) {
            dict[list->val]++;
            list = list->next;
        }
    
    ListNode dummy(INT_MIN);
    ListNode *tail = &dummy;

    for(auto it : dict) {
        while(it.second--) {
            ListNode *new_node = new ListNode(it.first);
            tail->next = new_node;
            tail = tail->next;
        }
    }
    return dummy.next;
}
```

### 3. Merge with Divide and Conquer
Another very clever solution is to use the divide and conquer.

It merges the two lists from front and end, and keeps this procedure until there's only one list in the `lists`.

```C++
ListNode* mergeKLists(vector<ListNode*>& lists) {
    if(lists.empty())
        return nullptr;
    
    int k = lists.size();
    while(k > 1) {
        for(int i = 0; i < k / 2; i++)
            lists[i] = merge2Lists(lists[i], lists[k - i - 1]);
        k = (k + 1) / 2;
    }
    return lists.front();
}

ListNode* merge2Lists(ListNode *list1, ListNode *list2) {
    ListNode dummy(INT_MIN);
    ListNode *tail = &dummy;

    while(list1 && list2) {
        if(list1->val < list2->val) {
            tail->next = list1;
            tail = tail->next;
            list1 = list1->next;
        }
        else {
            tail->next = list2;
            tail = tail->next;
            list2 = list2->next;
        }
    }

    tail->next = list1 ? list1 : list2;
    return dummy.next;
}
```