# 3Sum

🌾 Source: [LeetCode problem](https://leetcode.com/problems/3sum/)
- 2023/08/21
- 2024/01/19

## Description

* Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.
* Notice that the solution set must not contain duplicate triplets.

## Solution

### Two-pointer

#### Intuition

- To avoid duplicates, we need to skip duplicate number for the three pointers
  - We can also use a HashSet (although slower)

    ```Python
    res = set()
    res.add((nums[i], nums[l], nums[r]))
    return [list(x) for x in res]
    ```

#### Complexity

- Time complexity: $O(n^2)$
- Space complexity: $O(n)$

#### Code

```Python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = []
        n = len(nums)
        nums.sort()
        for i in range(n):
            # Skipping duplicate numbers
            if i > 0 and nums[i] == nums[i-1]:
                continue
            target = -nums[i]
            l , r = i+1, n-1
            while l < r:
                if nums[l] + nums[r] == target:
                    res.append([nums[i], nums[l], nums[r]])
                    l += 1
                    r -= 1
                    # Skipping duplicate numbers
                    # Skipping for one of the two pointers is enough
                    # We still need to check if l < r
                    # There's no need to check if l > 0 because we just added at least 1 to l
                    while l < r and nums[l] == nums[l-1]:
                        l += 1
                elif nums[l] + nums[r] < target:
                    l += 1
                else:
                    r -= 1
        return res
```