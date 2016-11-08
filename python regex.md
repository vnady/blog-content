title: python正则匹配
date: 2016-09-06 12:21:03
Tags: python,regex
category: python
---
匹配子串,输出子串：
```python
import re
while(1):
    str=raw_input(">")
    if str == "":
        break
    m = re.match(r'^[aeiouAEIOU]*',str)
    if m != None:
        print m.group()
```
`r'^[aeiouAEIOU]*'`只会匹配方括号中重复出现的字符串并返回匹配上的局部字符串，`r'^[aeiouAEIOU].*'`会把开头是元音字母的字符串整个返回（而不是只返回元音字母）。这体现了`*`的作用是前一个字母或整体的重复。
