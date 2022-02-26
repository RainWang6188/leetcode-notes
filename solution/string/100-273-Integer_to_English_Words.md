# 273. Integer to English/Chinese Words

## Part 1. Integer to Chinese Words

### Description
Convert a non-negative long integer `num` to its Chinese words representation. `num` is ranged in $[1, 10^{13})$.
**Example:**
```
Input: num = 12345678
Output: "一千两百三十四万五千七百六十八"
```
### My Solution
There's a pattern in the Chinese representation: every consecutive four digits uses the same representation in Chinese despite the `"万"` or `"亿"` when it carries.

Since the maximum `num` will be $k * 10^{12}$, which is `"万亿"` in Chinese representation. We need to add a `"万"` if the current radix is $10^{12}$.

As a result, we can process digits by digits only using `"个十百千"`. Each time we convert the last digit into the Chinese and append its current radix. When the current radix is $10^4$, we will append a `"万"`; when it reaches $10^8$, we will append a `"亿"`; and when it reaches $10^12$, we will add another `"万"` (for `"万亿"`). 

Note that for `0` as the current last digit, we need to be careful. Since we won't append a `"零"` (omitting the radix) unless there's a non-zero digits in the lower bit. 

Here's my implementation

```C++
string solution(long val) {
//radix: 0    0    0   0    0    0    0    0    0   0   0   0   0
//     万亿 千亿  百亿 十亿   亿  千万  百万 十万    万  千  百   十  个
    unordered_map<int, string> radix {{1, ""}, {10, "十"}, {100, "百"}, {1000, "千"}};
    unordered_map<int, string> int2str {{1, "一"}, {2, "二"}, {3, "三"}, {4, "四"}, {5, "五"}, {6, "六"}, {7, "七"}, {8, "八"}, {9, "九"}, {0, "零"}};

    long total_radix = 1;
    int curr_radix = 1;
    string res = "";
    bool has_1_to_9 = false;

    while(val) {
        if(total_radix == 10000) {
            res = "万" + res;
            has_1_to_9 = false;
        } 
        else if(total_radix == 100000000) {
            res = "亿" + res;
            has_1_to_9 = false;
        }
        else if(total_radix == 1000000000000) {
            res = "万" + res;
        }

        int last_digit = val % 10;

        if(last_digit == 0 && has_1_to_9) {
                res = int2str[last_digit] + res;
        }        
        else {
            has_1_to_9 = true;
            res = int2str[last_digit] + radix[curr_radix] + res;
        }

        val /= 10;
        curr_radix = 10 * curr_radix == 10000 ? 1 : 10 * curr_radix;
        total_radix *= 10;
    }

    return res;
}
```

## Part 2. Integer to English Words (leetcode 273)

### Description
Convert a non-negative integer `num` to its English words representation, where `num` in ranges of $[0, 2^{31}-1]$

**Example 1:**
```
Input: num = 123
Output: "One Hundred Twenty Three"
```

**Example 2:**
```
Input: num = 12345
Output: "Twelve Thousand Three Hundred Forty Five"
```

**Example 3:**
```
Input: num = 1234567
Output: "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"
```

### Classic Solution
The pattern in English word is similar, but we need to pay attention to the numbers which are less than `20` since they have their own representations.

Here we can use recursion to finish the job and convert 3 digits in each iteration.

Before returning `res`, we should trim all the spaces at the end of the string.

```C++
class Solution {
public:
    string numberToWords(int num) {
        if(!num)
            return "Zero";
        
        string res = "";
        int thousands_counter = 0;
        while(num) {
            if(num % 1000 != 0)
                res = helper(num % 1000) + thousands[thousands_counter] + " " + res;
            num /= 1000;
            thousands_counter++;
        }
        
        int index = res.size() - 1;
        while(res[index] == ' ')
            index--;
        
        return res.substr(0, index+1);
    }
    
private:
    vector<string> less_than_20 = {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
    
    vector<string> tens = {"", "Ten", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
    
    vector<string> thousands = {"", "Thousand", "Million", "Billion"};
    
    string helper(int num) {
        if(num == 0)
            return "";
        else if(num < 20)
            return less_than_20[num] + " ";
        else if(num < 100)
            return tens[num/10] + " " + helper(num%10);
        else
            return less_than_20[num/100] + " Hundred " + helper(num%100);
    }
};
```