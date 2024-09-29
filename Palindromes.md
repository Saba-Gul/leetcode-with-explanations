## Valid Palindrome
### Problem Statement:
The task is to check if a given string is a valid palindrome. A **palindrome** is a string that reads the same backward as forward, ignoring case and non-alphanumeric characters.

### Example:
- Input: `"A man, a plan, a canal: Panama"`
- Output: `True` (ignoring case and punctuation, the string becomes `"amanaplanacanalpanama"` which is a palindrome)

- Input: `"race a car"`
- Output: `False` (`"raceacar"` is not a palindrome)

### Approach:
We need to clean up the string by:
1. **Lowercasing all the letters.**
2. **Removing all non-alphanumeric characters** (ignoring spaces, punctuation, etc.).
3. **Comparing the cleaned string to its reverse.**

If the cleaned string is the same when reversed, it is a palindrome.

### Solution Explanation:

```python

  def isPalindrome(self, s):
      # Step 1: Clean the string
      # Convert the string to lowercase and filter out non-alphanumeric characters
      cleaned = ''.join(char.lower() for char in s if char.isalnum())
      
      # Step 2: Check if the cleaned string is a palindrome
      # Compare the cleaned string to its reverse
      return cleaned == cleaned[::-1]
```

### Steps Breakdown:

1. **Cleaning the String:**
   - `char.lower()` converts each character in the string to lowercase.
   - `char.isalnum()` ensures that only letters (a-z, A-Z) and numbers (0-9) are kept. All other characters, such as punctuation and spaces, are ignored.
   - `''.join(...)` combines the cleaned characters into a new string without spaces or special characters.

   Example:
   - Input: `"A man, a plan, a canal: Panama"`
   - Cleaned: `"amanaplanacanalpanama"`

2. **Palindrome Check:**
   - `cleaned == cleaned[::-1]`: This compares the cleaned string to its reverse. If they are equal, the string is a palindrome.

### Key Concepts:

- **String Slicing (`[::-1]`)**: This is Python’s way to reverse a string. It creates a new string where the characters are in reverse order.
  
### Example Walkthrough:

Let's use the input string: `"A man, a plan, a canal: Panama"`.

1. **Step 1: Clean the string:**
   - The string becomes `"amanaplanacanalpanama"`.

2. **Step 2: Compare the cleaned string to its reverse:**
   - Reverse of `"amanaplanacanalpanama"` is `"amanaplanacanalpanama"`.
   - Since both strings are equal, the function returns `True`.

### Time Complexity:
- **Cleaning the string:** We iterate over all characters in the string, so this step takes **O(n)**, where `n` is the length of the string.
- **Palindrome check:** Reversing the string and comparing it to the original also takes **O(n)**.
  
Thus, the overall time complexity is **O(n)**.

### Space Complexity:
- We create a new string (`cleaned`), so the space complexity is **O(n)**.
--
  
## Find the longest Palindrome in a string

### Problem:
We want to find the **longest sequence of characters** in a string that reads the same forward and backward. This is called a **palindrome**.

For example:
- In the string `"babad"`, the longest palindromic substring could be `"bab"` or `"aba"`.
- In the string `"cbbd"`, the longest palindromic substring is `"bb"`.

### Basic Idea:
We check **every possible center** of a palindrome in the string and then expand outwards from the center to see how far the palindrome goes. There are two types of centers:
1. **Odd-length palindromes** (like `"racecar"`): These have a single character in the center.
2. **Even-length palindromes** (like `"abba"`): These have two characters in the center.

### Step-by-Step Explanation:

1. **Start with an empty result**:
   - We initialize a variable `p` to store the longest palindrome we find. Initially, it’s an empty string (`p = ""`).

2. **Check each position in the string**:
   - For each character in the string (from the first to the last), we assume that it might be the center of a palindrome.
   
3. **Find the longest palindrome for both odd and even centers**:
   - For **odd-length palindromes**, we check around a single character (like `"racecar"`, centered at the middle `e`).
   - For **even-length palindromes**, we check between two characters (like `"abba"`, centered between the two middle `b`s).
   
   We do this by calling the helper function `get_palindrome`.

4. **Expand outward from the center**:
   - The helper function `get_palindrome(s, l, r)` tries to expand outward from the center. It starts with two pointers `l` (left) and `r` (right) and keeps moving them outward (`l--` and `r++`) as long as the characters on both sides are the same.
   - Once the characters don’t match, it stops and returns the longest palindromic substring found.

5. **Update the longest palindrome**:
   - After checking for both odd and even-length palindromes at each position, we compare the current longest palindrome (`p`) with the ones we just found (`p1` and `p2`).
   - We keep the longest one.

6. **Return the result**:
   - After going through all the characters, `p` will contain the longest palindromic substring.

