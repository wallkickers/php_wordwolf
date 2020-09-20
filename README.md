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
   `docker exec -it 60c959e8d29d ash`
   ash：alpine を操作する際に使用するコマンド
