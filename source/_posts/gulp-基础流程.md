---
title: gulp 入门文档
date: 2017-08-28 10:54:37
tags: [笔记,教程,gulp]
copyright: true
---
## 新建 gulpfile.js

>gulp是基于Nodejs的自动任务运行器， 她能自动化地完成 javascript、coffee、sass、less、html/image、css 等文件的测试、检查、合并、压缩、格式化、浏览器自动刷新、部署文件生成，并监听文件在改动后重复指定的这些步骤。在实现上，她借鉴了Unix操作系统的管道（pipe）思想，前一级的输出，直接变成后一级的输入，使得在操作上非常简单。
>

## npm init 
## 新建gulpfile.js文件
## 写入 gulp任务

<!--more-->
```javascript
var gulp = require('gulp'),
    less = require('gulp-less'),
    minifyCSS = require('gulp-minify-css');


gulp.task('testLess', function () {
  gulp.src('src/less/index.less')
    .pipe(less())
    .pipe(gulp.dest('dist/css'));
});

gulp.task('css', function () {
  gulp.src('dist/css/*.css')
    .pipe(minifyCSS())
    .pipe(gulp.dest('dist/css'))
})

gulp.task('default',['css']);
```

## 命令行 gulp 或者gulp css/testLess

***

　gulp的使用流程一般是：首先通过gulp.src()方法获取到想要处理的文件流，然后把文件流通过pipe方法导入到gulp的插件中，最后把经过插件处理后的流再通过pipe方法导入到gulp.dest()中，gulp.dest()方法则把流中的内容写入到文件中。例如：
　***
```javascript
var gulp = require('gulp');
gulp.src('script/jquery.js')         // 获取流的api
    .pipe(gulp.dest('dist/foo.js')); // 写放文件的api
```