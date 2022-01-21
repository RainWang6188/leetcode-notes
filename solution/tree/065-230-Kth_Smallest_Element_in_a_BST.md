# 230. Kth Smallest Element in a BST

## Description
Given the root of a binary search tree, and an integer `k`, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.

**Example:**
```
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3
```
## My Solution
### 1. Recursive Version
We all know that inorder traversal of a BST produces an ascending array. So the Kth smallest element in a BST is the Kth elemenent during the inorder traversal.

Here's an recursive version.
```C++
int kthSmallest(TreeNode* root, int k) {
    int res = 0;
    traverse(root, k, res);
    return res;
}

void traverse(TreeNode* root, int& k, int& res) {
    if(!root)
        return;
    
    traverse(root->left, k, res);
    if(--k == 0)
        res = root->val;
    traverse(root->right, k, res);
}
```
### 2. Iterative Version
Here's an iterative version.
```C++
int kthSmallest(TreeNode* root, int k) {
    TreeNode *cur = root;
    stack<TreeNode*> stk;
    while(cur || !stk.empty()) {
        while(cur) {
            stk.push(cur);
            cur = cur->left;
        }
        
        cur = stk.top();
        stk.pop();
        
        if(--k == 0)
            return cur->val;
        
        cur = cur->right;
    }
    return 0;
}
```
