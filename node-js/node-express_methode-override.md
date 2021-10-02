# [node-exrpess] method-override

> method-override 는 Express 에서 PUT 과 DELETE를 메소드를 사용할 수 있도록 해주는 express 미들웨어다.

RestAPI에는 변경된 정보를 수정하는 PUT, PATCH, DELETE가 있지만 GET/POST 만 지원는 PUT/DELETE를 처리할 수 없다. 이럴 때 사용할 수 있는 **method-override** 미들웨어이다.

쉽게 말해 `form`, `a` 를 사용할 때 메소드 속성에는 POST/GET인 생성, 읽기 작업만 가능하기 때문에 PUT(수정), DELETE(삭제) 작업을 할 수 있도록 매핑시켜주는 도구이다.

## install

```
npm install method-override
yarn add method-override
```

## How to Use

```js
const express = require('express');
const app = express();

const methodOverride = require('method-override');
app.use(methodOverride('_method'));
```
