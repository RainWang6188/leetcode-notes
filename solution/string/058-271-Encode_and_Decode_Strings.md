# 271. Encode and Decode Strings

## Description
Design an algorithm to encode a list of strings to a string. The encoded string is then sent over the network and is decoded back to the original list of strings.

Machine 1 (sender) has the function:
```C++
string encode(vector<string> strs) {
  // ... your code
  return encoded_string;
}
```
Machine 2 (receiver) has the function:
```C++
vector<string> decode(string s) {
  //... your code
  return strs;
}
```

So Machine 1 does:
```C++
string encoded_string = encode(strs);
```

and Machine 2 does:
```C++
vector<string> strs2 = decode(encoded_string);
```

`strs2` in Machine 2 should be the same as `strs` in Machine 1.

Implement the encode and decode methods.

**Note:**

- The string may contain any possible characters out of 256 valid ascii characters. Your algorithm should be generalized enough to work on any possible characters.
Do not use class member/global/static variables to store states. Your encode and decode algorithms should be stateless.

- Do not rely on any library method such as eval or serialize methods. You should implement your own encode/decode algorithm.
## My Solution
My method appends non-ASCII delimiter to the end of each string. 

```C++
// Encodes a list of strings to a single string.
string encode(vector<string>& strs) {
    string res = "";
    for (string str : strs) res += str + to_string(static_cast<char>(258));
    return res;
}
// Decodes a single string to a list of strings.
vector<string> decode(string s) {
    vector<string> res;
    stringstream ss(s);
    string t;
    while (getline(ss, t, static_cast<char>(258))) {
        res.push_back(t);
    }
    return res;
}
```
