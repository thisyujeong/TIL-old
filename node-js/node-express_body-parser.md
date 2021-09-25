# Node - Express 미들웨어 body-parser

> 클라이언트 POST request data의 body로부터 파라미터를 편리하게 추출하는 라이브러리

body-parser는 요청의 본문을 해석해주는 node.js의 모듈, 미들웨어다.
보통 `form` 데이터나 `Ajax` 요청의 데이터를 처리한다.

### Install body-parser

```
npm install body-parser
yarn add body-parser
```

### How to Use

```js
/* express */
const express = require('express');
const app = express();

/* body parser */
const bodyParser = require('body-parser');
app.use(bodyParser.urlencoded({ extended: true }));
```

`urlencoded` 메서드의 `({ extended: true })` 옵션이 있다. 해당 옵션이 false 면 기본으로 내장된 노드의 querystring 모듈을 사용하고, true면 별도의 설치가 필요한 qs 모듈읠 사용하여 쿼리스트링을 해석한다.

### POST 요청으로 데이터를 서버에 전송하는 법

POST 요청으로 해당 경로의 데이터를 서버에 전송할 수 있다.

```js
app.post('/add', (req, rep) => {
  console.log(req.body);
});
```

POST 요청으로 서버에 데이터를 전송할 때 `form` 데이터를 경우 form HTML도 세팅이 필요하다.
form의 **action**과 **method** 속성은 데이터를 전송할 때 **꼭 필요한 속성**이다.  
`action` 은 어떤 경로로 요청할 것인지 설정하는 속성이며, `method` 속성은 GET/POST 중 어떤 요청을 할 것인지 설정하는 속성이다.
추가로 `input` 요소에 `name` 속성을 꼭 추가해주지 않으면 서버는 이 데이터가 어떤 input에서 전달된 데이터인지 알 수 없기 때문에 꼭 설정해주는 것이 좋다.

### HTML

```html
<form action="/add" method="POST">
  <div class="form-group">
    <label>오늘의 할일</label>
    <input type="text" class="form-control" name="title" />
  </div>
  <div class="form-group">
    <label>날짜</label>
    <input type="text" class="form-control" name="date" />
  </div>
  <button type="submit" class="btn btn-danger">Submit</button>
</form>
```

### JS

```js
app.post('/', function (req, res) {
  var post = req.body;
  console.log(post);
});
```

각 input에 정보를 입력하고 submit 버튼을 누르게되면 해당 데이터가 객체로 전달된 것을 터미널에서 확인할 수 있다.

```
[nodemon] restarting due to changes...
[nodemon] starting `node serve.js server.js`
listening on 8080
{ title: 'hello', date: 'hi' }
```

또는 이와 같이 `send`를 사용하여 전달해주면 서버 화면에서 확인할 수 있다.

```js
app.post('/', function (req, res) {
  res.send(req.body);
});
```
