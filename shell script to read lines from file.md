title: shell script to read lines from file
date: 2016-06-20 00:17
Tags: shell,linux,unix,bash
category: shell
----------------------------
<p>How would you print just the 10th line of a file with the shell script?
Shell script is as below:</p>
```bash
#!/bin/bash
k=0
while IFS='' read -r line || [[ -n "$line" ]]
do
    ((k++))
    if [ $k -eq 10 ]
    then
        echo "Text read from file: $line"
    fi
done < "$1"
```
<span>
If you read a exist file, replace the last line with `done < file.txt`(file.txt is the file you read lines from)
</span>
<br>
[Read it on leetcode.com](https://leetcode.com/problems/tenth-line/)
