# Event とは

- Ethereum における Event とはログのこと
- Solidity では、Event は State Variables のすぐ下に定義する
- コントラクトの実行中に Event を emit(送出)すると、その内容が Ethereum Network に保存される
  - ログ情報の保存コストは Storage への保存コストよりも小さい
- ex.
  - event Transfer(address indexed from, address indexed to, uint value);
    - このような形式のイベントの場合、以下のようなデータが保存される
      - event を emit したコントラクトのアドレス
      - from, to, value の値
- Node に過去の Event 情報を問い合わせるとき、indexed キーワードが付いている情報に関してフィルタリングが可能
  - 例えばコントラクトからの Transfer イベントで、from="ho32hofheowo"となっているものは容易に得られる
