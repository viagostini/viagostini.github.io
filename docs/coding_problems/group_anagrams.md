# Group Anagrams

## Problem Statement
Given an array of strings `words`, group the anagrams together. You can return the answer **in any order**.

!!! note
    An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

## Solution
The words that should be on the same group are the words that have the same frequencies for each letter on the alphabet. So all we have to do is count the frequencies for each word and somehow track the ones that are equal.

```python
from collections import defaultdict


def init_counter() -> list[int]:
    """Utility function to create empty counter for alphabet letters"""
    return [0] * 27


def idx(ch: str) -> int:
    """Utility function to map character to 0..26"""
    return ord(ch) - ord("a")


def count_to_str(count: list[int]) -> str:
    """Utility function to represent a frequency count as a hashable str"""
    return ",".join(map(str, count))


def group_anagrams(words: list[str]) -> list[list[str]]:
    anagrams = defaultdict(list)

    for word in words:
        count = init_counter()

        for ch in words:
            count[idx(ch)] += 1

        hash_key = count_to_str(count)
        anagrams[hash_key].append(word)

    return [key for _, key in anagrams.items()]
``` 

