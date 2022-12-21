# node.js / Expressのメモ

Express学習のメモをざっくりとここに記述する。

### ルーティング基本形

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
