# 669. Trim a Binary Search Tree

## Description



## My Solution

### Recursive Solution

This problem can be solve recursively in a descent manner.

For each `root` node of a subtree, `root->val` can fall into the following cases:

- `root->val < low`

    Then we should trim this `root` as well as its left child.

- `root->val > high`

    Then we should trim this `root` as well as its right child.

- `low <= root->val <= high`

    - `root->val == low`: we can set its left child as `nullptr`

    - `root->val == high`: we can set its right child as `nullptr`

    Then we trim it left child and right child recursively.

```C++
TreeNode* trimBST(TreeNode* root, int low, int high) {
    if(!root)
        return nullptr;
    
    if(root->val < low)
        root = trimBST(root->right, low, high);
    else if(root->val > high)
        root = trimBST(root->left, low, high);
    else {
        if(root->val == low)
            root->left = nullptr;
        else if(root->val == high)
            root->right = nullptr;
        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);
    }
    return root;
}
```

## Classic Solution

Here's an iterative version of solution using preorder traversal.

```java
public TreeNode trimBST(TreeNode root, int low, int high) {
        while(root!=null&&(root.val<low||root.val>high)){
            if(root.val<low)root=root.right;
            if(root.val>high)root=root.left;
        }
        Stack<TreeNode> stack=new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode node=stack.pop();
            boolean matchInterval=true;
            if(node!=null&&node.left!=null&&node.left.val<low){node.left=node.left.right;matchInterval=false;}
            if(node!=null&&node.right!=null&&node.right.val>high){node.right=node.right.left;matchInterval=false;}
            if(matchInterval){
                if(node.left!=null){
                    stack.push(node.left);
                }
                if(node.right!=null){
                    stack.push(node.right);
                }
                matchInterval=false;
            }else{
                stack.push(node);
            }
        }
        return root;
    }
```