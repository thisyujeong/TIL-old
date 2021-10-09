# [node-express] router

실제 사이트를 개발할 때 `app.get()`, `app.post()` 와 같은 코드가 수십, 수백개가 될 수 있다. 이런 라우트 코드들을 다른 파일에서 관리해 복잡했던 코드를 깔끔하게 사용할 수 있는데, **express**를 사용하는 이유중 하나가 바로 라우팅을 깔끔하게 관리할 수 있다는 것이다.

## router 객체로 라우팅 분리

일반적으로 router 파일들은 `/routes` 디렉토리에서 관리한다.
해당 예제에서는 전체 서버를 관리하는 `/server.js` 파일과 shop 와 관련된 router를 관리하는 `/routes/shop.js` 파일로 분리하여 관리했다.

```js
/routes/user.js

const router = require('express').Router();

router.get('/shop/shirts', (req, res) => {
  res.send('셔츠를 파는 페이지입니다.')
});

router.get('/shop/pants', (req, res) => {
  res.send('바지를 파는 페이지입니다.')
});

module.exports = router;
```

```js
/server.js

app.use('/', require('./routes/shop.js'));
```

router 객체는 `require('express').Router()`로 만들었다.
분리 전 기존 코드에서는 `app.get()` / `app.post()`와 같이 작성했다면 분리된 파일에서는 router 객체를 사용해 작성한다.

router에서도 app 과 같이 **use**, **get**, **post**, **put**, **patch**, **delete** 와 같은 메서드를 사용할 수 있다. 또한 미들웨어 사용이 가능하다. (미들웨어를 적용하는 방법은 다음 예제에서 설명)

처음에 **require** 문을 통해 Router 사용을 알렸다면,
마지막에는 `module.exports = router`로 라우터 모듈을 만들어 다른 자바스크립트 파일에서도 router 변수 사용이 가능하도록 한다.

그리고 server.js 에서 app.use로 해당 라우터를 연결해 훨씬 깔끔하게 관리했다.

### 더 깔끔하게 작성하기

```js
/routes/shop.js

const router = require('express').Router();

router.get('/shirts', (req, res) => {
  res.send('셔츠를 파는 페이지입니다.')
});

router.get('/pants', (req, res) => {
  res.send('바지를 파는 페이지입니다.')
});

module.exports = router;
```

```js
/server.js

app.use('/shop', require('./routes/shop.js'));
```

공통된 경로를 반복적으로 작성하지 않고 server.js 에서 경로를 `/shop`으로 작성하면 이는 `/shop/shirts`, `/shop/pants` 로 연결된다. 이렇게 작성하면 어디에 해당하는 라우터인지 좀 더 직관적인 코드가 된다.

## router에 미들웨어 적용하기

```js
/routes/shop.js

const router = require('express').Router();

// 미들웨어 생성
function middleWare() {
  ...
}

router.get('/shirts', middleWare, (req, res) => {
  res.send('셔츠를 파는 페이지입니다.');
});

router.get('/pants', middleWare, (req, res) => {
  res.send('바지를 파는 페이지입니다.');
});

module.exports = router;
```

app 에서 사용하듯 경로와 콜백함수 사이에 미들웨어를 전달할 수 있다. 미들웨어가 먼저 실행된 후 콜백함수가 실행된다. 해당 페이지에 접속하기 위해 로그인 여부를 검사하는 미들웨어를 넣을수도 있을 것이다.

### 그런데 모든 라우터에서 사용하고 싶다면?

```js
/routes/shop.js

const router = require('express').Router();

// 미들웨어 생성
function middleWare() {
  ...
}

router.use(middleWare);

router.get('/shirts', (req, res) => {
  res.send('셔츠를 파는 페이지입니다.');
});

router.get('/pants', (req, res) => {
  res.send('바지를 파는 페이지입니다.');
});

module.exports = router;
```

정말 간단하게 `router.use` 를 사용해 해당 파일 내 전역 미들웨어를 사용하면 된다.

### 특정 라우터에만 사용하고 싶다면?

또는 이와 같이 작성하면 특정 url 에만 적용하는 미들웨어로 사용할 수 있다. 아래 코드는 `/shirts` 경로에만 적용된 코드이다.

```js
router.use('/shirts', middleWare);
```
