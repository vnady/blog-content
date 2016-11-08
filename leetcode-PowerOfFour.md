title: leetcode-Power of Four
date: 2016-09-18 17:09:54
Tags: math,bin
Category: python,leetcode
---
题目：判断一个整数是否是4的幂。
测试举例: 输入16，返回true；输入5，返回false。

选择用python实现：

```python
ass Solution(object):
    def isPowerOfFour(self, num):
        """
        :type num: int
        :rtype: bool
        """
        import math
        num_sqrt = int(math.sqrt(float(num)))
        a = int(math.log(num_sqrt)/math.log(2))
        return num == num_sqrt*math.pow(2, a)
```

通过分析可以发现2的偶数次幂就应该返回`true`，否则返回`false`。进行二进制转换就会发现**奥秘**，随后就看到了讨论区id为`weiheng4`的哥们的写法：

```python
return  1==bin(num)[::-1][::2].count('1') and bin(num).count('1')==1 and num>0
```

可能是我编程经验太少了，大家不要太奇怪。
