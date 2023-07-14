```
function getMessage() external pure returns (string memory) {
    return  "Hello World";
}
```

- external
  - コントラクトの外部からのみ呼び出すことができる
  - このコントラクトの他の関数からは呼び出せない
  - トランザクションまたは他のスマートコントラクトによって直接呼び出すことができる
- pure
  - 関数がコントラクトの状態を変更しないこと、つまりブロックチェーンに記録されたデータを読み書きしないことを示す
- returns (string memory)
  - 戻り値の型を示す
  - 関数はメモリ内の文字列を返す
  - 関数の実行中にのみ一時的に存在するデータを示す
    - ブロックチェーンに永遠に格納される storage を使うよりもガス代を安く抑えられる