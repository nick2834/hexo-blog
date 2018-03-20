---
title: Vue mapGetters 报语法错误解决办法
date: 2018-02-27 14:42:51
type: "tags"
tags: [ Vue, Element-ui, 前端 ]
---

### Vuex 全局控制，写mapGetters 报语法错误

```javascript
...mapGetters([
    'sidebar',
    'name',
    'avatar',
    'language',
    'errorLogs'
])
```
```bash
Syntax Error: Unexpected token (135:12)
```
解决办法如下：
```bash
npm install transform-object-rest-spread --save
```
根目录创建.babelrc文件
```javascript
"presets": ["es2015", "stage-2"],
"plugins": ["transform-runtime"],
"comments": false
```