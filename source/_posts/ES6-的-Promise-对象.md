---
title: ES6 的 Promise 对象
date: 2017-08-10 09:36:03
tags: "笔记"
copyright: true
---
# 目录
## 一段使用了`promise`的基本语句代码
```javascript
function helloWorld (ready) {
    return new Promise(function (resolve, reject) {
        if (ready) {
            resolve("Hello World!");
        } else {
            reject("Good bye!");
        }
    });
}

helloWorld(true).then(function (message) {
    alert(message);
}, function (error) {
    alert(error);
});
```
> 上面的代码实现的功能非常简单~~，`helloWord`函数接受一个参数，如果为 true 就打印 "Hello World!"，如果为 false 就打印错误的信息。`helloWord` 函数返回的是一个 ==Promise== 对象。

<!--more-->

***
## resolve 和 reject

在 Promise 对象当中有两个重要方法————resolve 和 reject。


> `resolve` 方法可以使 `Promise`对象的状态改变成成功，同时传递一个参数用于后续成功后的操作，在这个例子当中就是 Hello World! 字符串。

> `reject` 方法则是将 `Promise` 对象的状态改变为失败，同时将错误的信息传递到后续错误处理的操作。

***
## then 和 catch
### then

```javascript
function printHello(ready){
    return new Promise(function(resolve,reject){
            if(ready){
                resolve("hello")
            }else{
                reject("bad")
            }
    })
}

function printWorld(){
    alert('world')
}

function print1(){
    alert('!')
}

printHello(true).then(function(message){
    alert(message)
})
.then(printWorld)
.then(print1)
```

> `then` 可以使用链式调用的写法原因在于，每一次执行该方法时总是会返回一个 `Promise` 对象。
另外，在 `then onFulfilled`的函数当中的返回值，可以作为后续操作的参数，因此上面的例子也可以写成

```javascript
function printHello(ready){
    return new Promise(function(resolve,reject){
        if(ready){
            resolve("hello")
        }else{
            reject("err")
        }
    })
}

printHello(true).then(function(message){
    return message
}).then(function(message){
    return message+'world'
}).then(function(message){
    return message + '!'
}).then(function(message){
    alert(message)
}).catch(function(err){
    alert(err)
});
```

---

### catch

> `catch` 方法是 `then(onFulfilled, onRejected)` 方法当中 `onRejected `函数的一个简单的写法，也就是说可以写成 then(fn).catch(fn)，相当于 `then(fn).then(null, fn)`。使用 `catch`的写法比一般的写法更加清晰明确。

***

## Promise.all 和 Promise.race

> `Promise.all` 可以接收一个元素为 `Promise` 对象的数组作为参数，当这个数组里面所有的 `Promise` 对象都变为`resolve` 时，该方法才会返回。

```javascript
var p1 = new Promise(function (resolve) {
    setTimeout(function () {
        resolve("Hello");
    }, 3000);
});

var p2 = new Promise(function (resolve) {
    setTimeout(function () {
        resolve("World");
    }, 1000);
});

Promise.all([p1, p2]).then(function (result) {
    console.log(result); // ["Hello", "World"]
});
```
> 上面的例子模拟了传输两个数据需要不同的时长，虽然 p2 的速度比 p1 要快，但是 `Promise.all` 方法会按照数组里面的顺序将结果返回。 日常开发中经常会遇到这样的需求，在不同的接口请求数据然后拼合成自己所需的数据，通常这些接口之间没有关联（例如不需要前一个接口的数据作为后一个接口的参数），这个时候 `Promise.all` 方法就可以派上用场了。还有一个和 `Promise.all` 相类似的方法 `Promise.race`，它同样接收一个数组，不同的是只要该数组中的 Promise 对象的状态发生变化（无论是 `resolve` 还是 `reject`）该方法都会返回。

***
# 参考
[JavaScript Promise迷你书（中文版）](http://liubin.github.io/promises-book/)
 
