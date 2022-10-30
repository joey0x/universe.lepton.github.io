---
title: kitty+neovim打造丝滑个人终端编辑环境
top: false
cover: false
toc: true
mathjax: true
date: 2022-10-30 10:46:18
password:
summary:
tags: Terminal
categories:
---

最近在折腾写代码的环境，经过多方对比和试用后，终于找到了一套个人感觉用起来最舒服最丝滑的方案，喜欢用vim写代码，或者VS Code用腻的同学可以参考一下。

## Terminal

终端我选择了kitty，kitty是一个基于GPU的功能强大的轻量级终端模拟器。GPU 渲染，肉眼可见的速度提升，以降低系统负载和平滑的滚动效果。它支持图形，图像，Unicode，真彩色，鼠标协议，直接提取超链接，多个复制/粘贴缓冲等丰富功能。

不足的地方是如果是使用ssh连接到其他远程环境，可能命令行会出现一些异常。需要用kitty +kitten ssh来连接。为了避免每次都输一长串，也可以直接在你使用的shell配置文件里进行修改，如在.zshrc文件里添加：

```
alias ssh="kitty +kitten ssh"
```

kitty支持非常灵活的窗口布局，同时内部集成了对多个控制台的终端复用功能，省去了使用tmux的操作。

![**多窗口和zoom pannel的示例**](kitty+neovim%20%E2%80%94%20%E6%89%93%E9%80%A0%E4%B8%9D%E6%BB%91%E4%B8%AA%E4%BA%BA%E7%BB%88%E7%AB%AF%E7%BC%96%E8%BE%91%E7%8E%AF%E5%A2%83%20f2aacf41ad6a4101a2ae3048b64e5cc3/kitty.gif)

**多窗口和zoom pannel的示例**

要支持zoom panel的话需要自己实现一个函数，创建`~/.config/kitty/zoom_toggle.py:`

```python
def main(args):
    pass

from kittens.tui.handler import result_handler
@result_handler(no_ui=True)
def handle_result(args, answer, target_window_id, boss):
    tab = boss.active_tab
    if tab is not None:
        if tab.current_layout.name == 'stack':
            tab.last_used_layout()
        else:
            tab.goto_layout('stack')
```

然后在kitty的配置文件里进行快捷键映射：

```json
map cmd+z kitten zoom_toggle.py
```

这样就可以使用cmd+z进行窗口缩放啦

kitty还有很多强大的功能比如通过session file来控制startup session时的窗口布局，工作目录或者运行指定程序等等，详细可以查看官网： [https://sw.kovidgoyal.net/kitty/overview/](https://sw.kovidgoyal.net/kitty/overview/) ，总之就是一切皆命令，可以省去大量的手工操作。

## neovim+coc.nvim

这两个玩意儿可以直接把你的代码开发环境变得比IDE更加如丝般顺滑，函数定义跳转，引用查找，错误提示，自动代码补全等等完全不在话下。毕竟neovim的设计理念相对vim8要超前很多，同时开发者数量也要多很多。关于neovim及其相关插件的安装方法 Martins3在他的这篇博客里（[https://martins3.github.io/My-Linux-Config/docs/nvim.html](https://martins3.github.io/My-Linux-Config/docs/nvim.html)）已经讲的很清楚了，按照里面的步骤一步一步操作下来就可以。当然我配置这个过程还是挺崎岖的 😂，主要是有挺多依赖组件的版本问题。直接上图展示最终的整体效果吧，看完之后你可能就会心动了。

![**自动代码补全**](kitty+neovim%20%E2%80%94%20%E6%89%93%E9%80%A0%E4%B8%9D%E6%BB%91%E4%B8%AA%E4%BA%BA%E7%BB%88%E7%AB%AF%E7%BC%96%E8%BE%91%E7%8E%AF%E5%A2%83%20f2aacf41ad6a4101a2ae3048b64e5cc3/nvim_codecompletion.gif)

**自动代码补全**

[https://www.notion.so](https://www.notion.so)

查找符号

[https://www.notion.so](https://www.notion.so)

打开文件树

[https://www.notion.so](https://www.notion.so)

直接查看代码的git commit记录