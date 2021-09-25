# nodemon 라이브러리

> 수정사항이 있을 때마다 server를 껐다 켜는 작업없이 자동적으로 server에 반영해주는 라이브러리

```
npm install -g nodemon
yarn add global nodemon
```

### nodemon 서버 실행하기

```
nodemone server.js
```

실행 후 아래와 같이 뜨면 성공적으로 nodemon 설치 및 실행된 것이다.

```
> nodemon server.js
[nodemon] 2.0.13
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node server.js`
listening on 8080
```
