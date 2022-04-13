# stringstream in C++

A `stringstream` associates a `string` object with a stream allowing you to read from the string as if it were a stream (like `cin`). To use stringstream, we need to include `sstream` header file. The stringstream class is extremely useful in parsing input. 

Here're some basic methods:
1. `clear()`- To clear the stream.
2. `str()`- To get and set string object whose content is present in the stream. 
3. operator `<<` - Add a string to the stringstream object. 
4. operator `>>` - Read something from the stringstream object.


## Applications

### 1. Count the Number of Words of a String

```C++
// C++ program to count words in
// a string using stringstream.
#include <iostream>
#include <sstream>
#include<string>
using namespace std;

int countWords(string str)
{
	// Breaking input into word
	// using string stream

	// Used for breaking words
	stringstream s(str);

	// To store individual words
	string word;

	int count = 0;
	while (s >> word)
		count++;
	return count;
}

// Driver code
int main()
{
	string s = "geeks for geeks geeks "
			"contribution placements";
	cout << " Number of words are: " << countWords(s);
	return 0;
}

```

This can also be used to find frequencies of individual words in a given string

```C++
void printFrequency(string st)
{
    // Each word it mapped to
    // it's frequency
    map<string, int>FW;
   
    // Used for breaking words
    stringstream ss(st);
   
    // To store individual words
    string Word;
 
    while (ss >> Word)
        FW[Word]++;
 
    map<string, int>::iterator m;
    for (m = FW.begin(); m != FW.end(); m++)
        cout << m->first << "-> "
             << m->second << "\n";
}
```


### 2. Removing Spaces from a String

#### 2.1 Using `getline()`
```C++
// C++ program to remove spaces using stringstream
// and getline()
#include <bits/stdc++.h>
using namespace std;

// Function to remove spaces
string removeSpaces(string str)
{
	// Storing the whole string
	// into string stream
	stringstream ss(str);
	string temp;

	// Making the string empty
	str = "";

	// Running loop till end of stream
	// and getting every word
	while (getline(ss, temp, ' ')) {
		// Concatenating in the string
		// to be returned
		str = str + temp;
	}
	return str;
}
// Driver function
int main()
{
	// Sample Inputs
	string s = "This is a test";
	cout << removeSpaces(s) << endl;

	s = "geeks for geeks";
	cout << removeSpaces(s) << endl;

	s = "geeks quiz is awesome!";
	cout << removeSpaces(s) << endl;

	s = "I love	 to	 code";
	cout << removeSpaces(s) << endl;

	return 0;
}
```

#### 2.2 Using `eof`
```C++
// C++ program to remove spaces using stringstream
#include <bits/stdc++.h>
using namespace std;

// Function to remove spaces
string removeSpaces(string str)
{
	stringstream ss;
	string temp;

	// Storing the whole string
	// into string stream
	ss << str;

	// Making the string empty
	str = "";

	// Running loop till end of stream
	while (!ss.eof()) {

		// Extracting word by word from stream
		ss >> temp;

		// Concatenating in the string to be
		// returned
		str = str + temp;
	}
	return str;
}

// Driver function
int main()
{
	// Sample Inputs
	string s = "This is a test";
	cout << removeSpaces(s) << endl;

	s = "geeks for geeks";
	cout << removeSpaces(s) << endl;

	s = "geeks quiz is awesome!";
	cout << removeSpaces(s) << endl;

	s = "I love	 to	 code";
	cout << removeSpaces(s) << endl;

	return 0;
}
```

### 3. Converting Strings to Numbers

```C++
// A program to demonstrate the use of stringstream
#include <iostream>
#include <sstream>
using namespace std;

int main()
{
	string s1 = "12345";

	// object from the class stringstream
	stringstream geek(s1);

	// The object has the value 12345 and stream
	// it to the integer x
	int x = 0;
	geek >> x;

	// Now the variable x holds the value 12345
	cout << "Value of x : " << x << endl;


    string s2 = "123.45";
    stringstream str(s2);
    double x2 = 0;
    str >> x2;

    cout << "Value of x2 : " << x2 << endl;

	return 0;
}
```