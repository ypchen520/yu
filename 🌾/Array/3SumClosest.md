# 3Sum Closest

🌾 Source: [LeetCode problem](https://leetcode.com/problems/3sum-closest/description/)
- 2021/07/27
- 2024/01/19

## Description

* Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

## Solutions

### Two-pointer

* Sort the array
* one pointer i to iterate through the array
  * two pointer j and k at each i to search for the answer
* If current sum is less than the target
  * increase the left pointer
  * else decrease the right pointer

#### Code

```Python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        n = len(nums)
        nums.sort()
        diff = math.inf
        for i in range(n):
            tar = target - nums[i]
            l, r = i+1, n-1
            while l < r:
                curr_sum = nums[l] + nums[r]
                if abs(tar - curr_sum) < abs(diff):
                    diff = tar - (curr_sum)
                if nums[l] + nums[r] == tar:
                    return target
                elif nums[l] + nums[r] < tar:
                    l += 1
                else:
                    r -= 1
        
        return target - diff
```
