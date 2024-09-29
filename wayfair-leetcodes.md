
## Longest Common Substring
### Problem Statement:
We are given two strings, and we need to find the longest sequence of characters that appears in both strings continuously (as a substring). 

For example:
- For strings `s1 = "abcdef"` and `s2 = "zbcdf"`, the longest common substring is `"bcd"`.

### Code:

```python
def longest_common_substring(s1, s2):
    # Get lengths of both strings
    m, n = len(s1), len(s2)
    
    # Variables to keep track of the longest length and its ending position
    longest = 0  # This stores the length of the longest common substring found
    ending_index = 0  # This stores where the longest substring ends in s1
    
    # Create a 2D table to store lengths of common substrings for each pair of indices
    # dp[i][j] will be the length of the longest common suffix for s1[0:i] and s2[0:j]
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Loop through each character of both strings
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            # If characters match, increase the length of common substring
            if s1[i - 1] == s2[j - 1]:
                # The length of the common substring ending at s1[i-1] and s2[j-1] is 1 + the length of the common substring ending at s1[i-2] and s2[j-2]
                dp[i][j] = dp[i - 1][j - 1] + 1
                
                # Update longest and ending_index if the current common substring is the longest we've found so far
                if dp[i][j] > longest:
                    longest = dp[i][j]
                    ending_index = i
            else:
                dp[i][j] = 0  # If characters don't match, no common substring ends here
    
    # The longest common substring ends at index ending_index in s1, with length 'longest'
    # So, we return the substring from s1 that starts at 'ending_index - longest' and ends at 'ending_index'
    return s1[ending_index - longest: ending_index]
```

### Explanation:

1. **Initialize Variables:**
   - `m` and `n` are the lengths of strings `s1` and `s2`, respectively.
   - `longest` keeps track of the length of the longest common substring found so far.
   - `ending_index` helps us remember where this longest common substring ends in `s1`.
   - `dp` is a 2D table (a list of lists) where `dp[i][j]` stores the length of the longest common substring that ends at `s1[i-1]` and `s2[j-1]`.

2. **Dynamic Programming Table (`dp`):**
   - The idea is to fill the `dp` table, where each cell represents the length of the common substring ending at a particular index of `s1` and `s2`.
   - For example, `dp[i][j]` will store how long the common substring is between `s1[0:i]` and `s2[0:j]` if they end with the same character.

3. **Character Matching:**
   - As we loop through the strings using the indices `i` and `j`, we check if the characters `s1[i-1]` and `s2[j-1]` are equal.
   - If they match, it means we've found a part of a common substring, so we update `dp[i][j] = dp[i-1][j-1] + 1`. This tells us that the length of the common substring ending at these characters is one more than the length of the common substring that ended at the previous characters.

4. **Track the Longest Substring:**
   - Whenever we update `dp[i][j]` (i.e., we found a common substring), we check if its length is greater than the current `longest`. If it is, we update `longest` and store the `ending_index` where this longest substring ends.

5. **Return the Longest Common Substring:**
   - After we finish building the `dp` table, the longest common substring is the one that ends at `ending_index` in `s1` and has length `longest`.
   - We use slicing to extract this substring from `s1`, starting from `ending_index - longest` to `ending_index`.

### Example Walkthrough:

For `s1 = "abcdef"` and `s2 = "zbcdf"`:

- We compare each character of `s1` with each character of `s2`.
- We find that characters `"bcd"` are common in both strings (in order).
- The longest common substring is `"bcd"` with length 3.
  

So, the code will return `"bcd"`.

### Time and Space Complexity:
- **Time Complexity**: O(m * n), where `m` and `n` are the lengths of the two strings. This is because we are looping through every pair of characters from `s1` and `s2`.
- **Space Complexity**: O(m * n) for storing the `dp` table.
