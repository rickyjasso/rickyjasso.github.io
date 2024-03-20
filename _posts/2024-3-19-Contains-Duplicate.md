---
layout: post
title: Neetcode Challenge Day 1
---

## Contains Duplicate

[Contains Duplicate](https://leetcode.com/submissions/detail/1208353109/) was a pretty easy algorithm to take on as the first one.

![problem description from leetcode]({{ site.baseurl }}/images/alg1/description.png)

For this problem, the first thing that came to mind was brute forcing it. I quickly discarded this solution because of the time complexity O(n^2), I would have to iterate through the whole array n^2 of times with two nested for loops.

![brute force example]({{ site.baseurl }}/images/alg1/brute_alg1.png)

Then, I thought of a faster way of doing things. I could iterate through nums once while checking at another list if that number had appeared before. So I created another array called _viewed_ that would have all the numbers that previously appeared on the original nums array. I was basically asking: "Is this number, from the nums array, on my _viewed_ array?" If so, that number has already appeard and there's a duplicate. Return True. If not, I would append that number to _viewed_.
This has a time complexity of O(n) because at most, I need to iterate through the entire list once. Also, the space complexity is of O(n) because at worst, I need to append all the elements that _nums_ has to _viewed_.

![optimal example]({{ site.baseurl }}/images/alg1/optimal_alg1.png)

The solution worked, but I was faced with another challenge.

The computational time it takes to search through a python list is too long. I faced this issue because I decided to use a list (array) instead of a set. The reason why a set is a better data structure in our case is because of two reasons.

1. A set can't contain duplicates.
2. A set is implemented using a hash function.

Basically, whenever we add an item to a set, the item placed in a specific location through a hash function. The hash function takes the item, converts it to a unique number, and uses that unique number to determine where the item should be stored. So when we want to find if the item exists, we just use the hash function to know where the item should be and check if it's there or not. A list, on the other hand, has to look through each item one by one until you find (or don't find) the item you are looking for.
![set vs list 's in x']({{ site.baseurl }}/images/alg1/setvslist.png)

For this reason, searching for an item in a set has a time complexity of O(1), no matter the size of the set, and searching through a list has a time complexity of O(n).
![set vs list time performance graph]({{ site.baseurl }}/images/alg1/setlistgraph.png)

So after this small research detour, I implemented a set and passed all test cases with no trouble.

My final solution was:

```python
def containsDuplicate(self, nums: List[int]) -> bool:
        viewed = set()
        for n in nums:
            if n in viewed:
                return True
            viewed.add(n)
        return False
```
