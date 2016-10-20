Title: LeetCode: 235. Lowest Common Ancestor of a Binary Search Tree
Date: 2016-10-20
Category: Online-Judge

The problem is here: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/ .
Using recursive function solve this fast, because of the nature of BST.
For each node in BST, 
every node on left is smaller than root and every node on right.
Therefore, the ancestor of two nodes in BST must be a root of sub-tree of root.
The following is my solution:

    # Definition for a binary tree node.
    # class TreeNode(object):
    #     def __init__(self, x):
    #         self.val = x
    #         self.left = None
    #         self.right = None

    class Solution(object):
        def lowestCommonAncestor(self, root, p, q):
            """
            :type root: TreeNode
            :type p: TreeNode
            :type q: TreeNode
            :rtype: TreeNode
            """
            if p.val == root.val or q.val == root.val:
                return root
                
            if p.val < root.val:
                if q.val < root.val:
                    return self.lowestCommonAncestor(root.left, p, q)
                else:
                    return root
            else:
                if q.val < root.val:
                    return root
                else:
                    return self.lowestCommonAncestor(root.right, p, q)
        
Recursive function is more elegant than other forms, 
but if the depth of BST is large, the recursive stack may overflow.
Generally, the size of a balanced BST grows exponentially, 
we won't worry the problem of stack overflow.
If it is not balanced, then we could traverse the tree to do this.
The following is my solution, no recursion:

    # Definition for a binary tree node.
    # class TreeNode(object):
    #     def __init__(self, x):
    #         self.val = x
    #         self.left = None
    #         self.right = None

    class Solution(object):
        def lowestCommonAncestor(self, root, p, q):
            """
            :type root: TreeNode
            :type p: TreeNode
            :type q: TreeNode
            :rtype: TreeNode
            """
            cur = root
            while cur is not None:
                if p.val == cur.val or q.val == cur.val:
                    break
                if p.val < cur.val:
                    if q.val < cur.val:
                        cur = cur.left
                        continue
                    else:
                        break
                else:
                    if q.val < cur.val:
                        break
                    else:
                        cur = cur.right
                        continue
            return cur