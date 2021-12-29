# 371. Sum of Two Integers
## Description
Given two integers `a` and `b`, return the sum of the two integers without using the operators `+` and `-`.

**Example:**
```
Input: a = 1, b = 2
Output: 3
```
## My Solution
I didn't figure out the correct solution... Bit manipulation is indeed very tricky.

## Classic Solution
It is obvious that we should use bit manipulation to solve this question, but how to choose the correct operator requires thinking. Here's a process describing how [kishynivas10](https://leetcode.com/problems/sum-of-two-integers/discuss/132479/Simple-explanation-on-how-to-arrive-at-the-solution) arrives at the answer.

Eg: Let's try this with our hand `3 + 2 = 5` , the carry will be with in the brackets i.e "()"
```
3 =>   011 
2 =>   010
     _____
    1(1)01
```
Here we will forward the carry at the second bit to get the result.

So which bitwise operator can do this ? A simple observation says that **XOR** can do that,but it just falls short in dealing with the carry properly, but correctly adds when there is no need to deal with carry.
For Eg:
```
1   =>  001 
2   =>  010 
1^2 =>  011 (2+1 = 3) 
```
So now when we have carry, to deal with, we can see the result as :
```
3  => 011 
2  => 010 
3^2=> 001  
```
Here we can see **XOR** just fell short with the carry generated at the second bit.

So how can we find the carry ? The carry is generated when both the bits are set, i.e `(1,1)` will generate carry but `(0,1)` or `(1,0)` or `(0,0)` won't generate a carry, so which bitwise operator can do that ? **AND** gate of course.

To find the carry we can do
```
3    =>  011 
2    =>  010 
3&2  =>  010
```

now we need to add it to the previous value we generated i.e ( `3 ^ 2`), but the carry should be added to the left bit of the one which genereated it.
so we left shift it by one so that it gets added at the right spot.

Hence `(3&2)<<1 => 100`
so we can now do
```
3 ^2        =>  001 
(3&2)<<1    =>  100 
```
Now xor them, which will give `101(5)` , we can continue this until the carry becomes zero.

The C++ code is shown below. Note that any of the two operands should not be negative. otherwise it will trigger a runtime error since it is an undefined behavior.
```C++
int getSum(int a, int b) {
    while(b != 0) {
        unsigned int c = a & b;
        a = a ^ b;
        b = c << 1;
    }
    return a;
}
```
