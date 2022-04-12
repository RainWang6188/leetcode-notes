# 150. Evaluate Reverse Polish Notation


## Description

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are `+,` `-,` `*,` and `/`. Each operand may be an integer or another expression.

Note that division between two integers should truncate toward zero.

It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.

**Example:**
```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

## My Solution

I used a stack to store the values. Once we encounter an operator, we pop two values from the top of the stack and execute that operation, then push the result back to the stack.

At last, the result will be the one remained in the stack.

```C++
int evalRPN(vector<string>& tokens) {
    stack<int> stk;
    for(auto& token : tokens) {
        if(token == "+" || token == "-" || token == "*" || token == "/") {
            int val1 = stk.top();
            stk.pop();
            int val2 = stk.top();
            stk.pop();
            int res = INT_MIN;
            if(token == "+")
                res = val2 + val1;
            else if(token == "-")
                res = val2 - val1;
            else if(token == "*")
                res = val2 * val1;
            else
                res = val2 / val1;
            stk.push(res);
        }
        else
            stk.push(stoi(token));
    }
    return stk.top();
}
```

## Classic Solution

Here's a very fancy solution using LAMBDA in C++.

Besides, the `unordered_map<string, function<int (int, int) > >` is valid as well.

```C++
int evalRPN(vector<string>& tokens) {
    unordered_map<string, function<int (int, int) > > map = {
        { "+" , [] (int a, int b) { return a + b; } },
        { "-" , [] (int a, int b) { return a - b; } },
        { "*" , [] (int a, int b) { return a * b; } },
        { "/" , [] (int a, int b) { return a / b; } }
    };
    std::stack<int> stack;
    for (string& s : tokens) {
        if (!map.count(s)) {
            stack.push(stoi(s));
        } else {
            int op1 = stack.top();
            stack.pop();
            int op2 = stack.top();
            stack.pop();
            stack.push(map[s](op2, op1));
        }
    }
    return stack.top();
}
```