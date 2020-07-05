# Sliding Window

I think this discussion on sliding windows is the most valuable part of this book because interviewers tend to like asking questions where you need to use this technique. This technique is a game changer because it is rarely taught in schools despite being used so often in the hiring process. If you really want the get the most value for the least amount of time, just read this discussion.

## Concept

Questions involving sliding windows are probably the most common in technical interviews. This topic is extra special for me because I failed an interview a long time ago because I failed to answer a sliding window question. I hope the same does not happen to you because I will show you all the possible ways of solving this type of question.

Sliding windows are just subarrays that move around a larger array. The window "slides" because it can move around the larger array and change its size. A left pointer and a right pointer are used to denote the position of the window in the larger array. Your job is to move these two pointers then save solutions whenever your subarray is a valid window that satisfies some condition.  

## Sliding Window Taxonomy

All sliding window problems involve two pointers. These two pointers can move at the same direction or they can move at different directions by either expanding or contracting. They can also move at the same time or at different times.

### Same direction + synchronous 

* The window size is fixed and moves from one end of the array to the other end.
* You could use this when you get asked a question to determine whether or not some string contains another string.
* This could also be used when they explicitly tell you to use subarrays of size **k** to find solutions.

```text
+-----------+
| a   b   c | d   e   f   g   h
+-----------+
    +-----------+
  a | b   c   d | e   f   g   h
    +-----------+
        +-----------+
  a   b | c   d   e | f   g   h
        +-----------+
            +-----------+
  a   b   c | d   e   f | g   h
            +-----------+
                +-----------+
  a   b   c   d | e   f   g | h
                +-----------+
                    +-----------+
  a   b   c   d   e | f   g   h |
                    +-----------+
```

### Same direction + asynchronous

* This is usually the approach for questions where the requirement is to construct a window such that it contains all of the characters of a target string \(anagram questions\).
* You usually use a combination of a hash table \(of value counts\) and a counter to know if the current window contains what you are looking for.

```text
+---+
| a | b   c   d   e   f
+---+
+-------+
| a   b | c   d   e   f
+-------+
+-----------+
| a   b   c | d   e   f
+-----------+
        +---+
  a   b | c | d   e   f
        +---+
        +-------+
  a   b | c   d | e   f
        +-------+
            +---+
  a   b   c | d | e   f
            +---+
            +-------+
  a   b   c | d   e | f
            +-------+
            +-----------+
  a   b   c | d   e   f |
            +-----------+
                +-------+
  a   b   c   d | e   f |
                +-------+
                    +---+
  a   b   c   d   e | f |
                    +---+
```

### Different directions + synchronous

* The window can collapse \(move inwards\) or expand \(move outwards\) at the same time.
* You can use this to confirm is something is a palindrome.

```text
+-------------------------------+
| a   b   c   d   e   f   g   h |
+-------------------------------+
    +-----------------------+
  a | b   c   d   e   f   g | h
    +-----------------------+
        +---------------+
  a   b | c   d   e   f | g   h
        +---------------+
            +-------+
  a   b   c | d   e | f   g   h
            +-------+

```

### Different directions + asynchronous

* The window can collapse \(move inwards\) or expand \(move outwards\) at different times.
* The challenge with this variant of the sliding window is determining which of the two pointers to move. If you are not sure which pointer to move, try to list down all the possibilities of what could happen by moving either pointer for each possible condition.

```text
+--------------------------------+
| a   b   c   d   e   f   g   h  |
+--------------------------------+
+---------------------------+
| a   b   c   d   e   f   g | h
+---------------------------+
+-----------------------+
| a   b   c   d   e   f | g   h
+-----------------------+
    +-------------------+
  a | b   c   d   e   f | g   h
    +-------------------+
        +---------------+
  a   b | c   d   e   f | g   h
        +---------------+
        +-----------+
  a   b | c   d   e | f   g   h
        +-----------+
            +-------+
  a   b   c | d   e | f   g   h
            +-------+
            +---+
  a   b   c | d | e   f   g   h
            +---+
```

### Special cases

* Some problems require you to go through an array element by element and treat element as a starting point for one of the sliding window patterns above. For example, you could get a question where you have to process elements in an array and treat each element as an expanding window.
* Sometimes you might have to move the pointers by more than one step. It is up to you to determine when to do this.

```text
Find the longest palindromic substring in "banana"

    +---+
  Ø | b | a   n   a   n   a   Ø  - pass, "b"
    +---+
+-----------+
| Ø   b   a | n   a   n   a   Ø  - fail
+-----------+
        +---+
  Ø   b | a | n   a   n   a   Ø  - pass, "a"
        +---+
    +-----------+
  Ø | b   a   n | a   n   a   Ø  - fail
    +-----------+
            +---+
  Ø   b   a | n | a   n   a   Ø  - pass, "n"
            +---+
        +-----------+
  Ø   b | a   n   a | n   a   Ø  - pass "ana"
        +-----------+
    +-------------------+
  Ø | b   a   n   a   n | a   Ø  - fail
    +-------------------+
                +---+
  Ø   b   a   n | a | n   a   Ø  - pass, "a"
                +---+
            +-----------+
  Ø   b   a | n   a   n | a   Ø  - pass, "nan"
            +-----------+
        +-------------------+
  Ø   b | a   n   a   n   a | Ø  - pass, "anana"
        +-------------------+
    +-----------------------+
  Ø | b   a   n   a   n   a | Ø - fail, return "anana", exceeds remaining length
    +-----------------------+
```



