# 155. Min Stack

## Description

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element val onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

## My Solution

I used a vector to store the values and perform the stack operations.

```C++
class MinStack {
public:
    MinStack() {
        this->stack.clear();      
    }
    
    void push(int val) {
        stack.push_back(val);
    }
    
    void pop() {
        stack.pop_back();
    }
    
    int top() {
        return stack.back();
    }
    
    int getMin() {
        int minVal = INT_MAX;
        for(auto& val : stack)
            minVal = min(minVal, val);
        return minVal;
    }
private:
    vector<int> stack;
};
```

## Classic Solution
Here's the best solution for this problem.

```C++
class MinStack {
private:
    class Node {
    public:
        int val;
        int min;
        Node* next;
        Node(int val, int min, Node* next) : val(val), min(min), next(next) {}
    };
    
    Node* head;
    
public:
    MinStack() {
        this->head = new Node(INT_MAX, INT_MAX, nullptr);
    }
    
    void push(int val) {
        head = new Node(val, min(val, head->min), head);
    }
    
    void pop() {
        Node* deleted = head;
        head = head->next;
        delete deleted;
    }
    
    int top() {
        return head->val;
    }
    
    int getMin() {
        return head->min;
    }
};
```