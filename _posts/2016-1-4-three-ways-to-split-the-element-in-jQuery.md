---
layout: post
title: jQuery模板中元素切分的三种方法
---

### the 1st way to split the data list

$(“#flowerTmpl”).template(data).filter(“*”).slice(0,3)
.appendTo(“#row1”).end().end().slice(3).appendTo(“#row2”)


### the second way to split the data list

var templateHtml = $(“#flowerTmpl”).template(data).filter(“*”);
templateHtml.slice(0, 3).appendTo(‘#row1’);
templateHtml.slice(3).appendTo(‘#row2’);


### the 3rd way to aplit the data list

var tElem = $(“#flowerTmpl”);
tElem.template({flowers: data.flowers.slice(0, 3)}).appendTo(‘#row1’);
tElem.template({flowers: data.flowers.slice(3)}).appendTo(‘#row2’);


三种方法的主要区别在于切分点的选择，第一种方法与第二种方法本质上是一致的，均是把全部的元素引入，然后再进行切分；不同点就是第一种方法直接用链式依次执行任务，第二种方法引入了一个中间变量templateHtml, 第三种方法则是在模板（template(data)）处进行的切分。
