---
typora-copy-images-to: ..\assets
---

## mock.js和json-server使用记录

### 一、[Mock.js](http://mockjs.com/)

关于mockjs，官网描述的是：生成随机数据，拦截 Ajax 请求。

#### 1.1 安装

```
npm install mockjs --save-dev
```

#### 1.2 使用 Mock

```
/**
 * mock.js
 * 创建数据集
 */
 
import Mock from 'mockjs'

Mock.mock('/demo/test', /post|get|put|delete/i, {
  "code": "SUCCESS",
  "msg": "成功",
  "data": {
      // 与后台返回格式一致
  }
}

/**
 * main.js
 * 引入拦截器
 */ 
 
 require('mock.js')
 
 /**
  * xxApiSource.js
  * 接口模块化引用
  */
  
  import { fetch } from '@/api/fetch'
  
  export function getDemoParams(params) {
    return hsNewService({
      url: `/demo/test`,
      method: 'get',
      params: params,
    })
   }

```

#### 二、Json-server

JSON-Server 是一个 Node 模块，运行 Express 服务器，你可以指定一个 json 文件作为 api 的数据源。

#### 2.1  安装

```
npm install -g json-server
```

#### 2.2  使用json-server

##### 2.2.1、创建db.json

```
{
  "fruit": [
    {
      "id": 1,
      "name": "苹果"
    },{
      "id": 2,
      "name": "梨子"
    },{
      "id": 3,
      "name": "香蕉"
    }
  ]
}
```

启动服务在package.json中，配置启动命令,把`db.json`文件托管成一个 web 服务

```
json-server --watch --port 8088 db.json
```

##### 2.2.2  默认路由

`json-server`为提供了`GET`,`POST`, `PUT`, `PATCH` ,`DELETE`等请求的API

```
# 获取全部数据
GET  /fruit

# 获取id=1的数据
GET  /fruit/1

# 添加信息，请求body中必须包含fruit的属性数据，json-server自动保存。
POST /fruit

# 修改信息，请求body中必须包含fruit的属性数据
PUT    /fruit/1
PATCH  /fruit/1

# 删除信息
DELETE /fruit/1
```

除此之外，为满足模拟数据集db.json，访问路径，与服务端api路径保持一致，例如：

```
{
  "/api/fruit": [
    {
      "id": 1,
      "name": "苹果"
    }
  ]
}
```

以上默认路径则会报错，默认无法支持路径中带'/'。所以，为了支持此种情形，需自定义路由：

##### 2.2.3 自定义路由

创建自定义路由文件routes.json

```
{
  "/api/*": "/$1",    //   /api/course   <==>  /course
  "/:resource/:id/show": "/:resource/:id",
  "/posts/:category": "/posts?category=:category",
  "/articles\\?id=:id": "/posts/:id"
}
```

修改打包命令：

```
json-server --watch --port 8088 db.json --routes routes.json
```

修改数据集：

```
{
  "fruit": [
    {
      "id": 1,
      "name": "苹果"
    }
  ]
}
```

运行结果：

![result](..\assets\result.png)

访问地址：http://localhost:8011/api/fruit/1 等价于 http://localhost:8011/fruit/1 .依照上图Other routes!!!

##### 2.2.4 发布接口服务

修改打包命令

```
json-server --watch --port 8011 db.json --routes routes.json -H 192.168.11.165
```

访问地址：http:/192.168.11.165:8011/api/fruit/1 

### 写在最后的话

更多内容请访问官网，同时关于Json-server部分内容参考自：（https://www.cnblogs.com/fly_dragon/p/9150732.html）！！！