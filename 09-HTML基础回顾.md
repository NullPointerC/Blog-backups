---
title: 09-HTML基础回顾
date: 2021-03-21 15:58:34
categories: Html
tags: frontend
---

##  html 的常见元素

html 的常见元素主要分为两类：head 区域的元素、body 区域的元素。

###  1、head 区域的 html 元素

> head 区域的 html 元素，不会在页面上留下直接的内容。

- meta
- title
- style
- link
- script
- base

**base元素的介绍**：

```html
<base href="/">
```

base 标签用于指定基础的路径。指定之后，所有的 a 链接都是以这个路径为基准。

### 2、html 元素（body 区域）

> body 区域的 html 元素，会直接出现在页面上。

- div、section、article、aside、header、footer
- p
- span、em、strong
- 表格元素：table、thead、tbody、tr、td
- 列表元素：ul、ol、dl、dt、dd
- a
- 表单元素：form、input、select、textarea、button

div 是最常见的元素，大多数场景下，都可以用div（实在不行就多包几层div）。可见，**div 是比较通用的元素，这也决定了 div 的的语义并不是很明确**。

**常见标签的重要属性**：

- a[href,target]
- img[src,alt]
- table td[colspan,rowspan]。设置当前单元格占据几行几列。在合并单元格时，会用到。
- form[target,method,enctype]
- input[type,value]
- button[type]
- selection>option[value]
- label[for]

### html 文档的大纲

我们平时在写论文或者其他文档的时候，一般会先列出大纲，然后再写具体的内容。

同样，html 网页也可以看成是一种文档，也有属于它的大纲。

一个常见的html文档，它的结构可以是：

```html
    <section>
        <h1>一级标题</h1>

        <section>
            <h2>二级标题</h2>
            <p>段落内容</p>
        </section>

        <section>
            <h2>二级标题</h2>
            <p>段落内容</p>
        </section>

        <aside>
            <p>广告内容</p>
        </aside>

    </section>

    <footer>
        <p>某某公司出品</p>
    </footer>
```

##  html 元素的分类

按照样式分类：

- 块级元素
- 行内元素
- inline-block：比如`form`表单元素。对外的表现是行内元素（不会独占一行），对内的表现是块级元素（可以设置宽高）。

##  html 元素的嵌套关系

- 块级元素可以包含行内元素。
- 块级元素**不一定**能包含块级元素。比如 div 中可以包含 div，但 p 标签中不能包含 div。
- 行内元素**一般**不能包含块级元素。比如 span 中不能包含 div。但有个特例：在 HTML5 中， a 标签中可以包含 div。

## html 元素的默认样式和 CSS Reset

比如下拉框这种比较复杂的元素，是自带默认样式的。如果没有这个默认样式，则该元素在页面上不会有任何表现，则必然增加一些工作量。

同时，默认样式也会带来一些问题：比如，有些默认样式我们是不需要的；有些默认样式甚至无法去掉。

如果我们不需要默认的样式，这里就需要引入一个概念：**CSS Reset**。

<hr/>

###  常见的 CSS Reset 方案

**方案一**：

CSS Tools: Reset CSS。链接：https://meyerweb.com/eric/tools/css/reset/

**方案二**：

雅虎的 CSS Reset。链接：https://yuilibrary.com/yui/docs/cssreset/

我们可以直接通过 CDN 的方式引入：

```
<link rel="stylesheet" type="text/css" href="http://yui.yahooapis.com/3.18.1/build/cssreset/cssreset-min.css">
```

**方式三**：（比较有争议）

```
*{
    margin: 0;
    padding: 0;
}
```

上面何种写法，比较简洁，但也有争议。有争议的地方在于，可能会导致 css 选择器的性能问题。

###  Normalize.css

上面的几种 css reset 的解决思路是：将所有的默认样式清零。

但是，[Normalize.css](https://necolas.github.io/normalize.css/) 的思路是：既然浏览器提供了这些默认样式，那它就是有意义的。**既然不同浏览器的默认样式不一致，那么，`Normalize.css`就将这些默认样式设置为一致**。

