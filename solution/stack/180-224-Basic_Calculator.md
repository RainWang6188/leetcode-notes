# 224. Basic Calculator

## Description

Given a string `s` representing a valid expression, implement a basic calculator to evaluate it, and return the result of the evaluation.

Note: You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

**Example:**
```
Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23
```

## Classic Solution

We use a stack to store the previous result sum and the sign.

The algorithm is basically similar to the [Decide String](https://leetcode.com/problems/decode-string/) problem.

We traverse the string from left to right and for the current character `ch`, there're 6 cases:

- `ch` is a digit: then we calculate the whole value and add it to the current `sum` according to the current `sign`.

- `ch == '+'`, then we set the current `sign` as `1`.

- `ch == '-'`, then we set the current `sign` as `-1`.

- `ch == '('`, then we push the `sum` and the `sign` onto the stack, then set `sum = 0` and `sign = 1` for the upcoming new turn.

- `ch == ')'`, then current sum calculate is over, we pop the previous `sum` and previous `sign` from the stack and add it to the current `sum`.

```C++
int calculate(string s) {
    stack<int> stk;
    int sum = 0;
    int sign = 1;
    
    int index = 0;
    while(index < s.size()) {
        if(isdigit(s[index])) {
            int val = 0;
            while(index < s.size() && isdigit(s[index])) {
                val = 10 * val + (s[index] - '0');
                index++;
            }
            sum += sign * val;
        }
        else if(s[index] == '+') {
            sign = 1;
            index++;
        }
        else if(s[index] == '-') {
            sign = -1;
            index++;
        }
        else if(s[index] == '(') {
            stk.push(sum);
            stk.push(sign);
            sum = 0;
            sign = 1;
            index++;
        }
        else if(s[index] == ')') {
            int prevSign = stk.top();
            stk.pop();
            int prevSum = stk.top();
            stk.pop();
            sum = sum * prevSign + prevSum;
            index++;
        }
        else
            index++;
    }
    return sum;
}
```