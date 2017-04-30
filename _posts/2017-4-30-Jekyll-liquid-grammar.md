---
layout: post
title: Jekyll/Liquid Grammar"
---


Jekyll/Liquid常用语法记录。

# What is Jekyll

Jekyll是一个静态网站生成器。
通过标记语言Markdown或textile和模版引擎liquid转换生成网页。
Jekyll依赖于ruby开发平台。

# 目录结构

通过```$ jekyll new SiteName```目录可以迅速在当前目录下生成一个基本的Jekyll结构目录。

```
.
|-- _config.yml
|-- _includes/
|   |-- footer.html
|   |-- head.html
|-- _layouts/
|-- _posts/
|   |-- 2016-01-01-title.markdown
|   |-- 2016-01-02-title.markdown
|-- _drafts/
|   |-- title.markdown
|-- _data/
|   |-- feed.yml
|-- _site/
|-- index.html
_config.yml
```

全局配置文件，其中包含网站所有基础信息，便于需要时调用。

### _drafts
存放未发布的文章。

### _includes
存放关于网站header, footer, sidebar等公共部分，便于需要时调用。

### _layouts
存放页面基础配置文件，如page, post页面第一层设置，使用html语法编写。
页面设置可以多层嵌套。
在使用第一层配置时，使用layout来指定:
```
---
layout: page
---
```
### _posts

存放已发布内容，命名必须按照Y-M-D-title.markdown来命名。
语法必须使用Markdown, HTML或texile。
### _data

存放.yml, yaml, json, csv等文件。用于设置全局变量。

### _site
Jekyll用于生成网站的文件，需要在.gitignore中屏蔽。

### index.html
主页文件。
所有存放在根目录下并且不以下划线开头的活页夹中有格式的文件都会被处理成page。

# 全局变量配置

全局配置拥有默认配置，可以手动配置需要修改的地方。

### 时区
```
# timezone: America/Argentina/San_Luis
timesone: Timesone
```
### 链接格式

Post中的URL格式通过permalink来配置。

```
# /2016/01/01/title.html
permalink: /:year/:month/:day/:title.html

# /01-01-2016/title.html
permalink: /:month-:day-:year/:title.html
```

同时也有已有配置可以直接设置。
```
# /:category/:year/:month/:day/:title.html
permalink: date

# /:category/:year/:month/:day/:title.html
permalink: pretty

# /:category/:title.html
permalink: none
```

### 分页

文章分页变量：paginate, paginate_path。

```
paginate: 5
paginate_path: "blog/page:num/"
```

其中/blog/目录为baseurl目录。page是字符常量，变量是:num分页页码，自动从第二页开始编码。
默认值设定
设置author, layout等默认值。
在设定后可以随时通过post题头覆盖这些默认值。

```
# in _cofig.yml
defaults:
 - 
  scope:
    path: ""  # empty means all files in the project
    type: "posts"  
  values:
    layout: "post"
    author: "Ben"

  -
   scope:
    path: "project"
    type: "pages"
   values: 
     layout: "project"
```

# Jekyll模版、变量

Jekyll模版分为两部分，头部定义以及Liquid语法。

### 头部定义

定义制定模版的变量，这里的所有设置都可以覆盖全局变量中的基础配置。

```
---
layout: post
title: title
subtitle: subtitle
date: date
category: category
decription: description
published: true
permalink: none
---
```
所有的变量都是树节点，连接全局变量配置根节点。
全局根节点有：
site: config_yml全局配置信息
page: 页面布局信息。
content: 包含页面子视图，用于引入子节点内容，不能在post和page文件中使用。
paginator: 分页信息。
post变量仅作用于for循环内部。

### site 变量
变量	描述

| site.time               | 当前时间                            |
| :-----------------------| :-----------------------------------|
| site.pages              | 所有页面列表                        |
| site.posts              | 按时间逆排序所有文章列表            |
| site.related_posts      | 包含最多近期十篇文章列表            |
| site.static_files       | 所有静态文件列表                    |
| site.html_pages         | 所有HTML页面列表                    |
| site.collections        | 自定义对象集合列表                  |
| site.data               | data目录下YAML文件数据列表          |
| site.documents          | 所有Collections文檔列表             |
| site.categories         | Category 所有Category类别下文章列表 |
| site.tags               | Tag 所有Tag标签下列表               |
| site.Configuration_Data | 其他自定义变量                      |

### page 变量

| 变量	| 描述  |
| :-----| :-----|
| page.content 	           |   页面的内容 |
| page.title 	           |   页面的标题 |
| page.excerpt 	           |   未渲染的摘要 |
| page.url 	               |   不带域名的页面链接 |
| page.date 	           |   指定每一篇 post 的时间 |
| page.id 	               |   每一篇 post 的唯一标示符(在RSS中非常有用) |
| page.categories 	       |   post 隶属的一个分类列表，可在 YAML头部指定 |
| page.tags 	           |   post 隶属的一个标签列表，可在 YAML头部指定 |
| page.path 	           |   页面的源码地址 |
| page.next 	           |   按时间顺序排列的下一篇文章 |
| page.previous 	       |   按时间顺序排列的上一篇文章 |

