title: Tkinter, a Gui for python
date: 2013-02-02 01:26
Tags: Tkinter, GUI
category: python
---

Tkinter 编程实现python的GUI。Tk GUI工具包包含了非常方便、简洁的python编程接口，要实现一个图形界面仅需以下几步：

- 导入Tkinter 模块
- 创建这个图形界面的主窗口
- 添加部件（如按钮、菜单、标题）到图形界面
- 进入主事件循环，采取行动应对用户引发的事件

下面是我今天写的一个小程序：
``` python

#!/usr/bin/env python

import Tkinter

top = Tkinter.Tk()
quit = Tkinter.Button(top, text='Hello world!', command = top.quit, bg='blue', fg='white')
quit.pack(fill=Tkinter., expand=1)
label = Tkinter.Label(top, text='Hello World!')
label.pack()
Tkinter.mainloop()

```
实现一个按钮关闭程序。
