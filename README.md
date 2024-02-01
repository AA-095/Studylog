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

## DNS
- IPパケットを送信する場合、送信先の指定にIPアドレスを使用するが直感的ではないため、ドメイン名を指定する。IPアドレスとドメイン名などの変換を行うのがDNSである。
- DNSが開発される以前はHOSTSファイルという一つのファイルに世界中のドメイン名とそのIPアドレスが記述されたファイルを使用していた。
- DNSの特徴
    - IPアドレスとドメイン名の参照テーブルを世界規模で管理できる
    - 1か所に問い合わせが集中しないよう分散が可能
    - 情報更新を特定機関に依存せず、インターネット全体に伝播できる
    - 情報を常に最新の状態にできる
- ドメイン名と呼ばれる属性情報を各組織で固定しておけば、各組織で自由なホスト名を割り当てることができる。
- DNSでは分散された情報を効率よく検索するため、ドメイン名検索にドメインツリーを用いる。根に当たるルートの情報をもとに葉であるドメイン名まで節点を分岐していくことで目的の情報にたどり着けるトップダウンの木構造になっている。

# HTTP
## HTTPとは
- Webシステムでは、アプリケーションプロトコルにHTTPを使用する。HTTPは、IPやTCPの上位層プロトコルに当たるアプリケーションプロトコルで、処理やリソースをリクエストするWebクライアントと、要求に対してレスポンスを返すWebサーバー間の通信に利用される。
- アプリケーションプロトコルは半角の英字、アラビア数字、記号などを用いたテキストベースプロトコル、もう一つはコンピューター処理に最適化されたバイナリーベースプロトコルである。HTTPはテキストベースプロトコルの1つである。

## HTTPリクエスト・HTTPレスポンス
- Webシステムでは、WebクライアントからWebサーバーに対して行われる要求をHTTPリクエストと呼ぶ。リクエストを受け取ったサーバーは、応答としてHTTPレスポンスを返す。
- 1回のリクエストで取得できるリソースは1つであるため、クライアントが2つのリソースをダウンロードする異は2回のリクエストを送信する必要がある。また、リクエストとレスポンスは常に1対なのでどちらかのみが発生することはない。

## ステートレスプロトコル
- HTTPが軽量プロトコルと言われる理由はステートレス性である。それぞれのHTTPリクエスト・レスポンスは、その前後に行われるHTTP接続の状態を管理・維持しない。このようにお互いの状態を管理しないプロトコルをステートレスプロトコル、管理・維持できるプロトコルをステートフルプロトコルと呼ぶ。
- HTTPは1回1回のリクエスト・レスポンスが短いため、すぐに他のサーバーに切り替えることができる。これによってサーバーを分散させたり、台数を増やしてパフォーマンスの向上を図るスケールアウトも容易に行える。
- HTTPがステートレスプロトコルであるはずなのに、ログイン状態が維持されていたり、カートに入れた商品が会計まで保存されていたりするのは、クッキーやセッション変数などの技術によってHTTPのステートレス性を補っているためである。
- クッキー（HTTP Cookie）は、WebサーバーからWebブラウザーに送信される極めて小さなデータである。Webブラウザーでクッキーを保存しておき、再度同じWebサーバーにリクエストを送信する際に保存しておいたクッキーを一緒に送信する。Webサーバーはクッキーが以前送ったものと同じかどうかを確認して同じものであれば以前アクセスしてきたWebブラウザーだと判断する。このような仕組みによってHTTPのステートレス性が補われている。

## HTTPメッセージ
- クライアントからサーバーへのリクエストで使用されるものをリクエストメッセージ、レスポンスで使用されるものをレスポンスメッセージと呼ぶ。
- 空行には改行文字のCR（キャリッジリターン）とLF（ラインフィード）が用いられる。

## リクエストメッセージ
- リクエストメッセージの1行目にあるのがリクエストラインである。メソッドやリクエストURL、HTTPバージョンなどで構成されている。
- メソッドには、サーバー上のHTMLファイルや画像ファイルなどのリソースに対する処理内容を指定する。通常のブラウジングではGET・HEAD・POSTメソッド以外は利用されない。ブラウジングでPUTやDELETEが利用されると、コンテンツの書き換えや削除が簡単に行えてしまうからである。
    - GET：クライアントが指定したリソースを取得する。
    - POST：クライアントからサーバーにデータを送信する。
    - PUT：指定したURLにリソースを保存・更新する。
    - HEAD：ヘッダー情報のみ取得する。
    - DELETE：指定したリソースを削除する。
