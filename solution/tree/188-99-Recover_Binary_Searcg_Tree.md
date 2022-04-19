# 99. Recover Binary Search Tree

## Description

You are given the `root` of a binary search tree (BST), where the values of **exactly** two nodes of the tree were swapped by mistake. Recover the tree without changing its structure.

**Example:**

![exp](https://assets.leetcode.com/uploads/2020/10/28/recover2.jpg)

```
Input: root = [3,1,4,null,null,2]
Output: [2,1,4,null,null,3]
Explanation: 2 cannot be in the right subtree of 3 because 2 < 3. Swapping 2 and 3 makes the BST valid.
```

## Classic Solution

Actually, we can solve this problem simply using in-order traversal.

In this problem, we need to find the first and second elements that are not in order when according to the in-order traversal.

How do we find these two elements? For example, we have the following tree that is printed as in order traversal:
```
6, 3, 4, 5, 2
```
We compare each node with its next one and we can find out that 6 is the first element to swap because 6 > 3 and 2 is the second element to swap because 2 < 5.

Here's the key to this problem: **The first element is always larger than its next one while the second element is always smaller than its previous one.**

So we can record the first and second element during the in-order traversal and swap their values afterwards.

```C++
void recoverTree(TreeNode* root) {
    stack<TreeNode*> stk;
    TreeNode *curr = root;
    TreeNode *prev = nullptr;
    TreeNode *first = nullptr;
    TreeNode *second = nullptr;
    
    while(curr || !stk.empty()) {
        while(curr) {
            stk.push(curr);
            curr = curr->left;
        }
        
        curr = stk.top();
        stk.pop();
        
        if(!first && prev && prev->val > curr->val)
            first = prev;
        if(first && prev->val > curr->val) {
            second = curr;
            break;
        }
        
        prev = curr;
        curr = curr->right;
    }
    
    swap(first->val, second->val);
}
```