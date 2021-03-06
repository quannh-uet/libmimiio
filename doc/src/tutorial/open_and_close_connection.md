接続の開始と終了 {#open_and_close_connection}
==========================

@ingroup tutorial

## 接続の開始

`mimi_open()` 関数を呼び出すことで，mimi(R) リモートホストへの接続を開き，クライアント側・サーバー側双方の初期化を実施します．この時点では，音声の送信は開始されていないことに留意してください．
第1引数と，第2引数には，別途指定される mimi(R) リモートホスト名及びポート番号を指定します．第3引数には，ユーザー定義コールバック関数 `txfunc()`, 第4引数にはユーザー定義コールバック関数 `rxfunc()` を指定します．第5引数と第6引数には，それぞれ，`txfunc() ` ，`rxfunc()` に渡すユーザー定義データを指定します．

第7引数には，音声の送信フォーマットを指定します．通常，mimi(R) クラウドサービスを用いる場合は，リモートホストは flac 形式のみを受け付けます．指定できるフォーマットは，`mimiio.h` で定義された `::MIMIIO_AUDIO_FORMAT` です．`MIMIIO_RAW_PCM`, `MIMIIO_FLAC_PASS_THROUGH` 以外のフォーマットが指定された場合は，libmimiio は内蔵エンコーダーによって，透過的にエンコーディングを行います．第8引数には，サンプリングレート，第9引数には，チャネル数を指定します．それぞれ，通常は 16000 Hz, 1 ch となりますが，利用するクラウドサービスによって異なる値とするべき場合があります．

第10引数には，後述するユーザー定義HTTPリクエストヘッダの配列の先頭ポインタ．第11引数には，同配列の長さを指定します．第12引数には，後述するアクセストークン，第13引数には，libmimiio が内部から出力するログのログレベルを指定します．ログレベルは，`MIMIIO_LOG_DEBUG`, `MIMIIO_LOG_INFO`, `MIMIIO_LOG_WARNING`, `MIMIIO_LOG_ERROR` の四段階を指定することができます． 

接続が出来なかった場合，初期化に失敗した場合などは，`mimi_open()`関数は NULL を返します．その場合，エラーコードが第11引数に記録されます．このエラーコードは，mimi_strerror() 関数で文字化することができます．

~~~~~~~~~~~~~~~~~~~~~{.cpp}
MIMI_IO *mio = mimi_open(
	mimi_host,	/* ホスト名 */
	mimi_port, 	/* ポート番号 */
	txfunc,    	/* txfunc() */
	rxfunc,    	/* rxfunc() */
	NULL,      	/* txfunc()に与えるユーザー定義データ */
	NULL,      	/* rxfunc()に与えるユーザー定義データ */
	MIMIIO_RAW_PCM, /* 送信フォーマット */
	16000, 		/* 送信音声のサンプリングレート */
	1, 		/* 送信音声のチャネル数 */
	NULL,      	/* ユーザー定義HTTPリクエストヘッダ配列の先頭ポインタ */
	0,         	/* 上配列の長さ */
	NULL,		/* アクセストークン */
	MIMIIO_LOG_DEBUG, /* ログレベル */
	&errorno   	/* mimi_open()が失敗した場合のエラーコード */
);

if(mio == NULL){
       fprintf(stderr, "Could not initialize mimi(R) service. mimi_open() failed: %s (%d)\n", mimi_strerror(errorno), errorno);
       return 1;
}
~~~~~~~~~~~~~~~~~~~~~
[Line 84 of file mimiio_file.cpp](mimiio__file_8cpp_source.html#l00084)

### API 方式

libmimiio 2.1.x では，コールバック型のAPIのみを提供しています．従って，`txfunc()`, `rxfunc()` の指定は必須となっています．

### 認証方式

#### アクセストークン方式

`mimi_open()` の第六引数にアクセストークンが指定された場合は，OAuth 2.0 に準拠したアクセストークンを用いた認証方式でアクセスします．全ての通信は SSL によって保護されます．

#### 無認証方式

`mimi_open()` のアクセストークンに nullptr が指定された場合は，無認証でアクセスを開始しようとします．全ての通信は平文で行われます．mimi(R) クラウドサービスは，無認証では利用できません．この方式が利用されるのは，全ての通信がクライアント端末環境内に閉じて行われる場合等，特殊な場合に限られます．

### ユーザー定義HTTPリクエストヘッダ

ユーザー定義HTTPリクエストヘッダは，`mimiio.h` で定義されている下記の構造体の配列への先頭ポインタです．下記の構造体は，`key` がHTTPリクエストヘッダーのキー，`value`がそのキーの値を示します．ユーザー定義HTTPリクエストヘッダを送信する必要がない場合は，NULL を指定します．リクエストヘッダをユーザー定義する場合は，libmimiio が内部的に利用しているヘッダー名称との衝突を避けるため，`X-` 接頭辞をつけることを推奨します．

~~~~~~~~~~~~~~~~~~~~~{.cpp}

  typedef struct{
    char key[1024]
    char value[1024]
  } MIMIIO_HTTP_REQUEST_HEADER;

~~~~~~~~~~~~~~~~~~~~~

@note より詳細には，ここで定義したHTTPリクエストヘッダは，libmimiio が WebSocket 接続を Upgrade する際に，RFC 2822 に従って，`key:value\r\n` の形式で，同時に与えられます．

### エラー時の接続再試行について

いくつかのエラーの場合は，mimi_open() を再試行することが有効である場合があります．これは，ユーザーの環境にも依存しますが，接続の再試行を検討できるエラーコードを下記に一覧します．

|errorno|説明|
|---|---|
|701|このエラーコードは，mimi_open() が指定したホスト名を見つけられなかったことを示します．クライアント環境の DNS が停止している場合などに発生します．再試行が有効であるかどうかは，クライアント環境に依存します．|
|703|このエラーコードは，mimi_open() がタイムアウトにより失敗したことを示します．タイムアウトは，まれに，サーバー側の極めて過大な混雑等で発生します．クライアント側のネットワークの問題である可能性もあります．|
|705|このエラーコードは，ユーザーからのリクエストが，契約上処理できる同時最大処理数を越えていることを示します．一定時間後に接続を再試行することが出来ます．|
|806|このエラーコードは，アクセストークンが認証拒絶されたことを示します．アクセストークンの有効期限が切れている可能性があるので，アクセストークン更新後に，接続を再試行することが出来ます．|

この他のエラーコードの一覧については，\ref errorcodes を参照して下さい．

## 接続の終了

`mimi_close()` 関数を呼び出すことで，接続を終了することができます．`mimi_close()` 関数は，`mimi_open()` が成功した後は，ユーザーは任意のタイミングで呼び出すことが出来ます．`mimi_close()` は接続が終了し，関連するリソースが全て適切に開放されるまでブロックされます．

~~~~~~~~~~~~~~~~~~~~~{.cpp}
mimi_close(mio);

~~~~~~~~~~~~~~~~~~~~~
[Line 121 of file mimiio_file.cpp](mimiio__file_8cpp_source.html#l00121)



Previous: \ref writing_callback | Next: \ref start_communication
