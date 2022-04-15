# 385. Mini Parser


## Description

Given a string `s` represents the serialization of a nested list, implement a parser to deserialize it and return the deserialized NestedInteger.

Each element is either an integer or a list whose elements may also be integers or other lists.

**Example:**
```
Input: s = "[123,[456,[789]]]"
Output: [123,[456,[789]]]
Explanation: Return a NestedInteger object containing a nested list with 2 elements:
1. An integer containing value 123.
2. A nested list containing two elements:
    i.  An integer containing value 456.
    ii. A nested list with one element:
         a. An integer containing value 789
```

## Classic Solution

This solution uses a stack to record the NestedInteger's.

At the very beginning, an empty NestedInteger is placed in the stack. This NestedInteger will be regarded as a list that holds one but only one NestedInteger, which will be returned in the end.

Logic: 

- When encountering `'['`, the stack has one more element.

- When encountering `']'`, the stack has one less element.

Complexities:

Time: O(n)
Space: worse-case O(n) (worse case: `[1,[2,[3,[....[n-1,[n]]]....]`)

```C++
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Constructor initializes an empty nested list.
 *     NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     NestedInteger(int value);
 *
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Set this NestedInteger to hold a single integer.
 *     void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     void add(const NestedInteger &ni);
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class Solution {
public:
    NestedInteger deserialize(string s) {
        function<bool(char)> isNumber = [](char c) {return c == '-' || isdigit(c);};
        
        stack<NestedInteger> stk;
        stk.push(NestedInteger());
        
        for(int i = 0; i < s.size(); i++) {
            char ch = s[i];
            
            if(isNumber(ch)) {
                int j = i;
                while(i < s.size() && isNumber(s[i]))
                    i++;
                
                int val = stoi(s.substr(j, i - j));
                i--;
                stk.top().add(val);
            }
            else if(ch == '[') {
                stk.push(NestedInteger());
            }
            else if(ch == ']') {
                NestedInteger curr = stk.top();
                stk.pop();
                stk.top().add(curr);
            }
        }
        return stk.top().getList().front();
    }
};
```