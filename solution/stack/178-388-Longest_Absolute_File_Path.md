# 388. Longest Absolute File Path


## Description

The description can be viewed [here](https://leetcode.com/problems/longest-absolute-file-path/).

**Example:**
```
Input: input = "dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext"
Output: 32
Explanation: We have two files:
"dir/subdir1/file1.ext" of length 21
"dir/subdir2/subsubdir2/file2.ext" of length 32.
We return 32 since it is the longest absolute path to a file.
```

## Classic Solution

The idea is that the vector will be overwritten but the value of max ans will sustain.

```C++
int lengthLongestPath(string input) {
    input.push_back('\n');
    vector<int> levels(300, 0);
    int level = 0;
    bool isFile = false;
    int ans = 0;
    int length = 0;
        
    for(char c: input){
        switch(c){
            case '\n': { level = 0; length = 0; isFile=false; break; }
            case '\t': { level++; break; }
            case '.' : isFile = true;
                
            default:
                length++;
                levels[level] = length;
                if(isFile){
                    ans = max(ans, accumulate(levels.begin(), levels.begin() + level + 1, 0) + level);
                }
        }
    }
    
    return ans;
}
```