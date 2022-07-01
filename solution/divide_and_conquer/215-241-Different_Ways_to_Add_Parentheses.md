# 241. Different Ways to Add Parenthses

## Description

Given a string `expression` of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. You may return the answer in any order.

The test cases are generated such that the output values fit in a 32-bit integer and the number of different results does not exceed $10^4$.

**Example:**
```
Input: expression = "2-1-1"
Output: [0,2]
Explanation:
((2-1)-1) = 0 
(2-(1-1)) = 2
```

## Classic Solution

We can solve it using divide and conquer.

Suppose we want to calculate the possible results of `X op Y`, and the result of `X` and `Y` is already calculated, then we can compute all possible combinations from the results of `X` and `Y` using `op` operator.

The recursion entrance would be the input `expression` is simply an operand (each of the element is digit).

```C++
vector<int> diffWaysToCompute(string expression) {
    vector<int> res;
    bool allDigit = true;

    for(int i = 0; i < expression.size(); i++) {
        if(expression[i] == '+' || expression[i] == '-' || expression[i] == '*') {
            allDigit = false;
            vector<int> left = diffWaysToCompute(expression.substr(0, i));
            vector<int> right = diffWaysToCompute(expression.substr(i + 1));

            for(const auto& val1 : left) {
                for(const auto& val2 : right) {
                    switch(expression[i]) {
                        case '+':
                            res.push_back(val1 + val2);
                            break;
                        case '-':
                            res.push_back(val1 - val2);
                            break;
                        case '*':
                            res.push_back(val1 * val2);
                            break;
                        default:
                            cout << "error" << endl;
                    }
                }
            }
        }
    }

    if(allDigit)
        res.push_back(stoi(expression));
    return res;
}
```