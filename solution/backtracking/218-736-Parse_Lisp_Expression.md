# 736. Parse Lisp Expression

## Description

You are given a string `expression` representing a Lisp-like expression to return the integer value of.

The syntax for these expressions is given as follows.

- An expression is either an integer, let expression, add expression, mult expression, or an assigned variable. Expressions always evaluate to a single integer.
- (An integer could be positive or negative.)
- A let expression takes the form `"(let v1 e1 v2 e2 ... vn en expr)"`, where `let` is always the string `"let"`, then there are one or more pairs of alternating variables and expressions, meaning that the first variable `v1` is assigned the value of the expression `e1`, the second variable `v2` is assigned the value of the expression `e2`, and so on sequentially; and then the value of this let expression is the value of the expression `expr`.
- An add expression takes the form `"(add e1 e2)"` where `add` is always the string `"add"`, there are always two expressions `e1`, `e2` and the result is the addition of the evaluation of `e1` and the evaluation of `e2`.
- A mult expression takes the form `"(mult e1 e2)"` where `mult` is always the string `"mult"`, there are always two expressions `e1`, `e2` and the result is the multiplication of the evaluation of `e1` and the evaluation of `e2`.

For this question, we will use a smaller subset of variable names. A variable starts with a lowercase letter, then zero or more lowercase letters or digits. Additionally, for your convenience, the names `"add"`, `"let"`, and `"mult"` are protected and will never be used as variable names.

Finally, there is the concept of scope. When an expression of a variable name is evaluated, within the context of that evaluation, the innermost scope (in terms of parentheses) is checked first for the value of that variable, and then outer scopes are checked sequentially. It is guaranteed that every expression is legal. Please see the examples for more details on the scope.
 

***Example 1:***
```
Input: expression = "(let x 2 (mult x (let x 3 y 4 (add x y))))"
Output: 14
Explanation: In the expression (add x y), when checking for the value of the variable x,
we check from the innermost scope to the outermost in the context of the variable we are trying to evaluate.
Since x = 3 is found first, the value of x is 3.
```

## Classic Solution

We can use the recursion to solve this question.

`parseInt` and `parseVar` are functions used to parse integer values and string variables. We also used an `unordered_map<string, vector<int>>` as the variable scope.

Here's how we parse expression recursively:

- if the next character of `expression` is not `'('`

    - if it's a lowercase character, then this expression is a variable string, we use `parseVar` to parse this variable and return the value in the innermost scope (i.e. the last value in the `scope`).

    - otherwise, it's a value, we can simply return the result of `parseInt`.

- skip the `'('`, and check the next character

    - if it's `l`
        - if the next character is not a lowercase character, or the next character after parsing a variable is `)`, it means the current expression is the returning expression of this let expression. We calculate and return its value.

        - Otherwise, the following expression is the `vi` and `ei` repeatedly, we update the value of `vi` in the `scope`.

        - After the whole parsing process, we need to clear the scope variables of this let expression.

    - if it's `a`, we calculate the sum of two expressions and return.

    - if it's `m`, we calculate the product of two expressions and return.

```C++
class Solution {
private:
    unordered_map<string, vector<int>> scope;

public:
    int evaluate(string expression) {
        int start = 0;
        return innerEvaluate(expression, start);
    }

    int innerEvaluate(const string &expression, int &start) {
        if (expression[start] != '(') { // 非表达式，可能为：整数或变量
            if (islower(expression[start])) {
                string var = parseVar(expression, start); // 变量
                return scope[var].back();
            } else { // 整数
                return parseInt(expression, start);
            }
        }
        int ret;
        start++; // 移除左括号
        if (expression[start] == 'l') { // "let" 表达式
            start += 4; // 移除 "let "
            vector<string> vars;
            while (true) {
                if (!islower(expression[start])) {
                    ret = innerEvaluate(expression, start); // let 表达式的最后一个 expr 表达式的值
                    break;
                }
                string var = parseVar(expression, start);
                if (expression[start] == ')') {
                    ret = scope[var].back(); // let 表达式的最后一个 expr 表达式的值
                    break;
                }
                vars.push_back(var);
                start++; // 移除空格
                int e = innerEvaluate(expression, start);
                scope[var].push_back(e);
                start++; // 移除空格
            }
            for (auto var : vars) {
                scope[var].pop_back(); // 清除当前作用域的变量
            }
        } else if (expression[start] == 'a') { // "add" 表达式
            start += 4; // 移除 "add "
            int e1 = innerEvaluate(expression, start);
            start++; // 移除空格
            int e2 = innerEvaluate(expression, start);
            ret = e1 + e2;
        } else { // "mult" 表达式
            start += 5; // 移除 "mult "
            int e1 = innerEvaluate(expression, start);
            start++; // 移除空格
            int e2 = innerEvaluate(expression, start);
            ret = e1 * e2;
        }
        start++; // 移除右括号
        return ret;
    }

    int parseInt(const string &expression, int &start) { // 解析整数
        int n = expression.size();
        int ret = 0, sign = 1;
        if (expression[start] == '-') {
            sign = -1;
            start++;
        }
        while (start < n && isdigit(expression[start])) {
            ret = ret * 10 + (expression[start] - '0');
            start++;
        }
        return sign * ret;
    }

    string parseVar(const string &expression, int &start) { // 解析变量
        int n = expression.size();
        string ret;
        while (start < n && expression[start] != ' ' && expression[start] != ')') {
            ret.push_back(expression[start]);
            start++;
        }
        return ret;
    }
};
```