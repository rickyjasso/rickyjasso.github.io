---
layout: post
title: Day 4 - Group Anagrams
---
[Group Anagrams](https://leetcode.com/problems/group-anagrams/description/) was the first problem where I really had to think hard.

![problem description from leetcode]({{ site.baseurl }}/images/alg4/description.png)

Initially, this may seem like a simple, straightforward problem but the more you think about it and the deeper you go, you realize how tricky this can be. You need a way to check which words are anagrams of each other and find a way to group them. If we try to find anagrams like we did before we would need to use two for loops and end up having a time complexity of at least O(n^2) and we don't want that. The other obvious solution would be to sort all the words and arrange them but that's the easy way out and I don't want that.

After some deep thinking, I figured out a great solution.

To solve this problem we need to figure out two things:
1. We need to figure out a way to group our anagrams. How can we group different number of different anagrams all in one data structure?
2. How do we know where to store the words as we go? We know we have to store common anagrams together but how can we place them next to each other? How do we know where each word goes?

Solving point number one is really simple. We use a 2D array. The general array will store anagram-based arrays. For example, if we get the input `['bat', 'cat', 'act']` we would then store `[['bat'], ['cat', 'act']]`.

After that is where it gets tricky. How does the algorithm know where to store `'act'`? Should we check through all arrays to see if its an anagram of an element inside it? No, that would lead us to the solution we don't want. Instead, consider this:

Part of solving an algorithmic problem is knowing the algorithm's constraints. In this particular case, we are given the following ones:
* 1 <= strs.length <= 104
* 0 <= strs[i].length <= 100
* strs[i] consists of lowercase English letters.

This tells us three things.
1. Every input will have at least 1 element inside strs array and no more than 104.
2. An element inside the strs array can be empty `""`, or have up to 100 characters.
3. Each non-empty element inside strs array will be of **lowercase English letters**.

Point three is KEY to solving this problem. Each character in the computer world has a unique unicode code, which is an int value. This, of course, applies to all lowercase English letters. For example 'a' has a value of 97, 'b' of 98, up to 'z' which has a value of 122. Using this information, we can create an array full of zeroes representing each lowercase letter of the alphabet. In other words, an array that looks like this:
![character array]({{ site.baseurl }}/images/alg4/chararray.png)

The first 0 represents 'a', the second 'b', the third 'c', and so on. For each character in a word, we will add 1 to its respective position. For example, `'cat'`'s array will look like this.
![cat's character array]({{ site.baseurl }}/images/alg4/catarray.png)

How do we get the position for each character? We use Python's `ord()` method which returns the number representing the unicode code of a specified character. We need some wit for the following solution: we need 'a' to take the first place in our character list. The value for 'a' is 97, so if we subtract 97 to 'a', we get 0, the first index of the list. This means that we can essentially subtract 97 from any lowercase english letter to reduce their values from the ranges 97-122 to 0-25. 0 to 25 are the indices of our character array.
![example on how the character array is filled with the word 'cat']({{ site.baseurl }}/images/alg4/catex.png)

We can then use this array as a key for a hash map, and have the value be the word that matches this array. For example, for `'cat'` and `'act'`: 
![hash map example for cat and act]({{ site.baseurl }}/images/alg4/catarray.png)
This works because we know that `'cat'` and `'act'` will produce the exact same character array, so when `'act'` comes along, we will know where to store it. For a new word, we will simply create that array.

A few more pythonic things: 
1. We need to, once again, use the defaultdict function in order to initialize the values of our hash map as lists, which will be the lists that store the words. `groups = defaultdict(list)`
2. Lists can NOT be keys because lists are mutuable. Luckily, tuples aren't, so we need to either set our character array as a tuple or convert it when creating the key value pair. I chose the latter. `groups[tuple(charArray)].append(word)`

After getting everything, we need to output our answer. At first I created another for loop which would iterate through my groups map and do the following: `for each item in my groups hash map, append the item's value (list of anagrams) to my result array. when done, return result array`, which resulted in wasted computational time. It works and all the tests pass, but we want maximum efficiency. Instead, I realized I could simply return the values of the hash map.

Python's dictionary type offers the `.values()` method which returns a view object that contains a list of all the values in the dictionary. Exactly what we want.

My final solution was:

```python
def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
    groups = defaultdict(list)
    for word in strs:
        char = [0]*26
        for c in word:
            char[ord(c) - 97] += 1
        groups[tuple(char)].append(word)
    return groups.values()
```
