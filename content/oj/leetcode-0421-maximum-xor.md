Title: LeetCode: 421. Maximum XOR of Two Numbers in an Array
Date: 2016-10-28
Category: Online-Judge

The problem is https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/. 
If just follow the process defined as described in problem,
it is quite simple except the runtime is O(n**2):

    class Solution(object):
        def findMaximumXOR(self, nums):
            """
            :type nums: List[int]
            :rtype: int
            """
            if len(nums)<2:
                return 0
            ret = nums[0]^nums[1]
            for i in range(len(nums)-1):
                for j in range(i+1, len(nums)):
                    tmp = nums[i]^nums[j]
                    if tmp > ret:
                        ret = tmp
            return ret
            
And as expected, the result is "time limit exceeded".