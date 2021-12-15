# 299. Bulls and Cows
## Description
You are playing the Bulls and Cows game with your friend.

You write down a secret number and ask your friend to guess what the number is. When your friend makes a guess, you provide a hint with the following info:

- The number of "bulls", which are digits in the guess that are in the correct position.
- The number of "cows", which are digits in the guess that are in your secret number but are located in the wrong position. Specifically, the non-bull digits in the guess that could be rearranged such that they become bulls.


Given the secret number `secret` and your friend's guess `guess`, return the *hint for your friend's guess*.

The hint should be formatted as `"xAyB"`, where `x` is the number of bulls and `y` is the number of cows. Note that both `secret` and `guess` may contain duplicate digits.

## My Solution
My solution is naive, which simply employs brute force. 

First, I look for all the bulls in the string, and mark that element is `BULL` in `used` vector.

Second, for each element in the `guess` string, I first check if it is bull by examining its status in the `used` vector. If not, I would loop for each character in the `secret` string for an unmatched cow, and marked it as `COW` in `used[]`.

Time Complexity: $O(N^2)$, Space Complexity: $O(N)$. It sucks by the way...

```C++
    string getHint(string secret, string guess) {
        const int BULL = 1, COW = 2;
        int bulls = 0, cows = 0;
        vector<int> used(secret.size());
        
        for(int i = 0; i < guess.size(); i++) {
            if(guess[i] == secret[i]) {
                used[i] = BULL;
                bulls++;
            }
        }
        
        for(int i = 0; i < guess.size(); i++) {
            if(used[i] == BULL)
              continue;
            
            for(int j = 0; j < secret.size(); j++) {
                if((!used[j]) && secret[j] == guess[i]) {
                    used[j] = COW;
                    cows++;
                    break;
                }
            }           
        }
        string res = to_string(bulls) + "A" + to_string(cows) + "B";   
        return res;
    }
```

## Classic Solution
### 1. Two-Pass Solution
It employs another vector to record the number of non-bull digits, for example `s[0]` indicates the number of non-bull `'0'` in `secret`.

In the first pass, we record the number of bulls and all those non-bull digits in `secret` and `guess`.

In the second pass, we calculate the number of cows in digits ranging from `'0'` to `'9'`.

```C++
string getHint(string secret, string guess) {
    int bulls = 0, cows = 0;
    vector<int> s(10, 0);
    vector<int> g(10, 0);

    for(int i = 0; i < secret.size(); i++) {
    if(secret[i] == guess[i])
        bulls++;
    else {
        s[secret[i] - '0']++;
        g[guess[i] - '0']++;
    }
    }

    for(int i = 0; i < 10; i++) {
    cows += min(s[i], g[i]);
    }

    return to_string(bulls) + "A" + to_string(cows) + "B";
}      
```

### 2. One-Pass Solution
Quite hard for me to understand... We use two unordered maps to record the digits in `secret` and `guess` under current circumstance. So we can check if cow exist by examing current digit is already in the other map.

For index `i` ranging from `0` to `secret.size()`:

- check bull condition: if `secret[i]` is equal to `guess[i]`
- check if `guess[i]` is already in `s_map`
    
    - if exist: decrement `s_map[guess[i]]` and increment COW.
    - if not: increment `g_map[guess[i]]`.
- check if `secret[i]` is already in `g_map`

    - if exist: decrement `g_map[secret[i]]` and increment COW.
    - if not: increment `s_map[secret[i]]`.

```C++
string getHint(string secret, string guess) {
    unordered_map<char, int> s_map;
    unordered_map<char, int> g_map;
    int n = secret.size();
    int A = 0, B = 0;
    for (int i = 0; i < n; i++) {
        char s = secret[i], g = guess[i];
        if (s == g)
            A++;
        else {
            (s_map[g] > 0) ? s_map[g]--, B++ : g_map[g]++;
            (g_map[s] > 0) ? g_map[s]--, B++ : s_map[s]++; 
        }
    }
    return to_string(A) + "A" + to_string(B) + "B";;
}     
```