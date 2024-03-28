---
layout: post
title: Day 5 - Top K Frequent Elements
---
[Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/) was straightforward but a bit tricky.

![problem description from leetcode]({{ site.baseurl }}/images/alg5/description.png)

This problem is telling us to get the _k_ most frequent elements in a list, where _k_ is an arbitrary integer.
The first solution that comes to mind is simple making a count of each element, sorting the count and returning the k most frequent values. This is a bit slower than what we're looking for but there are some things we can take out of this approach.

To solve this problem we need to do the following:
1. Get the count of each element and insert it on map.
![step 1 of solution]({{ site.baseurl }}/images/alg5/step1.png)

2. Get the key of the biggest count and add it to result.
![step 2 of solution]({{ site.baseurl }}/images/alg5/step2.png)

3. Set that current biggest element's count to 0.
![step 3 of solution]({{ site.baseurl }}/images/alg5/step3.png)

4. Repeat steps 2-4 _k_ number of times.
![step 4 of solution]({{ site.baseurl }}/images/alg5/step4.png)

This solution has a time complexity of **O(n + k log n)** where _n_ is the number of elements in the _nums_ array, and _k_ is the number of elements we want to find. Getting the count of each element has a time complexity of **O(n)** and finding the maximum element in the map k times has a time complexity of **(O(klogn))** since searching for max has a time complexity of O(logn).

The space complexity for this solution is of O(n) because we only create one map of size n.

I applied the algorithm and all tests passed succesfully.

My final solution was:

```python
def topKFrequent(self, nums: List[int], k: int) -> List[int]:
    elementCount = defaultdict(int)
    result = []

    for n in nums:
        elementCount[n] += 1

    for i in range(k):
        biggest = max(elementCount, key=elementCount.get)
        result.append(biggest)
        elementCount[biggest] = 0

    return result
```

It should be noted that there's actually a O(n) solution for this problem, albeit a bit more complex. I'll talk about it in the next post.
