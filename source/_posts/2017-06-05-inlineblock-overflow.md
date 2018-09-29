---
layout: single
classes: wide
title: 当inline-block遇上overflow
date: 2017-06-05 23:17:57 +0800
categories:
- 前端开发
tags:
- CSS
- 前端
---

# inline-block的现象
在使用inline-block的时候，会发现有的时候inline-block元素会出现不能对齐的现象。

比如当使用如下结构的html时：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>block</title>
  <style>
    .container {
      display: block;
      position: relative;
      top:32px;
      left:32px;
      width: 600px;
      height: 600px;
      background-color: transparent;
      padding: 0;
      line-height: 2;

      /*text-size-adjust: 100%;*/
    }
    .item {
      padding: 1rem;
      display: inline-block;
      position: relative;
      font-family: "Microsoft YaHei UI", "Helvetica Neue", sans-serif;
      font-size: 16px;
      line-height: 1;
      overflow: hidden;
      background-color: blue;
    }
    .item2 {
      padding: 1rem;
      display: inline-block;
      position: relative;
      font-family: "Microsoft YaHei UI", "Helvetica Neue", sans-serif;
      font-size: 16px;
      line-height: 1;
      background-color: blue;
    }
  </style>
</head>
<body>
<div class="container" tabindex="-1">
    <div class="item">111111</div>
    <div class="item">111111</div>
    <div class="item2">111111</div>
    <div class="item2">111111</div>
</div>
<script>
</script>
</body>
</html>

```

<!--more-->


# 原因和解决方法
此时渲染出来的html如下图所示：
![image](https://yj7bpw.bn1302.livefilestore.com/y4miLzUkEsArevLsYzHPW_eUvamtXf2mLeG8jGO7aHqU-pqjDEagVhfJBwzjfkJV_hq_TycNg83_p-NcbFe8pcHWuubavca-nmqH1edNRGSRlgApvoeCQ-HYgGUiO2fdqhK9Pr2RM7Ah-tzptIOCQ58ycNKsCshWXH9hpjmKgx35Pq5iTz16vsYxFmkQlKxGbw42CwcIE2Yy8InwlaXm37rBA?width=396&height=94&cropmode=none)>
 
 可以看出，当左侧item设置了overflow为hidden的时候，与右边的item2不能对齐了。
 
 导致这一情况的原因是css规范中，当inline-block的overflow设置为hidden后，其vertical-align的baseline会设置为overflow为hidden的底端，也就是说，此时容器的baseline到了item的底端，所以导致不能对齐。
 
 解决方式很简单，将item1和item2的vertical-align都设置为middle或者bottom就可以了。
 
 详细情况参见文章[overflow:hidden影响inline-block元素周围元素下移](http://www.cnblogs.com/AliceX-J/p/5731755.html)
 
 还有[css规范](https://www.w3.org/TR/CSS2/visudet.html#strut)