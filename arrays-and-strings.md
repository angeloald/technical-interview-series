# Arrays and Strings

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

## 2. Container With Most Water

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

## 3. Find All Anagrams in a String

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

## 4. Permutation in String

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

