# Webページが表示される仕組み
インターネットに接続したPCやスマホでブラウザーを起動し、URLと呼ばれるWebサイトのアドレスを指定すると、以下のようなプロセスが実行される。
1. WebサーバーにHTTPリクエストが送信される
2. リクエストを受信したWebサーバーがリクエストに応じてコンテンツを読みだす
3. サーバーで処理されたコンテンツがブラウザーに返される
4. ブラウザーが受信したコンテンツを解析して画面に表示する
- ブラウザーではサーバーから最初に受け取るHTMLファイルの内容を確認し、その内容に応じて必要なデータをサーバーに要求する。
- サーバーから受け取ったデータをもとにクラフィック化して画面に表示する機能をレンダリングと呼ぶ。

# プロトコルとは
ネットワーク通信において、送られる側と受け取る側で事前にルールを取り決めておく必要がある。この取り決めをプロトコルと呼ぶ。

## プロトコルの標準化
世界中のWebサーバーにアクセスし、表示することができるように標準化と甥う作業が必要になる。標準化とは何もしなければ秩序が取れないものに標準となる規定を取り決め、互換性や品質を確保して技術の普及を図ることである。

## プロトコルの階層化
ブラウザーがWebサーバーと通信を行い、データをダウンロードするという一連の流れをプロトコルの階層構造で表現することができる。
1. Webサーバーのアプリケーションを操作してデータを読み取る際に使用されるプロトコルがHTTP
2. その読み取ったデータをアプリケーションに渡すプロトコルがTCP
3. ブラウザーが入ったPCまでデータを届けるプロトコルがIP
4. 実際にデータを伝送するためのプロトコルがイーサネット

## IPプロトコル
- IPは上位層のTCPと、下位層のイーサネットを取り持つ。物理的なネットワークであるイーサネットでは特性上の制約が存在するので、IPのレイヤーで最適化を行う。
- IPパケットを送信する場合は、送信先のIPアドレスを指定し、特定のコンピューターにパケットを送信する。その際に送信元と送信先が同じLAN内にあれば直接通信が行えるが、他のネットワークにある場合は、ルーターという危機を経由する。ルーターはルーティングテーブルという経路情報によってIPアドレスから送信先のコンピューターがネットワークに接続されているか知ることができる。
- 自分のネットワーク内に送信先コンピューターがない場合はデフォルトゲートウェイと呼ばれるルーターに送られる。
- IPによる通信は相手の情報を確認せずに送信するため、確実にパケットが送信先に届く到達性は保証していない。そこで通信品質や信頼性の向上については、TCPなどの上位プロトコルが対応する。

## TCP
- TCPは上位層のアプリケーションプロトコルと下位層のIPを取り持つプロトコルである。送受信されるパケットに欠損がないかチェックし、欠損があった場合はパケットの再送を行うなどのエラー訂正機能を備えている。
- TCPでは、どのアプリケーションに渡せばよいかをポート番号を使って区別する。ポート番号には16ビットの範囲の整数値が使われる。よく使用されるポート番号は0~1023の範囲であらかじめ予約されており、これをウェルノウンポートと呼ぶ。
    - FTP(20)：ファイル転送(データの転送用)
    - FTP(21)：ファイル転送(制御用)
    - SSH(22)：セキュアシェル
    - SMTP(25)：メールの送信
    - HTTP(80)：Webページの閲覧
    - POP3(110)：受信メールの操作
    - HTTPS(443)：セキュアなWebページの閲覧

## TCPの信頼性を挙げる仕組みとUDP
TCPの信頼性を高める機能の一つに3ウェイハンドシェイクがある。3ウェイハンドシェイクとは送信前にパケットを受け取る準備ができているか確認するやり取りのことである。また、送信パケットにはシーケンスと呼ばれる通し番号が付与されており、その順番通りにパケットを受信しているかを確認したり、届いていないパケットがないかを検知できるようになっている。
- SYNパケット：受信可能かどうか問い合わせを行うためのパケット
- ACKパケット：受信可能なことを返答するためのパケット
- SYN+ACKパケット：受信可能なことを返答すると同時に、相手に対してパケットを受信できるかどうかも問い合わせするパケット
- UDP：高速化を重視し、TCPの代わりに使われる。通信の信頼性を重視する場合はTCP、信頼性より転送効率を重視する場合はUDPと使い分けられている。

## IPv4アドレス
- IPv4アドレスは10進数で表現された4つの整数とドットで表記される。しかしネットワーク上では32ビットのビット列が使われている。私たちが目にするIPv4アドレスはこの32ビットのビット列を8桁ごと(オクテット)に区切って、それを10進数に変換した後、ドットで区切ったものである。
- IPv4アドレスはネットワーク部とホスト部に分けられる。例えば「192.168.1」をネットワーク部とした場合、2進数の残り8ビットを0で埋めたものをネットワークアドレスとして利用する。
- ブロードキャストアドレスは、同一ネットワーク上のすべてのホストに対し、同時勝つ同じデータを送信する際に使用する。32ビットのIPアドレスのうち、ホスト部をすべて「1」としたアドレスになる。
- ループバックアドレスは、ホスト内部のアプリケーション同士が通信する際に使用する。アドレスには「127.0.0.1」を使うのが一般的。
- インターネット上で利用されるアドレスをグローバルIPアドレス、閉鎖されたネットワーク内で使うアドレスをプライベートIPアドレスと呼ぶ。

## IPv6アドレス
- IPv6アドレスは128ビットのビット列を先頭から16ビットごとに8つに区切って、16進数に変換してコロンで連結する。
- 前半64ビットはネットワークプレフィックス、後半64ビットはインターフェイスIDに分けられている。ネットワークプレフィックスはネットワークを識別するために使用される。また、ネットワークプレフィックスは、デフォルトルーターから送信されるRAによって割り当てられる。
- インターフェイスIDはIPv6アドレスをユニークなものにする。同一リンク内にインターフェイスIDが重複しないよう、DADという仕組みが用意されている。また、MACアドレスから個人が特定されないように、匿名アドレスと呼ばれるランダムに生成されたインターフェイスIDが使用されるケースもある。
- ユニキャストアドレス：1対1の通信で使用されるアドレス。
- エニーキャストアドレス：複数のインターフェイスに割り当てられるアドレス。グループに属する1つのインターフェイスにだけパケットが到達し、それ以上は配送されない。
- マルチキャストアドレス：複数のインターフェイスと通信するためのアドレス。

## ARP
- IPアドレスに対応するMACアドレスを調べるには、イーサネットでつながっているすべてのコンピューターに対す手、MACアドレスの問い合わせを実行する。このとき使われるプロトコルをARPと呼び、問い合わせをARPリクエストと呼ぶ。また、すべてのコンピューターに対して問い合わせの送信を行うことをブロードキャストと呼ぶ。
- ブロードキャストを受信したPCの中で、問い合わせ対象となるPCはそのリクエストに対して返信を行う。これをARPリプライと呼ぶ。
- ARPリクエストが頻発すると、ネットワークには大きな負荷がかかる。そこでMACアドレスとIPアドレスの対応付けが解決したものについては、しばらくの間ARPテーブルと呼ばれるキャッシュに保存される。

# DI
- DI(Dependency Injection)はオブジェクトの注入という意味
- DIはパターン、DIコンテナはDI実現を手伝うためのフレームワークである。

## DIコンテナのメリット
- 引数の数や内容が変わっても呼び出し元を修正する必要がない
- 引数が多くなってもコードをすっきりさせることができる
