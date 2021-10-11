# [node-react] concurrently 라이브러리

> concurrently는 서버와 클리이언트를 동시에 실행할 수 있는 라이브러리이다.

### install

```
npm install concurrently --save
yarn add concurrently --save
```

### Usage

```json
"script":{
  "start": "concurrently \"command2 arg\" \" command2 arg \""
}
```

concurrently 를 사용하고싶다면 위 코드와 같이 앞에 `concurrently` 를 명시해주고 `\" \"` 안에 차례로 실행할 명령어를 작성해주면 된다.

### Example

```json
"script": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "start": "server.js",
  "backend": "nodemon server.js",
  "dev": "concurrently \"npm run backend\" \"npm run start --prefix client\""
}
```

root 경로의 명령어를 실행하는 것이 아닌 서브 폴더의 명령어를 실행하고 싶다면 `명령어 --prefix 폴더명` 을 입력해주면 된다. 위 예시 코드와 같이 작성했다면 `npm run dev` 명령어로 실행시켜 server와 client 가 동시에 실행된 것을 확인할 수 있다.
