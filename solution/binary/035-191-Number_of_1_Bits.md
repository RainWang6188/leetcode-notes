# 191. Number of 1 Bits
## Description
Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).

Note:

- Note that in some languages, such as Java, there is no unsigned integer type. In this case, the input will be given as a signed integer type. It should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using `2's complement` notation. Therefore, in Example `3`, the input represents the signed integer. `-3`.

**Example:**
```
Input: n = 00000000000000000000000000001011
Output: 3
Explanation: The input binary string 00000000000000000000000000001011 has a total of three '1' bits.
```
## My Solution - Bit Shifting
We always check if the least significant bit (LSB) is 1 or not by AND it with `0x1`. and shift the input number 32 times so as to check all the bits.

```c++
int hammingWeight(uint32_t n) {
    int count = 0;
    uint32_t anchor = 0x01;
    
    for(int i = 0; i < 32; i++) {
        if(n & anchor)
            count++;
        n = n >> 1;
    }
    return count;
}
```

## Classic Solution - N &= (N - 1)
This solution is a lot more tricky than the above one.

For any number `N`, 1 subtracted by that number will CLEAR the lowest set bit and SET all the LSBs of `N`. (e.g. we subtract `1` from `4(0100)` will get `3(0011)`)  

As a result, we keeping doing `N & (N - 1)` will clear al the set bits from the lowest to the highest, and inrementing the `count` along the way to record the number of this operations.

```C++
int hammingWeight(uint32_t n) {
    int count = 0;
    while(n) {
        n &= (n - 1);
        count++;
    }
    return count;
}
```