# 105. Construct Binary Tree from Preorder and Inorder Traversal

## Description
Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return the binary tree.

**Example:**
```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```
## Classic Solution
Basic idea is:

- preorder[0] is the root node of the tree
- preorder[x] is a root node of a sub tree
- In in-order traversal

    - When inorder[index] is an item in the in-order traversal
    - inorder[0]-inorder[index-1] are on the left branch
    - inorder[index+1]-inorder[size()-1] are on the right branch
    - if there is nothing on the left, that means the left child of the node is NULL
    - if there is nothing on the right, that means the right child of the node is NULL

**Algorithm:**

- Start from index 0
- Find preorder[index] from inorder, let's call the index pivot
- Create a new node with inorder[pivot]
- Create its left child recursively
- Create its right child recursively
- Return the created node.

The implementation is self explanatory. 

```C++
TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    int index = 0;
    return build(preorder, inorder, index, 0, inorder.size() - 1);
}

TreeNode* build(vector<int>& preorder, vector<int>& inorder, int& index, int start, int end) {
    if(start > end)
        return nullptr;
    
    int root_val = preorder[index];
    int pivot = start;
    while(inorder[pivot] != root_val) 
        pivot++;
    
    index++;
    TreeNode *root = new TreeNode(root_val);
    root->left = build(preorder, inorder, index, start, pivot - 1);
    root->right = build(preorder, inorder, index, pivot + 1, end);
    
    return root;
}
```
Here `index` will be increased in the left tree and then be used in next right. That's why we need to use call by reference instead of simply passed as value.