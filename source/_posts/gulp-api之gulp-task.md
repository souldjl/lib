---
title: gulp-api之gulp-task
date: 2017-08-28 11:39:35
tags: [笔记,教程,gulp]
copyright: true
---
# gulp.task
`gulp.task`方法用来定义任务，内部使用的是`Orchestrator`(用于排序、执行任务和最大并发依赖关系的模块)，其语法为：
```javascript
gulp.task(name[, deps], fn)
```
`name` 为任务名；

`deps` 是当前定义的任务需要依赖的其他任务，为一个数组。当前定义的任务会在所有依赖的任务执行完毕后才开始执行。如果没有依赖，则可省略这个参数；

`fn `为任务函数，我们把任务要执行的代码都写在里面。该参数也是可选的。

当你定义一个简单的任务时，需要传入任务名字和执行函数两个属性。
```javascript
gulp.task('greet', function () {
   console.log('Hello world!');
});
```
执行`gulp greet`的结果就是在控制台上打印出“Hello world”。
<!--more-->

也可以定义一个在gulp开始运行时候默认执行的任务，并将这个任务命名为“default”：

```javascript
gulp.task('default', function () {
   // Your default task
});
```
当有多个任务时，需要知道怎么来控制任务的执行顺序。

>可以通过任务依赖来实现。例如我想要执行one,two,three这三个任务，那我们就可以定义一个空的任务，然后把那三个任务当做这个空的任务的依赖就行了：
```
//只要执行default任务，就相当于把one,two,three这三个任务执行了
gulp.task('default',['one','two','three']);
```
>如果任务相互之间没有依赖，任务就会按你书写的顺序来执行，如果有依赖的话则会先执行依赖的任务。但是如果某个任务所依赖的任务是异步的，就要注意了，gulp并不会等待那个所依赖的异步任务完成，而是会接着执行后续的任务。例如：
```javascript
gulp.task('one',function(){
  //one是一个异步执行的任务
  setTimeout(function(){
    console.log('one is done')
  },5000);
});
 
//two任务虽然依赖于one任务,但并不会等到one任务中的异步操作完成后再执行
gulp.task('two',['one'],function(){
  console.log('two is done');
});
```

>上面的例子中我们执行two任务时，会先执行one任务，但不会去等待one任务中的异步操作完成后再执行two任务，而是紧接着执行two任务。所以two任务会在one任务中的异步操作完成之前就执行了。
***
>那如果想等待异步任务中的异步操作完成后再执行后续的任务，该怎么做呢？

有三种方法可以实现：

###### 1.：在异步操作完成后执行一个回调函数来通知gulp这个异步任务已经完成,这个回调函数就是任务函数的第一个参数。
```javascript
gulp.task('one',function(cb){ //cb为任务函数提供的回调，用来通知任务已经完成
  //one是一个异步执行的任务
  exec(function(){
    console.log('one is finish');
    cb();  //执行回调，表示这个异步任务已经完成
  },5000);
});
 
//这时two任务会在one任务中的异步操作完成后再执行
gulp.task('two',['one'],function(){
  console.log('two is finish');
});
```
###### 2. ：定义任务时返回一个流对象。适用于任务就是操作gulp.src获取到的流的情况。
```javascript
gulp.task('one',function(cb){
  var stream = gulp.src('client/**/*.js')
      .pipe(exec()) //exec()中有某些异步操作
      .pipe(gulp.dest('build'));
    return stream;
});
 
gulp.task('two',['one'],function(){
  console.log('two is done');
});
```
###### 3. ：返回一个promise对象，例如
```javascript
var Q = require('q');
gulp.task('one', function() {
  var deferred = Q.defer();
 
  // 执行异步的操作
  setTimeout(function() {
    deferred.resolve();
  }, 1);
  return deferred.promise;
});
 
gulp.task('two',['one'],function(){
  console.log('two is done');
});
```