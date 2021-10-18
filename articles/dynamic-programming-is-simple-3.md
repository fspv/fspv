# Dynamic programming is simple #3

Now when you got the idea, let's look at one problem that does't fit the pattern if you apply it blindly.

Solution to the problem I about to show you might seem obvious to many of you if you followed the previous article. However, I've seen people who got stuck here because of a simple nuance.

## The problem

We're about to discuss this problem [https://leetcode.com/problems/longest-increasing-subsequence/](https://leetcode.com/problems/longest-increasing-subsequence/).

This one is one of the classic DP examples. This is a quite an easy one as well, so people who are already familiar with DP should be able to come up with the solution almost immediately.

## First attempt

Let's try to write down the solution, using the pattern we already know.

I'll point out that the solution below is incorrect. Still this is a solution I often see people implement, and struggle to understand, why it doesn't work.

I'll implement the top-down solution, because it is simplier to see that way how the answer if formed.


```python
from functools import lru_cache
from typing import List


class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        @lru_cache(None)
        def dp(pos: int) -> int:
            max_length = 1  # We have 1-element subsequence anyway

            for next_pos in range(pos + 1, len(nums)):
                if nums[pos] < nums[next_pos]:
                    max_length = max(max_length, dp(next_pos) + 1)

            return max_length

        return dp(0)
```

It looks like everything is fine here. We have defined what we do each step (try to pick the next element in the array and add it to the existing subsequence), each step is valid and the recurision itself is valid. So there is nothing wrong with recursion. Still when we run this solution, it doesn't work. What's wrong here?

Many of you have already noticed the problem. The problem is that we take into consideration only a subsequence which includes the first element of the `nums` array.

Let's check the call graph to illustrate it better.




FIXME: callgraph here



Hope it is clear now, that by calling only `dp(0)` at the beginning we're missing calling `dp()` at many other positions. We only call it for those positions, reachable from `dp(0)`.

This is where it diverges a bit from the initial pattern. Earlier for all the solutions to seed the recursion we needed to only call `dp()` from some initial position (day 0 for "buying and selling stocks" and for edit distance we just put pointers to 0 positions at both strings). This case is different because of how we defined the recursion step and that needs to be fixed from outside the recursive call.

## Correct solution

You might already have come up with the fix, so I'll just the correct solution down.

```python
from functools import lru_cache
from typing import List


class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        @lru_cache(None)
        def dp(pos: int) -> int:
            max_length = 1  # We have 1-element subsequence anyway

            for next_pos in range(pos + 1, len(nums)):
                if nums[pos] < nums[next_pos]:
                    max_length = max(max_length, dp(next_pos) + 1)

            return max_length

        return max(dp(pos) for pos in range(len(nums)))
```

Now we check all possible starting positions and this gives us a correct answer.

Let's check a new call graph.





FIXME: call graph pic




Hooray! It works.

Let's try to write down a bottom-up solution as well using the well-known set of transformation rules.


```python
from typing import List


class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = [1] * len(nums)

        for pos in reversed(range(len(nums))):
            max_length = 1

            for next_pos in range(pos + 1, len(nums)):
                if nums[pos] < nums[next_pos]:
                    max_length = max(max_length, dp[next_pos] + 1)

            dp[pos] = max_length

        return max(dp)
```

And now we're done. Time to celebrate another solved dp problem.

## Final thoughts

This was a one-dimension example. There are some multi-dimensinal problems that require the same technique. The general idea is that not always all the nodes in the DP table are reachable from just a single location. So the call graph is not a tree anymore as it was for the previous problems. Instead, it is a multi-root tree. In order to construct a solution you shold make queries from every possible tree root and choose the results that is the best for a given problem statement. So one more step is added to your solution.

I'm not giving you the list of such problems intentionally. You should be able to recognize them yourself.

That's it for now. In the next article I'll show how bitmasks might be useful for DP problems. Stay tuned.