### paginator 变量 
|变量 |	描述  |
| :--- | :--- |
|paginator.per_page |	每一页的 post 数量 |
|paginator.posts |	当前页面上可用的 post 列表 |
|paginator.total_posts |	所有 post 的数量 |
|paginator.total_pages |	分页总数 |
|paginator.page |	当前页的页码，或者 nil |
|paginator.previous_page |	上一页的页码，或者 nil |
|paginator.previous_page_path |	上一页的路径，或者 nil |
|paginator.next_page |	下一页的页码，或者 nil |
|paginator.next_page_path |	下一页的路径，或者 nil |

# Liquid 语法

Liquid是Ruby的一个模版引擎库，Jekyll中用到Liquid标记有两种：输出和标签。
Output标记：变成文本输出，被2层成对花括号包住。
Tag标记：执行命令，被成对的花括号和百分号包住。

# Jekyll 输出 Output

```
Hello  { { name } }
Hello  { { user.name } }
Hello  { { 'Ben' } }
```
Output标记可以使用过滤器Filters对输出内容进行简单处理。 多个Filters间用竖线隔开，从左到右依次执行，Filter左边总是输入，返回值为下一个Filter的输入或最终结果。

```
Hello { { 'Ben' | upcase } }  # 转换为大写输出
Hello Ben has { { 'Ben' | size } } letters. # 字符串长度
Hello { { '*Ben*' | markdownify | upcase } }  # 将Markdown字符串转换成HTML大写文本输出
Hello { { 'now' | date: "%Y %M %D" } }  # 按照指定格式输出当前时间
```

# 过滤器Filters

下面是常用的过滤器方法，更多API需要查阅源码，一般主要看两个Ruby Plugin文件：filters.rb（Jekyll）和standardfilters.rb（Liquid）。
- date - 將時間戳轉化為另一種格式
- capitalize - 輸入字符串首字母大寫
- downcase - 輸入字符串轉換為小寫
- upcase - 輸入字符串轉換為大寫
- first - 返回數組中第一個元素
- last - 返回數組數組中最後一個元素
- join - 用特定的字符將數組連接成字符串輸出
- sort - 對數組元素排序
- map - 輸入數組元素的一個屬性作為參數，將每個元素的屬性值映射為字符串
- size - 返回數組或字符串的長度
- escape - 將字符串轉義輸出
- escape_once - 返回轉義後的HTML文本，不影響已經轉義的HTML實體
- strip_html - 刪除 HTML 標籤
- strip_newlines - 刪除字符串中的換行符(\n)
- newline_to_br - 用HTML<br/> 替換換行符 \n
- replace - 替換字符串中的指定內容
- replace_first - 查找並替換字符串中第一處找到的目標子串
- remove - 刪除字符串中的指定內容
- remove_first - 查找並刪除字符串中第一處找到的目標子串
- truncate - 截取指定長度的字符串，第2個參數追加到字符串的尾部
- truncatewords - 截取指定單詞數量的字符串
- prepend - 在字符串前面添加字符串
- append - 在字符串後面追加字符串
- slice - 返回字符子串指定位置開始、指定長度的子串
- minus - 減法運算
- plus - 加法運算
- times - 乘法運算
- divided_by - 除法運算
- split - 根據匹配的表達式將字符串切成數組
- modulo - 求模運算

### Jekyll Tag

标签用于模版中的执行语句。下面是目前Jekyll/Liquid标准标签库：

|Tags |	Description |
|:---| :----|
|assign |	为变量赋值 |
|capture |	用捕获到的文本为变量赋值 |
|case |	条件分支语句 case…when… |
|comment |	注释语句 |
|cycle |	某些特定值间循环选择，如颜色、DOM类 |
|for |	循环语句 |
|if |	if/else 语句 |
|include |	包含另一个模版，文件在 _includes 目录 |
|raw |	禁用范围内的 Tag 命令，避免语法冲突 |
|unless |	if 语句的否定语句 |

### Comments
起到注释Liquid代码作用。

```
This is Benjamin. { % comment % } Comments { % endcomment % }
```

### Raw

临时禁止执行Jekyll Tag命令。

```
{ % raw % }
 This is a raw line. { % includ head % } will not show.
{ % endraw % }
```

### If/Else
条件语句，可以使用关键词有：if, unless, elseif, else。

### Case

适用于条件实例很多的情况。

```
{ % case template % }
{ % when 'label' % }
{ % when 'product' % }
{ % else % }
{ % endcase % }
```
### Cycle
经常需要在相似的任务间选择时，可以使用cycle标签。
```
{ % cycle 'one', 'two', 'three' % }
{ % cycle 'one', 'two', 'three' % }
```

