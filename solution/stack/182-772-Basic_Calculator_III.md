# 772. Basic Calculator III

## Description

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open `(` and closing parentheses `)`, the plus `+` or minus sign `-`, non-negative integers and empty spaces .

The expression string contains only non-negative integers, `+`, `-,` `*`, `/` operators , open `(` and closing parentheses `)` and empty spaces. The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of `[-2147483648, 2147483647]`.

**Some examples:**
```
"1 + 1" = 2
" 6-4 / 2 " = 4
"2*(5+5*2)/3+(6/2+8)" = 21
"(2+6* 3+5- (3*14/7+2)*5)+3"=-12
```

Note: Do not use the `eval` built-in library function.

## Classic Solution


1.遇到数字
```C++
    num=num*10+s[i]-'0'
```
2.遇到左括号

    要找到右括号，同时递归计算括号里面的内容

```C++
    int j=i,cnt=0;  
    for(;i<n;i++){
        if(s[i]=='(') cnt++;
        if(s[i]==')') cnt--;
        if(cnt==0) break;
    }
    num=calculate(s.substr(j+1,i-j-1));
```

3. 遇到基本运算符

    根据上一个运算符:计算当前的值

    遇到'+','-'运算符:计算总的结果，并且更新上一个运算符和num。


```C++
int calculate(string &s) {
    int currRes = 0;
    int res = 0;
    char prevOp = '+';
    int val = 0;

    for(int i = 0; i < s.size(); i++) {
        char ch = s[i];

        if(isdigit(ch))
            val = 10 * val + (ch - '0');
        else if (ch == '(') {
            int j = i;
            int count = 0;
            for(j = i; i < s.size(); i++) {
                if(s[i] == '(')
                    count++;
                else if(s[i] == ')')
                    count--;
                
                if(!count)
                    break;
            }
            string nextStr = s.substr(j + 1, i - j - 1);
            val = calculate(nextStr);
        }

        if(ch == '+' || ch == '-' || ch == '*' || ch == '/' || i == s.size() - 1) {
            switch(prevOp) {
                case '+': currRes += val; break;
                case '-': currRes -= val; break;
                case '*': currRes *= val; break;
                case '/': currRes /= val; break;
            }

            if(ch == '+' || ch == '-' || i == s.size() - 1) {
                res += currRes;
                currRes = 0;
            }

            val = 0;
            prevOp = ch;
        }
    }
    return res;
}
```