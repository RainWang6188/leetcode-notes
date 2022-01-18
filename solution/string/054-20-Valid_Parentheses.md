# 20. Valid Parentheses
## Description
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

- Open brackets must be closed by the same type of brackets.
- Open brackets must be closed in the correct order.

**Example:**
```
Input: s = "()[]{}"
Output: true
```

## My Solution
I used a vector as a stack. 

When an open parenthesis is encountered, its corresponding close parenthesis will be pushed onto the stack; 

When an close parenthsis is encountered, we'll check if the top element of the stack is itself, otherwise, its not valid.

**Note:**

- vector.pop_back() will return void instead of the top element.
- vector.back() will return the last element.
- There is a `stack<T>` integrated in STL.
```c++
bool isValid(string s) {
    vector<char> stack;
    for(int i = 0; i < s.size(); i++) {
        char c = s[i];
        switch(c) {
            case('('): stack.push_back(')');
                        break;
            case('['): stack.push_back(']');
                        break;
            case('{'): stack.push_back('}');
                        break;
            case(')'): if(stack.empty() || stack.back() != ')')
                            return false;
                        stack.pop_back();
                        break;
            case(']'): if(stack.empty() || stack.back() != ']')
                            return false;
                        stack.pop_back();
                        break;
            case('}'): if(stack.empty() || stack.back() != '}')
                            return false;
                        stack.pop_back();    
                        break;
            default: ;
        }
    }
    return stack.empty();
}
```
## Classic Solution
It can be implemented using `stack<char>` in STL instead of `vector<char>`.

```C++
bool isValid(string s) {
    stack<char> stk;
    for(auto& c : s) {
        switch(c) {
            case('('): stk.push(')');
                        break;
            case('{'): stk.push('}');
                        break;
            case('['): stk.push(']');
                        break;
            default:
                    if(stk.empty() || stk.top() != c)
                        return false;
                    else
                        stk.pop();
        }
    }
    return stk.empty();
}
```