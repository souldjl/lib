---
layout: vue
title: 利用本地存储解决vue开发的刷新后页面空白问题
date: 2017-10-26 09:06:09
tags: [笔记,教程]
---
---
## 用vue开发的单页面应用，为什么不能在内页刷新？
***
即使改成hash模式，刷新页面后store里的值还是会丢失啊，所以用了本地存储来解决问题
***
<!--more-->
### 一 .新建 plugins.js
```javascript
import {
  STAGE_MUTATION_USER_OPTIONS,
  STAGE_MUTATION_SERVER_CONDITIONS_1,
} from './mutations'

const localStoragePlugin = store => {
  store.subscribe((mutation, state) => {
    console.log( 'mutation', mutation, 'state', state )

    if( STAGE_MUTATION_SERVER_CONDITIONS_1 === (mutation.type) ) {
      console.log( mutation )
      window.localStorage.setItem(mutation.payload.key, JSON.stringify(mutation.payload.value))
    }

    if( STAGE_MUTATION_USER_OPTIONS.indexOf(mutation.type) >= 0 ) {
      window.localStorage.setItem("options", JSON.stringify(state.options))
      console.log( state.options )
    }
    if( mutation.type == 'getCategoryById' ) {
        window.localStorage.setItem("categoryInfo", JSON.stringify(state.categoryInfo))
    }
    if( mutation.type == 'getQuestionsToCategory' ) {
        window.localStorage.setItem("qList", JSON.stringify(state.qList))
    }

    if( mutation.type == 'userSelectQids' || mutation.type == 'userSelectAnswer' ||  mutation.type == 'getQuestionsToCategory' ) {
        window.localStorage.setItem("userAnswerInfos", JSON.stringify(state.userAnswerInfos))
    }
  })
};

export default [localStoragePlugin]
```

### 二. mutations.js
***
```javascript
const userAnswerInfoJSON = window.localStorage.getItem('userAnswerInfos');
const userAnswerInfos = JSON.parse(userAnswerInfoJSON) || {};

console.log('init','options', optionsJSON, options);


export const state = {
  provinces  : JSON.parse(window.localStorage.getItem('provinces')) || [],
  stages: JSON.parse(window.localStorage.getItem('stages')) || [],
  examTypes: JSON.parse(window.localStorage.getItem('examTypes')) || [],
  // subjects:[],
  options:{
    examType: options.examType || '',
    area: options.area || '',
    preferredSubjects: options.preferredSubjects || [],
    subject: options.subject > 0 ? options.subject : 0,
    preferredCategories:options.preferredCategories || [],
    category: options.category > 0 ? options.category : 0,
    subCategory: options.subCategory > 0 ? options.subCategory : 0,
  },
  categoryInfo:JSON.parse(window.localStorage.getItem('categoryInfo')) || [],
  qList: JSON.parse(window.localStorage.getItem('qList')) ||[],
  userAnswerInfos:{
    qids: userAnswerInfos.qids || [],
    answer: userAnswerInfos.answer ||[],
    trueAnswer: userAnswerInfos.trueAnswer ||[]
  },

};

export const STAGE_MUTATION_USER_OPTIONS = [
  'stageExamType',
  'stageArea',
  'stagePhase',
  'stagePreferredSubjects',
  'stageSubject',
  'stagePreferredCategories',
  'stageCategory',
  'stageSubCategory',
];

export const STAGE_MUTATION_SERVER_CONDITIONS_1 = "stageServerConditions1";
```

### 三 .项目目录结构 
![](http://otkpvfszt.bkt.clouddn.com/1.png)
![](http://otkpvfszt.bkt.clouddn.com/5.png)

>解决思路：页面的显示很多依靠vuex的store取值的，而vuex的存储会造成页面刷新的时候值为空，所以解决方法就是每次更改store的时候再storage里面存储一份，取值之前 则先从storage里面取值，否则直接给 默认值(空对象或空数组)

