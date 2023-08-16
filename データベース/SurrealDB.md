# 概要

- Surreal(現実離れした)DB
- 最近登場した Rust 製のデータベース
- とにかくなんでもできる夢のデータベース
  - 「ぼくのかんがえたさいきょうのデータベース」みたいなイメージ？

# 特徴

- リレーショナル、ドキュメント、グラフ、あらゆる種類のデータ構造を扱うことができ、かつインメモリ、単一ノード、分散環境など様々な環境で動かすことができる
- HTTP や WebSocket によるアクセスと柔軟なユーザ認証、認可機能とが DB 本体に内包されている
  - 将来的には GraphQL でも扱えるようになるらしい
- ブラウザから直に接続する WebDB としても使える
- SurrealQL という組み込み関数が豊富な SQL に似た言語を採用しており、比較的簡易に抽出が行える
  - 制約付けも可能
- 複雑なリレーションを JOIN を使うのではなく、Record Links や Graph Connections で達成できる
  - 多対多関係や複数のリレーションを使った深い関係クエリでも SQL より短く簡潔に記述できる
- JS の独自のロジックを埋め込んで柔軟にクエリすることもできる

# デプロイについて

- 参考
  - https://surrealdb.com/docs/deployment
- GCP ではまだ使えない
  - https://surrealdb.com/docs/deployment/google

# 実際に触ってみた

## インストール

Docker で実行する場合は以下。

```
$ docker run --rm --pull always -p 8000:8000 surrealdb/surrealdb:latest start --user root --pass root
```

参考: https://surrealdb.com/docs/installation/running/docker

## バージョンチェック(動作確認)

とりあえずバージョンチェック。（動いていることを確認）

`curl http://localhost:8000/version`

## レコード追加

account テーブルの一つのテーブルと一つのレコードを作成。

```
curl POST -H "Accept: application/json" -H "NS: test_namespace" -H "DB: test_db" -u "root:root" -d "
CREATE account
SET
  name = 'hide',
  created_at = time::now()
;
" http://localhost:8000/sql | jq
```

返答を見てわかるように、レコードには id（上記では account:ghllyncxi8dqe11ykv4w）が振られる。ID にはテーブル名が含まれることが特徴で、`table 名:任意の値`の形式になっている。SurrealDB ではこの ID を使って直接レコードにアクセスできる。ちなみに ID は以下のように明示的に指定することもできる。

```
curl POST -H "Accept: application/json" -H "NS: test_namespace" -H "DB: test_db" -u "root:root" -d "
CREATE account:hide
SET
  name = 'hide',
  created_at = time::now()
;
" http://localhost:8000/sql | jq
```

次に author テーブルとテーブルに含まれるレコードを作成する。

```
curl POST -H "Accept: application/json" -H "NS: test_namespace" -H "DB: test_db" -u "root:root" -d "
CREATE author:taro
SET
  name.first = 'Taro',
	name.last = 'Yamada',
	name.full = string::join(' ', name.first, name.last),
	age = 29,
	admin = true
;
" http://localhost:8000/sql | jq
```

ここまでで account と author の二つのテーブルを作成できてる。

ブログの記事を作ってみる。先ほど作った著者とアカウントを記事に関連させる。
下記の例では著者のテーブル名を含んだレコード ID を直接指定（レコードリンク） している。アカウントのレコードリンクはサブクエリを使って指定している。

```
curl POST -H "Accept: application/json" -H "NS: test_namespace" -H "DB: test_db" -u "root:root" -d "
CREATE article
SET
	author = author:taro,
	title = 'マネーの本',
	text = 'マネーの本ですよ',
	account = (SELECT id FROM account WHERE name = 'hide' LIMIT 1),
	created_at = time::now()
;
" http://localhost:8000/sql | jq
```

## レコード取得

article を取得する。

```
curl GET -H "Accept: application/json" -H "NS: test_namespace" -H "DB: test_db" -u "root:root" -d "
SELECT * FROM article;
" http://localhost:8000/sql | jq
```

SurrealDB では複数のテーブルから同時にレコードを取得することも可能。

```
curl GET -H "Accept: application/json" -H "NS: test_namespace" -H "DB: test_db" -u "root:root" -d "
SELECT * FROM article, author, account;
" http://localhost:8000/sql | jq
```

## グラフ

SurrealQL の特徴の一つにリレーションを辿るのが非常に簡単という点がある。

SurrealDB では、JOIN を使用することなく関連するレコードを効率的に習得することができる。

RELATE 文を使うことで、レコード同士に graph edges を追加できる。これにより、データをより効率的に arrow を使ってクエリできる。

参考: https://surrealdb.com/docs/surrealql/statements/relate

メールを表すレコードを追加し、そのメールが著者に送られたことを表現する辺を追加する。

```
curl POST -H "Accept: application/json" -H "NS: test_namespace" -H "DB: test_db" -u "root:root" -d "
CREATE email:hoge
SET
  subject = 'こんにちは',
  text = '本についてです'
;
RELATE email:hoge->to->author:taro
SET
  opened = false
;" http://localhost:8000/sql | jq
```

さらに別の著者を追加し、このメールの送信者であることを表現する。
レコード追加には CONTENT を使った別の書き方をしている。（SET でもいいけど CONTENT でも書ける）

```
curl POST -H "Accept: application/json" -H "NS: test_namespace" -H "DB: test_db" -u "root:root" -d "
CREATE author:jiro
CONTENT {
  name: { first: 'Jiro', last: 'Maeda' }
};
RELATE author:jiro->send->email:hoge
CONTENT {
  time: '2022-10-18T07:31:49Z',
};
" http://localhost:8000/sql | jq
```

まだ Jiro からのメールを開いていない受信者でかつ管理者を全て取得する。

```
curl POST -H "Accept: application/json" -H "NS: test_namespace" -H "DB: test_db" -u "root:root" -d "
SELECT ->send->(email as email)->(to WHERE opened = false)->(author WHERE admin = true as receiver)
FROM author:jiro FETCH email, receiver;
" http://localhost:8000/sql | jq
```

# 感想

参考: https://goodpatch-tech.hatenablog.com/entry/2022/12/20/122116

Cloud Firestore を超えてくる可能性がありそう。

私は以前 Cloud Firestore は最高だぜ！という記事を書いていました。

- ユーザーが frontend から直接アクセスできる
- データベース側で認証認可を適切に設定できる
- リアルタイムでデータが同期できる

という３拍子揃った DB はなかなかありません。

Google が提供している Cloud Firestore は Security Rules で細かい認可の設定が可能で、リアルタイムでのデータ同期もサーバー側のコードや設定なしで行えます。
また、Identity Platform などの認証システムや、他 PaaS とも連携が良く、開発・運用工数の削減効果は目覚ましいものがあります。

しかしながら、デメリットが全くないわけではありません。 たとえば、

- クラウド上に構築するしかなく、GCP 以外の選択肢がない
- Cloud Firestore の利用制限を超えてしまう可能性がある
- Security Rules では制御しきれないケースがある

などの懸念は抱えています。

そんな中でてきた SurrealDB は上記で挙げた利点を持ちながら欠点を相殺できる可能性を秘めています。

- データベース側で認証の仕組みを持っている
- WebSocket を利用した同期の仕組みも備わっている
- 自分でスケールアップ可能である
- ほとんどのケースで µs（マイクロ秒）で処理が終わる
  - Rust は速い
- スキーマ定義の柔軟性が高い
