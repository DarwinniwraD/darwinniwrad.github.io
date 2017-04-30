---
layout: post
title: About a way to resovle the “Notice: Undefined variable” and “Notice: Undefined index”
---

There are 7 ways at least to resolve the e-notice, like the @ opreator , it would be a fast, and direct way to solve that problem, but it would be imply many security problem. Some other would be like isset(), empty() function, the former one would be used like $value = isset($_POST[‘value’]) ? $_POST[‘value’] : ”; and the later one would be used like this, echo “Hello ” . (!empty($user) ? $user : “”);  but  at some time it still can’t avoid that boring NOTICE. Here is my recommendation:
```
//First, import a variability for other places.

$fahrenheit = filter_input(INPUT_POST, “fahrenheit”);

//Second , we can use the filter_input() function to valid the e-notice, like this,

if (filter_input(INPUT_POST, “fahrenheit”)) {

#Coding 

} ;
```
 

MORE RECOMMENDATIONS

(ENGLISH VERSION)[PHP: “Notice: Undefined variable” and “Notice: Undefined index”](http://stackoverflow.com/questions/4261133/php-notice-undefined-variable-and-notice-undefined-index)

(中文版) [PHP Notice: undefined index 完美解决方法](http://alfredwebdesign.blogspot.com/2013/05/php-notice-undefined-index.html)
