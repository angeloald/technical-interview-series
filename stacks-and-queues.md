# Stacks and Queues

### 1. Valid Parentheses

#### Problem

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

#### How we'll solve it

1. We return `False`  if the string has an odd number of elements because it is not able to form a a palindrome with the given characters.
2. We just append characters into the stack, but we pop them if they should be popped. We can pop a character if they are symmetrical to the incoming character. 
3. Once we consume all the characters in the input string, we check if the stack is empty. An empty stack means our string was a palindrome.

#### Solution

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



