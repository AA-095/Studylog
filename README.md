# Webページが表示される仕組み
インターネットに接続したPCやスマホでブラウザーを起動し、URLと呼ばれるWebサイトのアドレスを指定すると、以下のようなプロセスが実行される。
1. WebサーバーにHTTPリクエストが送信される
2. リクエストを受信したWebサーバーがリクエストに応じてコンテンツを読みだす
3. サーバーで処理されたコンテンツがブラウザーに返される
4. ブラウザーが受信したコンテンツを解析して画面に表示する
- ブラウザーではサーバーから最初に受け取るHTMLファイルの内容を確認し、その内容に応じて必要なデータをサーバーに要求する。
- サーバーから受け取ったデータをもとにクラフィック化して画面に表示する機能をレンダリングと呼ぶ。

# DI
- DI(Dependency Injection)はオブジェクトの注入という意味
- DIはパターン、DIコンテナはDI実現を手伝うためのフレームワークである。

## DIコンテナのメリット
- 引数の数や内容が変わっても呼び出し元を修正する必要がない
- 引数が多くなってもコードをすっきりさせることができる