- リクエストURLには、リクエストするリソースの位置情報を指定する。ただし、HTTP/1.1ではHTTP接続の確立前に、TCP接続でWebサーバーに接続されるため、わざわざプロトコルやホスト名を明示する必要がないので、スキーム（http:）やオーソリティ（//www.example.jp）を省略するのが一般的。
- 末尾のHTTPバージョンには、使用するプロトコル名とバージョンを指定する。
- リクエストメッセージの2行目から空行までがヘッダーである。リクエストメッセージのみ使用されるヘッダーやレスポンスメッセージにも使用される一般ヘッダー、転送されるコンテンツに関するエンティティヘッダーなどの種類がある。
- ヘッダーの後の空行を挟んで続くのがボディで、省略可能である。

## レスポンスメッセージ
- レスポンスメッセージの1行目に当たるのがステータスラインである。先頭のHTTPバージョンには、使用するHTTPバージョンがセットされる。次のステータスコードには100～500番台で値がセットされる。最後のテキストにはステータスコードの概要を説明する短い文章がセットされる。
    - 100番台：情報（処理が継続されていることを通知する。）
    - 200番台：成功（リクエストの処理に成功したことを通知する。）
    - 300番台：リダイレクト（リクエストを完遂するにはさらに新たな動作が必要なことを通知する。）
    - 400番台：クライアントエラー（リクエストの内容に問題があるためリクエスト処理に失敗したことを通知する。）
    - 500番台：サーバーエラー（リクエスト処理中にサーバーエラーが発生したことを通知する。）
- レスポンスメッセージの2行目から空行までがヘッダー。レスポンスメッセージで使用されるヘッダーには、一般ヘッダーやコンテンツに関するエンティティヘッダーがある。
- ヘッダーの後の空行を挟んで続くのがボディ。サーバーからクライアントに対して転送したいデータがあるときに使用する。ブラウジングにおいては、ボディを使ってHTMLテキストや画像など、WEｂページの表示に必要なデータの転送に使用する。リクエストメッセージと同様にボディが省略される場合がある。

## 転送効率を上げる仕組み
- 大量のアクセスに対応するために、HTTP/1.1では、持続的接続を採用した。持続的接続では、１つのTCPコネクションを使いまわして1回目のリクエスト・レスポンスで使用したものを2回目以降も再利用する。HTTPの持続的接続機能をHTTPキープアライブと呼ぶ。
- HTTP/1.1以降、クライアントがレスポンスを待たずに続けてリクエストを送信できるパイプライン処理にも対応し、さらに効率よくリクエスト・レスポンスを処理することが可能になった。
- 圧縮転送はコンテンツを変換することから、コンテンツエンコーディングと呼ぶ。それに対し、分割転送は転送方式を最適化することから転送エンコーディングと呼ぶ。
- 圧縮転送とは、コンテンツを圧縮してサイズを小さくしてから転送することである。ネットワークにかかる負担を抑え、大量のデータ転送を可能にする。コンテンツを圧縮転送するには、クライアントからサーバーへのリクエスト時に「Accept-Encoding:」ヘッダーでクライアントが対応する圧縮方式をサーバーに通知する。それに対応するレスポンスとして「Content-Encoding:」ヘッダーで実際に使用した圧縮方式が記述され、ボディには圧縮されたデータが埋め込まれる。レスポンスを受け取ったクライアントがボディを回答しリソースを取り出す。
- 分割転送とは、サーバーからクライアントに大きなサイズのデータを転送する際、サーバーでデータを分割してから転送することである。たとえば、全データの受信完了を待たずに、受信データから順番に画面に表示できるようになる。大きなファイルのダウンロード中にいったん接続が途切れた後でも再度同じところからダウンロードする機能（レジューム）や、Google
マップのように画面のスクロールに応じて画像をダウンロードする機能（レンジリクエスト）があるが、これは分割転送とは異なる方法で実現されている。

# HTTPS・HTTP/2
## HTTPのセキュリティ機能の問題点
- HTTP/1.1では、WebクライアントからWebサーバーへのリクエストや、それに対するレスポンスのどちらも通信内容を暗号化していないヘイブンでやり取りされるため、通信経路上では通信内容の盗聴が可能になっている。
- HTTPでは、URLによってWebサーバーを指定するが、URLで指定したWebサーバーと、レスポンスを返してきたWebサーバーが同じものとは断定できないため、なりすましを防ぐことができない。また、Webサーバーから見ても、レスポンスを返すクライアントが正規の物か十分に確認する手段がないので、意味のないリクエストを大量に受け付けてサービス不能に陥ってしまうDoS攻撃などのリスクを抱えている。
- Webサーバーに侵入してWebページの改ざんや、不正なプログラムによるユーザー情報の搾取、コンテンツ書き換えによるウイルスのバラマキによって大きな被害が出ることがある。また、通信経路上でも通信内容が盗聴されたり、書き換えられる中間者攻撃の可能性もある。

