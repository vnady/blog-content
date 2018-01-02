title: 多路终端复用器tmux入门配置
date: 2016/06/26 18:27
Tags: tmux,linux,terminal,vim
category: linux,dev-tools
---

[tmux](http://tmux.github.io/) 是一个优秀的终端复用软件，类似 GNU Screen ，但来自于 OpenBSD ，采用 BSD 授权。使用它最直观的好处就是，通过一个终端登录远程主机并运行 tmux 后，在其中可以开启多个控制台而无需再“浪费”多余的终端来连接这台远程主机；当然其功能远不止于此。 [[百度百科介绍]](http://baike.baidu.com/link?url=jND2oVyLUFsGLIWBXU9sIeH7c_nrjyTM6Sns8DleW3EnuL4t8ewBA0-WmTFQdF_1HdsMbU9iw3__2HKuL6mnBa)

我的配置文件主要进行如下定制：

- 修改 prefix 键为 C-a

- 分屏快捷键为 | -

- 窗格选择移动键与 vim 移动键一致

- 窗格尺寸调整，边界移动键 GHJK (与 vim 移动键一致，只是变成大写) （<font color="red" size=3>已删除，鼠标调整大小更方便</font>）

- 状态栏设置

- 使能鼠标操作

<strong>配置文件   .tmux.conf</strong>
```

unbind C-b
set -g prefix C-a
bind-key C-a send-prefix
bind r source-file ~/.tmux.conf \; display "`Reloaded configure file!"
setw -g mode-keys vi
set -g default-terminal "screen-256color"

# split window
unbind '"'
bind - splitw -v

# vertical split (prefix -)
unbind %
bind | splitw -h # horizontal split (prefix |)

# select pane
bind k selectp -U # above (prefix k)
bind j selectp -D # below (prefix j)
bind h selectp -L # left (prefix h)
bind l selectp -R # right (prefix l)

# resize pane（已删除，鼠标调整大小更方便）
bind -r K resizep -U 10 # upward (prefix Ctrl+k)
bind -r J resizep -D 10 # downward (prefix Ctrl+j)
bind -r H resizep -L 10 # to the left (prefix Ctrl+h)
bind -r L resizep -R 10 # to the right (prefix Ctrl+l)

set -g status-right '#[fg=green][#[fg=cyan]%Y-%m-%d #[fg=cyan]%H:%M#[fg=green]]'
set -g status-bg black
set -g status-fg white
set-option -g status-justify centre
set-option -g status-left '#[bg=black,fg=green][#[fg=cyan]#S#[fg=green]]'
set-option -g status-left-length 20
setw -g automatic-rename on
set-window-option -g window-status-format '#[dim]#I:#[default]#W#[fg=grey,dim]'
set-window-option -g window-status-current-format '#[fg=cyan,bold]#I#[fg=blue]:#[fg=cyan]#W#[fg=dim]'

# panes
set -g pane-border-fg colour235
set -g pane-active-border-fg cyan

set -g mouse on

```

<strong>使用截图</strong>

<img src="https://camo.githubusercontent.com/f64db69efc62f192db214d5c722e13c5bc5743c9/687474703a2f2f7777332e73696e61696d672e636e2f6c617267652f38303131613936656a77316635377777693262666f6a32316b77307a6b6e36392e6a7067">


接下来会继续探索一个init.sh脚本用于一条命令准备好工作环境。欢迎交流！

***

<font color="blue"> 2016-12-09 16:04:19 为了添加鼠标scroll，我在.tmux.conf中添加如下两行：</font>


```
bind -n WheelUpPane if-shell -F -t = "#{mouse_any_flag}" "send-keys -M" "if -Ft= '#{pane_in_mode}' 'send-keys -M' 'select-pane -t=; copy-mode -e; send-keys -M'"
bind -n WheelDownPane select-pane -t= \; send-keys -M
```

- 插件推荐
> TPM 插件管理工具，安装使用说明见下面github列表
>
> tmux-resurrect 会话保存恢复工具，很方便，安装使用说明见下面github列表

[Github插件列表...](https://github.com/tmux-plugins)
