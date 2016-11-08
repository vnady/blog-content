title: shell script to evaluate expression
date: 2016-06-19 11:50
Tags: shell,linux,unix,bash,bc
category: shell 
--------------------------------
<a>To evaluate expressions involving decimal places (floating points) "bc -l" is very useful.
Your task is to evaluate the expression and display the output correct to  decimal places.
Shell script is as below:
</a>
```bash
#!/bin/bash
read expression
printf "%.3f\n" $(echo "scale=10;$expression" | bc -l)
```
>**input:**
>
>23 + 12*(34 - 45) + 67/3
>
- - -
>**output:**
>
>-86.667

[Read it on hackerrank.com](https://www.hackerrank.com/challenges/bash-tutorials---arithmetic-operations)
