---
title: gulp-api之gulp-watch
date: 2017-08-28 11:26:42
tags: [笔记,教程,gulp]
copyright: true
---
# gulp.watch
　gulp.watch()用来监视文件的变化，当文件发生变化后，我们可以利用它来执行相应的任务，例如文件压缩等。其语法为

## gulp.watch(glob[, opts], tasks); 
`glob` 为要监视的文件匹配模式，规则和用法与`gulp.src()`方法中的glob相同。
`opts` 为一个可选的配置对象，通常不需要用到。
` tasks` 为文件变化后要执行的任务，为一个数组。
<!--more-->

```javascript
gulp.task('uglify',function(){
  //do something
});
gulp.task('reload',function(){
  //do something
});
gulp.watch('js/**/*.js', ['uglify','reload']);
```

`gulp.watch()`还有另外一种使用方式：

`gulp.watch(glob[, opts, cb]);`

`glob`和`opts`参数与第一种用法相同;

`cb`参数为一个函数。每当监视的文件发生变化时，就会调用这个函数,并且会给它传入一个对象，该对象包含了文件变化的一些信息，type属性为变化的类型，可以是`added`,`changed`,`deleted`；`path`属性为发生变化的文件的路径。

```javascript
gulp.watch('js/**/*.js', function(event){
    console.log(event.type); //变化类型 added为新增,deleted为删除，changed为改变 
    console.log(event.path); //变化的文件的路径
}); 
```