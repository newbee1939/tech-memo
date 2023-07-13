1. MetaMask の Export で秘密鍵を取得
   - Account Details(アカウントの詳細)
   - Show Private Key
2. .env を作成して値を設定
   - PRIVATE_KEY
   - SEPOLIA_URL
3. export $(cat .env | xargs)
4. scripts/deployHelloWorld.ts を作成
