---
layout: module
title: セットアップ
---

## Herokuボタンを使ってHerokuへデプロイ

あなたの専用のNibsを以下のHerokuボタンを利用してデプロイすることが可能です:

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy)

## コマンドラインでHerokuへデプロイする

コマンドラインからHerokuへデプロイする事も可能です:

1. リポジトリをクローン

    ```
    git clone https://github.com/SalesforceDevelopersJapan/nibs-jp
    ```

1. Herokuアプリケーションの作成

    ```
    cd nibs-jp
    heroku create
    ```

1. Postgresプラグインのインストール

    ```
    heroku addons:add heroku-postgresql:dev
    ```

1. Herokuへデプロイ

    ```
    git push heroku master
    ```

1. アプリケーションの起動:
    - ブラウザでアプリケーションを開きます:

        ```
        heroku open
        ```
    - サインアップボタンからアカウントを作成します

    > Facebookログインはページ下部にあるFacebookインテグレーションのステップを完了しなければ動きません。


## ローカル環境へのインストール

Nibsをあなたのローカル環境にインストールすることも可能です:

1. リポジトリのクローン

    ```
    git clone https://github.com/SalesforceDevelopersJapan/nibs-jp
    ```

1. サーバ側の依存ファイルをインストール

    ```
    cd nibs
    npm install
    ```

1. ローカルデータベースの作成
    - [Postgres](http://www.postgresql.org/) をローカルマシンへインストールし、起動します
    - **nibs** という名前のデータベースを作成します
    - 仮にデータベースが **postgres://@127.0.0.1:5432/nibs** といったURLで使用可能ならば、他になにもすることはありません
    - もし別のURLが設定されているならば、Shell環境から **DATABASE_URL** を編集するか、**server/config.js** に、定義されたデフォルトのURLを設定します

1. サーバを起動

    ```
    node server
    ```

1. アプリケーションの起動
    - Webブラウザで以下のURLにアクセスします:
        [http://localhost:5000](http://localhost:5000)
    - サインインボタンからアカウントを作成します

    > Facebookログインはページ下部にあるFacebookインテグレーションのステップを完了しなければ動きません。


## Facebookインテグレーション

1. Facebookアプリケーションを作成します
    - Facebookへログインします
    - https://developers.facebook.com/apps へアクセスしAdd New Appのボタンをクリックします
    - Advanced Setupのリンクをクリックします
    - Display NameとCategoryを定義し、Create App IDをクリックします
    - 左側のナビゲーションからSettingsメニューをクリックします
    - Advancedタブをクリックします
    - Client OAuth Settings セクションの、 Valid OAuth redirect URIsに、該当するURLを入力します:
        http://[YOUR-HEROKU-APP-NAME].herokuapp.com/oauthcallback.html
        http://localhost:5000/oauthcallback.html (ローカルへのインストールの場合)
        https://www.facebook.com/connect/login_success.html (Cordovaからアクセスする場合)
    - Save Changes ボタンをクリックします

2. Nibsアプリの設定をFacebookアプリを使用するように設定します
    - nibs-jp/client/js/config.jsを開きます
    - YOUR\_FB\_APP\_ID を先程作成したFacebookアプリのIDで上書きまします

3. 変更をHerokuへPushします

    > 実際に動かすには、FBアプリケーションにpublish_actions及びread_streamの権限がなければ動どうさしません。動作を確認するには、FBアプリケーションのテストユーザ作成機能を利用します。

## Cordovaシェルのビルド

以下の手順を行うことで、アプリケーションをあなたのデバイスで動作させることができます:

1. Cordovaのインストール

    ```
    npm install -g cordova
    ```

    Macの場合には、sudoを使う必要があります:

    ```
    sudo npm install -g cordova
    ```

1. cordovaアプリケーションを作成

    ```
    cordova create nibs-shell com.nibs.loyalty Nibs
    ```

1. wwwフォルダ内のコンテンツを調整

    nibs/client 内のコンテンツを nibs-shell/www へ、もしくはnibs-shell内のwwwフォルダを削除して、nibs-shell/wwwにシンボリックリンクを作成します

    以下はMacの場合の例です:

    ```
    cd nibs-shell
    rm -rf www
    ln -s [path-to-nibs]/client www
    ```

1. Cordovaプラグインのインストール

    **nibs-shell** ディレクトリにいることを確認し、以下のコマンドを入力します:

    ```
    cordova plugins add org.apache.cordova.device
    cordova plugins add org.apache.cordova.console
    cordova plugins add org.apache.cordova.statusbar
    cordova plugins add org.apache.cordova.geolocation
    cordova plugins add org.apache.cordova.dialogs
    cordova plugins add org.apache.cordova.inappbrowser
    ```

3. プラットフォームを追加

    この例ではiOSのプラットフォームを追加します:

    ```
    cordova platforms add ios
    ```

4. プロジェクトをビルド

    この例では、iOSのプラットフォームへ向けてビルドをします:

    ```
    cordova build ios
    ```

5. エミュレータもしくはデバイス上でアプリケーションを動作させます。例えばiOSの場合では、XCodeの中でプロジェクトをオープン (platforms/ios/Nibs.xcodeproj)し、アプリをエミュレータもしくはiOSデバイスで実行します。
