Title: LeetCode: 195. Tenth Line
Date: 2016-10-25
Category: Online-Judge

The problem is here: https://leetcode.com/problems/tenth-line/. 
It's a bash programming, reading the 10th line of a file and output in stdout.
I think there are lots of methods to do this, here I write some.

If the file has more than 10 lines, I can output like this:

    $ head -n 10 file.txt | tail -n 1
    
BUT, reality is not, above command failed.
Then I have the following:

    i=0
    while read line
    do
        i=$(($i+1))
        if [ $i -eq 10 ]; then
            echo $line
            break
        fi
    done < file.txt
    
This time works.