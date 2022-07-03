# 556. Next Greater Generation

## Description

Given a positive integer `n`, find the smallest integer which has exactly the same digits existing in the integer `n` and is greater in value than `n`. If no such positive integer exists, return `-1`.

Note that the returned integer should fit in 32-bit integer, if there is a valid answer but it does not fit in 32-bit integer, return `-1`.

**Example:**
```
Input: n = 12
Output: 21
```

## My Solution

We can search from the lsb to msb, checking if an inverse pair exists (i.e. `val[i] > val[i-1]`).

If it exists, then we replace `val[i-1]` with the next greater digits from the latter part, and rearrange the latter part in non-descending order, which is the next greater generation this question asks for. 

Here to rearrange the latter part into non-descending order, I simply used sorting.

```C++
int nextGreaterElement(int n) {
    string str = to_string(n);
    vector<char> digits;

    for(int i = str.size() - 1; i > 0; i--) {
        digits.push_back(str[i]);
        if(str[i] > str[i-1]) {
            char msb = str[i-1];
            digits.push_back(str[i-1]);
            sort(digits.begin(), digits.end());
            int j = 0;
            while(j < digits.size() && digits[j] <= msb)
                j++;
            char nextMSB = digits[j];
            digits.erase(next(digits.begin(), j));

            string res = str.substr(0, i - 1) + nextMSB;
            for(const auto& ch : digits)
                res += ch;
            
            long ret = stol(res);
            if(ret > INT_MAX)
                return -1;
            return static_cast<int>(ret);
        }
    }
    return -1;
}
```

## Classic Solution

In my solution, I used sorting to make the latter part non-descending.

However, since the latter part is actually non-increasing, we can simply use two pointers to swap the elements starting from head and tail of the latter part of the array.

```C++
int nextGreaterElement(int n) {
    vector<int> digits;
    while(n) {
        digits.push_back(n % 10);
            n /= 10;
    }

    int index = -1;
    for(int i = 0; i < digits.size() - 1; i++)
        if(digits[i] > digits[i+1]) {
            index = i + 1;
            break;
        }
    
    if(index == -1)
        return -1;
    
    int nextGreaterIndex = 0;
    while(nextGreaterIndex < index && digits[nextGreaterIndex] <= digits[index])
        nextGreaterIndex++;
    swap(digits[nextGreaterIndex], digits[index]);

    for(int low = 0, high = index - 1; low < high; low++, high--)
        swap(digits[low], digits[high]);

    long res = 0;
    for(int i = digits.size() - 1; i >= 0; i--)
        res = res * 10 + digits[i];
    
    if(res > INT_MAX)
        return -1;
    
    return static_cast<int>(res);
}
```