### Solution Explanation:

```python
def longestPalindrome(self, s):
    p = ''  # Start with an empty result
    
    for i in range(len(s)):  # Check each character in the string
        # Find the longest even-length palindrome centered between i and i+1
        p1 = self.get_palindrome(s, i, i+1)
        
        # Find the longest odd-length palindrome centered at i
        p2 = self.get_palindrome(s, i, i)
        
        # Keep the longest palindrome found so far
        p = max([p, p1, p2], key=len)
    
    return p  # Return the longest palindrome

def get_palindrome(self, s, l, r):
    # Expand outward as long as the characters on both sides are the same
    while l >= 0 and r < len(s) and s[l] == s[r]:
        l -= 1  # Move left pointer outward
        r += 1  # Move right pointer outward
    
    # Return the palindromic substring found between l+1 and r
    return s[l+1:r]
```

### How it works:

- For the string `"babad"`, it will check:
  - At index `0`, it will expand around `"b"`, and find no bigger palindrome.
  - At index `1`, it will expand around `"a"`, and find `"aba"`.
  - It continues like this and finally returns `"aba"` (or `"bab"` since both are equally long).

### Summary:

- The code checks every possible center of a palindrome in the string.
- It expands outwards from each center to find the longest palindrome.
- It returns the longest palindromic substring found.

## Palindrome Partitioning:

### Problem Overview:
You are given a string `s`, and your goal is to split this string into multiple parts (substrings) such that **every substring** in the split is a palindrome. You need to find **all possible ways** to do this.

- A **palindrome** is a string that reads the same forward and backward, like "aa", "aba", or "racecar".

### Example:

- **Example 1**:
  - Input: `s = "aab"`
  - Output: `[['a', 'a', 'b'], ['aa', 'b']]`
  - Explanation:
    1. In the first way, you can split "aab" into `['a', 'a', 'b']` where all the parts are palindromes.
    2. In the second way, you can split "aab" into `['aa', 'b']` where "aa" and "b" are both palindromes.

### Solution Approach:

We use **backtracking** to solve this problem. The idea is to try every possible way to split the string and check if each part is a palindrome.

**Steps:**

1. Start from the first character of the string.
2. Try to take all possible substrings starting from this character.
3. For each substring:
   - If it's a palindrome, we try to split the remaining part of the string.
   - If we successfully reach the end of the string with palindromes, we have a valid partition.
4. If a partition is valid, we save it.

### Solution Explanation:

```python

    def partition(self, s):
        result = []  # This will store the final list of palindrome partitions

        # Function to check if a substring is a palindrome
        def is_palindrome(sub):
            return sub == sub[::-1]  # A string is a palindrome if it equals its reverse

        # Backtracking function to find all palindrome partitions
        def backtrack(start, current_partition):
            # If we reach the end of the string, we have a valid partition
            if start == len(s):
                result.append(current_partition[:])  # Save the current partition
                return

            # Try to split the string into palindromes from the 'start' index
            for end in range(start, len(s)):
                substring = s[start:end + 1]  # Get the substring from 'start' to 'end'
                if is_palindrome(substring):  # Check if it's a palindrome
                    current_partition.append(substring)  # Add the palindrome to the current partition
                    backtrack(end + 1, current_partition)  # Try to partition the remaining string
                    current_partition.pop()  # Backtrack (undo the last choice)

        backtrack(0, [])  # Start the backtracking from the 0th index
        return result
```

### How it Works:

- **`is_palindrome(sub)`**: This checks if a given substring is a palindrome by comparing it to its reverse.
  
- **`backtrack(start, current_partition)`**:
  - `start`: The current position in the string where we're trying to make a partition.
  - `current_partition`: The current list of palindrome substrings we've found.
  - The function keeps trying to split the string starting from `start`. Every time it finds a palindrome, it adds it to the current partition and tries to partition the rest of the string.
  - Once it reaches the end of the string (when `start == len(s)`), it adds the partition to the result.

### Example Walkthrough:

For `s = "aab"`:

1. Start at the first character `'a'`.
   - `'a'` is a palindrome. So, we partition it and check the rest of the string `"ab"`.
   
2. For the remaining string `"ab"`:
   - `'a'` is a palindrome, and `'b'` is a palindrome. So, the first valid partition is `['a', 'a', 'b']`.

3. Backtrack and check other possibilities:
   - `'aa'` is a palindrome. The remaining string is `'b'`, which is also a palindrome. So, another valid partition is `['aa', 'b']`.

In the end, we have two partitions: `[['a', 'a', 'b'], ['aa', 'b']]`.

### Conclusion:

This solution uses backtracking to explore every possible way to split the string, and only keeps the partitions where all substrings are palindromes.
