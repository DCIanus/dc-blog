---
title: 【备忘】资源管理器右键在此打开MSYS2终端
date: 2017-06-05 16:30:44
urlname: settings_of_open_here_for_msys2
tags:
    - MSYS2
    - 环境配置
---
**本文没有任何技术含量**

    很早之前在Linux上体会过oh-my-zsh的畅快之后，一直期望能够在Windows上重现这种快乐。

    后来了解到了MSYS2，在MSYS2上成功安装了oh-my-zsh后尝试实现git bash的“右键在此打开”功能。

1. 打开注册表编辑器，在`HKEY_CLASSES_ROOT\Directory\Background\shell`下新建一个项`msys2`。

<!--more-->

2. 将`msys2`的内容设置为"Open MSYS2 Here"或者其他你喜欢的内容，这将是之后右键时的项目名。

3. 在`msys2`新建一个字符串值，内容为MSYS2的图标路径。

4. 在`msys2`下新建一个项`command`，此处项名不可自定义，只能使用`command`。

5. 设置`command`值内容为`<msys2_path>\usr\bin\mintty.exe /bin/bash -lc 'cd "$(cygpath "%V")"; export CHERE_INVOKING=1; exec <shell_name> --login -i'`。
需要注意的是，其中`<msys2_path>`代指你的MSYS2安装目录，`<shell_name>`代指你想要使用的shell，请自行更换。
如我的内容为：`D:\Software\msys64\usr\bin\mintty.exe /bin/bash -lc 'cd "$(cygpath "%V")"; export CHERE_INVOKING=1; exec zsh --login -i'`

参考自：

<a href="https://gist.github.com/magthe/a60293fe395af7245a9e" rel="nofollow">https://gist.github.com/magthe/a60293fe395af7245a9e</a>