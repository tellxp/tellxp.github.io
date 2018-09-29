---
layout: single
classes: wide
title: Sass插值函数
date: 2017-04-30 22:56:57 +0800
categories:
- FE
tags:
- Sass
---

# Sass中的插值
通过 #{} 插值语句可以在选择器或属性名中使用变量，插值函数实质是将标识的内容替换为变量内容。

```scss
$name: foo;
$attr: border;
p.#{$name} {
  #{$attr}-color: blue;
}

```
编译为

```css
p.foo {
  border-color: blue; 
}

```

<!--more-->

# 插值的用途
如上面所述，插值可以引用变量，实际上是另外一种形式的字符串拼接，这时候我们可以使用插值函数来实现一些sass字符串拼接不好完成的事情。

比如font-family的设置。

下面是一个常规的font-family设置

```scss
$basefont: "Microsoft YaHei", Roboto, sans-serif;
```
当我们使用sass来定义上面这段css代码的时候，如果将font-family设置为变量是没有问题的，但是如果font-family是一个map，比如像下面这样：

```scss
$font-map: (
  font-family1: "Microsoft YaHei", Roboto, sans-serif,
  font-family2: "Microsoft JhengHei", Roboto, sans-serif,
  font-family3: "Microsoft JhengHei UI", Roboto, sans-serif
);
```
这时候问题来了，map中逗号","是作为特殊字符，来分割每个键值对的，首先我们就要去除逗号，可以这样处理。


```scss
font-family:"Microsoft JhengHei UI"+'\,'+Roboto

```
这里使用了一个"\"作为转义字符，可以实现逗号的输入，但是双引号如果也使用转义，比如想下面这样：

```scss
font-family:'\"'Microsoft JhengHei UI'\"'+'\,'+Roboto;

```
编译后的css如下：

```css
font-family: '"' Microsoft JhengHei UI '",Roboto';

```
这不是我们想要的结果，但是这种情况在首选字符含有双引号的的时候，采用字符串拼接无法达到，因为一旦使用转义，双引号会被作为字符串来编译，所以要么出现' " '这样的情况，要么就会变成整个属性都是字符串的情况，如下所示：
```scss
font-family:'\"'+" Microsoft JhengHei UI" +'\"'+'\,'+Roboto;

```
编译后：
```css
font-family: '" Microsoft JhengHei UI",Roboto';

```
当我们必须要使用map来指定这种带有字符串和逗号混用的key的时候，就只能使用插值函数了，插值函数只是单纯地将{}包裹的地方变成那个字符串，而不加上其他类型。


最终实现如下：

```scss
font-family: #{'\"'}#{Microsoft YaHei}#{'\"'}#{','} Roboto#{','} #{'\"'}#{Helvetica Neue}#{'\"'}#{','} sans-serif,
```
编译后：

```css
font-family: "Microsoft YaHei", Roboto, "Helvetica Neue", sans-serif;
```
这就是我们想要的结果了。