---
layout: post
title: about the encoding of php
---


### 一.HTML页面转UTF-8编码问题 

## 1.在head后，title前加入一行：

<meta http-equiv=’Content-Type’ content=’text/html; charset=utf-8′ />
顺序不能错，一定要在

显示的标题有可能是乱码！

## 2.html文件编码问题:

点击编辑器的菜单：“文件”->“另存为”，可以看到当前文件的编码，确保文件编码为：UTF-8，
如果是ANSI，需要将编码改成：UTF-8。
## 3.HTML文件头BOM问题：
将文件从其他的编码转换成UTF-8编码时，有时候会在文件的最开始加上一个BOM标签，
在个BOM标签可能会导致浏览器在显示中文的时候出现乱码。
删除这个BOM标签的方法：

- 1.可以用Dreamweaver打开文件，并重新保存，即可以去除BOM标签！
- 2.可以用EditPlus打开文件，并在菜单“首选项”->“文件”->”UTF-8标识”，设置为：“总是删除签名”，
然后保存文件，即可以去除BOM标签！

## 4.WEB服务器UTF-8编码问题：
如果你按以上所列的步骤做了，还是有中文乱码问题，
请检查你的所使用的WEB服务器的编码问题
如果你使用的是Apache，请将配置文件里的：charset 设成：utf-8(这里仅列出方法，具体格式请参考apache的配置文件)
如果你使用的是Nginx，请将nginx.conf里的：charset 设成 utf-8，
具体找到 “charset gb2312;”或者类似的语句，改成：“charset utf-8;”。

### 二.PHP页面转UTF-8编码问题
## 1.在代码开始出加入一行：
```
header(“Content-Type: text/html;charset=utf-8”);
```
## 2.PHP文件编码问题

点击编辑器的菜单：“文件”->“另存为”，可以看到当前文件的编码，确保文件编码为：UTF-8，
如果是ANSI，需要将编码改成：UTF-8。

## 3.PHP文件头BOM问题：
PHP文件一定不可以有BOM标签
否则，会出现session不能使用的情况，并有类似的提示：
```
Warning: session_start() [function.session-start]: Cannot send session cache limiter – headers already sent
```
这是因为，在执行session_start() 的时候，整个页面不能有输出，但是当由于前PHP页面存在BOM标签，
PHP把这个BOM标签当成是输出了，所以就出错了！
所以PHP页面一定要删除BOM标签
删除这个BOM标签的方法：
- 1.可以用Dreamweaver打开文件，并重新保存，即可以去除BOM标签！
- 2.可以用EditPlus打开文件，并在菜单“首选项”->“文件”->”UTF-8标识”，设置为：“总是删除签名”，
然后保存文件，即可以去除BOM标签！
## 4.PHP以附件形式保存文件的时候，UTF-8编码问题：
PHP以附件形式保存文件，文件名必须是GB2312编码，
否则，如果文件名中有中文的话，将是显示乱码：
如果你的PHP本身是UTF-8编码格式的文件，
需要将文件名变量由UTF-8转成GB2312：
iconv(“UTF-8”, “GB2312”, “$filename”);

## 5.截断显示文章标题时，出现乱码或者“？”问号的问题：
一般文章标题很长的时候，会显示一部分标题，会对文章标题进行截断，
由于一个UTF-8编码格式的中文字符会占用3个字符宽度，
截取标题的时候，有时会只截取到一个中文字符的1个字符或2字符宽度，
没截取完整，将出现乱码或“？”问号的情况，
用下面的函数截取标题，就不会有问题：

复制代码 代码如下:
```
function get_brief_str($str, $max_length)
{
echo strlen($str) .”<br>”;
if(strlen($str) > $max_length)
{
$check_num = 0;
for($i=0; $i < $max_length; $i++)
{
if (ord($str[$i]) > 128)
$check_num++;
}
if($check_num % 3 == 0)
$str = substr($str, 0, $max_length).”…”;
else if($check_num % 3 == 1)
$str = substr($str, 0, $max_length + 2).”…”;
else if($check_num % 3 == 2)
$str = substr($str, 0, $max_length + 1).”…”;
}
return $str;
}
```

### 三.MYSQL数据库使用UTF-8编码的问题

## 1.用phpmyadmin创建数据库和数据表
创建数据库的时候，请将“整理”设置为：“utf8_general_ci”
或执行语句：
```
CREATE DATABASE dbname DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
```
创建数据表的时候：如果是该字段是存放中文的话，则需要将“整理”设置为：“utf8_general_ci”，

