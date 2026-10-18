Twitch Chat Lens（初期試作版）
================================

できること
- iPhoneの縦画面に合わせた、暗色のコメント専用画面
- コメント文字の拡大・縮小、自動スクロール、表示コメント消去
- Twitch OAuth（user:read:chat）で認証し、EventSub WebSocketからチャットを受信
- PWA manifest / ホーム画面追加用アイコン / 基本的なオフラインシェル
- 接続前に画面を確認できるプレビュー

重要：最初に必要な設定
----------------------
このアプリの実コメント受信は、ローカルでファイルを開くだけでは動きません。
HTTPSで公開してTwitchアプリに登録する必要があります。

1. このフォルダのファイル一式を、HTTPS対応の静的ホスティングにアップロードします。
   例：GitHub Pagesなど（公開URLが必要です）。
2. Twitch Developer Consoleでアプリを登録します。
   https://dev.twitch.tv/console/apps
3. OAuth Redirect URLに、公開した index.html のあるページのURLを完全一致で登録します。
   例：https://ユーザー名.github.io/twitch-chat-lens/
   実際に表示されるURLを使ってください。末尾のスラッシュやパスも一致させます。
4. 登録したアプリの Client ID をコピーします。Client Secretは使いません。
5. iPhoneのSafariで公開URLを開き、設定にチャンネル名とClient IDを入力して「Twitchに接続」を押します。
6. Twitchの認証画面で許可すると、コメント受信を開始します。
7. Safariの共有メニューから「ホーム画面に追加」を選ぶと、専用アプリ風に起動できます。

トラブルシュート
----------------
- 「redirect URI mismatch」：Twitch側のRedirect URLと画面のURLが完全一致していません。
- Client IDエラー：Client IDを確認してください。Client Secretは入力しないでください。
- 認証は通るが購読に失敗：認証に user:read:chat 権限が含まれているか、対象チャンネルにアクセスできるアカウントか確認してください。
- 実コメントの接続にはインターネット接続が必要です。ホーム画面追加後もオンラインで使用してください。
- トークンはLocalStorageへ保存せず、ブラウザのURLフラグメントから読み取った後にURLから除去します。ただし、これは試作版なので公開運用の前には追加のセキュリティ検証を行ってください。

使用API
-------
- Twitch OAuth Implicit Grant（クライアント側アプリ向け）
- Helix Get Users
- EventSub WebSocket / channel.chat.message

本ファイルは初期試作版であり、Twitchの仕様変更やアカウント条件によって接続できない場合があります。
