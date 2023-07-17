# URL

- https://zenn.dev/g4rds/articles/287c53498d17a1

# React Server Components とは

- React Server Components は、React コンポーネントをサーバーサイドでレンダーする新しい技術です。
- 一部のコンポーネントをサーバーサイドでレンダーしてしまうことで、アプリケーションのパフォーマンスを上げることを目的とします。

# Server / Shared / Client Components

- React Server Components は、コンポーネントを三種類に分類します。
- Server Components
  - Server Components はサーバーでのみレンダーされるコンポーネント
  - 拡張子を .server.js とすると Server Component として解釈される
  - Server Components は以下の制約を持つ
    - ステートを持てないため、useState や useReducer は使えない
  - Server Component は、Client Component を子コンポーネントに持つことができる
- Client Components
  - 従来のクライアントでのみレンダーされるコンポーネントは、Client Components と呼ばれる
  - 従来のコンポーネントですが、拡張子を .client.js とする必要がある
  - Clinet Component では Server Components を import できないという制約がある
- Shared Components
  - Shared Components は、サーバーとクライアントのどちらでもレンダリング可能なコンポーネント
  - Server Component から呼ばれた場合は Server Component として、Client Component から呼ばれた場合は Client Component として振る舞う

# 使い分け

- 基本的に、パフォーマンス有利な Server Component にすることを考える
- ステートが必要だったり、イベントをハンドルしたかったりなど、Server Component の制約に引っかかる場合は、Client Component にする
- Client Components 内でも使いたい Server Component は、Shared Component にする

# 恩恵と特徴

- React Server Components がもたらす恩恵は、パフォーマンス面が主
  - バンドルサイズの減少
    - Server Components のコードはクライアントがダウンロードするバンドルに含まれない
    - クライアントはレンダーした結果の JSX を受け取るだけで、リレンダーが必要な場合は再度サーバーにレンダーリクエストする
    - そのため、ユーザーのインタラクションに反応しないような大部分のコンポーネントを、クライアントのバンドルから取り除くことができる
  - データフェッチにかかる時間の減少
    - Server Components はデータのあるサーバー上で実行されるため、データに直接アクセスできることがあります。API を介したとしても、レイテンシーは大きく抑えられるでしょう。

# SSR との共存

- さて、ここまで React Server Components を解説してきましたが、「既にコンポーネントをサーバーでレンダーする技術はあるじゃないか」と疑問をお持ちの方もいらっしゃるかもしれません
- Next.js などで実現できる SSR は、サーバー上で HTML にレンダーして初期表示を速くする技術です。最初にクライアントに送信される文書の時点で、最初の画面が既にレンダーされているため、SEO にも効果があります
- これとは逆に、React Server Components の場合だと、最初に送信される文書の段階では何もレンダーされていません。動的に meta タグを生成することはできませんし、初期表示も速くならないでしょう。しかし、マウントするコンポーネントの数はかなり減らすことができます
- このように、React Server Components と SSR の担当領域は異なるのですが、組み合わせることができます
  - 組み合わせることで、SSR によって初期表示を速くしつつ、React Server Components によってバンドルサイズを最小限にすることができます

# レンダリングの流れ

- Next.js の React Server Components デモのレンダリングの流れ
