---
title: 【Rust】self和Self的用处
date: 2017-07-24 22:51:23
urlname: the_use_of_self_and_Self
tags:
    - Rust
---

**本文没有任何技术含量，仅做记录并方便后来者。**

在Rust语言中，有两个类似的关键字:`self`和`Self`，查找Rust文档，没有对于后者的介绍。

查找资料后写了这个笔记记录一下，也好方便后来者。

<!-- more -->

Rust中`self`关键字类似于Python中的`self`或是C++中的`this`，即作为类方法的第一个参数，用于指代调用方法的对象。

如果说`self`表示调用对象，那么`Self`就是表示调用类，如下示例

```rust
impl Clone for MyType {
    // 可以直接写具体类型
    fn clone(&self) -> MyType;
    // 也可以用Self代替
    fn clone(&self) -> Self;
}

impl MySuperLongType {
    // 用Self写起来更短
    fn new(a: u32) -> Self { ... }
}
```

另外，Rust中函数参数均需要注明类型，但是`self`则不需要，这是一个语法糖,以下示例中两两等价：

```rust
impl MyType{
    fn do_something(self){}
    fn do_somethinf(self: Self){}

    fn do_something(&self) {}
    fn do_something(&self: &Self){}

    fn do_something(&mut self) {}
    fn do_something(&mut self:&mut Self) {}
}
```

参考资料: 

<https://stackoverflow.com/questions/32304595/whats-the-difference-between-self-and-self>