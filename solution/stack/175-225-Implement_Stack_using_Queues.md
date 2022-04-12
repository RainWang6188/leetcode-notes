# 225. Implement Stack using Queues

## Description

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (push, top, pop, and empty).

Implement the `MyStack` class:

- `void push(int x)` Pushes element x to the top of the stack.
- `int pop()` Removes the element on the top of the stack and returns it.
- `int top()` Returns the element on the top of the stack.
- `boolean empty()` Returns true if the stack is empty, false otherwise.

**Notes:**

- You must use only standard operations of a queue, which means that only push to back, peek/pop from front, size and is empty operations are valid.
- Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue) as long as you use only a queue's standard operations.



## My Solution

We will use two queues.

Before any operation, there'll be only one of two contains elements, denoted as `A`.

Suppose we want to push, then we'll simply push the new element to the back of the `A` queue.

If we want to pop or peek, we'll pop elements from the front of queue `A` and push them into the back of queue `B` untill there's one element remaining. And that remained element will be the element on the top of the stack.

```C++
class MyStack {
public:
    MyStack() {
        
    }
    
    void push(int x) {
        if(empty())
            q1.push(x);
        else if(q1.empty())
            q2.push(x);
        else
            q1.push(x);
            
    }
    
    int pop() {
        int res = INT_MAX;
        if(q1.empty()) {
            while(q2.size() > 1) {
                q1.push(q2.front());
                q2.pop();
            }
            res = q2.front();
            q2.pop();
        }
        else {
            while(q1.size() > 1) {
                q2.push(q1.front());
                q1.pop();
            }
            res = q1.front();
            q1.pop();
        }
        return res;
    }
    
    int top() {
        int res = INT_MAX;
        if(q1.empty()) {
            while(!q2.empty()) {
                res = q2.front();
                q1.push(res);
                q2.pop();
            }
        }
        else {
            while(!q1.empty()) {
                res = q1.front();
                q2.push(res);
                q1.pop();
            }
        }
        return res;
    }
    
    bool empty() {
        return q1.empty() && q2.empty();
    }

private:
    queue<int> q1;
    queue<int> q2;
};
```

## Classic Solution
A solution using only one queue.

```C++
class Stack {
public:
	queue<int> que;
	// Push element x onto stack.
	void push(int x) {
		que.push(x);
		for(int i=0;i<que.size()-1;++i){
			que.push(que.front());
			que.pop();
		}
	}

	// Removes the element on top of the stack.
	void pop() {
		que.pop();
	}

	// Get the top element.
	int top() {
		return que.front();
	}

	// Return whether the stack is empty.
	bool empty() {
		return que.empty();
	}
};
```