# 148. Sort List

## Description
Given the `head` of a linked list, return the list after sorting it in **ascending** order.

**Example:**
```
Input: head = [-1,5,3,4,0]
Output: [-1,0,3,4,5]
```

## My Solution
I traverse the linked list and store the pointers of all the nodes in a vector.

Then I sorted the vector according to the value of each node, and connect them in ascending order.

```C++
ListNode* sortList(ListNode* head) {
    vector<ListNode*> nodes;
    
    ListNode *curr = head;
    while(curr) {
        nodes.push_back(curr);
        curr = curr->next;
    }
    
    auto cmp = [](ListNode *node1, ListNode *node2) {
        return node1->val < node2->val;
    };
    
    sort(nodes.begin(), nodes.end(), cmp);
    ListNode dummy(INT_MIN);
    ListNode *tail = &dummy;
    for(auto& node : nodes) {
        tail->next = node;
        tail = tail->next;
    }
    tail->next = nullptr;
    return dummy.next;
}
```

## Classic Solution
### Merge Sort (Recursive Version)
We can perform a merge sort on the linked list.

However, the space complexity is not `O(1)`, but `O(log n)`, considering the space of stack frame during recursion.

```C++
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if(!head || !head->next)
            return head;
        
        ListNode *fast = head;
        ListNode *slow = head;
        ListNode *prev = nullptr;
        
        while(fast && fast->next) {
            prev = slow;
            slow = slow->next;
            fast = fast->next->next;
        }
        
        prev->next = nullptr;
        
        ListNode *l1 = sortList(head);
        ListNode *l2 = sortList(slow);
        
        return merge(l1, l2);
    }
    
private:
    ListNode *merge(ListNode *l1, ListNode *l2) {
        ListNode dummy(INT_MIN);
        ListNode *curr1 = l1;
        ListNode *curr2 = l2;
        ListNode *curr = &dummy;
        
        while(curr1 && curr2) {
            if(curr1->val < curr2->val) {
                curr->next = curr1;
                curr1 = curr1->next;
            }
            else {
                curr->next = curr2;
                curr2 = curr2->next;
            }
            curr = curr->next;
        }
        
        curr->next = curr1 ? curr1 : curr2;
        return dummy.next;
    }
};
```


### Merge Sort (Iterative Version)
To achieve the `O(1)` space comleixty, we can use the iterative version of merge sort (i.e. bottom-up merge sort).

```C++
/**
 * Merge sort use bottom-up policy, 
 * so Space Complexity is O(1)
 * Time Complexity is O(NlgN)
 * stable sort
*/
class Solution {
public:
	ListNode *sortList(ListNode *head) {
		if(!head || !(head->next)) return head;
		
		//get the linked list's length
		ListNode* cur = head;
		int length = 0;
		while(cur){
			length++;
			cur = cur->next;
		}
		
		ListNode dummy(0);
		dummy.next = head;
		ListNode *left, *right, *tail;
		for(int step = 1; step < length; step <<= 1){
			cur = dummy.next;
			tail = &dummy;
			while(cur){
				left = cur;
				right = split(left, step);
				cur = split(right,step);
				tail = merge(left, right, tail);
			}
		}
		return dummy.next;
	}
private:
	/**
	 * Divide the linked list into two lists,
     * while the first list contains first n ndoes
	 * return the second list's head
	 */
	ListNode* split(ListNode *head, int n){
		//if(!head) return NULL;
		for(int i = 1; head && i < n; i++) head = head->next;
		
		if(!head) return NULL;
		ListNode *second = head->next;
		head->next = NULL;
		return second;
	}
	/**
	  * merge the two sorted linked list l1 and l2,
	  * then append the merged sorted linked list to the node head
	  * return the tail of the merged sorted linked list
	 */
	ListNode* merge(ListNode* l1, ListNode* l2, ListNode* head){
		ListNode *cur = head;
		while(l1 && l2){
			if(l1->val > l2->val){
				cur->next = l2;
				cur = l2;
				l2 = l2->next;
			}
			else{
				cur->next = l1;
				cur = l1;
				l1 = l1->next;
			}
		}
		cur->next = (l1 ? l1 : l2);
		while(cur->next) cur = cur->next;
		return cur;
	}
};
```