如果该字段是存放英文或数字的话，默认就可以了。

相应的SQL语句，例如：

复制代码 代码如下:
```
CREATE TABLE test (
id INT NOT NULL ,
name VARCHAR( 10 ) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL ,
PRIMARY KEY ( id )
) ENGINE = MYISAM ;
```

## 2.用PHP读写数据库

在连接数据库之后：
```
[hide]$connection = mysql_connect($host_name, $host_user, $host_pass);
```
加入两行：

复制代码 代码如下:
```
mysql_query(“set character set ‘utf8′”);//读库
mysql_query(“set names ‘utf8′”);//写库
```
就可以正常的读写MYSQL数据库了。

### 四.JS相关的UTF-8编码问题
## 1.JS读Cookie的中文乱码问题

PHP写cookie的时候需要将中文字符进行escape编码，
否则JS读到cookie中的中文字符将是乱码。
但php本身没有escape函数，我们新写一个escape函数：

复制代码 代码如下:
```
function escape($str)
{
preg_match_all(“/[\x80-\xff].|[\x01-\x7f]+/”,$str,$r);
$ar = $r[0];
foreach($ar as $k=>$v)
{
if(ord($v[0]) < 128)
$ar[$k] = rawurlencode($v);
else
$ar[$k] = “%u”.bin2hex(iconv(“UTF-8″,”UCS-2”,$v));
}
return join(“”,$ar);
}
```

JS读cookie的时候，用unescape解码，

然后就解决cookie中有中文乱码的问题了。

## 2.外部JS文件UTF-8编码问题

当一个HTML页面或则PHP页面包含一个外部的JS文件时，

如果HTML页面或则PHP页面是UTF-8编码格式的文件，

外部的JS文件同样要转成UTF-8的文件，

否则将出现，没有包含不成功，调用函数时没有反应的情况。

点击编辑器的菜单：“文件”->“另存为”，可以看到当前文件的编码，确保文件编码为：UTF-8，

如果是ANSI，需要将编码改成：UTF-8。

### 五.FLASH相关的UTF-8编码问题

FLASH内部对所有字符串，默认都是以UTF-8处理

## 1.FLASH读文普通本文件(txt,html)
要将文本文件的编码存为UTF-8
点击编辑器的菜单：“文件”->“另存为”，可以看到当前文件的编码，确保文件编码为：UTF-8，
如果是ANSI，需要将编码改成：UTF-8。

## 2.FLASH读XML文件
要将XML文件的编码存为UTF-8
点击编辑器的菜单：“文件”->“另存为”，可以看到当前文件的编码，确保文件编码为：UTF-8，
如果是ANSI，需要将编码改成：UTF-8。
在XML第1行写：

## 3.FLASH读PHP返回数据
如果PHP编码本身是UTF-8的，直接echo就可以了
如果PHP编码本身是GB2312的，可以将PHP转存成UTF-8编码格式的文件，直接echo就可以了
如果PHP编码本身是GB2312的，而且不允许改文件的编码格式，
用下面的语句将字符串转换成UTF-8的编码格式
```
$new_str = iconv(“GB2312”, “UTF-8”, “$str”);
```
再echo就可以了

## 4.FLASH读数据库(MYSQL)的数据
FLASH要通过PHP读取数据库中的数据
PHP本身的编码不重要，关键是如果数据库的编码是GB2312的话，
需要用下面的语句将字符串转换成UTF-8的编码格式
```
$new_str = iconv(“GB2312”, “UTF-8”, “$str”);
```

## 5.FLASH通过PHP写数据
一句话，FLASH传过来的字符串是UTF-8格式的，
要转换成相应的编码格式，再操作（写文件、写数据库、直接显示等等）
还是用iconv函数转换
## 6.FLASH使用本地编码(理论上不推荐使用)
如果想让FLASH不使用UTF-8编码，而是使用本地编码
对于中国大陆地区而言，本地编码是GB2312或GBK
AS程序内，可以添加以下代码：
```
System.useCodepage = true;
```
那么FLASH内所有字符都是使用GB2312的编码了
所有导入到FLASH或者从FLASH导出的数据，都应该做相应的编码转换
因为使用本地编码，会造成使用繁体中文地区的用户产生乱码，所以不推荐使用。
