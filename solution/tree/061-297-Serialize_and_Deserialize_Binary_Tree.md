# 297. Serial and Deseralize Binary Tree

## Description
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Clarification:** The input/output format is the same as [how LeetCode serializes a binary tree](https://leetcode.com/faq/#binary-tree). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.


**Example:**
```
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
```

## Classic Solution
We'd better use **pre-order** traversal to solve this problem.


In the serialization, we traverse the tree in a pre-order way and use `'|'` as the delimiter of each node. So `[1,2,3,null,null,4,5]` will become `"1|2|||3|4|5"` after the serialize process.

In the deserialization, we employ the `stringstream` to process the serialized string (using a `vector` would result in TLE), and reconstruct the tree in a pre-order as well.

``` C++
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if(!root)
            return "|";
        
        return to_string(root->val) + "|" + serialize(root->left) + serialize(root->right);
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        TreeNode *root = nullptr;
        stringstream ss(data);
        return buildTree(ss);
    }
    
private:
    TreeNode *buildTree(stringstream& ss) {
        string token;
        getline(ss, token, '|');
        
        if(token == "")
            return nullptr;
        
        TreeNode *node = new TreeNode(stoi(token));
        node->left = buildTree(ss);
        node->right = buildTree(ss);
        
        return node;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec ser, deser;
// TreeNode* ans = deser.deserialize(ser.serialize(root));
```
