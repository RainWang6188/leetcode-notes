# 98. Validate Binary Search Tree

## Description
Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A **valid BST** is defined as follows:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example:**
```
input: root = [5,4,6,null,null,3,7]
output: false
```

## My Solution
After reading the description, I thought this is a simple problem, since we only need to guarantee the following two properties for any `parent->root` structure:

1. If `root` is the left child of `parent`, `root->val < parent->val` must hold.
2. If `root` is the right child of `parent`, `root->val > parent->val` must hold.

However, for inputes like `[5,4,6,null,null,3,7]`, this naive solution can't work. That is because any node is not only in the subtree of its direct (adjacent) parent, it is also in the subtrees of indirect parents, where the it's in the left or right subtree decides which of the above property should hold.

As a result, I use two vectors:

- `prev_vals<int>` records the values of its every ancestor nodes.
- `is_lefts<bool>` records the information of it's in the left or right subtree.

```C++
bool isValidBST(TreeNode* root) {
    return isBST(root->left, {root->val}, {true}) && isBST(root->right, {root->val}, {false});
}

bool isBST(TreeNode* root, vector<int> prev_vals, vector<bool> is_lefts) {
    if(!root)
        return true;
    
    int n = prev_vals.size();
    for(int i = 0; i < n; i++) {
        if(is_lefts[i] && root->val >= prev_vals[i])
            return false;
        if(!is_lefts[i] && root->val <= prev_vals[i])
            return false;
    }
    
    prev_vals.push_back(root->val);
    
    vector<bool> is_lefts_new = is_lefts;
    is_lefts.push_back(true);
    is_lefts_new.push_back(false);
    
    return isBST(root->left, prev_vals, is_lefts) && isBST(root->right, prev_vals, is_lefts_new);
}
```

Also, we can replace the two vectors with an `unordered_multimap<int, bool>`, but that will cost more space.

## Claissc Solution
### 1. Inorder Traversal & Check if it's sorted
The most comprehensible solution is storing the values of nodes in a vector during the **inorder** traversal, and then check if it's ascending.

The inorder traversal of a valid BST must be ascending.

```C++
bool isValidBST(TreeNode* root) {
    vector<int> vals;
    inorder(root, vals);
    return is_sorted(vals);
}

void inorder(TreeNode* root, vector<int>& vals) {
    if(!root)
        return;
    
    inorder(root->left, vals);
    vals.push_back(root->val);
    inorder(root->right, vals);
}

bool is_sorted(vector<int>& vals) {
    for(int i = 0, n = vals.size(); i < n - 1; i++)
        if(vals[i] >= vals[i+1])
            return false;
    return true;
}
```
### 2. Inorder Traversal - iterative version
As what is just mentioned before, the inorder traversal of a valid BST must be ascending, so we don't need to traverse the whole BST and before checking it's validity. We can record its previous node during the inorder traversal, and checking if the the value of current node is greater than that of the previous node.

Here's an solution which employs the code of iterative inorder traversal.
```C++
bool isValidBST(TreeNode* root) {
    stack<TreeNode*> stk;
    TreeNode *cur = root;
    TreeNode *prev = nullptr;
    
    while(cur || !stk.empty()) {
        while(cur) {
            stk.push(cur);
            cur = cur->left;
        }
        
        cur = stk.top();
        stk.pop();
        if(prev && cur->val <= prev->val)
            return false;
        
        prev = cur;
        cur = cur->right;
    }
    return true;
}
```


### 3. Inorder Traversal - recursive version
Also, the inorder traversal can be implemented in a recursive way.

```C++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if(!root)
            return true;
        
        TreeNode *cur = root;
        
        if(!isValidBST(cur->left))
            return false;
        if(prev && cur->val <= prev->val)
            return false;
        prev = cur;
        if(!isValidBST(cur->right))
            return false;
        
        return true;
    }
private:
    TreeNode* prev;
};
```