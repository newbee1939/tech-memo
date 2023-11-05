Hardhat は Solidty の開発環境です。 コントラクトのテストをローカルで実行できたり、果てはコントラクトのデプロイまで、Solidity でスマートコントラクトを開発するのに便利なツールです。 個人的な推しポイントはデバッグの簡単さです。 EVM 上のスタックトレースの表示やプリントでバッグなどの機能が便利です。

似たようなものとしては、Truffle Suite が該当します。

参考: https://dev.classmethod.jp/articles/hardhat-quick-start/#toc-11

---

以下、公式ドキュメントの記述。（https://hardhat.org/docs）

Hardhat は Ethereum ソフトウェアの開発環境です。これは、スマートコントラクトと dApps を編集、コンパイル、デバッグ、展開するためのさまざまなコンポーネントから構成されており、すべてが連携して完全な開発環境を構築します。

はじめに以下のセクションをご覧ください：

一般的な概要
クイックスタートガイド
ステップバイステップのチュートリアル
コンポーネント別にブラウズすることもできます。具体的なコンポーネントが不明な場合は、ここから始めることができます。

1. Hardhat Runner: Hardhat Runner は、Hardhat を使用する際に対話する主要なコンポーネントです。スマートコントラクトや dApps の開発に固有の繰り返しタスクを管理し、自動化する柔軟で拡張可能なタスクランナーです。

2. Hardhat Network: Hardhat には Hardhat Network が組み込まれており、開発向けに設計されたローカルの Ethereum ネットワークノードが含まれています。これにより、コントラクトの展開、テストの実行、コードのデバッグをローカルマシン内で行うことができます。

3. Hardhat Ignition: Hardhat Ignition は、デプロイプロセスのメカニクスを気にせずにスマートコントラクトを展開できる宣言型のデプロイメントシステムです。

4. Hardhat for Visual Studio Code: Hardhat for Visual Studio Code は、Solidity の言語サポートを追加し、Hardhat プロジェクトにエディター統合を提供する VS Code 拡張機能です。

5. Hardhat Chai Matchers: Hardhat Chai Matchers は、Chai アサーションライブラリに Ethereum 固有の機能を追加し、スマートコントラクトのテストを簡単に記述および読み取るのに役立ちます。

6. Hardhat Network Helpers: Hardhat Network Helpers は、Hardhat Network の JSON-RPC 機能に便利な JavaScript インターフェースを提供します。
