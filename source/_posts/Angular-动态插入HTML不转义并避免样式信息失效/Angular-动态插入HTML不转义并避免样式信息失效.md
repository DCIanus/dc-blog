---
title: 【Angular】动态插入HTML不转义并避免样式信息失效
date: 2017-07-09 06:45:40
urlname: insert_raw_html_into_angular_template_and_using_style_sheet
tags:
  - angular
  - TypeScript
---

**本文没有任何技术含量**

angular中有时需要动态插入HTML内容，直接使用差值表达式插入内容，将会以类似于`innerText`的方法插入，其中的`HTML tag`将会被转义，难以获得预期效果。
<!--more-->
在`stack overflow`找到该问题的[解决方案](https://stackoverflow.com/questions/31548311/angular-2-html-binding):

即使用以下形式进行绑定:
```html
<div [innerHTML]="content"></div>
```
且根据官网文档，使用该方法插入的HTML内容将会过滤掉其中的`<script>`标签内容以保证安全。

但随后发现，原模板中的CSS信息对使用该方法插入的HTML内容无效，经过查找，发现有人发了[issue](https://github.com/angular/angular/issues/7845)，并有人提供了[解决方案](https://stackoverflow.com/questions/36265026/angular-2-innerhtml-styling/36265072#36265072)

即在component装饰器中添加以下选项
```typescript
@Component({
    encapsulation: ViewEncapsulation.None
})
```