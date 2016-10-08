Title: Simple Fizz Buzz Quizz
Date: 2016-10-08 11:43
Category: Online-Judge

The problem is [Fizz Buzz](https://leetcode.com/problems/fizz-buzz/). 
It's quite simple!

    class Solution(object):
        def fizzBuzz(self, n):
            """
            :type n: int
            :rtype: List[str]
            """
            ret = []
            for i in range(1, n+1):
                if i % 3 == 0:
                    if i % 5 == 0:
                        ret.append('FizzBuzz')
                    else:
                        ret.append('Fizz')
                elif i % 5 == 0:
                    ret.append('Buzz')
                else:
                    ret.append('%s' % i)
            return ret
            
