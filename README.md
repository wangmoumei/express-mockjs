# express-mockjs

### 原作者[52cik/express-mockjs](https://github.com/52cik/express-mockjs)  

### 基于Mockjs

随机参数返回random用法 详见文档：[Mock.js 0.1 文档](http://mockjs.com/0.1/#Mock)  
* [Mock 例子](http://mockjs.com/examples.html)  


## 如何使用

### 安装

``` sh
$ npm install --save-dev express-mockjs
```


### 例子

``` js
var path = require('path')
var express = require('express')
var mockjs = require('express-mockjs')

var app = express()

// 使用默认路径（不推荐） /
// app.use(mockjs(path.join(__dirname, 'mocks')))

// 自定义路径 /api 映射mocks文件夹
app.use('/api', mockjs(path.join(__dirname, 'mocks')))

// 这里可以添加你的自定义代码.

app.listen(3000);
```

然后你可以访问 <http://localhost:3000/api> 查看API文档。

**推荐使用 [nodemon][nodemon] 监视自动重启服务**





### 例子

```
.
├── mocks
    ├── demo.json
    ├── demo1.js
    ├── demo2.js

```



## demo.json


> 假设我们有个文件 'mocks/demo.json' 内容为:

``` js
/**
 * 接口描述
 *
 * @url /api-access-path
 *
 * GET: 请求方法及参数
 *   uid 这是请求的用户ID
 *
 * 参数描述和其他说明。
 * uid: 用户ID
 * name: 用户名
 * email: 邮箱
 * 等等其他描述.
 */

{
  "code": 0,
  "result|5": [
    {
      "uid|+1": 1,
      "name": "@cname",
      "email": "@email"
    }
  ]
}
```

接口访问地址

``` js
  @url /api-access-path
```

即可以访问 <http://localhost:3000/api/api-access-path> 查看实际效果.

当然，你也可以直接使用js文件书写。

``` js
// /mocks/demo1.js
/**
 * 首页 - 友情链接
 *
 * @url /home-links
 *
 * 在这里你可以写详细的说明参数的信息
 */

module.exports = {
  "code": function () { // 1/10 的概率返回错误码 1.
    return Math.random() < 0.1 ? 1 : 0;
  },
  "list|5-10": [
    {"title": "@title", "link": "@url"}
  ]
};
```

或者直绑定函数：

``` js
// /mocks/demo2.js
/**
 * 用户页面 - 用户信息
 *
 * @url /user?uid=233
 *
 * GET: 请求方法及参数
 *   uid 这是请求的用户ID
 *
 * 在这里你可以写详细的说明参数的信息
 */

module.exports = function (req) {
  // express 的 req 对象
  var uid = req.query.uid;

  if (!uid) { // 当没有用户ID时返回错误信息
    return {
      code: -1,
      msg: 'no uid',
    }
  }

  return { // 返回mock的用户信息，但用户id固定
    code: 0,
    data: {
      "uid": +uid,
      "name": "@cname",
      "age|20-30": 1,
      "email": "@email",
      "date": "@date",
    },
  };
};
```



[travis-url]: https://travis-ci.org/52cik/express-mockjs
[travis-image]: https://img.shields.io/travis/52cik/express-mockjs/master.svg?label=linux

[coveralls-url]: https://coveralls.io/github/52cik/express-mockjs?branch=master
[coveralls-image]: https://coveralls.io/repos/52cik/express-mockjs/badge.svg?branch=master&service=github

[license-url]: https://opensource.org/licenses/MIT
[license-image]: https://img.shields.io/badge/license-MIT-blue.svg

[dependencies-url]: https://david-dm.org/52cik/express-mockjs
[dependencies-image]: https://img.shields.io/david/52cik/express-mockjs.svg?style=flat

[node-url]: https://nodejs.org
[node-image]: https://img.shields.io/badge/node-%3E%3D%200.10.0-brightgreen.svg


[nodemon]: https://nodemon.io