## セキュアなHTTPS
- HTTPのセキュリティ面での不安を解消し、よりセキュアなWebアクセスを行うためのプロトコルがHTTPSである。HTTPSでは、SSLやTLSなどのセキュアなプロトコルによって、送受信データを暗号化してネットワーク経路上での盗聴を防ぐことができる。そのような経緯から古い資料ではHTTPSをHTTP overSSL/TLSと表記している場合もある。

## HTTPSへの対応
- 「https://」で始まるURLを指定することで、HTTPSによる暗号化通信を開始する。WebサーバーによってはHTTPでリクエストがあった場合もプロトコルを自動的にHTTPSに切り替え、ブラウザに対してHTTPSリクエストを再送するように要求する。これをリダイレクトと呼ぶ。HTTPにしか対応していないWebサーバーへのアクセスを危険とみなし、保護されていないことを知らせる警告マークとメッセージが表示されることもある。
- WebサーバーでHTTPSに対応するには、サーバー証明書が必要である。サーバー証明書には、通信の暗号化に必要な鍵とWebサイト運営者や所有者の情報を含んでおり、サーバー証明書によって通信の暗号化、なりすましの防止、改ざんの防止などが可能になる。WebサーバーがHTTPSに対応していない場合は、WebサーバーのフロントエンドにHTTPSに対応したリバースプロキシサーバーを置くことで、クライアントからのHTTPSリクエストに応えられるようになる。
- HTTPS対応に必要なサーバー証明書は、認証局と呼ばれる信頼のおける第三者の発行期間でWebサイトの運営者や所有者の審査を経た上で発行される。

## HTTPSのしくみ
- SSLやTLSは、インターネット上での通信を暗号化するための技術である。現在実際に使用されているのはTLSのみで、SSLは使用されていない。
- SSL/TLSによる暗号化通信では、共通鍵暗号方式と公開鍵暗号方式の2つの暗号化方式を使用する。共通鍵暗号方式では、暗号化と復号に同じ鍵を使用するのに対し、公開鍵暗号方式では、暗号化と復号に別々の鍵を使用する。共通鍵暗号方式では鍵を送付しておくなどして、あらかじめ共有しておく必要がある。公開鍵暗号方式では、暗号化に公開鍵、復号に秘密鍵を使用する。公開鍵だけを送信側の暗号鍵として公開し、その公開鍵で暗号化された通信内容を秘密鍵を使用して復号する。
- 公開鍵暗号方式は、サーバーやブラウザの負荷が高いため、通信内容の暗号かは負荷の小さい共通鍵暗号方式を用いて、共通鍵の交換で公開鍵暗号方式を使用するようになっている。この一連の流れをTLS/SSLハンドシェイクと呼ぶ。

## サーバー証明書とは
- サーバー証明書は、Webサイト運営者の実在性を確認し、ブラウザーとWebサーバー間で暗号化通信で使用される電子証明書である。主な役割はWebサイト運営者の信頼性や実在性の証明、通信データを暗号化する「鍵」がある。
- サーバー証明書は、認証局と呼ばれる第三者機関で発行される電子証明書である。その中で厳正な審査を受けて公に認められた認証局をパブリック認証局と呼ぶ。一般的なブラウザーには、認証局の正当性を証明するルート証明書が組み込まれている。サーバー証明書をパブリック認証局で発行してもらうには、Webサイト経営者の情報と暗号化通信に必要な鍵などを送る。パブリック認証局では、それらの情報に加えて発行者の署名データを付けてサーバー証明書を発行する。
- 一方、独自に認証局を立ち上げて、サーバー証明書を発行することもできる。このような認証局をプライベート認証局と呼ぶ。ブラウザーに組み込まれているルート証明書ではプライベート認証局が発行したサーバー証明書の正当性を検証できないため、プライベート認証局の証明書を信頼されたルート証明機関としてインポートする必要がある。
- 自前でサーバー証明書を作成することを自己署名または自己発行と呼ぶ。

## なりすましと改ざんの防止
- 他の人のコンテンツを利用するなどして、その人であると誤認させ、悪用しようとする行為をなりすましと呼ぶ。これらの行為からの被害を防ぐために正規の認証局で発酵されたサーバー証明書を利用する。ブラウザーに組み込まれたルート証明書によって、サーバー証明書が認証局によって発行された正当なものかどうか確認することもできる。ただし、ルート証明書で検証できるのは、ルート認証局と呼ばれる最上位にある限られた認証局だけである。国内の多くの認証局は、ルート認証局によって承認された中間認証局であるため、実際には中間CA証明書をダウンロードし、検証する。
- HTTPSを利用することで通信経路上の盗聴を防げるほか、通信経路上で悪意のある第三者がデータを書き換える、改ざんと呼ばれる行為を防ぐこともできる。改ざんを防ぐためにWebサーバーからの送信データとブラウザーでの受信データを比較して検証するメッセージ認証を利用する。

