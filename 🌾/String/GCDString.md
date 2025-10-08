# Greatest Common Divisor of Strings

🌾 Source: [LeetCode](https://leetcode.com/problems/greatest-common-divisor-of-strings/)
- 2023/02/01
- 2025/10/08

## Solution

### Intuition

It's essential to find the greatest common divisor `k` of `(len1, len2)` first. And then we use `k` to find the base. If the multiplying the base does not result in `str1` and `str2`, we find the next greatest common divisor.

### Complexity

- Time complexity: $O(n)$
- Space complexity: $O(1)$

### Code

```Python
class Solution:
    def gcdOfStrings(self, str1: str, str2: str) -> str:
        len1, len2 = len(str1), len(str2)
        def valid(k):
            """
            Find valid length.
            """
            if len1 % k == 0 and len2 % k == 0:
                return True
            return False

        for i in range(min(len1, len2), 0, -1):
            if valid(i):
                n1, n2 = len1 // i, len2 // i
                base = str2[:i]
                if str1 == base * n1 and str2 == base * n2:
                    return base
        return ""
```