# Lambda Expressions in C++

## The problem
C++ includes useful generic functions like `std::for_each` and `std::transform`, which can be very handy. Unfortunately they can also be quite cumbersome to use, particularly if the functor you would like to apply is unique to the particular function.
```C++
#include <algorithm>
#include <vector>

namespace {
  struct f {
    void operator()(int) {
      // do something
    }
  };
}

void func(std::vector<int>& v) {
  f f;
  std::for_each(v.begin(), v.end(), f);
}
```
If you only use `f` once and in that specific place it seems overkill to be writing a whole class just to do something trivial and one off.

In C++03 you might be tempted to write something like the following, to keep the functor local:
```C++
void func2(std::vector<int>& v) {
  struct {
    void operator()(int) {
       // do something
    }
  } f;
  std::for_each(v.begin(), v.end(), f);
}
```
however this is not allowed, `f` cannot be passed to a template function in C++03.

## The new solution
C++11 introduces **lambdas** allow you to write an inline, anonymous functor to replace the struct `f`. For small simple examples this can be cleaner to read (it keeps everything in one place) and potentially simpler to maintain, for example in the simplest form:
```C++
void func3(std::vector<int>& v) {
  std::for_each(v.begin(), v.end(), [](int) { /* do something here*/ });
}
```
Lambda functions are just syntactic sugar for anonymous functors.

## Parts of a lambda expression
The ISO C++ Standard shows a simple lambda that is passed as the third argument to the `std::sort()` function:

```C++
#include <algorithm>
#include <cmath>

void abssort(float* x, unsigned n) {
    std::sort(x, x + n,
        // Lambda expression begins
        [](float a, float b) {
            return (std::abs(a) < std::abs(b));
        } // end of lambda expression
    );
}
```
This illustration shows the parts of a lambda:

![parts_of_lambda](https://docs.microsoft.com/en-us/cpp/cpp/media/lambdaexpsyntax.png?view=msvc-170)

It includes the following parts:

1. *capture clause* (Also known as the *lambda-introducer* in the C++ specification.)

2. *parameter list* Optional. (Also known as the *lambda declarator*)

3. *mutable specification* Optional.

4. *exception-specification* Optional.

5. *trailing-return-type* Optional.

6. *lambda body*.

> Here's a specification of the *capture clause*, other parts of a lambda expression will be omitted since they're quite ordinary:
> You can capture by both **reference** and **value**, which you can specify using `&` and `=` respectively:
> 
> - `[&epsilon, zeta]` captures `epsilon` by reference and `zeta` by value
> - `[&]` captures all variables used in the lambda by reference
> - `[=]` captures all variables used in the lambda by value
> - `[&, epsilon]` captures all variables used in the lambda by reference but captures `epsilon` by value
> - `[=, &epsilon]` captures all variables used in the lambda by value but captures `epsilon` by reference

## Examples
For me, the most popular scenarios where lambda expressions are used is in a heap (i.e. priority queue) definition.

Here is an example of a min-heap whose elements are `TreeNode*`:
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

    auto cmp = [&](TreeNode* a, TreeNode* b) {
        return a.val > b.val;
    }
    priority_queue<TreeNode*, vector<TreeNode*>, decltype(cmp)> pq(cmp);
```