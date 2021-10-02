# [node-exrpess] method-override

> method-override 는 Express 에서 PUT 과 DELETE를 메소드를 사용할 수 있도록 해주는 express 미들웨어다.

RestAPI에는 변경된 정보를 수정하는 PUT, PATCH, DELETE가 있지만 HTML에서는 PUT/DELETE를 처리하지 않기 때문에 사용할 수 없다. 이럴 때 사용할 수 있는 express 미들웨어인 **method-override** 가 있다.

쉽게 말해 `<form>`, `<a href="">` 를 사용할 때 메소드 속성에는 POST/GET인 생성, 읽기 작업만 가능하기 때문에 PUT(수정), DELETE(삭제) 작업을 할 수 있도록 매핑시켜주는 도구이다.

## install

```
npm install method-override
yarn add method-override
```

## How to Use

### JS

```js
// server.js
const express = require('express');
const app = express();

const methodOverride = require('method-override');
app.use(methodOverride('_method'));
```

## Example

간단히 `<form>` 을 예로 들어 이처럼 작성할 수 있다.

### HTML

```html
<form action="/path?_method=PUT" method="POST">
  <label>name</label>
  <input type="text" name="name" />
</form>
```

기존 HTML에서 `action="/path"`라고 적었다면 **method-override** 를 사용하여 PUT 처리를 할 땐 이와 같이 `action="/path?_method="PUT"`로 작성한다. 이렇게 작성하게 되면 해당 경로(`/path`)로 **PUT** 요청이 가능해진다. 단, method 속성으로는 그대로 POST/GET 을 전달해야한다.

이제 PUT 요청을 하는 코드를 작성하면 이와 같다.

### JS

```js
/* server.js */
app.put('/path', function (req, res) {
  // edit code ...
});
```
