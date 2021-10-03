# node.js - Passport를 이용한 인증관리

> Passport.js는 인증 요청을 처리해주는 Node.js의 인증 미들웨어이다.

기본적인 로그인 방법은 username과 password를 일치하여 확인하는 방식이지만, 최근
SNS증가로 Facebook과 Twitter 등에서 **OAuth**를 제공함에 따라 SNS들의 계정을 통해 서비스를 가입하고 인증하는 방법이 생겼다.

> > Outh는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들이 정보를 접근 권한으로 부여할 수 있는 개방형 표준이다.

## session 방식 로그인 기능 구현하기

### Install

```
npm install passport passport-local express-session
```

### How to Use

```js
const passport = require('passport');
const LocalStrategy = require('passport-local').Strategy;
const session = require('express-session');

app.use(session({ secret: 'secret code', resave: true, saveUninitialized: false }));
app.use(passport.initialize());
app.use(passport.session());
```

### 1. 로그인 페이지 불러오기

```js
app.get('/login', (req, res) => {
  res.render('login.ejs');
  // res.sendFile('/login.html');
});
```

### 2. POST 요청하기

```js
app.post('/login', passport.authenticate('local' {
  failureRedirect: '/fail',
}), (req, res) => {
  // 로그인 후 실행할 코드
  res.redirect('/');
})
```

여기서 `passport`는 로그인 기능을 쉽게 구현할 수 있도록 도와주는데,
post 요청의 두번째 파라미터로 `passport.authenticate()`를 전달하면 로그인 시 아이디와 비밀번호를 검사한다. (인증해주세요~)

`authenticate()`를 local strategy(인증하는 방법을 Strategy 라고 칭한다)로 호출한다. 두번째 파라미터로 setting을 할 수 있다. `failureRedirect` 는 회원 인증에 해당 경로로 이동하며,
회원 인증에 성공할 시 `res.redirect`로 경로 이동을 설정했다.

### 3. 아이디 비밀번호를 인증하는 세부 코드

```js
passport.use(
  new LocalStrategy(
    {
      usernameField: 'id', // form 의 name="id"
      passwordField: 'pw', // form 의 name="pw"
      session: true, // 로그인 후 세션정보 저장 여부
      passReqToCallback: false, // 아이디, 패스워드 외의 다른 정보 검증시
    },
    function (입력한아이디, 입력한비번, done) {
      // passReqToCallback: true 일때 function (req, 입력한아이디, 입력한비번, done)
      db.collection('login').findOne({ id: 입력한아이디 }, (err, result) => {
        if (err) return done(err); // 에러

        // DB에 입력한 아이디가 없을 때
        if (!result) return done(null, false, { message: '존재하지 않는 아이디입니다.' });
        // DB에 입력한 아이디가 있을 때
        if (입력한비번 == result.pw) {
          // 입력한 비번과 result.pw를 비교
          return done(null, result);
        } else {
          // 입력한 비번과 result.pw가 틀리면
          return done(null, false, { message: '비밀번호가 일치하지않습니다.' });
        }
      });
    }
  )
);
```

위 세부 코드들은 로그인을 할 때만 실행되는 코드이다.  
정확히 말하면 위 **2. POST 요청 예제**에서 아래 부분 때문에 동작하는 코드이다.

```js
passport.authenticate('local' {
  failureRedirect: '/fail',
}
```

`LocalStrategy( { 설정 }, function(){ 아이디비번 검사하는 코드 } )`

- usernameField, passwordField : 사용자가 제출한 아이디가 어떤 input 인지 name 속성 값 작성 :
  > usernameField, passwordField 는 해당 input의 name 속성 값과 동일해야한다. 이 값으로 데이터베이스 값과 비료해 인증절차를 진행하는데, 만약 인증에 실패한 경우 done(False, null), 성공한 경우 done(null, 유저정보객체) `serializeUser`를 넘기게된다.
- session : 로그인 후 세션 정보를 저장할 것인지 여부, true 이면 나중에 재 로그인하지 않아도 됨.
- passReqToCallback : 사용자의 아이디/비밀번호가 아닌 다른 정보를 검사해야할 경우 true를 전달하는데, 이 경우 `LocalStrategy`의 두번째 파라미터인 콜백함수의 첫번재 파라미터로 기타 정보를 전달할 수 있다. 얘를 들어 아래와 같이 작성할 수 있다.
  ```js
  passport.use(new LocalStrategy({
    ...
  }, function (req, 입력한아이디, 입력한비번, done) {
    console.log(req.body);
    ...
  }))
  ```

`done()`은 세개의 파라미터를 가질 수 있다. 첫번째 파라미터는 서버에러, 두번째 파라미터는 인증 성공 시 사용자 DB데이터(result)를 보낸다. 하지만 아이디/비번이 일치하지 않으면 해당 파리머트에는 false를 전달한다.

하지만 여기서 문제점이 있는데, 입력한 비밀번호를 암호화 작업없이 DB와 비교하기 때문에 보안이 좋지 않다.

### session 세션 등록

로그인(인증)에 성공하면 세션(로그인을 했었는지)을 만들어주어야한다.
