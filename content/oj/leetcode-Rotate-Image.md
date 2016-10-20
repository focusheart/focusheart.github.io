Title: LeetCode: 48. Rotate Image
Date: 2016-10-20
Category: Online-Judge

The problem is [Rotate Image](https://leetcode.com/problems/rotate-image/). 
If I can use more space, just copy the number for original matrix to new matrix.
And the rule of rotate 90 degrees is simple, 
using original y coordinate as new x coordinate, lenght minus x as new y.
After copying to new matrix, copy back to origin.

    class Solution(object):
        def rotate(self, matrix):
            """
            :type matrix: List[List[int]]
            :rtype: void Do not return anything, modify matrix in-place instead.
            """
            n = len(matrix)
            matrix_r = [[0 for j in range(n)] for i in range(n)]
            for i in range(n):
                for j in range(n):
                    matrix_r[j][n-1-i] = matrix[i][j]
            for i in range(n):
                for j in range(n):
                    matrix[i][j] = matrix_r[i][j]
            
If we do this in space, the exact positions of each number must be caculated.
In each loop, four numbers change their position in circle.

    class Solution(object):
        def rotate(self, matrix):
            """
            :type matrix: List[List[int]]
            :rtype: void Do not return anything, modify matrix in-place instead.
            """
            n = len(matrix)
            
            if n == 1:
                return
            
            for i in range(n/2 + n%2):
                for j in range(i, n-1-i):
                    tmps = [matrix[i][j], matrix[j][n-1-i], matrix[n-1-i][n-1-j], matrix[n-1-j][i]]
                    matrix[i][j] = tmps[3]
                    matrix[j][n-1-i] = tmps[0]
                    matrix[n-1-i][n-1-j] = tmps[1]
                    matrix[n-1-j][i] = tmps[2]

I used a four elements array to temporarily store old values,
but it's not necessary because one variable is enough.
I can swap each pair of adjacent values in anticlockwise.