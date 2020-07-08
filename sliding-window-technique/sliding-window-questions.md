# Sliding Window Questions

## 1. Reverse String

### Problem

Write a function that reverses a string in-place.

### How we'll solve it

We just need to set two pointers at both ends of the string. We move each towards the center of the string while swapping their values at each step.

### Solution

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        p1 = 0
        p2 = len(s) - 1
        
        while p1 < p2:
            temp = s[p1]
            s[p1] = s[p2]
            s[p2] = temp
            p1 += 1
            p2 -= 1
```

## 2. Valid Parentheses

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

## 3. Container With Most Water

### Problem

Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate \(i, ai\). n vertical lines are drawn such that the two endpoints of line i is at \(i, ai\) and \(i, 0\). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

**Note:** You may not slant the container and n is at least 2.

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

The above vertical lines are represented by array \[1,8,6,2,5,4,8,3,7\]. In this case, the max area of water \(blue section\) the container can contain is 49.

### How we'll solve it

We create two pointers at either end and just decide if we either move the left pointer to the right, or the right pointer to the left. We also calculate the max area spanned by the two pointers whenever one of them moves.

To determine which one moves, we just decide to move the smaller pointer value by a step, and move the left pointer to the right if both pointer values are equal.

The above can be proven formally, but this is the shorter version: moving inwards means less width, so less volume. Moving the larger pointer value means we would always be restricted with the height of the shorter pointer value and the decreasing width. We would always end up with a smaller area if we move the larger pointer value, therefore, we should be moving the lower pointer value to potentially get an increase of volume.

### Solution

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        
        p1 = 0
        p2 = len(height) - 1
        max_area = 0
        
        while p1 < p2:
            min_h = min(height[p1], height[p2])
            x_length = p2 - p1
            
            area_calculated = min_h * x_length
            
            if max_area < area_calculated:
                max_area = area_calculated
            
            # increment/decrement the pointer with the smaller value
            # increment p1 if equal
            if height[p1] < height[p2]:
                p1 += 1
            elif height[p1] > height[p2]:
                p2 -= 1
            else:
                p1 += 1
        
        return max_area
```

## 4. Find All Anagrams in a String

### Problem

Return the indices where anagrams of string **S** starts in string **P**. All given strings only contain the lowercase letters a, b, c, ..., z. 

```text
P: "potato"
S: "toa"

"ota" and "ato" have the same charactesr as (are anagrams of) "toa"
solutions: [1, 3]
```

### How we'll solve it

We'll use the sliding window technique to solve this. We just need to figure out if a contiguous \(side-by-side\) block of characters in the target string forms an anagram with the input string. An anagram of some string X is just a string that has the same characters as X in any order. An example is that "star wars" is an anagram of "war stars". 

For this problem, we just slide a window with the same size as our input string and check if the string covered by that window is an anagram of our input string. To avoid checking whether the characters in the window are contained in the input string, we can make use of an array that counts the characters included in the window. Sliding the window causes the array to change its value counts. 

If the character count array is identical to the input string's character count, then we can claim that the current position of the window is an anagram for the input string.

```text
target_string: "acbab"
input_string: "abc"

input string characters count:
{a: 1, b: 1, c: 1}

windows:
[a, c, b] { a: 1, b: 1, c: 1 } -> solution at index 0
[c, b, a] { a: 1, b: 1, c: 1 } -> solution at index 1
[b, a, b] { a: 1, b: 2, c: 0 } -> not a solution

solution:
[0, 1]
```

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        # our solutions
        solution_indices = []
        
        # target and window lengths
        target_length = len(s)
        window_length = len(p)
        
        # return early is p is longer than s
        if window_length > target_length:
            return solution_indices
        
        # our hash arrays
        # we can use a dictionary, but we only deal with 26 contiguous characters
        s_hash = [0 for _ in range(26)]
        p_hash = [0 for _ in range(26)]
        
        # populate initial solutions
        l = 0
        r = 0
        while r < window_length:
            # ord(s[r]) - 97 will give us the index of characers froms { a, b, ...z }
            s_hash[ord(s[r]) - 97] += 1
            p_hash[ord(p[r]) - 97] += 1
            r += 1
        r -= 1
        
        # iterate through target
        while r < target_length:
            # if both arrays are the same, there is a solution
            if s_hash == p_hash:
                solution_indices.append(l)
                
            # slide the window
            r += 1
            if r < target_length:
                s_hash[ord(s[r]) - 97] += 1
            s_hash[ord(s[l]) - 97] -= 1
            l += 1
            
        return solution_indices
