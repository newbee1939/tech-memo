# 日本語バージョン

- https://twitter.com/newbee1939/status/1680942624600117250

連休最終日、React Server Components および Next.js の App Router(App Directory)について、主に以下の記事を使って学んだので、自分なりに理解したことを連投でまとめてみます。
・https://zenn.dev/suzu_4/articles/2e6dbb25c12ee5
・https://zenn.dev/uhyo/articles/react-server-components-multi-stage
・https://zenn.dev/g4rds/articles/287c53498d17a1
・https://eh-career.com/engineerhub/entry/2023/07/14/093000

そもそも、Next.js には SSR（Server Side Rendering）という、React を Node.js 上で動作させる仕組みがあります。SSR を使用することで、初期レンダリング時にサーバー側で HTML を生成してレンダリングすることができます。これにより、コンテンツがしっかりと詰まった状態の初期画面が表示されるため、SEO の面でも効果があるとされています。

しかし、SSR には「ハイドレーション」という仕組みが存在します。これは、SSR で HTML 等をレンダリングした後に、フロントエンドでイベントリスナを登録したり、インタラクティブな動作を追加する機能です。

これは SSR を実現する上でとても重要な機能です。

しかし、ハイドレーションを実行するためには、フロント側にコンポーネント（アプリ）全体のデータを送信する必要があり、その結果 bundle サイズが増加してしまいます。bundle サイズの増加はパフォーマンスの悪化に直結します。

そこで生まれたのが React Server Components とその仕組みを組み込んだ Next.js の App Router（App Directory）です。

React Server Components は React のコンポーネントをサーバー側で実行する仕組みです。React アプリケーションをサーバー側で処理する部分とクライアント側で処理する部分に分け、サーバー側で実行可能なコンポーネントをサーバー側で実行することで、クライアント側のコードが減る（クライアントに送る JavaScript の bundle サイズが減る）ため、パフォーマンスの改善が期待できます。

そして、Next.js の App Router は React Server Components を組み込んだ仕組みです。

App Router では、デフォルトで RSC（React Server Components）が適用されます。つまり、作成したコンポーネントがサーバー側で実行されるということです。クライアント側で実行させるには、`use client` を定義する必要があります。

できるだけサーバー側に処理を寄せることで、パフォーマンスの改善を図るという意図が読み取れます。

React Server Components と SSR は同じ仕組みでも相反する仕組みでもなく、それぞれ異なる責任領域を持つ仕組みです。

これらを組み合わせることで、SSR によって初期表示を速くしつつ、React Server Components によって（今まではクライアント単体だった）コンポーネントをクライアントとサーバー用に分割し、できるだけサーバー側で処理させることで、SSR のハイドレーション時の bundle サイズを最小限に抑えることができます。

React Server Components と SSR を組み合わせることで、SEO の最適化を図る一方で、パフォーマンスの面でも最適化を図ることが可能です。

# 英語バージョン

- https://twitter.com/newbee1970/status/1680944831055695873

Today I learned about React Server Components and Next.js' App Router (App Directory), so I will summarize my own understanding in a series of posts.

To begin with, Next.js has SSR (Server Side Rendering), a mechanism that allows React to run on Node.js. SSR allows HTML to be generated and rendered on the server side during initial rendering. This is said to be effective in terms of SEO, as the initial screen is displayed with well-packed content.

However, SSR has a mechanism called "hydration". This is a function that registers event listeners and adds interactive behavior on the front end after rendering HTML and other content in SSR.

This is a very important feature to realize SSR.

However, in order to perform hydration, it is necessary to send the entire component (app) data to the front side, resulting in an increase in bundle size, which directly leads to performance degradation.

This is where React Server Components and Next.js' App Router (App Directory), which incorporates the React Server Components mechanism, come in.

React Server Components is a mechanism that executes React components on the server side by dividing the React application into two parts, one to be processed on the server side and the other on the client side, and executing the components that can be executed on the server side on the server side, This reduces the amount of client-side code (i.e., the size of the JavaScript bundle sent to the client), which can be expected to improve performance.

Next.js' App Router is a mechanism that incorporates React Server Components.

The App Router applies RSC (React Server Components) by default. This means that the components you create will be executed on the server side. To run them on the client side, you need to define `use client`.

The intention is to improve performance by putting as much processing as possible on the server side.

React Server Components and SSR are not the same mechanism, nor do they conflict with each other.

By combining them, SSR speeds up the initial display, while React Server Components split components (which used to be client-only) into client and server components and process them on the server side as much as possible, thereby reducing SSR's This minimizes bundle size during SSR hydration.

The combination of React Server Components and SSR allows for SEO optimization while also optimizing for performance.
