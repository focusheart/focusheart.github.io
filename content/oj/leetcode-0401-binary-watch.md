Title: LeetCode: 401. Binary Watch
Date: 2016-10-27
Category: Online-Judge

The problem is https://leetcode.com/problems/binary-watch/.
Because the input size is limited, generating every output is workable.
Simplely follow the input, using `itertools` could generate all combinations.

    import itertools

    digitals = [8, 4, 2, 1, 32, 16, 8, 4, 2, 1]

    def get_xtime(ps):
        h = 0
        m = 0
        for p in ps:
            if p<4: h += digitals[p]
            else: m += digitals[p]
        return (h, m)

    def is_valid(xt):
        if xt[0]<12 and xt[1]<60:
            return True
        return False

    def fmt_time(t):
        return "%d:%02d" % (t[0], t[1])

    # generate all
    all_outputs = {}

    for n in range(11):
        pses = itertools.combinations(range(10), n)

        for ps in pses:
            xt = get_xtime(ps)
            fg = is_valid(xt)
            if fg:
                all_outputs[n] = all_outputs.get(n, [])
                all_outputs[n].append(fmt_time(xt))
                
    print all_outputs
    
Then, the solution is simple:

    class Solution(object):
        all_outputs = {0:['0:00'], ...}  # A dict object
        
        def readBinaryWatch(self, num):
            """
            :type num: int
            :rtype: List[str]
            """
            try:
                return self.all_outputs[num]
            except:
                return []
                
The performance of above program is in the middle.
Also, using the origin program as follows is fine:

    class Solution(object):
        def readBinaryWatch(self, num):
            """
            :type num: int
            :rtype: List[str]
            """
            digitals = [8, 4, 2, 1, 32, 16, 8, 4, 2, 1]

            def get_xtime(ps):
                h = 0
                m = 0
                for p in ps:
                    if p<4: h += digitals[p]
                    else: m += digitals[p]
                return (h, m)
            
            def is_valid(xt):
                if xt[0]<12 and xt[1]<60:
                    return True
                return False
            
            def fmt_time(t):
                return "%d:%02d" % (t[0], t[1])
                
            pses = itertools.combinations(range(10), num)
            ret = []
            for ps in pses:
                xt = get_xtime(ps)
                fg = is_valid(xt)
                if fg:
                    ret.append(fmt_time(xt))
                    
            return ret
            
Fancy that `itertools` module is already imported!