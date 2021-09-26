# MongoDB

MongoDB Atlas 접속 : [MongoDB Atlas](https://www.mongodb.com/cloud/atlas/lp/try2?utm_content=rlsavisitor&utm_source=google&utm_campaign=gs_apac_rlsamulti_search_core_brand_atlas_desktop_rlsa&utm_term=mongodb%20atlas&utm_medium=cpc_paid_search&utm_ad=e&utm_ad_campaign_id=14412646476&gclid=Cj0KCQjwtMCKBhDAARIsAG-2Eu9HzTx9-UPpf_y3NBye5OhBkVwT7pLKya1WNQuDPIPtQO848-XzZbsaAouCEALw_wcB)

1. Cluster 생성
2. Database Access 에서 DB 접속용 아이디/비밀번호를 생성한다.
3. Network Access 에서 IP 추가
4. Database / collection 생성 (Browser Collections)  
   _Add My Own Data_ 클릭하여 원하는 DB 이름을 설정하면 설정한 이름의 데이터베이스가 생성된다.
5. 생성된 DB를 연결하기 위해 Clusters에서 connect(연결)하기 / **connect your application**  
   사용할 언어와 버전 선택 후 DB 연결을 위한 connection code 복사 후 사용

## 생성된 DB 연결하기

### MongoDB Install

MongoDB 라이브러리 설치

```
npm install mongodb
yarn add mongodb
```

### How to Use

```js
const MongoClient = require('mongodb').MongoClient;
MongoClient.connect('connection code', function (error, client) {
  // database 접속이 완료되면 실행될 코드
  if (error) return console.log(error); // 에러 발생 시 실행될 코드
});
```

`connect 함수`의 첫번째 파라미터로 Database connection code를 입력하여 연결한다.

ex / connection code

> mongodb+srv://_[username]_:_[password]_@cluster0.syrv2.mongodb.net/myFirstDatabase?retryWrites=true&w=majority

DataBase Access 에서 새로운 User를 생성할 때 입력한 아이디를 [username]에 입력하고, 비밀번호는 [password]에 입력한다.

## 데이터베이스 접근 및 자료 저장

어떤 DB에 데이터를 저장할 것인지 명시해주어야 하기 때문에 연결할 DB를 담을 수 있는 변수 생성이 필요하다.
DB 를 생성할 때 설정했던 Database name을 이와같이 연결하면 해당 db에 접근이 가능하다.

```js
const MongoClient = require('mongodb').MongoClient;

let db;
MongoClient.connect('connection code', function (error, client) {
  if (error) return console.log(error);
  db = client.db('database name');

  db.collection('collection name').insertOne(
    // 데이터 저장
    { name: 'John', age: 20 },
    function (error, result) {
      console.log('save comopleted');
    }
  );
});
```

위 예제는 연결한 db의 collection에 접근한 모습이다. `inserOne`은 db에 자료를 저장할 때 쓰이는 함수로 첫번째 파라미터로 **저장할 데이터**를 전달하고 두번째 파라미터로 콜백함수를 전달한다. 위 예시에서는 이름과 나이를 데이터에 저장한 모습이다. 저장이 완료되면 콜백함수를 전달한다.

### 위 예제의 데이터가 Mongo DB에 저장된 모습

```js
{
  _id: ObjectId('~');
  name: 'John';
  age: 20;
}
```

이처럼 DB에 자료를 저장하고 나면 MongoDB에서 해당 db의 collection에 저장된 데이터를 실시간으로 확인할 수 있다. 그런데 저장된 데이터에서 `_id`라는 자료가 저장되어있는 것을 확인할 수 있는데, 이는 데이터를 저장할 때 직접 `_id`를 설정하지 않으면 임시의 `_id`를 강제로 생성하기 때문에 데이터를 저장할때 `_id`를 함께 저장하는 것이 좋다.

## DB 데이터 불러오기

원하는 db의 컬렉션에서 `find` 함수를 사용하면 collection 안의 모든 데이터를 불러올 수 있다.

```js
db.collection('collection name').find();
```

주로, 이와같이 `toArray` 함수를 사용해 배열 형태로 데이터를 불러온다.

```js
db.collection('collection name')
  .find()
  .toArray((err, result) => {
    if (err) return console.log(err);
    console.log(result);
  });
```
