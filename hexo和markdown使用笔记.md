---
title: hexo和markdown使用笔记
date: 2018-08-22 13:37:50
categories: 常识
tags:
 - hexo
 - markdown
description: 本篇不叙述如何配置安装搭建hexo，主要记录在写作方面的基础，作为利用markdown写作的个人文档。
---
>之前一直使用有道云笔记，其同样提供了markdown的书写，所以使用hexo对博主来说比较容易上手，下面先把常用的markdown标签语法一一进行列举,比较少用的就随用随查了，慢慢就会熟记。

# 文字处理
- 加粗
- 斜体
- 删除线
- 下划线

## 加粗
```
**这里是需加粗文字**
```
>**效果**
## 斜体
```
*这里是需斜体文字*
```
>*效果*
## 删除线
```
~~这里是需加删除线文字~~
```
>~~效果~~
## 下划线
```
<u>这里是需下划线文字</u>
```
><u>效果</u>
# 标题
```
这是最常用的标签 #[null]
# 一级标题
## 二级标题
以此类推
######六级标题
```
# 分割线
```
三个连续的-
```
>效果如下
---
# 注释
```
由三个连续的 ` 包围
```
# 序列
```
- 普通序列
1. 数字序列
```
>效果1：普通序列
- 1
- 2
- 3

>效果2：数字序列
1. 1
1. 2
1. 3

# HTML
>markdown支持HTML嵌入
如下代码：

```
<html>
<a href="http://www.baidu.com">百度</a>
</html>
```
<html>
<a href="http://www.baidu.com">百度</a>
</html>
# 表格
```
header 1 | header 2
---|---
row 1 col 1 | row 1 col 2
row 2 col 1 | row 2 col 2
```
>效果如下

header 1 | header 2
---|---
row 1 col 1 | row 1 col 2
row 2 col 1 | row 2 col 2

# 代码块
>普通代码块

```
{% codeblock %}
alert("hello world");
{% endcodeblock %}
```
{% codeblock %}
alert("hello world");
{% endcodeblock %}

>指定语言

```
{% codeblock lang:javascript %}
alert("hello world");
{% endcodeblock %}
```
{% codeblock lang:javascript %}
alert("hello world");
{% endcodeblock %}

>附加说明和网址

```
{% codeblock 弹出提示窗 http://www.baidu.com 链接到百度 lang:javascript %}
alert("hello world");
{% endcodeblock %}
```
{% codeblock 弹出提示窗 http://www.baidu.com 链接到百度 lang:javascript %}
alert("hello world");
{% endcodeblock %}

# 资源文件
```
![这是图片提示](test.jpg)
```
>引入图片如下

![这是图片提示](test.jpg)
