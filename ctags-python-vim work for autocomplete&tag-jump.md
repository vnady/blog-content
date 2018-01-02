title: ctags&python-vim work for autocomplete&tag-jump
date: 2017-01-11 09:09:45
category: dev-tools
Tags: Linux,mac,vim
---

####设置
- 设置python vim module，在vimrc文件中添加如下几行
```
python << EOF
import os
import sys
import vim
for p in sys.path:
    if os.path.isdir(p):
        vim.command(r"set path+=%" % (p.replace(" ", r"\ ")))
EOF
```

####准备tags文件
- 运行ctags，生成tags文件，举例：
>$ctags -R -f ~/.vim/tags/python.tags /usr/lib/python2.7/

- 如果你在多个地方有python文件，使用`-a`追加tags
>$ctags -R -a -f ~/.vim/tags/python.tags /usr/lib/python2.7/


####在python文件编辑中使用tags进行自动补齐或跳转
- vim打开python文件或者工程目录后，设置tags路径(如果你有较强的vimrc定制能力，可以autocmd自动导入相关FileType的tags文件)
>:set tags+=~/.vim/tags/python.tags


如有不明白的地方，请联系我：<taojiang0101@gmail.com>
