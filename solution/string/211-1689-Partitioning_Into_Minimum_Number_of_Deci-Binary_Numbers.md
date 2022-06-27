# 1689. Partitioning Into Minimum Number of Deci-Binary Numbers

## Description

A decimal number is called deci-binary if each of its digits is either `0` or `1` without any leading zeros. For example, `101` and `1100` are deci-binary, while `112` and `3001` are not.

Given a string `n` that represents a positive decimal integer, return the minimum number of positive deci-binary numbers needed so that they sum up to `n`.

**Example:**
```
Input: n = "32"
Output: 3
Explanation: 10 + 11 + 11 = 32
```

## Classic Solution

If the input is only one digit, then we only need to add up as many ones as the value of this digit.

If the input has many digits, we can solve for each digit independently and merge the answers to form numbers that add up to that input.

As a result, the final answer is equal to the max digit.

![demo](https://assets.leetcode.com/users/images/5ecfed3e-0840-48c4-b1e9-a978c39fa412_1607831965.5998409.png)

If we are asked to print the deci-binary numbers, we just need to fill in the above table digit by digit and output each row.


```C++
int minPartitions(string n) {
    return *max_element(n.begin(), n.end()) - '0';
}
```

