---
title: "修改语法高亮"
date: 2019-11-07T18:02:08+08:00
draft: true
tags: 
- highlight
---

发现`Hugo`默认要求的`Pygments`无法渲染一些高亮代码，对于我这种经常贴代码的人来说是无法忍受的。这个主题本身采用的就是`highlightjs`渲染，但是主题我并不满意，还是要捏一个合适的。

<!--more-->

在`themes/zozo/static/css`目录下的`highlight.css`就是布局文件，默认是`github.css`样式，我做了一点自己的修改。

```css
code.hljs {
  display: block;
  white-space: pre;
  word-wrap: normal;
}

.hljs {
  display: block;
  overflow-x: auto;
  padding: 0.5em;
  background: #2b2b2b;
}

.hljs {
  color: #bababa;
}

.hljs-strong,
.hljs-emphasis {
  color: #a8a8a2;
}

.hljs-bullet,
.hljs-quote,
.hljs-link,
.hljs-number,
.hljs-regexp,
.hljs-literal {
  color: #6896ba;
}

.hljs-code,
.hljs-selector-class {
  color: #a6e22e;
}

.hljs-emphasis {
  font-style: italic;
}

.hljs-keyword,
.hljs-selector-tag,
.hljs-section,
.hljs-attribute,
.hljs-name,
.hljs-variable {
  color: #cb7832;
}

.hljs-params {
  color: #b9b9b9;
}

.hljs-string {
  color: #6a8759;
}

.hljs-subst,
.hljs-type,
.hljs-built_in,
.hljs-builtin-name,
.hljs-symbol,
.hljs-selector-id,
.hljs-selector-attr,
.hljs-selector-pseudo,
.hljs-template-tag,
.hljs-template-variable,
.hljs-addition {
  color: #e0c46c;
}

.hljs-comment,
.hljs-deletion,
.hljs-meta {
  color: #7f7f7f;
}
```

#### 参考

- [highlightjs](https://github.com/highlightjs/highlight.js)

