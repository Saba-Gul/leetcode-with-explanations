## Anagram Related Codes


### 1. Check Anagram

#### Problem Statement:
Given two strings `s` and `t`, return true if `t` is an anagram of `s`, and false otherwise.

#### Code:
```python
def isAnagram(s, t):
    # Sort both strings and check if they are equal
    return sorted(s) == sorted(t)

# Example Usage
print(isAnagram("anagram", "nagaram"))  # Output: True
print(isAnagram("rat", "car"))           # Output: False
```

#### Explanation:
- **Sorting**: The function sorts both strings. If they are anagrams, their sorted forms will be identical.
- **Comparison**: The sorted strings are compared directly to determine if they are anagrams.

#### Output:
```
True
False
```

---

### 2. Find All Anagram Indices

#### Problem Statement:
Given two strings `s` and `p`, return an array of all the start indices of `p`'s anagrams in `s`.

#### Code:
```python
from collections import Counter

def findAnagrams(s, p):
    result = []
    # Count character occurrences in p
    p_count = Counter(p)
    # Count character occurrences in the initial window of s
    s_count = Counter(s[:len(p)])

    # Sliding window over string s
    for i in range(len(p), len(s)):
        # If the counts match, it means an anagram is found
        if s_count == p_count:
            result.append(i - len(p))
        
        # Slide the window: include new character and exclude old character
        s_count[s[i]] += 1
        s_count[s[i - len(p)]] -= 1

        # Remove the count of the character if it becomes zero
        if s_count[s[i - len(p)]] == 0:
            del s_count[s[i - len(p)]]

    # Check for the last window
    if s_count == p_count:
        result.append(len(s) - len(p))

    return result

# Example Usage
print(findAnagrams("cbaebabacd", "abc"))  # Output: [0, 6]
print(findAnagrams("abab", "ab"))          # Output: [0, 1, 2]
```

#### Explanation:
- **`Counter`**: Used to count the frequency of characters in both `p` and the current window of `s`.
- **Sliding Window Technique**: The window slides over the string `s`, updating the count of characters as it moves.
- **Comparison**: If the counts of the current window match those of `p`, the starting index of the window is added to the results.

#### Output:
```
[0, 6]
[0, 1, 2]
```

---
### 3. Group Anagrams

#### Problem Statement:
Given an array of strings, group the anagrams together. An anagram is a word formed by rearranging the letters of another, such as "eat" and "tea".

#### Code:
```python
def groupAnagrams(strs):
    anagram_dict = {}

    # Step 1: Group words by their sorted letter sequence
    for word in strs:
        sorted_word = ''.join(sorted(word))  # Sort the word to get the "signature"
        if sorted_word in anagram_dict:
            anagram_dict[sorted_word].append(word)  # Add the word to the existing group
        else:
            anagram_dict[sorted_word] = [word]  # Create a new group for this sorted form
    
    # Step 2: Return all groups of anagrams
    return list(anagram_dict.values())

# Example Usage
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
print(groupAnagrams(strs))
```

#### Explanation:
- **`defaultdict(list)`**: Initializes a dictionary that automatically creates a new list for each new key.
- **Sorting**: Each word is sorted alphabetically to create a unique "signature" that represents its anagram group.
- **Appending**: Words that share the same sorted signature are grouped together.
- **Return**: The function returns a list of grouped anagrams.

#### Output:
```
[['eat', 'tea', 'ate'], ['tan', 'nat'], ['bat']]
```

---

### 4. Find Anagrams from Two Arrays

#### Problem Statement:
Given an array of strings `words` and an array of query strings `queries`, return an array of lists where each list contains the anagrams of the corresponding query from the `words` array. The output lists should be sorted in alphabetical order.

#### Code:
```python
def findAnagrams(words, queries):
    # Dictionary to store sorted signature of each word in words array
    anagram_dict = {}
    
    # Populate the anagram dictionary
    for word in words:
        # Sort the word alphabetically to get its signature
        sorted_word = ''.join(sorted(word))
        if sorted_word in anagram_dict:
            anagram_dict[sorted_word].append(word)
        else:
            anagram_dict[sorted_word] = [word]
    
    # Process each query and find matching anagrams
    result = []
    for query in queries:
        # Sort the query to get its signature
        sorted_query = ''.join(sorted(query))
        if sorted_query in anagram_dict:
            # Append the sorted list of anagrams to the result
            result.append(sorted(anagram_dict[sorted_query]))
        else:
            # If no anagrams found, append an empty list
            result.append([]) 
    
    return result

# Example Usage
words_array = ["cat", "act", "allot", "peach", "cheap", "dusty"]
queries_array = ["study", "peahc", "tac"]

output = findAnagrams(words_array, queries_array)
print(output)  # Output: [['dusty'], ['cheap', 'peach'], ['act', 'cat']]
```

#### Explanation:
- **Anagram Dictionary**: A dictionary is created to map sorted words (signatures) to their original forms.
- **Query Processing**: For each query, its sorted version is checked against the dictionary to find anagrams.
- **Sorting**: The results for each query are sorted before being added to the final result list.

#### Output:
```
[['dusty'], ['cheap', 'peach'], ['act', 'cat']]
```