同时可以指定循环名称进行分组处理。
```
{ % cycle 'group1': 'one', 'two', 'three' % }
For loops
```

循环遍历数组。

```
{ % for item in array % }
{ { item } }
{ % endfor % }
```

循环迭代Hash散列，item[0]是键，item[1]是值。

```
{ % for item in hash % }
{ { item[0] } }: { { item[1] } }
{ % endfor % }
```

每个循环周期，提供下面几个可用的变量：

```
forloop.length  # => length of the entire for loop
forloop.index  # => index of the current iteration
forloop.index0  # => index of the current iteration (zero based)
forloop.rindex  # => how many items are still left? 
forloop.rindex0  # => how many items are still left? (zero based)
forloop.first  # => is this the first iteration? 
forloop.last  # => is this the last iteration?
```

还有几个属性用来限定循环过程：
limit:int：限制循环迭代次数
offset:int：从第n个item开始迭代
reversed：反转循环顺序

### Variable Assignment
为变量赋值，用于输出或其他Tag。

```
{ % assign index = 1 % }
{ % assign name = 'freestyle' % }

{ % for t in collections.tags % }{ % if t == name % }
  Freestyle!
{ % endif % }{ % endfor % }


/ 变量是布尔类型 /

{ % assign freestyle = false % } 

{ % for t in collections.tags % }{ % if t == 'freestyle' % }
  { % assign freestyle = true % }
{ % endif % }{ % endfor % }

{ % if freestyle % }
  Freestyle!
{ % endif % }
```

capture允许将大量字符串合并为单个字符串并赋值给变量，而不会输出显示。

# 其他模版语句

### 格式化时间
```
{ { site.time | date_to_xmlschema } }     # => 2008-11-07T13:07:54-08:00
{ { site.time | date_to_rfc822 } }        # => Mon, 07 Nov 2008 13:07:54 -0800
{ { site.time | date_to_string } }        # => 07 Nov 2008
{ { site.time | date_to_long_string } }   # => 07 November 2008
```

### 代码语法高亮
安装pygments.rb的gem组件和Python 2.x后，配置文件添加highlighter:pygmnts，就可以使用语法高亮了。
```
{ % highlight ruby linenos % }
# some ruby codes
{ % endhighlight % }
```
参数
ruby：指定语言
linenos：显示行号
在给代码着色时，需配置相对应的CSS文件。

### 生成摘要

配置文件中设定excerpt_separator取值，每篇post都会自动截取从开始到这个值间的内容作为这篇文的摘要post.excerpt使用。
如果要禁用某篇文章的摘要，可以在该篇文章YAML头部设定excerpt_separator: ""。

```
{ % for post in site.posts % }
<a href="{ { post.url } }">{ { post.title } }</a>
{ { post.exceerpt | remove: 'test' } }
{ % endfor % }
```
### 删除HTML标签

这个在摘要作为head标签里的meta="description"内容输出时很有用。
{ { post.excerpt | strp_html } }

### 删除指定文本
过滤器remove可以删除变量中的指定内容。
```
{ { post.url | remove: 'http' } }
```

### CGI Escape
通常用于将URL中的特殊字符转义为%xx形式。

```
{ { "foo.bar;baz?" | cgi_escape } }  # => foo%2Cbar%3Bbaz%3F
```

### 排序

```
# Sort an array. Optional arguments for hashes:
#   1. property name
#   2. nils order ('first' or 'last')

{ { site.pages | sort: 'title', 'last' } }
```

# Assets 样式文件

Jekyll支持Sass和CoffeeScript，通过新建.sass, .scss, .coffee格式文件，并在开头添加一堆---来使用这个功能。

### 表格

```
| 表头 | 表头 | 表头 |
| --  | :-- | --: |
| 表格内容 | 表格内容(左对齐) | 表格内容(右对齐) |
| 表格内容 | 表格内容 | 表格内容 |
// 结果如下
```

| 表头 | 表头 | 表头 |
| --  | :-- | --: |
| 表格内容 | 表格内容(左对齐) | 表格内容(右对齐) |
| 表格内容 | 表格内容 | 表格内容 |

#### 在表格中添加管道（|）的方法

```
| 名称     | 符号 |
| ---      | ---       |
| 反撇号 | `         |
| 管道     | \|        |
```

| 名称     | 符号 |
| ---      | ---       |
| 反撇号 | `         |
| 管道     | \|        |

#### 在表格中添加行内代码块

```
| 命令 | 描述 |
| --- | --- |
| `git status` | List all *new or modified* files |
| `git diff` | Show file differences that **haven't been** staged |
```

| 命令 | 描述 |
| --- | --- |
| `git status` | List all *new or modified* files |
| `git diff` | Show file differences that **haven't been** staged |

# 更多参考内容

[emoji-cheat-sheet.com.](https://www.webpagefx.com/tools/emoji-cheat-sheet/)

[Basic writing and formatting syntax](https://help.github.com/articles/basic-writing-and-formatting-syntax/#using-emoji)
