Title: LeetCode: 112. Path Sum
Date: 2016-10-22
Category: Online-Judge

The problem is here: https://leetcode.com/problems/path-sum/ .
Fancy that the number can be negative numbers, I made a stupid 'optimization'.
I added a piece of code to check whether current node value is bigger than `sum`,
assuming that every node value is positive.
After droping this 'optimization', it works.

    # Definition for a binary tree node.
    # class TreeNode(object):
    #     def __init__(self, x):
    #         self.val = x
    #         self.left = None
    #         self.right = None

    class Solution(object):
        def hasPathSum(self, root, sum):
            """
            :type root: TreeNode
            :type sum: int
            :rtype: bool
            """
            if root is None:
                return False
                
            if root.left is None \
                and root.right is None \
                and root.val == sum:
                return True
                
            return self.hasPathSum(root.left, sum-root.val) \
                or self.hasPathSum(root.right, sum-root.val)
                
I think maybe this is a DSP?