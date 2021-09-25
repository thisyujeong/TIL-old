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

## Basic Get request

서버의 원하는 경로로 `get`요청 처리하기

```js
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
