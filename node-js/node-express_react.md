# Node + Express 서버와 React 연동하기

## Node + Express 세팅하기

1. 디렉토리 생성 후 npm init
   > /package.json  
   > "main": "server.js",
2. express 설치
   > npm install express
3. 최상위 루트에 server.js 파일 생성 후 다음 코드 작성

```js
const express = require('express');
const path = require('path');
const app = express();

const http = require('http').createServer(app);
http.listen(8080, function () {
  console.log('listening on 8080');
});
```

### HTML 불러오기

일반적으로 html, 이미지 등의 파일은 최상윌 루트에 `public` 폴더를 생성해 보관한다.

```js
app.use(express.static(path.join(__dirname, 'public')));
app.get('/', (req, res) => {
  res.sendFile(path.join(__dirname, 'public/index.html'));
});
```

public 폴더에 접근하기 위해 `express` 의 미들웨어인 `express.static` 을 사용해 정적 파일을 제공한다.  
위처럼 작성하면 public 디렉토리에 포함된 파일을 로드할 수 있다.

> ex) http://localhost:8080/index.html

## 리액트 프로젝트 생성 후 build

server.js 가 있는 최상위 루트에 서브 폴더로 리액트 프로젝트를 생성 후 작업한다.

```
yarn create react-app react-project
```

개발이 완료되면 터미널에서 아래 명령어 입력하면 build 파일이 생성되고 서버에 보낼 준비가 완료된 것이다.  
리액트로 개발을 마친 후 build를 하면 build 라는 폴더가 생기고 html, css, js 등 파일이 생성되는데,  
리액트 프로젝트는 SPA 이기때문에 그 중 html 파일은 아래 구조의 index.html 파일 하나이다.

```
npm run build
```

## Node + Express 서버와 연동하기

```js
app.use(express.static(path.join(__dirname, 'react-project/build')));
app.get('/', function (요청, 응답) {
  응답.sendFile(path.join(__dirname, '/react-project/build/index.html'));
});
```

`express` 의 미들웨어인 `express.static` 을 사용해 정적 파일을 제공한다. 이처럼 작성하면 build 폴더 안의 css, js, asset 파일들도 문제없이 사용할 수 있다. 이제 설정한 서버(본 예제에서는 localhost:8080)으로 접속하면 리액트 프로젝트가 성공적으로 로드되었을 것이다.

### 메인페이지가 아닌 서브 디렉토리에 리액트 앱을 발행하려면?

```js
app.use('/', express.static(path.join(__dirname, 'public')));
app.use('/react', express.static(path.join(__dirname, 'react-project/build')));

app.get('/', (req, res) => {
  res.sendFile(path.join(__dirname, '/index.html'));
});
app.get('/react', (req, res) => {
  res.sendFile(path.join(__dirname, 'react-project/build/index.html'));
});
```

메인 페이지가 아닌 원하는 경로에 접속할 때 리액트 앱을 보여주고 싶다면 `express.static`를 각각 설정해주면 된다.
단, 이대로 서버에 접속하면 리액트 프로젝트가 로드되지 않을것이다. 성공적으로 로드하기 위해선 리액트 프로젝트에서 한가지만 설정해주면 된다.

```json
/ react-project 내의 package.json

{
  "homepage": "/react",
  "version": "0.1.0",
  ...
}
```

리액트 프로젝트 내의 package.json 에서 `homepage`라는 항목을 원하는 서브 디렉토리명으로 기입해주면 성공적으로 서버에 로드된 것을 확인할 수 있을 것이다.

### 만약 리액트 라우터를 사용했다면?

리액트에서도 react-router-dom을 설치하면 서버가 하던 라우팅을 대신해줄 수 있다. 그렇다면 node.js 서버에서 라우팅이 필요없어질텐데, 리액트 라우팅으로 개발 후 실제 서버에서 확인하면 아무것도 로드되지 않는다.

이유는, 브러우저 URL 창에 입력한 주소는 서버에 요청하는 것이지 리액트 라우터에게 라우팅을 요청하는 것이 아니기 때문이다. 리액트가 라우팅할 수 있도록 권한을 넘기고 싶다면 다음과 같은 코드를 추가한다.

```js
app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname, '/react-project/build/index.html'));
});
```

여기서 `*` 별표 표시는 모든 문자라는 뜻이다.
