# Stacks and Queues

## 1. Valid Parentheses

### Problem

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

### How we'll solve it

1. We return `False`  if the string has an odd number of elements because it is not able to form a a palindrome with the given characters.
2. We just append characters into the stack, but we pop them if they should be popped. We can pop a character if they are symmetrical to the incoming character. 
3. Once we consume all the characters in the input string, we check if the stack is empty. An empty stack means our string was a palindrome.

### Solution

```python
class Solution(object):
    def isValid(self, s):
    
        length = len(s)
        if 1 == length % 2:
            return False
        
        stack = []
        
        for i in range(length):
            if 0 == len(stack):
                stack.append(s[i])
                continue
            
            top = stack[-1]
                
            if "(" == top and ")" == s[i]:
                stack.pop()
            elif "{" == top and "}" == s[i]:
                stack.pop()
            elif "[" == top and "]" == s[i]:
                stack.pop()
            
            else:
                stack.append(s[i])
        
        return 0 == len(stack)
```

## 2. Min Stack

### Problem

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

* push\(x\) -- Push element x onto stack.
* pop\(\) -- Removes the element on top of the stack.
* top\(\) -- Get the top element.
* getMin\(\) -- Retrieve the minimum element in the stack.

### How we'll solve it

We can just create a regular stack that keeps track of the minimum element in an auxiliary stack. We just push and pop to the auxiliary stack based on the value of its top element \(which we define as the minimum element of the main stack\).

### Solution

```python
class MinStack(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.min_stack = []
        self.stack = []
        

    def push(self, x):
        """
        :type x: int
        :rtype: None
        """
        if not self.stack or x <= self.min_stack[-1]:
            self.min_stack.append(x)
        
        self.stack.append(x)
        

    def pop(self):
        """
        :rtype: None
        """
        top = self.stack.pop()
        if top == self.min_stack[-1]:
            self.min_stack.pop()

    def top(self):
        """
        :rtype: int
        """
        return self.stack[-1]
        

    def getMin(self):
        """
        :rtype: int
        """
        return self.min_stack[-1]
```