## HTTP/2の誕生
- HTTP/1.1は原則一つずつしかリクエストを送ることができなかったが、のちに多重リクエストが可能になった。しかし処理はリクエストされた順番通りにしか行えないため、それ以降のリクエスト処理が待ち状態となりWebページの表示に時間がかかってしまう。HTTPリクエストやレスポンスでのヘッダーなどのデータ肥大化もHTTP/1.1の課題になっている。
- HTTP/2はGoogle社のSPDYがベースとなり解消して標準化が完了した。

## HTTP/2の特徴
- HTTP/2はHTTPセッションを張る際にTLSによる暗号化通信を行うことが大きな特徴である。
- ストリームによる多重化
    - HTTP/2では、1つのTCPコネクションを有効に使いまわせるよう、ストリームと呼ばれる仮想的な接続を生成する。ストリームの中で複数のリクエストとレスポンスを並列に処理することで多重化を可能にしている。ストリームにはストリームIDと呼ばれる一意のIDが付与される。通常HTTPでのやり取りはブラウザーから開始されるが、HTTP/2ではサーバープッシュによってWebサーバーからストリームを開始できる。
    - 1とのサーバー証明書を２つのWebサーバーで使用するには、ワイルドカード証明書を使用する。
- バイナリーフレーム
    - HTTP/1.1はテキストベースプロトコルである一方、HTTP/2はバイナリベースプロトコルである。コンピューター処理に最適化されたフレームと呼ばれるバイナリーメッセージでやり取りを行う。
- プロトコルネゴシエーション
    - URLやポート番号だけでWebサーバーがHTTP/2対応なのかどうかを判断できない。そこで、ブラウザーとWebサーバー間でどのバージョンのHTTPを使用するかを取り決めるプロトコルネゴシエーションが必要となる。HTTP/2のプロトコルネゴシエーションにはいくつかの方式がある。
    - アップグレード方式ではHTTP/2対応ブラウザーが最初はHTTP/1.1での手順でWebサーバーに接続を行い、WebサーバーがHTTP/2に対応していた場合はステータスコードとして「101」を返し、HTTP/2コネクションにアップグレードする。
    - ALPN方式では、ブラウザーが使用可能なプロトコルのリストをWebサーバーに送信し、使用するプロトコルを選択してHTTPレスポンスに含めて返す。ブラウザー側で指定されたプロトコルに問題なければ、そのプロトコルで通信を開始する。TLS暗号化通信を必須としているHTTP/2では、ALPN方式を用いるのが一般的である。
    - その他にWebサーバーがHTTP/2に対応していることが事前にわかっている場合は直接通信を開始するダイレクト方式もある。
- HTTPヘッダーの圧縮
    - リクエストとレスポンスには、Webページの画像やテキストなどの目に見えるデータ以外に、HTTPヘッダーと呼ばれるパートがあり、このデータの肥大化がHTTP/1.1の課題になっていた。そこで、HTTP/2ではHPACKという圧縮方式で転送することが可能になった。
- 優先度によるストリーム制御
    - ブラウザーはWebサーバーに対し、どのストリームを優先してほしいかを優先度をつけて指定できる。これにより、Webページの表示に必須のデータを先に処理できる。ストリーム制御により、ブラウザーが指定した通りの順番でコンテンツを受け取ることができ、Webページの表示を最適化できる。優先度の指定の際。DependencyとWeightというパラメータを用いる。
- サーバープッシュ
    - HTTP/2ではサーバープッシュにより、リクエストを待たなくても、Webサーバーからレスポンスを送信することが可能である。
- 常時SSL/TLS暗号化通信によるセキュリティの向上
    - 現在では、セキュリティーを高めるために、常時SSL/TLS暗号化通信を行い、Webサイトの全ページをHTTPSでやり取りするのが一般化しつつある。
- HTTP/2の普及と課題
    - HTTP/2の普及の妨げになっているのがTCP head-of-line ブロッキングである。TCPパケットを連続で送信する際、先頭のパケット送信でエラーが発生すると、その再送処理が完了するまで後続のパケットを送信できないというものである。HTTP/2ではすとりーむによってHTTP通信の多重化を可能にしているが、パケットロスが多いとTCP head-of-line ブロッキングが発生し、パフォーマンスを悪化させる可能性がある。

# URLとは
- URLはどのような手順でどこのWebサーバーにある、どのようなコンテンツにアクセスするかを示し、その内容によってリクエストを送信する。/index.htmlの場合は、/と呼ばれる場所にある「index.html」ファイルを送信する。

# DI
- DI(Dependency Injection)はオブジェクトの注入という意味
- DIはパターン、DIコンテナはDI実現を手伝うためのフレームワークである。

## DIコンテナのメリット
- 引数の数や内容が変わっても呼び出し元を修正する必要がない
- 引数が多くなってもコードをすっきりさせることができる