```

## 5. Permutation in String

### Problem

Return true if string **s2** contains a permutation of string **s1**.



### How we'll solve it

The approach here is the same as in the previous question.

### Solution

```python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        
        # our target and window lengths 
        target_length = len(s2)
        window_length = len(s1)
        
        # return early if window is longer than target
        if window_length > target_length:
            return False
        
        target_hash = [0 for _ in range(26)]
        window_hash = [0 for _ in range(26)]
        
        l = 0
        r = 0
        
        # populate our initial array hashes
        while r < window_length:
            target_hash[ord(s2[r]) - 97] += 1
            window_hash[ord(s1[r]) - 97] += 1
            r += 1
        r -= 1
        
        # iterate through target
        while r < target_length:
            if target_hash == window_hash:
                return True
            # slide the window
            r += 1
            if r < target_length:
                target_hash[ord(s2[r]) - 97] += 1
            target_hash[ord(s2[l]) - 97] -= 1
            l += 1
        
        return False
```

## 6. Minimum Window Substring

### Problem

Find the smallest substring in string **S** which will contain all the characters in **T.**

### How we'll solve it

We can construct a sliding window. The window expands outwards until the following condition is met: the window has all the characters of **T.**  When all characters of **T** are contained within the window, we collapse it inwards until a character from **T** is lost. We only save the position of a valid window when its width is smaller than previous valid windows. We can figure out if a window is valid by checking its character counts against the character counts of **T**. 

It looks like this:

```text
S: "BECDACCAB"
T: "ABC"

windows:
[B] -> invalid, expand
[B, E]
[B, E, C]
[B, E, C, D]

[B, E, C, D, A] -> valid, collapse
[B, E, C, D, A] -> save "BECDA" (length 5)

[E, C, D, A] -> invalid, expand
[E, C, D, A, C]
[E, C, D, A, C, C]
[E, C, D, A, C, C, B] -> valid, collapse
[C, D, A, C, C, B] -> ignore "CDACCB" (length 6)
[D, A, C, C, B]
[A, C, C, B] -> save "ACCB" (length 4)
[C, C, B] -> invalid


solution:
"ACCB"
```

As a tip, it's usually better to just return a substring without re-assigning a substring variable in the solution. In other words, avoid reassigning a substring into a variable while sliding the window. Just return the shortest possible substring which you can identify by storing the left and right indices of the smallest valid window you find. 

### Solution

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:        

        size_s = len(s)
        size_t = len(t)
        
        counts = {}
        counts_aux = {}
        for c in t:
            counts[c] = counts.get(c, 0) + 1
            counts_aux[c] = 0
            
        l = 0
        r = 0
        
        valid_characters = 0
        
        left = 0
        right = 0
        substr_l = 0
        substr_r = len(s)
        solutionExists = False
        
        for i, c in enumerate(s):
            
            if c not in counts:
                continue
                
            right = i
            counts_aux[c] += 1
            if counts[c] == counts_aux[c]:
                valid_characters += counts[c]

            # if a substring got found, move left pointer until window is no longer valid
            while left <= right and valid_characters == size_t:
                
                left_char = s[left]
                if left_char in counts:

                    # reset our counter
                    if counts_aux[left_char] >= counts[left_char]:
                        counts_aux[left_char] -= 1
                    if counts_aux[left_char] < counts[left_char]:
                        valid_characters -= counts[left_char]
                     
                    # update shortest substring
                    if right + 1 - left <= substr_r - substr_l:
                        substr_l = left
                        substr_r = right + 1
                        solutionExists = True
                left += 1

        if solutionExists:
            return s[substr_l:substr_r]
    
        return ""
```



