# 235. Lowest Common Ancestor of a Binary Search Tree

## Description
Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow **a node to be a descendant of itself**).”

**Example:**
```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```
 
## My Solution

### 1. Solution for general Binary Tree
I didn't read the description carefully, where I missed the information that the tree is actually a BST.

```C++
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    vector<TreeNode*> route_p;
    vector<TreeNode*> route_q;
    
    search_node(root, p, route_p);
    search_node(root, q, route_q);
    
    int min_len = min(route_p.size(), route_q.size());     
    int index = 0;
    while(index < min_len - 1 && route_p[index+1] == route_q[index+1])
        index++;
    
    return route_p[index];
}

bool search_node(TreeNode *root, TreeNode *target, vector<TreeNode*>& route) {
    if(!root)
        return false;
    
    route.push_back(root);
    if(root == target)
        return true;
    
    if(search_node(root->left, target, route) || search_node(root->right, target, route))
        return true;
    
    route.pop_back();
    return false;
}
```

### 2. Iterative Version
Here're the solution for BST.

If the current value is less than both of the target nodes, we should search in its right subtree, otherwise, we trace into left subtree.
```c++
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    TreeNode* cur = root;
    
    while(cur) {
        if(cur->val < p->val && cur->val < q->val)
            cur = cur->right;
        else if(cur->val > p->val && cur->val > q->val)
            cur = cur->left;
        else
            break;
    }
    return cur;
}
```
### 3. Recursive Version
Here's an recursive version.
```C++
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(!root)
        return nullptr;
    
    if(root->val < p->val && root->val < q->val)
        return lowestCommonAncestor(root->right, p, q);
    else if(root->val > p->val && root->val > q->val)
        return lowestCommonAncestor(root->left, p, q);
    else
        return root;
}
```