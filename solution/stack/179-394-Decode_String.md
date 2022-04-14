# 394. Decode String


## Description

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times. Note that `k` is guaranteed to be a positive integer.

You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, `k`. For example, there will not be input like `3a` or `2[4]`.

**Example:**
```
Input: s = "3[a]2[bc]"
Output: "aaabcbc"
```

## Classic Solution

### 1. Recursive Solution

Every time we meet a `'['`, we treat it as a subproblem so call our recursive function to get the content in that corresponding `'['` and `']'`. After that, repeat that content for 'num' times.

Every time we meet a `']'`, we know a subproblem finished and just return the `word` we got in this subproblem.

Please notice that the `pos` is **passed by reference**, use it to record the position of the original string we are looking at.

```C++
class Solution {
public:
    string decodeString(string s) {
        int pos = 0;
        return decode(s, pos);
    }
private:
    string decode(string s, int& pos) {
        string word = "";
        int k = 0;
        for(; pos < s.size(); pos++) {
            if(s[pos] == '[') {
                string str = decode(s, ++pos);
                while(k--)
                    word += str;
                k = 0;
            }
            else if(s[pos] == ']')
                return word;
            else if(s[pos] >= '0' && s[pos] <= '9')
                k = 10 * k + s[pos] - '0';
            else
                word += s[pos];
        }
        return word;
    }
};
```

### 2. Iterative Solution using Stack

We use two stacks:

- `valStk` stores the previous repeat times `k`
- `resStk` stores the previous decoded result.

```C++
string decodeString(string s) {
    stack<int> valStk;
    stack<string> resStk;
    string res = "";
    int k = 0;
    
    int index = 0;
    while(index < s.size()) {
        if(isdigit(s[index])) {
            k = 0;
            while(index < s.size() && isdigit(s[index])) {
                k = 10 * k + s[index] - '0';
                index++;
            }
        }
        else if(isalpha(s[index])) {
            res += s[index++];
        }
        else if(s[index] == '[') {
            valStk.push(k);
            resStk.push(res);
            res = "";
            index++;
        }
        else {  // s[index] == ']'
            string prevRes = resStk.top();
            resStk.pop();
            int repeatTime = valStk.top();
            valStk.pop();
            for(int i = 0; i < repeatTime; i++)
                prevRes += res;
            res = prevRes;
            index++;
        }
    }
    return res;
}
```