---
title: 【Rust】在使用Cargo镜像的情况下，使用Racer补全第三方库
urlname: completion_for_third_party_crates_by_racer_with_cargo_mirror
date: 2017-01-01 13:30:00
tags:
  - Rust
  - Racer
  - Cargo
---

**本文没有任何技术含量，仅做记录并方便后来者。**

### 起因

因为众所周知的原因，大陆地区相当一部分Rust爱好者(目前大部分只能叫爱好者)都会使用中科大的Cargo镜像以提高下载速度，但是通过镜像下载的第三方crate却无法使用racer自动补全，而使用代理下载的情况下则可以补全。
<!--more-->
于是上网搜索，发现有人遇到了与我[相同的问题](https://www.zhihu.com/question/49261154)。看到已经有人提了[issue](https://github.com/phildawes/racer/issues/513)。

Cargo将下载的第三方库缓存在.cargo/registry/
中，且不同的register缓存到不同的文件夹，如小弟使用的是默认register，故，该文件夹名为“github.com-1ecc6299db9ec823”。部分同学使用的是USTC提供的镜像，则其文件夹开头应为USTC镜像的域名。

在当前racer(2.0.6)项目中[cargo.rs](https://github.com/phildawes/racer/blob/c756eaef7a5dfd4bf7ecb4b8165136db9111d128/src/racer/cargo.rs#L448)文件中有这一段
```rust
if fname.starts_with("github.com-") {
      out.push(path.clone());
}
```
综上导致目前版本racer仅能匹配"github-"开头的文件夹。

#### 解决方法 1

下载racer源码到本地，修改cargo.rs中源码，将"github.com-"修改为你的缓存文件夹名称前半段后编译并将生成的可执行文件放在对应位置并设置好相关编辑器/IDE设定即可

#### 解决方法 2

生成一个以“github.com-”开头的软链接，指向第三方镜像对应的文件夹。

#### 解决方法 3 （推荐）

给自己的git挂上代理

### 已知的其他问题

截止本文发布，Racer进程在分析较为复杂的第三方库时可能会因为栈溢出而挂掉，早期VScode上的Racer插件在Racer进程挂掉时会有提示，但是目前版本下不会，所以如果以上方法尝试过之后发现仍然无法解决问题，建议静下心来泡杯茶，思考一下人生。