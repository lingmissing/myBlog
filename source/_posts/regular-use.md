---
title: 正则表达式（上篇）
date: 2016-03-28 09:59:50
tags: 正则表达式
categories: 正则表达式
---

## 元字符

- `.`  匹配除换行符以外的任意字符
- `\w` 匹配字母或数字或下划线或汉字
- `\s` 匹配一个空白字符，包括空格、制表符、换页符和换行符。
- `\d` 匹配数字
- `\b` 匹配单词的开始或结束
- `^`  匹配字符串的开始
- `$`  匹配字符串的结束
- `\n` 匹配一个换行符
- `\r` 匹配一个回车符
- `\0nn` ASCII代码中八进制代码为nn的字符
- `\xnn` ASCII代码中十六进制代码为nn的字符
- `\unnnn` Unicode代码中十六进制代码为nnnn的字符 `[\u4e00-\u9fa5]`

<!--more-->

如果我们要查找元字符本身需要加上 `\` 转义，如：`\*`  `\\w`

## 重复

- `*` 重复零次或更多次
- `+` 重复一次或更多次
- `?` 重复零次或一次
- `{n}` 重复n次
- `{n,}` 重复n次或更多次
- `{n,m}` 重复n到m次

## 字符类

- [a-zA-Z]
- [\w] 
- [0-9]

## 分枝条件

- `|` 或

## 分组

- (\d{1,3}){3}

## 反义

- `\W` 匹配任意不是字母，数字，下划线，汉字的字符
- `\S` 匹配任意不是空白符的字符
- `\D` 匹配任意非数字的字符
- `\B` 匹配不是单词开头或结束的位置
- `[^x]` 匹配除了x以外的任意字符
- `[^aeiou]` 匹配除了aeiou这几个字母以外的任意字符

## 操练

- 0\d{2}-\d{8}
- \(?0\d{2}\)?[- ]?\d{8}|0\d{2}[- ]?\d{8}
- ((2[0-4]\d|25[0-5]|[01]?\d\d?)\.){3}(2[0-4]\d|25[0-5]|[01]?\d\d?)
- /\(?\d{3}\)?([-\/\.])\d{3}\1\d{4}/

## 反向引用

## 捕获

## 零宽断言

## 贪婪与懒惰

## 其它

## Javascript 中的应用


## 相关参考： 
> [参考链接-01](http://deerchao.net/tutorials/regex/regex.htm)
> 
> [参考链接-02](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)
