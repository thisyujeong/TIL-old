# Express

## Express Install

> 서버를 쉽게 짤 수 있도록 도와주는 라이브러리

```
npm install express
```

```
yarn add express
```

## Express setting

express 라이브러리로 서버를 띄우기 위한 기본 셋팅

```js
server.js;

const express = require('express'); // 설치한 라이브러리 불러오기
const app = express(); // express 시작하기

app.listen(8080, function () {
  console.log('listening on 8080');
});
```

### listen 함수

listen 함수는 두가지 파라미터를 전달받는다. 첫번째 파라미터는 서버를 띄울 port 번호, 두번째 파라미터는 서버를 띄운 후 실행할 콜백함수를 전달한다.

## server 띄우기

```
> node server.js
  listening on 8080
```

서버를 띄울 땐 서버 코드를 작성한 파일을 node로 실행시켜주면 된다. 두번째 파라미터로 전달한 콘솔`listening on 8080`이 출력되는것을 확인 할 수 있다.

## server 접속하기

위에서 작성한 포트 번호를 브라우저의 주소창에서 `localhost:port` 의 port 자리에 입력후 브라우저에서 연다.  
ex / `localhost:8080`

## HTTP Methods

> - GET: getting data (조회)
> - POST: creating data (생성)
> - PUT: updating data (전체 수정)
> - DELETE: deleting data (삭제)
> - PATCH: deleting data (수정)

## GET request

> 서버의 원하는 경로로 `get`요청 처리

```js
/* with body-parser
 * 파라미터 변수 뜻
 * req: request 요청
 * res: response 응답
 */
app.get('/path', function (request, response) {
  response.send('hello');
});
```

위의 서버를 실행해 `locathost:8080/path`로 접속하면 hello 문구를 확인할 수 있다.

### 서버에서 HTML 파일 전달하기

```js
app.get('/path', function (request, response) {
  response.sendFile(__dirname + '/index.html');
});
```

## POST request

> POST 요청으로 해당 경로의 데이터를 서버에 전송

POST 요청으로 해당 경로의 데이터를 서버에 전송할 수 있다.  
보통 데이터를 생성할 때 사용된다. req.body에 데이터를 담아서 보낸다.

```js
app.post('/path', function (req, res) {
  var post = req.body;
});
```

## PUT request

> 데이터를 수정할 때 사용되는 Method
> 데이터와 조건을 보내면, 조건에 맞는 정보를 주어진 데이터로 수정한다.

```js
app.put('/path', function (req, res) {
  ...
}
```

## DELETE request

> 단일 데이터를 수정할 때 사용
> 보통 Query Params나 Path Variables와 Body를 혼합해서 잘 사용한다. 조건에 맞는 데이터를 삭제

```js
app.delete('/path', function (req, res) {
  ...
}
```

## + express 에서 status code 넣기

express에서 직접 응답코드를 요청할 수 있다.

### status()

파라미터로 응답코드를 전달한다. 동시에 이와 같이 `send`함수를 사용해 `cmd` 에 출력할 수 있다.

```js
app.get('/', (req, res) => {
  res.status(200).send('hello world');
});
```

또는 이와 같이 `send`를 사용해 message 직접 전달해 개발자도구의 `console`에 메세지를 출력할 수 있다.

```js
app.get('/', (req, res) => {
  res.status(200).send({ message: 'success' });
});
```

## 경로 이동하기

### res.redirect()

요청의 경로를 재설정한다.
이 예제는 `/edit` 경로에서 `<form>` 데이터를 수정 후 **`redirect`**를 사용해 경로를 재설정 한 예제이다.

```js
app.put('/edit', (req, res) => {
  db.collection('post').updateOne(
    { _id: parseInt(req.body.id) },
    { $set: { title: req.body.title, date: req.body.date } },
    (err, result) => {
      console.log('done fixing it');
      res.redirect('/list'); // reset the path
    }
  );
});
```

## app.use()

express에서 미들웨어를 사용할 때 `app.use(~)`를 작성하면 "나는 미들웨어를 사용할 것이다" 라는 의미이다.

```js
const express = require('express');
const app = express();

const bodyParser = require('body-parser');
app.use(bodyParser.urlencoded({ extended: true }));

const methodOverride = require('method-override');
app.use(methodOverride('_method'));
```

위처럼 다양한 미들웨어들을 `app.use`를 사용할 수 있다.

## Express middleware

Express 미들웨어란 쉽게말해 함수이다.Express에서는 사실 모든것들이 미들웨어라고 할 수 있다. 미들웨어 함수는 요청(req), 응답(res), 그리고 애플리케이션의 요청-응답 주기 중 그 다음의 미들웨어 함수에 대한 액세스 권한을 갖는 함수이다. 그 다음의 미들웨어 함수는 일반적으로 next라는 이름의 변수로 표시된다.
