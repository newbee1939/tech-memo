# ABI（Application Binary Interface）

- スマートコントラクトの公開メソッドを定義する JSON 形式のインターフェース
- スマートコントラクトが外部から呼び出すためのインターフェースを定義するデータ構造
- Ethereum のスマートコントラクトは、Solidity などの言語で記述され、EVM（Ethereum Virtual Machine）が理解できるバイトコードにコンパイルされる
- しかし、このバイトコードは人間が理解するには困難
- スマートコントラクトが公開しているメソッドや変数に対してどのようにアクセスすべきかを知るためには、その情報を何らかの形で知る必要がある
- ここで ABI が役立つ
- ABI は、スマートコントラクトがどのようなメソッドを公開しているか、それらのメソッドはどのような入力を受け付け、どのような出力を返すかなど、スマートコントラクトのインターフェースを定義する
- これにより、開発者は ABI を参照することで、スマートコントラクトとどのように対話すべきかを知ることができる
- ABI は通常、JSON 形式で表現され、スマートコントラクトのバイナリコードと一緒にデプロイ時に公開されます。この ABI 情報は、Ethereum ノード、DApps（分散型アプリケーション）、スマートコントラクトの開発ツール、ブロックチェーンエクスプローラなどがスマートコントラクトと対話する際に使用します
