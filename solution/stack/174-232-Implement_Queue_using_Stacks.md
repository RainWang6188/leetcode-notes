# 232. Implement Queue using Stacks

## Description

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (push, peek, pop, and empty).

Implement the `MyQueue` class:

- `void push(int x)` Pushes element x to the back of the queue.
- `int pop()` Removes the element from the front of the queue and returns it.
- `int peek()` Returns the element at the front of the queue.
- `boolean empty()` Returns true if the queue is empty, false otherwise.

**Notes:**

- You must use only standard operations of a stack, which means only push to top, peek/pop from top, size, and is empty operations are valid.
- Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

## My Solution

We will use two stacks. 

The first stack used for storing elements (So the element at the front of the queue will be at the bottom of the first stack). 

Once we need to `top` or `pop`, we pop all the elements from the first stack and push them into the second stack, so that the element at the front of the queue will be at the top of the second stack now.

```C++
class MyQueue {
public:
    MyQueue() {
        
    }
    
    void push(int x) {
        first.push(x);
    }
    
    int pop() {
        while(!first.empty()) {
            int val = first.top();
            first.pop();
            second.push(val);
        }
        int res = second.top();
        second.pop();
        while(!second.empty()) {
            int val = second.top();
            second.pop();
            first.push(val);
        }
        return res;
    }
    
    int peek() {
        while(!first.empty()) {
            int val = first.top();
            first.pop();
            second.push(val);
        }
        int res = second.top();
        while(!second.empty()) {
            int val = second.top();
            second.pop();
            first.push(val);
        }
        return res;
    }
    
    bool empty() {
        return first.empty();
    }
private:
    stack<int> first;
    stack<int> second;
};
```

## Classic Solution

I have one input stack, onto which I push the incoming elements, and one output stack, from which I `peek/pop`. I move elements from input stack to output stack when needed, i.e., when I need to `peek/pop` but the output stack is empty. When that happens, I move all elements from input to output stack, thereby reversing the order so it's the correct order for `peek/pop`.

The loop in peek does the moving from input to output stack. Each element only ever gets moved like that once, though, and only after we already spent time pushing it, so the overall amortized cost for each operation is `O(1)`.

```C++
class Queue {
    stack<int> input, output;
public:

    void push(int x) {
        input.push(x);
    }

    void pop(void) {
        peek();
        output.pop();
    }

    int peek(void) {
        if (output.empty())
            while (input.size())
                output.push(input.top()), input.pop();
        return output.top();
    }

    bool empty(void) {
        return input.empty() && output.empty();
    }
};
```