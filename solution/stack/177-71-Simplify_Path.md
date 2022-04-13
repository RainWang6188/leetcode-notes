# 71.Simplfy Path

## Description


Given a string `path`, which is an absolute path (starting with a slash `'/'`) to a file or directory in a Unix-style file system, convert it to the simplified canonical path.

In a Unix-style file system, a period `'.'` refers to the current directory, a double period `'..'` refers to the directory up a level, and any multiple consecutive slashes (i.e. `'//'`) are treated as a single slash `'/'`. For this problem, any other format of periods such as `'...'` are treated as file/directory names.

The canonical path should have the following format:

- The path starts with a single slash `'/'`.
- Any two directories are separated by a single slash `'/'`.
- The path does not end with a trailing `'/'`.
- The path only contains the directories on the path from the root directory to the target file or directory (i.e., no period `'.'` or double period `'..'`)

Return the simplified canonical path.

**Example:**
```
Input: path = "/home//foo/"
Output: "/home/foo"
Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.
```

## Classic Solution

[Here](https://www.cplusplus.com/reference/string/string/getline/)'s the document of `getline()` from cplusplus.


```C++
istream& getline (istream&  is, string& str, char delim);
```

`getline()` Extracts characters from `is` and stores them into `str` until the delimitation character `delim` is found.

The extraction also stops if the end of file is reached in `is` or if some other error occurs during the input operation.

If the delimiter is found, it is extracted and discarded (i.e. it is not stored and the next input operation will begin after it).

Note that any content in `str` before the call is replaced by the newly extracted sequence.

`getline()` returns the same as `is`.

```C++
string simplifyPath(string path) {
    vector<string> tokens;
    string res = "";
    string token;
    stringstream ss(path);
    
    while(getline(ss, token, '/')) {
        if(token == "." || token == "")
            continue;
        else if(token == "..") {
            if(!tokens.empty())
                tokens.pop_back();
        }
        else
            tokens.push_back(token);
    }
    
    if(tokens.empty())
        return "/";
    
    for(auto& t : tokens)
        res += "/" + t;
    
    return res;
}
```