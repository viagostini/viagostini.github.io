# Group Anagrams

## Problem Statement
Given an array of strings `words`, group the anagrams together. You can return the answer **in any order**.

!!! note
    An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

## Solution
The words that should be on the same group are the words that have the same frequencies for each letter on the alphabet. So all we have to do is count the frequencies for each word and somehow track the ones that are equal.

```python
from collections import defaultdict


def idx(ch: str) -> int:
    """Utility function to map character to 0..26"""
    return ord(ch) - ord("a")


def count_characters(word: str) -> list[int]:
    """Utility function to count characters of word"""
    count = [0] * 27
    
    for ch in word:
        count[idx(ch)] += 1

    return count


def count_to_str(count: list[int]) -> str:
    """Utility function to represent a frequency count as a hashable str"""
    return ",".join(map(str, count))


def group_anagrams(words: list[str]) -> list[list[str]]:
    anagrams = defaultdict(list)

    for word in words:
        count = count_characters(word)
        hash_key = count_to_str(count)
        anagrams[hash_key].append(word)

    return [key for _, key in anagrams.items()]
``` 
The idea behind this solution is that we can use a string representing the character frequencies as index to a dictionary
to group the words with the same letters.

Things to note:

 * The `idx` function is not strictly needed, but the array would need to be unnecessarily larger
 * It is important to create the string representing the count in a way that the numbers don't get mixed up
    * For example, without the commas you wouldn't be able to differ "111..." from "11,1,..."

## Complexity Analysis
=== "Time"
    First let's have a look at the utility functions:
    
    * `count_characters(word)`: $O(L)$ -- $L$ is the length of the words
    * `count_to_str(count)`: $O(1)$

    Finally, the main loop calls `count_characters` and `count_to_str` $N$ times, so in total we have $O(NL + N)$, as
    appending to the list and inserting in the dictionary are $O(1)$.

=== "Space"
    Here we only keep an array of size 27 and a dictionary which will hold the $N$ words in at most $N$ keys. Therefore, we have $O(N)$
    space complexity.