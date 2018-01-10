---
title: 关于javascrip中sort函数的使用
date: 2017-08-18 14:45:30
tags: "js"
copyright: true
---
需求有个这样的东西，后台接口各个城市的数据，前端根据数据按照首字母进行分类，排序，进行渲染。
![](http://otkpvfszt.bkt.clouddn.com/souldjlarea.png)
<!--more-->
在W3CSchool中看到，这个函数的一些说明。如下图
![](http://otkpvfszt.bkt.clouddn.com/sort.png)

原始数据格式如下：
```javascript
    [
        {"Name": "江苏省", "cityList": [...],"Header": "jiangsusheng"},
        {"Name": "江西省", "cityList": [...],"Header": "jiangxisheng"},
        {"Name": "辽宁省","cityList": [...],"Header": "liaoningsheng"}
    ]
```
我们只需要将数据格式更换为
```javascript
 [
        {  "name":"A",
            "city":[ {"Name": "辽宁省","cityList": [...],"Header": "liaoningsheng"}]
        },
        {  "name":"J",
            "city":[ 
                    {"Name": "江苏省", "cityList": [...],"Header": "jiangsusheng"},
                    {"Name": "江西省", "cityList": [...],"Header": "jiangxisheng"}
            ]
        },
]
```
## 第一步 将数组首字母相同的合并
```javascript
function merge(arr) {
    var res = [];
    var json = {};
    for (var i = 0; i < arr.length; i++) {
        if (!json[arr[i].name]) {
            var sJson={};
            sJson.name=arr[i].name;
            sJson.list=arr[i].list;
            res.push(sJson);
            json[arr[i].name] = 1;
        }else{
            for(var k=0; k<res.length; k++){
                if(arr[i].name==res[k].name){
                    res[k].list.push(arr[i].list[0]);
                    break;
                }
            }
        }
    }
    return res;
}
```
## 第二步 实现数组按照某个属性排序

```javascript
function rank (prop){
        return function (obj1, obj2) {
            var val1 = obj1[prop];
            var val2 = obj2[prop];
            if (!isNaN(Number(val1)) && !isNaN(Number(val2))) {
                val1 = Number(val1);
                val2 = Number(val2);
            }
            if (val1 > val2) { //升序排列
                return 1;
            } else if (val1 < val2) {
                return -1;
            } else {
                return 0;
            }
        };
}
```
eg:
```javascript
    [
     {name:'gpp',age:0},
     {name:'aop',age:18},
     {name:'yjj',age:8}    
    ]
   
```
效果
```javascript
arr.sort(rank('name'))
[
     {name:'aop',age:0},
     {name:'gpp',age:18},
     name:'yjj',age:8}    
]
```
至此，完结.



