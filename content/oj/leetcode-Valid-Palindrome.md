Title: 125. Valid Palindrome
Date: 2016-10-08 11:43
Category: Online-Judge

The problem is [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/). 
It's quite simple to make a simple one.

    class Solution(object):
        def isPalindrome(self, s):
            """
            :type s: str
            :rtype: bool
            """
            if s == '': return True
            
            cs = 'abcdefghijklmnopqrstuvwxyz0123456789'
            is_char = lambda c: c in cs
            to_sstr = lambda s: ''.join([ c if is_char(c) else '' for c in s])
            
            sl = s.lower()
            ss = to_sstr(sl)
            
            for i in range(len(ss)/2):
                if ss[i] != ss[len(ss)-1-i]:
                    return False
            return True
            
The solution is a dirty work, although it works. 
The runtime is 159ms, which beats 10.03% of python submissions.
It's really too bad, then I modified it:

class Solution(object):
    cs = 'abcdefghijklmnopqrstuvwxyz0123456789'
    
    def is_char(self, c):
        return c in self.cs
        
    def isPalindrome(self, s):
        """
        :type s: str
        :rtype: bool
        """
        if s == '' or len(s) == 1: return True
        s = s.lower()
        
        i = 0
        j = len(s) - 1
        flag = True
        
        while i < j:
            ci = s[i]
            if not self.is_char(ci):
                i += 1
                continue
            
            cj = s[j]
            if not self.is_char(cj):
                j -= 1
                continue
            
            if ci == cj:
                i += 1
                j -= 1
                continue
            else:
                flag = False
                break
        
        return flag
        
The runtime is 129ms, which beats 18.03%, better but not enough.