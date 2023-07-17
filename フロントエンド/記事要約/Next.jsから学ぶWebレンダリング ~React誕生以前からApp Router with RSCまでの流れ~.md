# 参考記事

- https://zenn.dev/suzu_4/articles/2e6dbb25c12ee5
- https://zenn.dev/uhyo/articles/react-server-components-multi-stage

# React 誕生までの Web

- iPhone と Ajax がリードした Web 2.0 時代
  - Web において Ajax という技術が注目され始める 2005~
  - iPhone 登場 2007~
  - スマホアプリの登場によりソフトウェアの UX に求められる質的変化
- Web アプリのフレームワーク
  - 複雑な Web アプリをピュアな JavaScript や jQuery だけを用いて構築/運用するのは難しかった
  - そのためベストプラクティスや規約を提供するフレームワークが群雄割拠した
    - Backbone.js(2010~)や AngularJS(2009~)や Knockout.js(2010~)が代表例
- Rails, PHP 時代のポイント
  - サーバーサイドのフレームワークに結びついたテンプレートエンジンが HTML や JS を生成する
  - ユーザーが最初に目にするインターフェース=HTML を返すのはサーバーの仕事だった
  - クライアントの仕事はテンプレートエンジンに渡されたオブジェクトを用いて、HTML と CSS を書き jQuery などを用いて DOM にイベントをアタッチしていくイメージ

# React の登場

- React を一言であらわすとしたら UI = f(state)
- 状態を定義すればそれが UI になるというもの。宣言的 UI と呼ばれるパラダイムを作ることになる。
- React SPA 時代のポイント
  - テンプレートエンジンの重要度が下がった
    - UI 形成やそれに伴うブラウザ上での UX の責任はクライアントの仕事に移ってきた

# Next.js の登場と Web レンダリングの変化

- Next.js による SSR とは何であったか
  - 2016 年に登場した Next.js は React を Node.js 上で動作させるようにした。
- Next.js SSR のポイント
  - JavaScript を書くエンジニアが HTML,JS を適切な状態でユーザーに届けるための責任をすべて負うことができるようになった。
  - サーバーサイドとフロントエンドの境界が難しくなり、BFF(SSR 含む)までをフロントエンドエンジニアの仕事とする組織が増えた。
  - SSR によって適切な HTML や CSS を返せるようになったことで、真っ白な画面が表示されるみたいな現象はなくなったが、一方でハイドレーションという技術課題が発生することになった。
    - ハイドレーション、またはリハイドレーションとは
    - サーバーサイドで React が実行出来ると UI を組み立てることが出来るが、その UI は絵（HTML と CSS が当たった状態）でしかない。たとえばボタンは見えるがクリックしても何も起こらない。
    - ボタンをクリックして何か起こすには、JavaScript がブラウザで実行されインタラクティブになるまで待つ必要がある。この絵が見えてからインタラクティブになるまでの過程を Web ではハイドレーションと呼んでいる。フリーズドライのカップヌードル(SSR した HTML と JS)が、お湯で食べられる状態(ブラウザ上でインタラクティブになる)に戻っていくみたいな。

# CDN の進化と SSG

- Next.js SSG のポイント
  - React が Server で動作することで、サーバーで都度 HTML を生成するのではなく、デプロイ時などにあらかじめ HTML/JS を生成して、CDN を通し Static として配信できるようになった。
  - Next.js が登場した同時代に Fastly や Cloud Flare といった CDN(Edge Side Cloud Computing)が進化しており出来ることが広がったため、会員制サイトなどでもこの SSG が用いられるようになった。
  - 日経電子版や Dev.to や Zenn.dev などの CDN と SSG で作られたサイトが速くて界隈で話題になった。

# React Server Components と Next.js App Router は Web レンダリングで何を目指しているのか

- React Server Components とは
  - RSC の最も基本的なメンタルモデルは、「React アプリケーションの中に、サーバーで実行される部分とクライアント側で実行される部分がある」ということです。
  - 「プログラムの評価を多段階に分けて処理」2 段階の計算の場合は「stage 0 のプログラム」と「stage 1 のプログラム」があります。
    - これまでの SSR は Stage1 で React を Node.js で動かした状態。今回の RSC は Stage0 を追加した。サーバーサイドでしか動作できないため、例えば useState といった状態管理の機能は使えない。
- 要は PHP みたいなもの
  - RSC が PHP と異なる点は、2 つの段階が両方とも React で書かれていることです。つまり、PHP が「HTML+JavaScript」を出力するプログラムであったならば、RSC（のサーバー側）は「クライアント側用の React アプリケーション」を出力するプログラムと言える
- RSC のポイント
  - Stage0 によって UI の構築に関係ないコードを Stage1 から除外できる
    - バンドルサイズの削減を実現できる可能性がある
    - React チームはゼロバンドルサイズの React にも取り組んでいる
  - よく Next.js や Nuxt.js では Client と Server のコードが混ざってしまう事によるセキュリティリスクなどが言われてきたが、Server Component は Client に配信されない
    - Client と Server のコードは共通化せずに（型やバリデーションやスキーマは共通化した方が良い場合も多いが）明示的に分けた方が良いことが、コミュニティの発展とともに共通認識になってきた。
  - Server Component は useState を使えない一方、Next.js でトップレベルページでしか使えなかった getServerProps()などがどこでも使えるようになる。

# Next.js App Router とは

- App Router は React Server Component (RSC) をふんだんに用いて構築されている
- RSC や Suspense によって React の UI は、Concurrent Rendering や Partial Hydration を目指すことが可能になってきた。そのため、この時代の Router の作りを Page から App へと見直している。
- また React 公式も RSC 以降に関しては、Create React App などで純粋な React SPA を作ることは非推奨となり、Next.js や Remix などのラッパーフレームワークを通すことを推奨している。

# Next.js App Router with RSC のトレードオフ

- メリット
  - SSR の中では最もクライアント的な体験の良いアプリを作れるポテンシャルがある
  - SSG を行う場合においても、RSC 使うことでバンドルサイズが減らせる可能性がある
    - 名前の印象からか誤解してる人もいるが、RSC した上(Stage0 を走らせた上)で SSG 出来るので単純に配信するバンドルサイズの削減に効果がある
    - もちろん CSR の場合でも同様なので RSC を使うことに開発的な難しさを除き、機能的なマイナスはない。
- デメリット
  - SSR を利用する場合は SSG や CSR と比べてサーバーは必要になる
  - 設計が変わる。良いアプリを作るための設計や書き方はより難しくなった（求められるコンセプトへの理解が増えた）印象。Next.js が難しくなったという意見もよく見る
  - 個人的には、Fetch と Cache 周りの状態管理などのベストプラクティスや設計が固まってこないと普及しないのではという印象
