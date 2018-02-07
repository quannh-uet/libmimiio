エラーコード一覧 {#errorcodes}
==========================

@ingroup tutorial

|errorno|説明|
|---|---|
|101|下記に分類できないエラーを示します．通常の場合は発生しません．|
|501|内蔵エンコーダーの初期化時にエラーが発生したことを示します．内蔵エンコーダーからのエラー詳細情報が表示されます．|
|502|内蔵エンコーダーの実行時にエラーが発生したことを示します．内蔵エンコーダーからのエラー詳細情報が表示されます．|
|600番台|600番台のエラーコードは，SSL関連のエラーであることを示します．サーバー証明書の期限切れ，不正なサーバー証明書等，サーバー側のSSLの問題で発生します．ただし，クライアントのSSL環境の問題で発生する可能性もあります．他のクライアントからは正常にアクセスできるのに，特定のクライアントからアクセス出来ないような場合には，クライアント側のSSL環境の問題である可能性があります．600番台のエラーが発生した場合は，接続の再試行は有効ではありません．|
|701|このエラーコードは，mimi_open() が指定したホスト名を見つけられなかったことを示します．クライアント環境の DNS が停止している場合などに発生します．再試行が有効であるかどうかは，クライアント環境に依存します．|
|703|このエラーコードは，mimi_open() がタイムアウトにより失敗したことを示します．タイムアウトは，まれに，サーバー側の極めて過大な混雑等で発生します．クライアント側のネットワークの問題である可能性もあります．|
|704|このエラーコードは，mimi_open() が，指定したホストに接続できなかった（指定したホストまで通信が到達していない）ことを示します．クライアント側のネットワークに問題がある場合に発生します．例えば典型的な例として，インターネットへの接続がない状態でインターネット上へのリモートホストにアクセスしようとすると発生します．また，リモートホストが，指定したポートで待ち受けていない場合にも発生します．|
|705|このエラーコードは，mimi_open() の新規接続リクエストは，指定したリモートホストまで到達したが，リモートホストによって切断されたことを示します．典型的には，契約上処理できる同時最大処理数を越えている場合に発生します．一定時間後の再試行が有効です．開発段階では，通信に SSL を要求しているリモートホストに対して，非SSLのリクエストを送った場合にも発生することにも留意してください．|
|790|何らかの一般的なネットワーク上のエラーが発生したことを示します．|
|791|通信中に，ネットワークが突然切断されたことを示します．|
|799|何らかの一般的なネットワーク上のエラーが発生したことを示します．|
|801|WebSocket 通信が許可されなかったことを示します．リクエストヘッダーの不正等により，サーバー側によって明示的に許可されなかった可能性があります．|
|802|WebSocket ハンドシェイクエラー．通常の場合は発生しません．|
|803|WebSocket ハンドシェイクエラー．通常の場合は発生しません．|
|804|WebSocket ハンドシェイクエラー．通常の場合は発生しません．|
|805|WebSocket ハンドシェイクエラー．通常の場合は発生しません．|
|806|このエラーコードは，アクセストークンが認証拒絶されたことを示します．アクセストークンの有効期限が切れている可能性があるので，アクセストークン更新後に，接続を再試行することが出来ます．|
|810|WebSocket プロトコルエラー．通常の場合は発生しません．|
|811|WebSocket プロトコルエラー．通常の場合は発生しません．|
|830|WebSocket 通信が開始された後，サーバー側からのレスポンスが，一定時間なかった場合に発生します．WebSocket による全二重通信の受信側タイムアウトエラーです．タイムアウトが発生した場合，接続は終了されます．|
|890|WebSocket プロトコルエラー．通常の場合は発生しません．|
|901|ユーザープログラムの開発上のエラーです．txfunc() コールバック関数が設定されていない場合に発生します．|
|902|ユーザープログラムの開発上のエラーです．rxfunc() コールバック関数が設定されていない場合に発生します．|
|903|ユーザープログラムの開発上のエラーです．txfunc() における送信チャンクの最大サイズには仕様制限があります．関数仕様を確認してください．|
|904|サーバーから不正な接続終了ステータスを受信したことを示します．通常は発生しません．|
|905|何らかの問題が発生し，指定した API が開始できなかったことを示します．通常は発生しません．|
|906|WebSocket プロトコルエラー．通常は発生しません．|
|907|WebSocket プロトコルエラー．通常は発生しません．|
|1000番台|WebSocket クローズフレームステータスコードを示します．|
|4000番台|リモートホストのエラーを示します．リモートホストのエラーについては，各リモートサービスのドキュメントを参照して下さい．|

Previous: \ref utilities | Next: \ref examples_src