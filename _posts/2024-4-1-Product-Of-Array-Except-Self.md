---
layout: post
title: Day 6 - Product of Array Except Self
---
[Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) is probably the first 'hard' one I've had to do. I definetly felt a bit stuck at some point.

![problem description from leetcode]({{ site.baseurl }}/images/alg6/description.png)

This problem is actually pretty simple if we ignore all the conditions. It's very easy to come up with a brute force solution for this one and even easier to come up with a workaround by using the division operation.

1. Multiply all the values in nums.
2. Divide product by each value in nums.
![solution using division operator]({{ site.baseurl }}/images/alg6/divisionsolution.png)

Unfortunately, we can't use the division operator for this problem. We also need to come up with a solution that runs in **O(n)** time.

To solve this problem, we need to compute the 'prefix' and 'suffix' products. This means that we need to know for any position in the nums array, what is the product of the elements coming next, and what is the product of the elements before it. After we get the prefix and suffix of an element, we multiply them to get the product of array except self (for that element).
![prefix and suffix explanation]({{ site.baseurl }}/images/alg6/explainpresu.png)

In this example, we can calculate `1 * 2 * 4 * 5 = 40` by calculating `1 * 2`, which comes before the 3 and multiplying it wiht `4 * 5`, which comes after the 3.

Once we know this, we need some way to compute the prefix and suffix for all the elements. To do this we do the following:

1. Create prefix and suffix arrays, initialized with 1s, of same length as nums array.
![step 1]({{ site.baseurl }}/images/alg6/step1.png)

2. Set a temporary variable **product** to 1 which we will use to store the previously calculated product.
`product = 1`

3. Loop through nums, for each num get the product of current num and the product variable. Set that calculated product to the element of the prefix array. Update product.
![step 3]({{ site.baseurl }}/images/alg6/step3.png)

4. Repeat step 3 for suffix array, loop through nums in reverse order.
![step 4]({{ site.baseurl }}/images/alg6/step4.png)

5. Loop through length of nums one final time. For each element, set the output array to the prefix * suffix. Keep in mind to edge case.
![step 5]({{ site.baseurl }}/images/alg6/step5.png)

The time complexity of this solution is **O(n)** where n is the size of the nums array because although we loop through nums array three times, each loop has a max length of n. O(3n) = O(n). 
The space complexity is also **O(n)** where n is the size of the nums array because we create three arrays of size n. O(3n) = O(n)

All tests passed and my submission was valid.

```python
def productExceptSelf(self, nums: List[int]) -> List[int]:
    prefix = [1] * len(nums)
    suffix = [1] * len(nums)
    res = [1] * len(nums)

    product = 1
    for i in range(len(nums)):
        prefix[i] = product * nums[i]
        product = prefix[i]

    product = 1
    for i in range(len(nums) - 1, -1, -1):
        suffix[i] = product * nums[i]
        product = suffix[i]

    for i in range(len(nums)):
        if i == 0:
            res[i] = suffix[i+1]
        elif i+1 == len(nums):
            res[i] = prefix[i-1]
            break
        else:
            res[i] = prefix[i-1] * suffix[i+1]
    return res
```