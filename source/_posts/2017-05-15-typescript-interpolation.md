---
layout: single
classes: wide
title: Typescript的插值
date: 2017-05-15 22:56:57 +0800
categories:
- 前端开发
tags:
- Typescript
- 前端
---

# 插值（Interpolation）
插值是非常有用的东西，可以说是高级的字符串拼接。

我们常见的插值有Angular中的插值，也有sass中的#{var}插值，一直认为插值是动态语言的特有技能，今天偶然发现typescript中也有插值。

<!--more-->

# Typescript中的插值
Typescript中叫做模板字符串（template strings），具体可以查看[Typescript官方文档](https://www.typescriptlang.org/docs/handbook/basic-types.html)。

具体描述如下：
> You can also use template strings, which can span multiple lines and have embedded expressions. These strings are surrounded by the backtick/backquote (`) character, and embedded expressions are of the form ${ expr }.

比如要插入一个style中的height语法在不使用插值的时候我们往往这么操作：

```typescript
let height = 200;
component.style.height = height + 'px';
```

在height要叠加一些表达式的时候会比较麻烦，而使用插值我们则可以按照下面这样的方式来操作：
```typescript
let height = 200;
component.style.height = `${height+16}px`;
```
使用这样的方式，会比使用传统的字符串类型转换要方便一些。