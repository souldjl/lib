---
title: gulp-api 之gulp.src
date: 2017-08-28 11:00:16
tags: [笔记,教程,gulp]
copyright: true
---
# gulp.src
`gulp.src()`方法是用来获取流的，但要注意这个流里的内容不是原始的文件流，而是一个虚拟文件对象流(`Vinylfiles`)，这个虚拟文件对象中存储着原始文件的路径、文件名、内容等信息。其语法为：
```javascript
gulp.src(globs[, options]);
```
`globs参数`是文件匹配模式(类似正则表达式)，用来匹配文件路径(包括文件名)，当然这里也可以直接指定某个具体的文件路径。
```javascript
gulp.src('script/jquery.js')         // 获取script文件下的jquery.js
```
当有多个匹配模式时，该参数可以为一个数组;类型为`String`或 `Array`。
```javascript
// 使用数组的方式来匹配多种文件`
gulp.src(['js/*.js','css/*.css','*.html'])
```
<!--more-->
## options.buffer

类型： `Boolean` 默认值：` true`

如果该项被设置为 `false`，那么将会以 `stream` 方式返回 `file.contents` 而不是文件 `buffer` 的形式。这在处理一些大文件的时候将会很有用。注意：插件可能并不会实现对 `stream` 的支持。

## options.read

类型： `Boolean` 默认值：` true`

如果该项被设置为 `false`， 那么 `file.contents` 会返回空值（`null`），也就是并不会去读取文件。

## options.base

类型： `String` ， 设置输出路径以某个路径的某个组成部分为基础向后拼接。

如, 请想像一下在一个路径为 `client/js/somedir` 的目录中，有一个文件叫 `somefile.js` ：
```javascript
gulp.src('client/js/**/*.js') // 匹配 'client/js/somedir/somefile.js' 现在 `base` 的值为 `client/js/`
  .pipe(minify())
  .pipe(gulp.dest('build'));  // 写入 'build/somedir/somefile.js' 将`client/js/`替换为build
 
gulp.src('client/js/**/*.js', { base: 'client' }) // base 的值为 'client'
  .pipe(minify())
  .pipe(gulp.dest('build'));  // 写入 'build/js/somedir/somefile.js' 将`client`替换为build
  ```
