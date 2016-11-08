title: Valid Phone Numbers with shell script
date: 2016-06-20 00:59
Tags: shell,bash,linux,regex
category: shell 
------------
<p>
Valid phone numbers must appear in one of the following two formats: (xxx) xxx-xxxx or xxx-xxx-xxxx. (x means a digit)
Given a text file file.txt that contains list of phone numbers (one per line), write a one liner bash script to print all valid phone numbers.
</p>
```bash
#!/bin/bash
while IFS='' read -r line || [[ -n $line ]]
do
    if [[ "$line" =~ ^[0-9]{3}-[0-9]{3}-[0-9]{4}$|^\([0-9]{3}\)\ [0-9]{3}-[0-9]{4}$ ]]
    then
        echo "$line"
    fi
done < file.txt
```
> **file.txt:**
>
> 156-355-0923
>
> 142-902-901
>
> (142) 892-0913
>
> (23) 991-0024
>
> (450)091-2034
>
> 13-092-9930
>
> 124-091
>
--------------------------
> **Code operation results:**
>
> 1156-355-0923
>
> (142) 892-0913
>

[Read it on leetcode.com](https://leetcode.com/problems/valid-phone-numbers/)
