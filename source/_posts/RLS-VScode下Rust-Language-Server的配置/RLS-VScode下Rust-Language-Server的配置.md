---
title: Rust开发环境配置——RLS与VScode
date: 2017-07-19 19:59:16
urlname: config_rust_language_server_with_visual_studio_code
tags:
    - Rust 
    - VScode
    - Rust Language Server
---

**本文没有任何技术含量**

目前(2017年7月19日)`Rust Language Server`(以下简称`RLS`)已经发布alpha版，相比`Racer`，`RLS`更快且更加强大，但是目前相关配置的教程较少，本文主要用于记录VScode环境下`RLS`的配置。

<!--more-->

# 准备

首先，执行命令`rustup self update`获取最新版本的`rustup`

随后，执行命令`rustup update nightly`更新`nightly`工具链到最新版。

由于`RLS`使用了部分nightly only的feature，故编译`RLS`时需要使用`nightly`工具链编译，使用时不需要nightly环境。

# 安装

首先，执行命令`rustup component add rls --toolchain nightly`使用`nightly`工具链编译并自动安装`RLS`。

随后，执行命令`rustup component add rust-analysis --toolchain nightly`获取`rust-analysis`。

最后，执行命令`rustup component add rust-src --toolchain nightly`获取Rust源码用于代码补全。

打开`VScode`，安装名为`Rust`的插件并重新加载以让插件生效
。(部分教程年代较为久远，仍然推荐使用的是名为`Rusty`，但是该插件很久前已经停止更新)

# 配置

首先，设置名为`RUST_SRC_PATH`的环境变量指向Rust源码路径。使用`rustup`获取的Rust源码默认位于`~/.rustup/toolchains/[your-toolchain]/lib/rustlib/src/rust/src`，其中`[your-toolchain]`表示你当前使用的工具链。

随后，使用VScode打开任意后缀名为rs的文件，Rust插件会自动检测配置文件选项，并引导未配置的用户进行配置，遵循指导自行选择所需选项即可。

# 总结

截至目前，你的VScode应该已经拥有包括自动补全、定义跳转等功能，可以满足轻度开发需求。

目前Rust的调试工具尚不发达，虽然可以使用，但是配置较为复杂，等待大佬们进行开发。