---
title: gulp-api之gulp-dest
date: 2017-08-28 11:19:34
tags: [笔记,教程,gulp]
copyright: true
---
# gulp.dest
`gulp.dest()`方法是用来写文件的，其语法为：
```
gulp.dest(path[,options])
```
`path`为写入文件的路径；

`options`为一个可选的参数对象，以下为选项参数：
<!--more-->

## options.cwd

类型： `String` 默认值： `process.cwd()`

输出目录的 cwd 参数，只在所给的输出目录是相对路径时候有效。

## options.mode

类型： `String` 默认值： `0777`

八进制权限字符，用以定义所有在输出目录中所创建的目录的权限。

```javascript
var gulp = require('gulp');
gulp.src('script/jquery.js')　       // 获取流
    .pipe(gulp.dest('dist/foo.js')); // 写放文件
```
下面再说说生成的文件路径与我们给`gulp.dest()`方法传入的路径参数
之间的关系。　`gulp.dest(path)`生成的文件路径是我们传入的`path`参数
后面再加上`gulp.src()`中有通配符开始出现的那部分路径。例如：
```javascript
var gulp = reruire('gulp');
//有通配符开始出现的那部分路径为 **/*.js
gulp.src('script/**/*.js')
    .pipe(gulp.dest('dist')); //最后生成的文件路径为 dist/**/*.js
    
```

如果 `**/*.js` 匹配到的文件为 `jquery/jquery.js` ,则生成的文件路径为 `dist/jquery/jquery.js`
用`gulp.dest()`把文件流写入文件后，文件流仍然可以继续使用。

