---
title: API接口文档设计
categories: 学习
---

### 功能说明

简单说明接口实现的相关功能，使用场景等等。

### 调用说明

#### 请求地址

```
http://www.xxx.com/api/demo
```

#### 请求方式

```
GET/POST
```

#### 请求头信息

一些需要设置请求头的需要加上特殊说明。
例如:
```
Content-Type：application/json
```

#### 请求参数

GET请求方式的列表方式列出请求所需参数，包含说明是否为必传字段。

|参数|类型|说明|必传|
|---|---|---|---|
|userId|int|用户ID|Y|
|timestamp|long|时间戳(毫秒值)|N|

POST方式需要传json参数的列出json样例格式

```js
{
	"userId":123,	//用户ID，必传
	"timestamp":3887717000	//时间戳(毫秒值)，非必传
}
```

### 返回

一般接口返回JSON或XML格式的数据，需列出相关返回格式样例数据。

```js
{
	"status": 0,
	"data":{
		"userId":123,	//用户ID
		"userName":"张三"	//用户名
	},
	"msg": "success"
}
```

```xml
<Result>
	<status>0</status>
	<msg>success</msg>
	<data>
		<userId value="123"/>
		<userName value="张三"/>
	</data>
</Result>
```
#### 返回状态

必要时需要列出所有返回状态对应的数据格式。

|状态值|状态说明|
|---|---|
|0|成功|
|500|系统异常|
|404|请求不存在|