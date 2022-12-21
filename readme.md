# node.js / Expressのメモ

Express学習のメモをざっくりとここに記述する。
基本はprogateのnode.js演習の内容をメモ。

## ルーティング基本形

```js
// まず、expressを使えるようにする。
const express = require('express');
const app = express();

// publicファイルを使えるようにする。
app.use(express.static('public'));

// 第１引数に、url。reqはリクエスト、resはレスポンス。
app.get('/', (req, res) => {
    // viewファイルの中身が見れるようになる。
    res.render('hello.ejs');
});

// トップ画面を表示するルーティングを作成してください
app.get('/top', (req, res) => {
  res.render('top.ejs');
});

// サーバー起動
app.listen(3000);
```

## ejs

`$ npm install ejs`

```ejs
<% %>は、はjsを利用したい場合。
<% const item = {id: 4, name: 'とまと'} %>


出力したい場合は、＝つける。
<%= %>
```

viewの中ではこう言う使い方も可能

```js
<%
const items = [
    {id: 1, name: 'じゃがいも'},
    {id: 2, name: 'にんじん'},
    {id: 3, name: 'たまねぎ'}
];
%>

<ul class="table-body">
    <% items.forEach((item) => { %>
    <li>
    <span class="id-column"><%= item.id %></span>
    <span class="name-column"><%= item.name %></span>
    </li>
    <% }) %>
</ul>
```

## DB接続

`$ npm install mysql`

app.jsでmysqlを呼び出す

```js
const mysql = require('mysql');

// 接続情報をcreateConnection()に与える。
const connection = mysql.createConnection({
  host: 'localhost',
  user: 'progate',
  password: 'password',
  database: 'list_app'
});
```

DBから取得して出力する

```js
app.get('/index', (req, res) => {
  // データベースからデータを取得
  connection.query(
    // 第１引数に、SQL文字列
    'SELECT * FROM items',
    // 第２引数に、callback。error, resultsを引数に入れてあげる。
    (error, results) => {
      console.log(results);
      // res.renderの第２引数にオブジェクトで、viewに渡す。viewでは、itemsのオブジェクトを使えるようになる。
      res.render('index.ejs', {items: results});
    }
  )
```


## formを利用する。

送信側

```html
<form action="/create" method="post">
    <input  type="text" name="itemName">
    <input type="submit" value="作成する">
</form>
```


`app.js`に以下追加

```js
// 基本は定型分
app.use(express.urlencoded({extended: false}));


app.post('/create', (req, res) => {
  // フォームから送信された値を出力してください
  console.log(req.body.itemName)
}


app.post('/create', (req, res) => {
  connection.query(
    // bindバリューを利用する。
    'INSERT INTO items (name) VALUES (?)',
    // フォームから送信された値はreq.body.{NAME}で受け取ることができる。
    [req.body.itemName],

    // 次の処理を書く(リダイレクトさせる。)
    (error, results) => {
      res.redirect('/index')
    }
  )
});
  
```


