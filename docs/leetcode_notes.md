# LC - Blind 75

## Arrays

### Two-sum (ez)

Intuition: Calculate the difference of target - i, if the difference exists in the list, then we have a sum of 2 numbers, [i, position_of_difference] that produces target.

Two-pass

```Python
class Solution:
  def twoSum(self, nums: List[int], target: int) -> List[int]:
    n_idx = {}
    for n, idx in enumerate(nums):
      n_idx[idx] = n

    for i, n in enumerate(nums):
      difference = target - n
      if n_idx[difference]:
        return [i, n_idx[difference]]

    return []
```

One-pass

```Python
def twoSum(self, nums: List[int], target: int) -> List[int]:
        scan = {}

        for i, num in enumerate(nums):
            difference = target - num

            if difference in scan:
                return [scan[difference], i]

            scan[num] = i
```

### Top-K Frequent elements (medium)

Intuition: Bucket Sort. Build a list of list of size nums. Each list index keeps track of the items that occur i times, e.g. `list[3]` are all items that occur 3 times in `nums`.

How to get res? After bucket is built, look for highest to lowest frequency and build the res list according to given `k`.

```Python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        k_set = {} # Count
        k_list = [] # Bucket
        for i in range (len(nums)+1): # Building Bucket
            k_list.append([])

        # Count the number of occurances of each n in nums
        for n in nums:
            k_set[n] = 1 + k_set.get(n, 0)

        # Put each occurance in their respective buckets
        for n, c in k_set.items():
            k_list[c].append(n)

        # Build res based on the bucket, highest frequency to lowest.
        res = []
        for i in range (len(k_list)-1, 0, -1):
            for n in k_list[i]:
                res.append(n)
                if len(res) == k:
                    return res
```

## Sliding Window

### Longest Substring without repeated Characters (medium)

Intuition: Dictionary (Hash Map) to store the leftmost occurance of each unique character. As we move right along the string, once a repeating character is detected,
update the leftmost occurance value, and reset pointer to the left of previously encountered leftmost occurance of that repeating character. Pointers always move right.

How to get rest? Check window size every iteration, overwrite if greater.

```Python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        mp = {}
        l = 0
        res = 0
        for r in range (len(s)):
            if s[r] in mp:
                l = max(l, mp[s[r]] + 1)
            mp[s[r]] = r
            res = max(res, r-l+1)
        return res
```
