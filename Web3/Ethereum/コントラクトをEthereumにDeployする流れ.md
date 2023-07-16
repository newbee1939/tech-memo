1. MetaMask の Export で秘密鍵(PRIVATE_KEY)を取得

- Account Details(アカウントの詳細)
- Show Private Key

2. .env を作成して値を設定

- PRIVATE_KEY
- SEPOLIA_URL

3. export $(cat .env | xargs)

- 不要かも

4. scripts/deployHelloWorld.ts を作成
5. コントラクトをコンパイルしたものをデプロイするときに送らないといけない

- Deploy 用のファイルで読み込む

6. Provider や signer 等のデプロイのための設定を記述

- deployHelloWorld.ts
- deploy メソッド等

7. npx ts-node scripts/deployHelloWorld.ts

- Ethreum ネットワークにデプロイできる
- sepolia の ETH が減る
