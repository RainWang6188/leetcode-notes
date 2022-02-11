# 8. String to Integer (atoi)
## Description
Implement the myAtoi(string `s`) function, which converts a string to a 32-bit signed integer (similar to C/C++'s `atoi` function).

The algorithm for myAtoi(string `s`) is as follows:

1. Read in and ignore any leading whitespace.
2. Check if the next character (if not already at the end of the string) is '-' or '+'. Read this character in if it is either. This determines if the final result is negative or positive respectively. Assume the result is positive if neither is present.
3. Read in next the characters until the next non-digit character or the end of the input is reached. The rest of the string is ignored.
4. Convert these digits into an integer (i.e. `"123" -> 123`, `"0032" -> 32`). If no digits were read, then the integer is 0. Change the sign as necessary (from step 2).
5. If the integer is out of the 32-bit signed integer range [$-2^{31}$, $2^{31} - 1$], then clamp the integer so that it remains in the range. Specifically, integers less than $-2^{31}$ should be clamped to $-2^{31}$, and integers greater than $2^{31} - 1$ should be clamped to $2^{31} - 1$.
6. Return the integer as the final result.

**Note:**

- Only the space character ' ' is considered a whitespace character.
- **Do not ignore** any characters other than the leading whitespace or the rest of the string after the digits.
## My Solution
This problem is rather simple, just do what the instructions tells you to do.

However, we need to take care of **overflow problems**. So I use `long` to store the result and break the loop once the current result exceeds `INT_MAX`.

```C++
    int myAtoi(string s) {
        long long res = 0;
        int index = 0;
        bool is_positive = true;
        
        while(index < s.size() && s[index] == ' ')
            index++;
        
        if(index < s.size() && (s[index] == '+' || s[index] == '-')) {
            is_positive = s[index] == '+' ? true : false;
            index++;
        }
        while(index < s.size() && isdigit(s[index])) {
            int curr_digit = s[index] - '0';
            res  = res * 10 + curr_digit;
            if(res >= INT_MAX)
                break;
            index++;
         }
        
        if(!is_positive)
            res = -res;
        if(res > INT_MAX)
            res = INT_MAX;
        else if(res < INT_MIN)
            res = INT_MIN;
        
        return static_cast<int>(res);
    }
```
## Classic Solution
