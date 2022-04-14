# 227. Basic Calculator II


## Description

Given a string `s` which represents an expression, evaluate this expression and return its value. 

The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of $[{-2}^{31}, 2^{31} - 1]$.

Note: You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

**Example:**
```
Input: s = "3+2*2"
Output: 7
```

## My Solution

My solution uses two vectors, one for the operator and the other for the operands.

For the input process, we take the vectors as stacks, traversing the string from left to right, and pushing the operands and operators to the back of the vectors. Before we push the current operand, we will check if the previous operator is `*` or `/`, if so, we'll implement the multiplication and division right away and push the result onto the operand stack. 

As a result, after the above procedure, there'll only be `+` or `-` on the stack.

Then, we need to traverse the operands in the vector from front to end, since the additions and subtractions are operated from left to right. 


```C++
int calculate(string s) {
    vector<int> operands;
    vector<char> operators;
    
    int index = 0;
    while(index < s.size()) {
        if(isdigit(s[index])) {
            int val = 0;
            while(index < s.size() && isdigit(s[index])) {
                val = 10 * val + (s[index] - '0');
                index++;
            }
            
            if(!operators.empty() && (operators.back() == '*' || operators.back() == '/')) {
                char op = operators.back();
                operators.pop_back();
                int val1 = operands.back();
                operands.pop_back();
                int res = 0;
                switch(op) {
                    case '*': res = val1 * val;
                        break;
                    case '/': res = val1 / val;
                        break;
                }
                operands.push_back(res);
            }
            else
                operands.push_back(val);
        }
        else if (s[index] == '+' || s[index] == '-' || s[index] == '*' || s[index] == '/') {
            operators.push_back(s[index]);
            index++;
        }
        else
            index++;
    }
    
    int res = 0;
    int sign = 1;
    for(int i = 0; i < operands.size(); i++) {
        res += sign * operands[i];
        sign = (i < operators.size() && operators[i] == '+') ? 1 : -1;
    }
    return res;
}
```

## Classic Solution

Actually, we can use only one stack, which contains the signed values.

Here're the possible cases:

- `+`: set `val` as `+val`;

- `-`: set `val` as `-val`;

- `*` or `/`: we pop the previous value from the stack and perform the operation, then we push the result onto the stack.


```C++
int calculate(string s) {
    stack<int> vals;
    
    s += '+';
    char op = '+';
    int val = 0;
    
    for(int i = 0; i < s.size(); i++) {
        char ch = s[i];
        
        if(isdigit(ch)) {
            val = 10 * val + (ch - '0');
            continue;
        }
        
        if(ch == ' ')
            continue;
        
        if(op == '+')
            vals.push(val);
        else if(op == '-')
            vals.push(-val);
        else if(op == '*') {
            int prev = vals.top();
            vals.pop();
            vals.push(prev * val);
        }
        else if(op == '/') {
            int prev = vals.top();
            vals.pop();
            vals.push(prev / val);
        }
        
        val = 0;
        op = ch;
    }
    
    int total = 0;
    while(!vals.empty()) {
        total += vals.top();
        vals.pop();
    }
    
    return total;
}
```

