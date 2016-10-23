Title: LeetCode: 201. Bitwise AND of Numbers Range
Date: 2016-10-21
Category: Online-Judge

The problem is here: https://leetcode.com/problems/bitwise-and-of-numbers-range/ .
I simply wrote a silly version and failed as expected,
it just did exactly as this problem described.

    class Solution(object):
        def rangeBitwiseAnd(self, m, n):
            """
            Attention! This is a bad version!
            :type m: int
            :type n: int
            :rtype: int
            """
            return reduce(lambda x,y: x&y, range(m, n+1))
            
Obviously, the memory used increase as `n` increase.
If `n` is big, too much memory will be exhausted.
Then, I should try some test cases to see if there are some rules.
After practicing some cases by hand, something interesting appears.
If n is much biggger than m, especially having more bits in binary, result is 0.
If both m and n have same size in binary, the result is not related to inner numbers.
The result is only related to whether value is equal at the same bits in same position. Then it is simple:

class Solution(object):
    def rangeBitwiseAnd(self, m, n):
        """
        :type m: int
        :type n: int
        :rtype: int
        """
        if m == n:
            return m
        bm = bin(m)[2:]
        bn = bin(n)[2:]
        
        if len(bm) != len(bn):
            return 0
        
        ret = 0
        for i in range(len(bm)):
            if bm[i] == bn[i]:
                ret += 2**(len(bm)-1-i)*int(bm[i])
            else:
                break
        return ret
        
I think there can be some modification to reduce time usage of above program in some cases.