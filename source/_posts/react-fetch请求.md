---
title: react fetch请求
date: 2018-03-08 17:58:22
tags: [react,fetch]
---


```
npm i whatwg-fetch es6-promise --save
```

##### 封装get请求


```
import 'whatwg-fetch'  
import 'es6-promise'  
  
export function get(url) {  
    // var result = fetch('http://www.mockhttp.cn'+url, { //打包apk时候使用  
    var result = fetch(''+url, {  
        credentails: 'include',  
        mode: "cors",  
        headers: {  
            'Accept': 'application/json, text/plain, */*',  
            'Content-Type': 'application/x-www-form-urlencoded'  
        }  
    });  
    return result;  
} 
```

##### 封装post请求


```
import 'whatwg-fetch'  
import 'es6-promise'  
  
//将json对象拼接成 key=val&key=val 的字符串形式  
function obj2params(obj) {  
    var result = '';  
    var item;  
    for(item in obj){  
        result += '&' + item + '=' +encodeURIComponent(obj[item]);  
    }  
  
    if(result) {  
        result = result.slice(1);  
    }  
    return result;  
}  
  
//发送 post 请求（首先会发送option）  
export function post(url, paramsObj) {  
    var result = fetch(url, {  
        method: 'post',  
        mode:'cors',  
        headers: {  
            'Accept': 'application/json',  
            'Content-Type': 'application/x-www-form-urlencoded'  
        },  
        body: obj2params(paramsObj)  
    });  
  
    return result;  
}  
```
##### 引用：(对外提供出口index.js)

```
import { get } from '../get'  
import { post } from '../post'  
  
export function postGpio(param) {  
    const result = post('*******',param)  
    return result  
}  
```

##### 在JSX语法中引用

```
postMessage(param) {  
        //send message  
        var params = {}  
        params.whichGpio = param  
        const result = postGpio(params)  
        result.then(res => {  
        //依据返回对象的不同
            return res.json()  // res.text()
        }).then(json => {  
            // get result  
            const data = json  
            if(data.code == '1'){  
                Toast.success('Send success !', 2);  
            }  
        }).catch(ex => {  
            // 发生错误  
            console.error('获取数据出错, ', ex.message)  
        })  
    }  
```


