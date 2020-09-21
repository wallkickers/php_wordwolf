# 開発環境構築(How to build development environment)

PHP,ngrok を使用して「HelloWorld」を表示するまで。

1. Dockerfile から image 作成
   `docker build -t laravel_linebot -f ./env/DockerfileForDev .`
   -f オプションは Dockerfile のファイル名が Dockerfile でないときに必要なオプション

2. コンテナ起動
   `docker run -d -p 8080:8080 -v /Users/moriyama/Desktop/study/laravel_linebot/web:/var/www/html -t laravel_linebot`

-d：コンテナをバックグランドで実行
-e：環境変数設定
-p：ポートの設定
-v：ホスト PC とのマウント設定。※コロンより左は自分がマウントしたい絶対パスを入力
-t：build 時に「go_alpine」という名前でタグをつけたので、タグ指定で起動することが可能

3. Laravel プロジェクト作成
   `composer create-project --prefer-dist laravel/laravel lravel-linebot`

4. コンテナの ID を確認
   `docker ps`

5. ID を指定してコンテナの中に入る。
   `docker exec -it 60c959e8d29d /bin/bash`
   ash：alpine を操作する際に使用するコマンド

# LINEBot 設定(LINEBot setting method)

1. LINE Developers コンソールでチャネルを作成

   > 参考：LINE Developers コンソールでチャネルを作成する
   > https://developers.line.biz/ja/docs/messaging-api/getting-started/#using-console

2. チャネル内の「Channel secret」と「Channel access token」をメモ
3. .env ファイルに下記の形で記載

   `envファイルの場所：/var/www/html/laravel-linebot/.env`

■.env ファイル

```
LINE_CHANNEL_SECRET=<メモしたChannel secret>
LINE_ACCESS_TOKEN=<メモしたChannel access token>
```

4. コンテナ内で `ngrok http localhost:8000` を実行。
   下記のように表示されるので https の方をメモ

(例)

```
Forwarding   http://2403c85009c4.ngrok.io -> http://localhost:8000
Forwarding   https://2403c85009c4.ngrok.io -> http://localhost:8000
```

5. LINE Developers 管理画面の「Webhook URL」に下記の形で記載。

```
Webhook URL
<ngrokで作成したhttpsのURL>/api/callback
```

6. 友達登録後、メッセージを送信し、返信があれば成功。

   友達登録は LINE Developers 管理画面の QR コードを読み込むことで可能。
